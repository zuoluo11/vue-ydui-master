<template>
  <yd-layout>

    <yd-navbar slot="navbar" title="今日提醒">
      <router-link to="/" slot="left">
        <yd-navbar-back-icon></yd-navbar-back-icon>
      </router-link>
    </yd-navbar>

    <yd-infinitescroll :callback="loadList" ref="lsdemo">
      <yd-list theme="4" slot="list">
        <div v-for="(item,index) in list" class="card">
          <div class="card-header">
            <span style="float:left;">{{item.CreateDate}}{{item.Title}} </span>
          </div>
          <div class="card-content">
            <div>{{item.Content}}</div>
            <span>{{item.AlertDate1}}
              <yd-switch v-model="item.Checked1" @change.native="saveChecked(item.Guid,1)"></yd-switch>
            </span>
            <span>{{item.AlertDate2}}
              <yd-switch v-model="item.Checked2" @change.native="saveChecked(item.Guid,2)"></yd-switch>
            </span>
            <span>{{item.AlertDate3}}
              <yd-switch v-model="item.Checked3" @change.native="saveChecked(item.Guid,3)"></yd-switch>
            </span>
            <span>{{item.AlertDate4}}
              <yd-switch v-model="item.Checked4" @change.native="saveChecked(item.Guid,4)"></yd-switch>
            </span>
            <span>{{item.AlertDate5}}
              <yd-switch v-model="item.Checked5" @change.native="saveChecked(item.Guid,5)"></yd-switch>
            </span>
              <yd-button size="small" @click.native="handleClick(item.Guid)" type="warning">删除</yd-button>
          </div>


        </div>
      </yd-list>

      <!-- 数据全部加载完毕显示 -->
      <span slot="doneTip">啦啦啦，啦啦啦，没有数据啦~~</span>

    </yd-infinitescroll>

    <yd-backtop></yd-backtop>
    <!-- <yd-tabbar slot="tabbar" :fixed="true">
            <yd-tabbar-item title="首页" link="/myindex">
                <i slot="icon" class="demo-icons-weixin"></i>
                <yd-badge slot="badge" type="danger">2</yd-badge>
            </yd-tabbar-item>
            <yd-tabbar-item title="今日" link="/">
                <i slot="icon" class="demo-icons-contact"></i>
            </yd-tabbar-item>
            <yd-tabbar-item title="全部" link="/allalarm" active dot>
                <i slot="icon" class="demo-icons-discover"></i>
            </yd-tabbar-item>
            <yd-tabbar-item title="新增" link="/add" >
                <i slot="icon" class="demo-icons-me"></i>
            </yd-tabbar-item>

        </yd-tabbar> -->
  </yd-layout>
</template>

<script type="text/babel">
import axios from "axios";

export default {
  data() {
    return {
      switch1: true,
      page: 1,
      pageSize: 10,
      list: []
    };
  },
  mounted: function() {
    axios
      .post("http://192.168.1.236:3095/EasyRemember/GetListToday", {
        page: this.page,
        pagesize: this.pageSize
      })
      .then(response => {
        console.log(response);
        const _list = response.data;
        this.list = [...this.list, ..._list];
        this.list.forEach(element => {
          element.CreateDate = element.CreateDate.replace("00:00:00", "");
        });
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  methods: {
    saveChecked(alarmGuid, i) {
      axios
        .post("http://192.168.1.236:3095/EasyRemember/SaveChecked", {
          Guid: alarmGuid,
          AlarmNum: i
        })
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.log(error);
          alert("网络错误，不能访问");
        });
    },
    handleClick(alarmGuid) {
      axios
        .post("http://192.168.1.236:3095/EasyRemember/Delete", {
          Guid: alarmGuid
        })
        .then(response => {
          console.log(response.data);
          if (response.data == "True") {
            this.list = this.list.filter(o => o.Guid != alarmGuid);
          }
        })
        .catch(error => {
          console.log(error);
          alert("网络错误，不能访问");
        });
    },
    loadList() {
      axios
        .post("http://192.168.1.236:3095/EasyRemember/GetListToday", {
          page: this.page,
          pagesize: this.pageSize
        })
        .then(response => {
          console.log(response);
          const _list = response.data;
          this.list = [...this.list, ..._list];
          this.list.forEach(element => {
            element.CreateDate = element.CreateDate.replace("00:00:00", "");
          });
          if (_list.length < this.pageSize || this.page >= 4) {
            // 所有数据加载完毕
            this.$refs.lsdemo.$emit("ydui.infinitescroll.loadedDone");
            return;
          }

          // 单次请求数据完毕
          this.$refs.lsdemo.$emit("ydui.infinitescroll.finishLoad");

          this.page++;
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
.card {
  padding-bottom: 0.1rem;
  background: #fff;
  box-shadow: 0 0.05rem 0.1rem rgba(0, 0, 0, 0.3);
  margin: 0.1rem;
  position: relative;
  border-radius: 0.1rem;
  font-size: 0.4rem;
}
.card-header {
  /* min-height: 2.2rem; */
  position: relative;
  /* padding: 0.1rem 0.75rem; */
  padding: 0.2rem;
  box-sizing: border-box;
  display: -webkit-box;
  display: -webkit-flex;
  display: flex;
  -webkit-box-pack: justify;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  -webkit-box-align: center;
  -webkit-align-items: center;
  align-items: center;
  text-align: left;
}

.card-content {
  padding: 0.2rem;
  position: relative;
  text-align: left;
}
.card-content span {
  margin-top: 5px;
  display: inline-block;
}
.yd-button {
  text-align: right;
}
input {
  display: inline-block;
}
</style>



