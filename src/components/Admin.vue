<template>
    <div class="row">
        <div class="col-sm-12 col-md-8">
            <!-- new pizza -->
            <app-new-pizza></app-new-pizza>
        </div>
        <div class="col-sm-12 col-md-4">
            <!-- 品种展示 -->
            <h3 class="text-muted my-5">菜单</h3>
            <table class="table">
                <thead class="table table-default">
                    <tr>
                        <th>品种</th>
                        <th>删除</th>
                    </tr>
                </thead>
                <tbody v-for="item in getMenuItems" :key="item.name">
                    <tr>
                        <td>{{item.name}}</td>
                        <td>
                            <button @click="deleteData(item)" class="btn btn-outline-danger btn-sm">&times;</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
import NewPizza from './NewPizza' 
export default {
      data(){
        return{
            // getMenuItems:[]
        }
      },
      components:{
    'app-new-pizza':NewPizza
  } ,
  computed:{
      getMenuItems:{
        //在vuex中获取数据
        get(){
            // return this.$store.state.menuItems
            //通过getters获取数据
            return this.$store.getters.getMenuItems
        },
        set(){

        }
      },
  },
  created(){
    fetch("https://wd5781997662vdkstn.wilddogio.com/menu.json")
    .then(res => {
        return res.json()
    })
    .then(data => {
       let menuArray = []
       for(let key in data){
           data[key].id = key
           menuArray.push(data[key])
       } 
    //    this.getMenuItems = menuArray
    
    //数据同步
    this.$store.commit('setMenuItems',menuArray)
    })
  },
  methods:{
      deleteData(item){
          fetch("https://wd5781997662vdkstn.wilddogio.com/"+item.id+"/.json",{
              method:"DELETE",
              headers:{
                  'Content-type':'application/json'
              }
          })
          .then(res => res.json())
        //   .then(data => this.$router.push({name:"menuLink"}))
          .then(data => {
            this.$store.commit('removeMenuItems',item)
          })
          .catch(err => console.log(err))
      }
  }
}
</script>