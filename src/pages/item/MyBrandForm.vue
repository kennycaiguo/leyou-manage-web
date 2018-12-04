<template>
  <v-form v-model="valid" ref="brandForm">
    <v-text-field required v-model="brand.name" label="品牌名称"
    :rules="[ v => !!v || '品牌名称不能空']"/>
    <v-text-field required v-model="brand.letter" label="品牌首字母(大写字母)"
     :rules="[ v => !!v || '品牌首字母不能空']"
      mask="A"/>
    <v-cascader
      url="/item/category/list"
      multiple
      required
      v-model="brand.categories"
      label="请选择商品分类"/>
    <v-layout>
      <v-flex xs4 class="subheader">品牌logo：</v-flex>
      <v-flex>
        <v-upload
          v-model="brand.image"
          url="/upload"
          :multiple="false"
        />
      </v-flex>
    </v-layout>
    <v-layout row class="pr-0 mt-5">
      <v-spacer/>
      <v-btn color="primary" @click="submit">提交</v-btn>
      <v-btn color="warning">重置</v-btn>
    </v-layout>
  </v-form>
</template>

<script>
  export default {
    name: "MyBrandForm",
    data(){
      return{
        valid:false,
        brand: {
          name: "",
          letter: "",
          image:"",
          categories: []
        }
      }
    },
    methods:{
      submit(){
        if (this.$refs.brandForm.validate()) {
          // 定义一个请求参数对象，通过解构表达式来获取brand中的属性
          const {categories, ...rest} = this.brand;
          // 数据库中只要保存分类的id即可，因此我们对categories的值进行处理,只保留id，并转为字符串
          rest.categories = categories.map(c => c.id).join(",");
          //将对象转换成字符串 让axios提交表单类型的请求  而不是json请求
          this.$http.post("/item/brand/save",this.$qs.stringify(rest)).then(resp =>{
            console.log(resp);
          }).catch(resp=>{
            alert(resp);
            console.log("新增失败");
          })
        }
      }
    }
  }
</script>

<style scoped>

</style>
