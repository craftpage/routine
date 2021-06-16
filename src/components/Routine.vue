<template>
  <h1>ルーチン確認リスト</h1>
  <div class="input-group mb-3">  
    <span class="input-group-text" id="basic-addon1">Edit:</span>
    <input type="text" class="form-control" v-model="textVal"/><button class="btn btn-primary addbutton" v-on:click="add">add</button><br />
  </div>


  <ul class="list-group">
        <li class="list-group-item"
            v-for="item in items" 
            v-bind:key="item.id"
        >{{item.text}}
        <button 
        class="btn btn-secondary okbutton"
        v-if="item.done"
        v-on:click="done(item.id)">OK</button>
        </li>
    </ul>
  <br /><br />
  
  <div class="input-group mb-3">
    <span class="input-group-text" id="basic-addon1">Remove:</span>
    <button
    class="btn btn-primary"
    v-on:click="remove">remove
  </button></div>

  <div class="input-group mb-3">
    <span class="input-group-text" id="basic-addon1">Entry:</span>
    <button class="btn btn-dark" v-on:click="entry">entry</button>
  </div>


</template>

<script>
var listStorage = {
  fetch: function(obj) {
    let list = window.localStorage.getItem(obj)
    return list
  }
}

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      items: []
    }
  },
  created(){
    this.get()
  },
  mounted() {
    this.autoRemove()
  },
  watch:{
      items() {
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
      }
  },
  methods:{
    get() {
        if (JSON.parse(listStorage.fetch('items')) !== null) {
            this.items = JSON.parse(listStorage.fetch('items'))
        } else {
            this.items = [{"id":1, "text":"ルーチン", "done":false}]
        }
    },
    add(){
        this.items.push({
            "id": this.items.length + 1,
            "text":this.textVal,
            "done":true,
        })
    },
    entry(){
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
        alert("登録しました")
    },
    autoRemove(){
      let yesterday = null
      if (listStorage.fetch('date') !== (null || undefined)) {
        yesterday = Number(listStorage.fetch('date'))
      } else {
        yesterday = Number(new Date().getDate())
      }
      console.log(yesterday +" 11")
      let today = Number(new Date().getDate())
      if (yesterday !== today){
        for (let i = 1; i < Object.keys(this.items).length; i++){
          this.items[i].done = true
        }
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
      }
    },
    remove(){
      if (window.confirm("すべて削除しますか?")) {
        window.localStorage.removeItem("items")
        this.items = [{"id":1, "text":"ルーチン", "done":false}]        
      }     
    },
    done(id){
        this.items[id - 1]["done"] = false
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
        window.localStorage.setItem("date", new Date().getDate())
    }
  }
}
</script>