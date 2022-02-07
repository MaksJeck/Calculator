<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <calculator />
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue';


export default {
  name: "App",
  components: {
    Calculator
  },
  data() {
    return {     
      modalWindowName: '',
      page: "",
      settings: {}
     
    };
  },
  
  methods: {
    setPage() {
      this.page = location.pathname.slice(1);
    },
    
    onShown(settings){
      this.modalWindowName = settings.name
      this.settings = settings
    },
    onHide(){
      this.modalWindowName = ''
      this.settings = {}
    }
  },
  mounted() {
    this.$modal.EventBus.$on('show', this.onShown);
    this.$modal.EventBus.$on('hide', this.onHide);
    const links = document.querySelectorAll("a");
    links.forEach((link) => {
      link.addEventListener("click", (event) => {
        event.preventDefault();
        history.pushState({}, '', link.href);
        this.setPage();
      });
    });
    this.setPage();
    window.addEventListener("popstate", this.setPage);
  },
  beforeDestroy() {
    window.removeEventListener("popstate", this.setPage);
    this.$modal.EventBus.$off('show', this.onShown);
    this.$modal.EventBus.$off('hide', this.onHide);
  },
  // fetchData() {
  //   return [
  //     {
  //       id: "1",
  //       date: "28.03.2020",
  //       category: "Food",
  //       value: 169,
  //     },
  //     {
  //       id: "2",
  //       date: "24.03.2020",
  //       category: "Transport",
  //       value: 360,
  //     },
  //     {
  //       id: "3",
  //       date: "24.03.2020",
  //       category: "Food",
  //       value: 532,
  //     },
  //   ];
  // },

  created() {
    this.$modal.show();
    this.$modal.hide();
    // this.fetchData(this.curPage);
    // this.fetchData();
     this.$store.dispatch('fetchData');
    //this.myMutation(this.fetchData());
    //this.$store.commit('setPaymentsListData', this.fetchData());
    //this.paymentsList = this.fetchData();
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
  margin-top: 60px;
}
</style>
