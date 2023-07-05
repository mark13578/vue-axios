<script>
import axios from "axios";
import { onMounted, reactive, ref } from "vue";
export default {
  setup() {
    const employeeItme = ref({});
    const activeId = ref("");
    const isOpen = ref(false);

    const transformData = (itemArr) => {
      const map = {};
      for (const item of itemArr) {
        map[item.userId] = item;
      }
      return map;
    };

    const handleOpen = () => (isOpen.value = !isOpen.value);

    const searchEmployee = () => {
      if (employeeItme.value[activeId.value]) {
        handleOpen();
        return;
      }
      alert("查無此人");
      activeId.value = "";
    };

    onMounted(() => {
      axios
        .get("https://610cd85966dd8f0017b76eb5.mockapi.io/api/employee_list")
        .then((res) => {
          employeeItme.value = transformData(res.data);
        });
    });

    return {
      employeeItme,
      activeId,
      isOpen,
      handleOpen,
      searchEmployee,
    };
  },
};
</script>


<template>
  <div id="search">
    <input type="text" placeholder="請輸入編號" v-model="activeId" />
    <button @click="searchEmployee">查詢</button>
  </div>

  <ul class="item">
    <li v-for="item in employeeItme" :key="item.id">
      <img :src="item.avatar" alt="" />
      <div>
        <p>員工編號: {{ item.userId }}</p>
        <p>姓名: {{ item.username }}</p>
      </div>
    </li>
  </ul>

  <div v-if="isOpen" class="popup">
    <div class="card">
      <img :src="employeeItme[activeId]?.avatar" alt="" />
      <div>
        <p>員工編號: {{ employeeItme[activeId]?.userId }}</p>
        <p>姓名: {{ employeeItme[activeId]?.username }}</p>
      </div>
      <button id="close" @click="handleOpen">x</button>
    </div>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html,
body {
  width: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
}
#search {
  width: 270px;
  height: 30px;
  margin: 0 auto 30px auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  input {
    display: block;
    height: 20px;
    padding-left: 5px;
  }
  button {
    display: block;
    width: 80px;
    height: 20px;
  }
}
ul.item {
  width: 98%;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin: 0 auto;
  li {
    width: 33%;
    height: 70px;
    border: 1px solid #000;
    font-size: 12px;
    border-radius: 5px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding-left: 10px;
    margin-bottom: 5px;
    img {
      display: block;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      margin-right: 10px;
    }
    div > p {
      text-align: left;
    }
  }
}
.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba($color: #000000, $alpha: 0.9);
  z-index: 2;
  display: flex;
  justify-content: center;
  align-items: center;
  .card {
    position: relative;
    width: 33%;
    height: 70px;
    border: 1px solid #000;
    font-size: 12px;
    border-radius: 5px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding-left: 10px;
    margin-bottom: 5px;
    background-color: #fff;
    #close {
      position: absolute;
      right: 20px;
      display: block;
      width: 30px;
      height: 30px;
      font-size: 20px;
    }
    img {
      display: block;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      margin-right: 10px;
    }
    div > p {
      text-align: left;
    }
  }
}
</style>
