<template>
  <div class="container">
    <h3>Alışveriş Listesi</h3>
    <hr />
    <div class="my-2">
      <input type="text" placeholder="Ne alacaksın?" @keydown.enter="onSave" />
    </div>
    <ul v-if="itemList.length > 0">
      <li v-for="item in itemList" :key="item.id" class="d-flex justify-content-between align-items-center">
        <span>{{ item.title }}</span>
        <button @click="onDelete(item)" class="sm red">Sil</button>
      </li>
    </ul>
    <div v-else class="bg-blue text-white">
      Herhangi bir kayıt bulunamadı..
    </div>
    <small class="text-red d-flex justify-content-end alignn-items-center mt-2">{{ itemCount }} Adet alınacak ürün vardır..</small>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      itemList: []
    };
  },
  mounted() {
    axios.get("http://localhost:3000/items").then(itemResponse => {
      console.log("itemResponse :>> ", itemResponse);
      this.itemList = itemResponse.data || [];
      console.log("this.itemList :>> ", this.itemList);
    });
  },
  methods: {
    onSave(e) {
      axios.post("http://localhost:3000/items", { title: e.target.value, created_at: new Date(), completed: false }).then(res => {
        console.log(res);
        this.itemList.push(res.data);
        e.target.value = "";
        e.target.focus();
      });
    },
    onDelete(item) {
      axios.delete(`http://localhost:3000/items/${item.id}`).then(res => {
        console.log(res);
        this.itemList = this.itemList.filter(i => i.id !== item.id);
      });
    }
  },
  computed: {
    itemCount() {
      return this.itemList.length || 0;
    }
  }
};
</script>
