<script setup lang="ts">
import { ref, Ref, watch } from 'vue'
import ChatList from './components/ChatList.vue'
import userMessageBtn from './components/userMessageBtn.vue'

// export
// 画面上部のチャットメッセージリストの型を定義
export type TypeChatList = {
  content: string,
  position: string
}

// 画面下部のメッセージリストの型定義
export type TypeMessageList = {
  type: string,
  content: string
  url?: string
}

// 画面上部に表示させるチャットのメッセージに表示場所を定義
const ownerPosition: string = 'left'
const userPosition: string = 'right'

// 画面上部に表示させるチャットのメッセージ内容
const messageList: Ref<TypeChatList[]> = ref([])

// オーナーが返信するメッセージの内容リストを定義
const ownerMessageList: TypeMessageList[] = [
  {
    type: 'default',
    content: '初めまして！ご用件は何でしょうか？'
  },
  {
    type: 'recruit',
    content: '採用のご検討、ありがとうございます！採用のお問い合わせは、以下のボタンからお願い致します。'
  },
  {
    type: 'business',
    content: 'お仕事のご依頼、ありがとうございます！お仕事のご依頼は、以下のボタンからお願い致します。'
  },
  {
    type: 'about',
    content: '経歴にご興味いただき、ありがとうございます！経歴の詳細は、以下のボタンからお願い致します。'
  },
  {
    type: 'works',
    content: '制作実績にご興味いただき、ありがとうございます！制作実績の詳細は、以下のボタンからお願い致します。'
  },
]

// オーナーが返信するメッセージの内容を、チャットに反映させる処理
const ownerSendMessage =((messageType: string) => {
  return new Promise((resolve: (value?: string) => void ) => {
    const message = ownerMessageList.filter((array) => array.type === messageType)
    const messageObject = {
      content: message[0].content,
      position: ownerPosition
    }
    messageList.value.push(messageObject)
    resolve()
  })
})

// デフォルトのオーナーメッセージを指定
ownerSendMessage('default')

// 画面下部に表示させるユーザーメッセージリストの定義
const userMessageList: Ref<TypeMessageList[]> = ref([])

// 画面下部に表示させるメッセージリストのステータス
const userMessageStatus: Ref<string> = ref('')

// 画面下部に表示させるメッセージリストのデフォルト
const defaultUserMessageList: TypeMessageList[] = [
  {
    type: 'recruit',
    content: '採用を検討しているのでお話をしたい',
  },
  {
    type: 'business',
    content: '仕事の依頼をしたい',
  },
  {
    type: 'about',
    content: '経歴を知りたい',
  },
  {
    type: 'works',
    content: '制作実績を見たい',
  },
]

// 採用のユーザーメッセージリスト
const recruitUserMessageList: TypeMessageList[] = [
  {
    type: 'outside',
    content: '採用のお問い合わせをする',
    url: 'https://smz97hiro.xsrv.jp/contact/'
  },
  {
    type: 'top',
    content: '最初に戻る',
  },
]

// 仕事のご依頼のユーザーメッセージリスト
const businessUserMessageList: TypeMessageList[] = [
  {
    type: 'outside',
    content: 'お仕事の依頼をする',
    url: 'https://smz97hiro.xsrv.jp/contact/'
  },
  {
    type: 'top',
    content: '最初に戻る',
  },
]

// 経歴のユーザーメッセージリスト
const aboutUserMessageList: TypeMessageList[] = [
  {
    type: 'outside',
    content: '経歴ページを見る',
    url: 'https://smz97hiro.xsrv.jp/about/'
  },
  {
    type: 'top',
    content: '最初に戻る',
  },
]

// 制作実績のユーザーメッセージリスト
const worksUserMessageList: TypeMessageList[] = [
  {
    type: 'outside',
    content: '制作実績ページを見る',
    url: 'https://smz97hiro.xsrv.jp/works/'
  },
  {
    type: 'top',
    content: '最初に戻る',
  },
]

// デフォルトのユーザーメッセージリストを設定
userMessageList.value = defaultUserMessageList

// ユーザーが選択したメッセージによって、次に表示するメッセージの内容を変更する
watch(userMessageStatus, () => {
  switch(userMessageStatus.value) {
    case 'recruit':
      userMessageList.value = recruitUserMessageList
      break
    
    case 'business':
      userMessageList.value = businessUserMessageList
      break

    case 'about':
      userMessageList.value = aboutUserMessageList
      break

    case 'works':
      userMessageList.value = worksUserMessageList
      break

    case 'top':
      userMessageList.value = defaultUserMessageList
      break

    default:
      userMessageList.value = defaultUserMessageList
      break
  }
})

// 画面上部のチャットに、ユーザーメッセージを反映させる処理
const userDisplayMessage = ((message: string, position: string = userPosition) => {
  const messageData: TypeChatList = {
    content: message,
    position: position,
  }  
  messageList.value.push(messageData)
})

// ユーザーメッセージ送信処理
const userMessageSubmit = ((to: string, event: any, url: any) => {
  // 最初に戻る場合、ユーザーのメッセージをチャットに表示しない
  if( to === 'top' ) {
    // 画面下部のメッセージが切り替わる
    userMessageStatus.value = to
    return
  }

  // ユーザーのメッセージをチャットに表示
  userDisplayMessage(event.target.textContent)

  // 画面上部のチャットに、オーナーのメッセージを反映
  setTimeout(async() => {
    await ownerSendMessage(to)
    .then(() => {
      console.log('返信成功')
      // 画面下部のメッセージが切り替わる
      userMessageStatus.value = to
    })
    .catch(() => {
      console.log('返信失敗')
    })
  }, 500)

})

</script>

<template>
  <div class="chat-wrap">
    <ChatList :messages="messageList" />
    <userMessageBtn @emitsUserMessageSubmit="userMessageSubmit" :messages="userMessageList" />
  </div>  
</template>

<style scoped>
.chat-wrap {
  border: 3px solid #e0b314;
  margin: 0 auto;
  width: 470px;
  height: 680px;
  border-radius: 50px;
  padding: 1.3%;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  align-items: center;
  overflow: hidden;
}
@media screen and (max-width: 600px) {
  .chat-wrap  {
    width: 90vw;
    height: 95vh;
    padding: 4% 2%;
  }
}
</style>
