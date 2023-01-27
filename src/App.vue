<template>
  <div class="app">
    <div v-if="state.account.id">안녕하세요, {{ state.account.name }}님</div>
    <div v-else>
      <label for="loginId">
        <span>ID:</span>
        <input type="text" id="loginId" v-model="state.form.loginId" />
      </label>
      <label for="loginPW">
        <span>PW:</span>
        <input type="password" id="loginPw" v-model="state.form.loginPw" />
      </label>
      <button @click="submit">로그인</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { reactive } from "vue";
export default {
  name: "App",
  components: {},
  setup() {
    const state = reactive({
      account: {
        id: null,
        name: "",
      },
      form: {
        loginId: "",
        loginPw: "",
      },
    });

    const submit = () => {
      const args = {
        loginId: state.form.loginId,
        loginPw: state.form.loginPw,
      };

      axios
        .post("/api/account", args)
        .then((res) => {
          alert("로그인 성공");
          state.account = res.data;
          console.log(res.data);
        })
        .catch(() => {
          alert("로그인 실패, 계정 정보를 확인해주세요");
        });
    };

    //로그인 했을 시 계정 정보 가져오기
    axios.get("/api/account").then((res) => {
      console.log(res.data);
      state.account = res.data;
    });

    return { state, submit };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
