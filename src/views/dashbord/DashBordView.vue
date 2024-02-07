<template>
  <h2>這是後台</h2>
  <nav>
    <RouterLink to="/admin/product">產品列表</RouterLink> |
    <RouterLink to="/admin/order">訂單列表</RouterLink> |
    <RouterLink to="/">回到前台</RouterLink> |
  </nav>
  <RouterView></RouterView>
</template>

<script>
import axios from 'axios';

const { VITE_URL } = import.meta.env;

export default {
  data() {
    return {
      products: [],
      templateProduct: {},
    };
  },

  methods: {
    checkUser() {
      console.log(VITE_URL);
      const api = `${VITE_URL}/v2/api/user/check`;
      axios.post(api)
        .then((res) => {
          console.log(res, '成功');
          // console.log(this.$router);
        })
        .catch((err) => {
          console.log(err, '失敗');
          // 這裡有問題~ 當移除token時部會跳回登入頁面
          this.$router.push('/login');
        });
    },
  },
  mounted() {
    // 取出token
    const token = document.cookie.replace(
      /(?:(?:^|.*;\s*)hexSchool\s*=\s*([^;]*).*$)|^.*$/,
      '$1',
    );
    //  當axios 發出api請求時 自動全部將header加入上去
    axios.defaults.headers.common.Authorization = token;
    this.checkUser();
  },
};
</script>
