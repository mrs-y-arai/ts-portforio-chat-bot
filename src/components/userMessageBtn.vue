<script setup lang="ts">
import { defineProps} from 'vue'
import { TypeMessageList } from '../App.vue'

// チャット画面のデータをPropsで受け取る。
type PropsMessageList = {
  messages: TypeMessageList[]
}
defineProps<PropsMessageList>()

// emitsで親の関数取得
const emits = defineEmits(['emitsUserMessageSubmit'])

// ユーザーコメントを、チャット画面に反映させる処理
const userMessageSubmit = ((to: string, event: any, url: any) => {  
  emits('emitsUserMessageSubmit', to, event, url)
})

// ポートフォリオサイトのお問い合わせフォームに送信するパラメーター
const recruitPram: number = 1
const businessPram: number = 2

</script>

<template>
  <div class="btn-wrap">
    <ul class="btn-list">
      <!-- ユーザーからのコメント -->
      <li
      class="btn-item"
      v-for="(message, index) in messages" :key="index"
      >
        <!-- 設定されているURLがお問い合わせページの場合 -->
        <form ref="form" v-if="message.url === 'https://smz97hiro.xsrv.jp/contact/'"
              class="btn-item__form" :action="message.url" 
              method="get" rel="noopener noreferrer" target="_blank">
          <input type="hidden" name="type"
                  :value="(message.content === '採用のお問い合わせをする') ? recruitPram : businessPram">
          <input class="btn" ref="submitBtn" type="submit" :value="message.content">
        </form>
        <!-- URLが設定されていない場合 -->
        <button v-else-if="!message.url" class="btn" @click="userMessageSubmit(message.type, $event, message.url)">
          {{ message.content }}
        </button>
        <!-- お問い合わせページ以外の何かしらのページURLが設定されている場合 -->
        <a class="btn" v-else  :href="message.url" target="_blank" rel="noopener noreferrer">
          {{ message.content }}
        </a>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.btn-wrap {
  width: 100%;
  height: 176px;
  box-sizing: border-box;
}
@media screen and (max-width: 600px) {
  .btn-wrap {
    height: 190px;
  }
}
.btn-list {
  width: 100%;
  display: flex;
  height: 100%;
  flex-direction: column;
  align-items: center;
}

.btn-item {
  width: 100%;
  height: 40px;
  margin-bottom: 4px;
}
@media screen and (max-width: 600px) {
  .btn-item {
    height: 41.5px;
    margin-bottom: 6px;
  }
}
.btn-item:last-of-type {
  margin: 0;
}

.btn-item__form {
  width: 100%;
  height: 40px;
}
.btn {
  background: transparent;
  border: 2px solid #e0b314;
  box-sizing: border-box;
  color: #e0b314;
  font-weight: 700;
  height: 100%;
  width: 90%;
  cursor: pointer;
  border-radius: 17px;
  margin: 0 auto;
  transition-duration: 0.5s;
}
@media screen and (max-width: 600px) {
  .btn {
    font-weight: 700;
    line-height: 35.5px;
  }
}

.btn:hover {
  color: #fff;
  background-color: #e0b314;
}
@media screen and (max-width: 600px) {
  .btn:hover {
    color: #e0b314;
    background-color: #fff;
  }
}
</style>