<template>
  <h1>ルーチン確認リスト</h1>
  <span>Edit:</span><input type="text" v-model="textVal"/><button v-on:click="add">add</button><br />
  <span>Remove:</span><button v-on:click="remove">remove</button>

  <ul>
        <li
            v-for="item in items" 
            v-bind:key="item.id"
        >{{item.text}}
        <button v-if="item.done"
        v-on:click="done(item.id)">OK</button>
        </li>
    </ul>

  <button v-on:click="entry">entry</button><br />

</template>

<script>
var listStorage = {
  fetch: function(json) {
    let list = window.localStorage.getItem(json)
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
      items: [],
      confirm:[]
    }
  },
  mounted() {
    this.get();
  },
  watch:{
      items() {
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
      }
  },
  methods:{
    get() {
        if (JSON.parse(listStorage.fetch('items')) !== null){
            this.items = JSON.parse(listStorage.fetch('items'))
            this.date = JSON.parse(listStorage.fetch('date'))
        } else {
            this.items = [{"id":1, "text":"ルーチン", "done":ture}]
            this.date = [{"date": new Date()}]
        }
    },
    add(){
        this.items.push({
            "id": this.items.length + 1,
            "text":this.textVal,
            "done":true
        })
    },
    entry(){
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
        alert("登録しました")
    },
    remove(){
        window.localStorage.removeItem("items")
        this.items = [{"id":1, "text":"ルーチン", "done":true}]
    },
    done(id){
        this.items[id - 1]["done"] = false
        window.localStorage.setItem("items", JSON.stringify(this.items, null, 4))
    }
  }
}
</script>