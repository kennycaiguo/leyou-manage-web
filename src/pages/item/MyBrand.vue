<template>
  <v-card>
    <v-card-title>
      <v-btn color="primary" @click="show = true">
        新增品牌
      </v-btn>
      <v-spacer></v-spacer> <!--将新增与搜索分开-->
      <v-text-field
        label="请输入搜索条件"
        append-icon="search"
        single-line
        class="flex sm3"
        v-model.lazy="search"
      ></v-text-field>
    </v-card-title>
    <v-divider></v-divider>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" alt=""/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="justify-center layout px-0">
          <v-btn icon @click="editBrand(props.item)">
            <v-icon small>edit</v-icon>
          </v-btn>
          <v-btn icon @click="deleteBrand(props.item)">
            <v-icon small>delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
    <v-dialog v-model="show" max-width="500" scrollable persistent>
      <v-card >
        <v-card-title class="blue lighten-1 elevation-2">
          <span class="white--text title">
            新增品牌
          </span>
          <v-spacer/>
          <v-btn icon class="white--text" @click="show = false">
            <v-icon>close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text class="primary--text px-5">
          <my-brand-form/>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
  import MyBrandForm from "./MyBrandForm";

  export default {
    name: "MyBrand",
    components: {MyBrandForm},
    data() {
      return {
        /*表头*/
        headers: [
          {
            text: "id",  /*表头显示名称*/
            value: "id", /*对应字段名称*/
            align: 'center', /*居中*/
            sortable: true
          },
          {
            text: "名称",
            value: "name",
            align: 'center',
            sortable: false
          },
          {
            text: "LOGO",
            value: "image",
            align: 'center',
            sortable: false
          },
          {
            text: "首字母",
            value: "letter",
            align: 'center',
            sortable: true
          },
          {
            text: "操作",
            align: 'center'
          }
        ],
        brands: [],
        pagination: {},
        totalBrands: 0,
        loading: false,
        search: "",
        show: ""
      }
    },
    watch: {
      pagination: { // 监视pagination属性的变化
        deep: true, // deep为true，会监视pagination的属性及属性中的对象属性变化
        handler() {
          // 变化后的回调函数，这里我们再次调用getDataFromServer即可
          this.getDataFromServer();
        }
      },
      search: { // 监视搜索字段
        handler() {
          this.getDataFromServer();
        }
      }
    },
    created() {
      this.getDataFromServer()
    },
    methods: {
      getDataFromServer() {
        //开启进度条
        this.loading = true;
        //发起ajax请求
        this.$http.get("/item/brand/page", {
          params: {
            desc: this.pagination.descending,
            rows: this.pagination.rowsPerPage,
            page: this.pagination.page,
            sortBy: this.pagination.sortBy,
            key: this.search
          }
        }).then(resp => {
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
        }).catch();

        this.loading = false;
      }
    }
  }
</script>

<style scoped>

</style>
