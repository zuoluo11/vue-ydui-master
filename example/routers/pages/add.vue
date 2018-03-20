<template>
    <yd-layout title="Slider">
        <yd-navbar slot="navbar" title="新增">
            <router-link to="/" slot="left">
                <yd-navbar-back-icon></yd-navbar-back-icon>
            </router-link>
        </yd-navbar>
        <yd-cell-group>
            <yd-cell-item>
                <span slot="left">日期：</span>
                <yd-input slot="right" type="date" required v-model="createDate" max="20" placeholder="请输入日期"></yd-input>
            </yd-cell-item>
            <yd-cell-item>
                <span slot="left">标题：</span>
                <yd-input slot="right" type="text" required v-model="title" ref="title" max="20" placeholder="请输入标题"></yd-input>
            </yd-cell-item>
            <yd-cell-item>
                <yd-textarea slot="right" v-model="content" placeholder="请输入您的内容" maxlength="100"></yd-textarea>
            </yd-cell-item>
            <yd-button-group>
                <yd-button size="large" @click.native="handleClick" type="primary">确定</yd-button>
            </yd-button-group>
        </yd-cell-group>
        <yd-tabbar slot="tabbar" :fixed="true">
            <yd-tabbar-item title="首页" link="/myindex">
                <i slot="icon" class="demo-icons-weixin"></i>
                <yd-badge slot="badge" type="danger">2</yd-badge>
            </yd-tabbar-item>
            <yd-tabbar-item title="今日" link="/">
                <i slot="icon" class="demo-icons-contact"></i>
            </yd-tabbar-item>
            <yd-tabbar-item title="全部" link="/allalarm" dot>
                <i slot="icon" class="demo-icons-discover"></i>
            </yd-tabbar-item>
            <yd-tabbar-item title="新增" link="/add" active>
                <i slot="icon" class="demo-icons-me"></i>
            </yd-tabbar-item>

        </yd-tabbar>
    </yd-layout>
</template>

<script type="text/babel">
import axios from "axios";
export default {
  data() {
    return {
      createDate: "",
      title: "",
      content: ""
    };
  },
  methods: {
    handleClick: function() {
      //   const input = this.$refs.title;
      //   console.log(this.$refs.title);
      //   var mes = `{<br />  valid：${input.valid}，<br />  errorMes：'${
      //     input.errorMsg
      //   }'，<br />  errorCode：'${input.errorCode}'<br />}`;
      //  this.$dialog.alert({mes: mes});
     
      const inputTitle = this.$refs.title;
    
      if (!inputTitle.valid) {
        this.$dialog.alert({ mes: inputTitle.errorMsg });
        return;
      }
     
      let data = {
        title: this.title,
        content: this.content,
        createDate: this.createDate
      };

      axios
        .post("http://192.168.1.236:3095/EasyRemember/add", data)
        .then(response => {
        //   console.log(response);
          if (response.data == "True") {
            this.$dialog.toast({
              mes: "添加成功",
              timeout: 1000,
              icon: "success",
              callback: () => {
                this.$router.push("/allalarm");
              }
            });
          }
        })
        .catch(error => {
          console.log(error);
          alert("网络错误，不能访问");
        });
      
    }
  }
};
</script>
<style scoped>

</style>
