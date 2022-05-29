<template>
  <div class="home">
    <h1>商品検索(タグ)<br>localStrage</h1>
    <p>入力フォームに登録したタグ名を入力し、<br>SPACEを入れた後、追加のタグ名を入力すると絞り込み検索となります。</p>
    <p><input type="text" v-model="keyword"></p>
    <div class="search__wrapper">
      <ul>
        <li v-for="(shop, index) in searchFiltered" :key="index">
          <h2>{{ shop.name }}</h2>
          <p>{{ shop.tagName }}</p>
        </li>
      </ul>
    </div><!-- /.search__wrapper -->
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      searchProducts: [],//検索対象の全データ
      keyword: '',//タグ検索のinput値
      labels: []//tag登録したオブジェクト(idとtagName)
    }
  },
  computed: {
    //and検索
    searchFiltered() {
      //全データから絞り込み
      return this.searchProducts.filter( searchProduct => {
        if(this.keyword) {
          return this.keyword.toLowerCase()
          //①フォームで入力した検索タグ名をSPACEで分割して絞り込み、それぞれkwとして渡し
          .split(/\s+/)
          .map(kw => {
            //②フォームに入力したタグ名(kw)と一致する登録タグ名を検索し
            const searchLabel = this.labels.find(label => label.tagName === kw);
            if(searchLabel) {
              //③その登録タグ名が持つIDと一致するタグIDを持つデータを全データから検索する
              return searchProduct.selectedTags.includes(`${searchLabel.id}`);
            }
          })
          .every( result => result === true );
        }
      })
    },
    // searchFiltered() {
    //   return this.searchProducts.filter(searchProduct => {
    //     if(this.keyword) {
    //       return searchProduct.selectedTags.includes(this.keyword);
    //     }
    //   })
    // }
  },
  mounted() {
    if (localStorage.getItem('products')) {
      try {
        this.searchProducts = JSON.parse(localStorage.getItem('products'));
        this.labels = JSON.parse(localStorage.getItem('tags'));
      } catch(e) {
        localStorage.removeItem('products');
        localStorage.removeItem('tags');
      }
    }
  },
}
</script>


<style scoped>
input {
  font-size: 16px;
}
.search__wrapper {
  background-color: rgb(207, 244, 253);
  text-align: left;
  padding: 40px 60px;
}
</style>
