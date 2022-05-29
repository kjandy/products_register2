<template>
  <div id="main__wrapper">
    <main>
        <h2 class="mv__ttl">Local Strage<br>タグ登録</h2>
        <div>
          <input v-model="newtag" class="add_tag">
          <button @click="addTag">Add</button>
        </div>
        <div v-for="(tag, index) in tags" :key="index">
          <p>
            <span class="tag">{{ tag.tagName }}</span>
            <button @click="removeTag(index)">X</button>
          </p>
        </div>
    </main>
  </div><!--main_wrapper-->
</template>

<script>
export default {
  data() {
    return {
      tags: [],
      count: 0,
      newtag: null
    }
  },
  mounted() {
    if (localStorage.getItem('tags')) {
      try {
        this.tags = JSON.parse(localStorage.getItem('tags'));
      } catch(e) {
        localStorage.removeItem('tags');
      }
      //最後にpushしたtagの続きのID
      this.count = this.tags.slice(-1)[0].id + 1;
    }
  },
  methods: {
    addTag() {
      // 実際に何かしたことを入力する
      //空文字でaddした場合の対策
      if (!this.newtag) {
        return;
      }
      // this.tags.push(this.newtag);
      this.tags.push({
        id: this.count++,
        tagName: this.newtag
      })
      this.newtag = '';
      this.saveTag();
    },
    removeTag(x) {
      this.tags.splice(x, 1);
      this.saveTag();
    },
    saveTag() {
      const parsed = JSON.stringify(this.tags);
      localStorage.setItem('tags', parsed);
    }
  }
}
</script>


<style>
input {
  font-size: 16px;
}
.tag {
  width: 100px;
  display: inline-block;
}
.add_tag {
  border: solid 1px #CCC;
  padding: 0.4em;
  border-radius: 10px;
}
</style>
