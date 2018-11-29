<template>
  <v-card>
    <v-card-title>
      <v-btn color="primary">
        新增品牌
      </v-btn>
      <v-spacer></v-spacer> <!--将新增与搜索分开-->
      <v-text-field
        label="请输入搜索条件"
        append-icon="search"
        single-line
        class="flex sm3"
        v-model="search"
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
        <td class="justify-center layout pt-3">
          <v-icon color="info"
                  small
                  class="mr-2"
                  @click="editItem(props.item)"
          >
            edit
          </v-icon>
          <v-icon color="error"
                  small
                  @click="deleteItem(props.item)"
          >
            delete
          </v-icon>
        </td>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
  export default {
    name: "MyBrand",
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
            value: "name",
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
        search:""
      }
    },
    watch: {
      pagination:{
        deep: true,
        handle(){
          this.getDataFormServer();
        }
      },
      search(){
          this.getDataFormServer();
      }
    },
    methods: {
      getDataFormServer() {
        //开启进度条
        this.loading = true;

      /* - 请求方式：查询，肯定是Get

        - 请求路径：分页查询，/brand/page
        - 请求参数：根据我们刚才编写的页面，有分页功能，有排序功能，有搜索过滤功能，因此至少要有5个参数：
        - page：当前页，int
        - rows：每页大小，int
        - sortBy：排序字段，String
        - desc：是否为降序，boolean
        - key：搜索关键词，String

        - 响应结果：分页结果一般至少需要两个数据
        - total：总条数
        - items：当前页数据
        - totalPage：有些还需要总页数*/
        this.loading = false;
      }
    },
    created() {
      this.getDataFormServer()
    }
  }
</script>

<style scoped>

</style>
