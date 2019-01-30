<template>
  <div class="login">
    <el-card class="xfn-login-card">
       <div slot="header">管理员登录</div>
       <div>
         <el-form label-width="100px">
            <el-form-item label="管理员:">
               <el-input v-model="formData.aname" placeholder="请输入管理员名"></el-input> 
            </el-form-item>

             <el-form-item label="登录密码:">
               <el-input type="password" v-model="formData.apwd" placeholder="请输入密码"></el-input> 
            </el-form-item>
            <el-form-item>
            <!--@click.native-->
              <el-button type="primary" @click.native="doLogin">登录</el-button>
              <el-button @click="doCancel">取消</el-button>
            </el-form-item>
         </el-form>
       </div>
    </el-card>
    {{$store.state.globalSettings}}
  </div>
</template>

<script>
  export default{
    data(){      //普通组件的模型数据是函数的返回值
      return{
        formData:{  //表单中用户输入的两个数据
          aname:'',
          apwd:''
        }
      }
    },
    methods:{
      doLogin(){//执行登录
      var url=this.$store.state.globalSettings.apiUrl+`/admin/login/${this.formData.aname}/${this.formData.apwd}`
      // console.log(url);
      this.$axios.get(url).then((res)=>{
        if(res.data.code==200){//登录成功
          //把用户名存到Vuex存储仓库
          this.$store.commit('setAdminName',this.formData.aname);
          //执行视图跳转
          this.$router.push('/main');
        }else{//登录失败
          this.$alert('用户名或密码错误','登录失败',{ type:'error'}).then(()=>{}).catch(()=>{});
        }
      }).catch((err)=>{
        console.log(err);
      })

      },
      doCancel(){//清除用户的输入
      this.formData.aname='';
      this.formData.apwd='';

      }
    }
  }
</script>

<style lang="scss">
  .xfn-login-card{
    width:450px;
    margin:160px auto;
    .el-card__header{ //样式嵌套
      text-align:center;
      font-size:1.5em;
    }

  }
  
  
</style>
