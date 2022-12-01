<script setup lang="ts">
import { ref, defineProps, onUpdated } from 'vue'
import { TypeChatList } from '../App.vue'

type  PropsChatList = {
  messages: TypeChatList[]
}
defineProps<PropsChatList>()

const chatList: any = ref(null)

onUpdated(() => {
  // チャット更新時、最下部までスクロール
  chatList.value.scrollTop = chatList.value.scrollHeight 
})

</script>

<template>
  <!-- 画面上部のチャット部分 ---leftクラスが付くと、左側にコメントが表示され、
    ---rightクラスが付くと、右側にコメントが表示される。 -->
  <ul ref="chatList" class="chat-list"> 
    <li class="chat-item"
        v-for="(message, i) in messages" :key="i"
        v-bind:class="{ '---left': message.position === 'left',
                        '---right': message.position === 'right', }">
      <div class="chat-item__icon-wrap"></div>
      <div v-html="message.content" class="chat-box__bubble"></div> 
    </li>
  </ul>
</template>

<style scoped>
.chat-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 450px;
  height: 410px;
  overflow-y: scroll;
  border-radius: 50px;
  box-sizing: border-box;
  padding: 1.5%;
  margin-bottom: 20px;
}
@media screen and (max-width: 600px) {
  .chat-list {
    width: 100%;
    height: 65%;
    margin-bottom: 25px;
  }
}
.chat-item {
  display: flex;
  align-items: center;
  margin-bottom: 30px;
  width: 100%;
  height: auto;
}
@media screen and (max-width: 600px) {
  .chat-item {
    margin-bottom: 15px;
  }
}
.chat-item.---right {
  flex-direction: row-reverse;
}
.chat-item:last-of-type {
  margin-bottom: 0;
}

.chat-item__icon-wrap {
  background-color: #e0b314;
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
}
@media screen and (max-width: 600px) {
  .chat-item__icon-wrap {
    width: 50px;
    height: 50px;
  }
}

.chat-item.---left .chat-item__icon-wrap {
  margin-right: 10px;
  background-image: url('../assets/profile-img.jpg');
  background-size: cover;
}
@media screen and (max-width: 600px) {
  .chat-item.---left .chat-item__icon-wrap {
    margin-right: 5px;
  }
}

.chat-item.---right .chat-item__icon-wrap {
  margin-left: 10px;
  background-image: url('../assets/person-icon.png');
  background-size: 78%;
  background-position: center;
  background-repeat: no-repeat;
}
@media screen and (max-width: 600px) {
  .chat-item.---right .chat-item__icon-wrap {
    margin-left: 5px;
  }
}

.chat-box__bubble {
  background-color: #e0b314;
  color: #fff;
  padding: 3%;
  text-align: start;
  border-radius: 10px;
  width: auto;
  max-width: 300px;
}
@media screen and (max-width: 600px) {
  .chat-box__bubble {
    max-width: 65%;
  }
}
</style>
