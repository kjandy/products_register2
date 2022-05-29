<template>
  <div id="main__wrapper">
    <main>
      <h2 class="mv__ttl">Local Strage<br>商品登録</h2>
      <div class="product__register">
        <div class="tagselect__wrapper">
          <h3>タグ選択</h3>
          <select v-model="selectedTags" name="tags" id="tags" multiple>
            <option v-for="registeredTag in registeredTags" :key="registeredTag.id">{{registeredTag.id}}</option>
          </select>
        </div><!--tagselect__wrapper-->
        <br><br>
        <div class="product__name">
          <label>商品名：<input type="text" v-model="productName"></label>
        </div><!--product__name-->
        <br><br>
        <div class="product__txt">
          <p><label for="product__txt">商品説明：</label></p>
          <textarea id="product__txt" v-model="productText" class="add_todo" rows="10" cols="60"></textarea>
        </div><!--product__txt-->
      </div><!--product__register-->
      <br><br>
      <button @click="addProduct">登録</button>
      <br><br><br><br>
      <h2 class="mv__ttl">Local Strage<br>登録済み商品リスト</h2>
      <div class="product__lists">
        <div v-for="(product, index) in products" :key="index">
          <h3>{{ product.name }}</h3>
          <p>{{ product.txt }}</p>
          <ul>
            <li v-for="(relatedTags, index) in product.selectedTags" :key="index">
              {{ relatedTags }}
            </li>
          </ul>
          <div class="removeProduct">
            <p>削除</p>
            <button @click="removeProduct(index)">X</button>
          </div>
        </div>
      </div><!--product__lists-->

    </main>
  </div><!--main_wrapper-->
</template>

<script>
export default {
  data() {
    return {
      products: [],
      newProduct: {},
      registeredTags: [],
      selectedTags: [],
      productName: '',
      productText: ''
    }
  },
  created() {
    if (localStorage.getItem('tags')) {
      try {
        this.registeredTags = JSON.parse(localStorage.getItem('tags'));
      } catch(e) {
        localStorage.removeItem('tags');
      }
    }
  },
  mounted() {
    // console.log(this.selectedTags);
    if (localStorage.getItem('products')) {
      try {
        this.products = JSON.parse(localStorage.getItem('products'));
      } catch(e) {
        localStorage.removeItem('products');
      }
    }
  },
  methods: {
    addProduct() {
      // 実際に何かしたことを入力する
      //空文字でaddした場合の対策
      if (!this.productName) {
        return;
      }
      //新規追加用の各プロパティ
      this.newProduct = {
        selectedTags: this.selectedTags,
        name: this.productName,
        txt: this.productText
      }
      //新規データの追加
      this.products.push(this.newProduct);
      this.saveProduct();
      this.selectedTags = [];
      this.productName = '';
      this.productText = '';
    },
    removeProduct(x) {
      this.products.splice(x, 1);
      this.saveProduct();
    },
    saveProduct() {
      const parsed = JSON.stringify(this.products);
      localStorage.setItem('products', parsed);
    }
  }
}
</script>


<style scoped>
input, textarea {
  font-size: 16px;
}
select {
  width: 4em;
}
.product__register {
  background-color: #EEE;
  text-align: left;
  padding: 40px 60px;
}
.product__lists {
  background-color: rgb(207, 244, 253);
  text-align: left;
  padding: 40px 60px;
}
.product__lists > div {
  margin-bottom: 4em;
}
</style>
