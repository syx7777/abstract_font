<template>
  <div class="home">
    <h1>文本内容</h1>
    <div class="text">{{ data }}</div>
    <button @click="sendMsg()" class="btn">生成摘要</button>
    <h2>摘要</h2>
    <div v-if="summary.value" class="text" style="height: 300px;margin-bottom: 20px;">{{ summary }}</div>
    
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import { ref } from 'vue';
const socket = ref(null)
const summary = ref('')

const created = ()=>{
    // 指定服务器地址及端口号
    socket.value = new WebSocket("ws://localhost:8765");

    socket.value.onopen = () => {
      console.log("WebSocket连接已打开");
    };

    socket.value.onmessage = (event) => {
      summary.value = event.data;
      console.log("收到回复：" + event.data);
    };

    socket.value.onclose = () => {
      console.log('WebSocket连接已关闭');
    };
    console.log(socket.value)
  }

const data = " New York (CNN)When Liana Barrientos was 23 years old, she got married in Westchester County, New York.\
A year later, she got married again in Westchester County, but to a different man and without divorcing her first husband.\
Only 18 days after that marriage, she got hitched yet again. Then, Barrientos declared 'I do' five more times, sometimes only within two weeks of each other.\
In 2010, she married once more, this time in the Bronx. In an application for a marriage license, she stated it was her 'first and only' marriage.\
Barrientos, now 39, is facing two criminal counts of 'offering a false instrument for filing in the first degree', referring to her false statements on the\
2010 marriage license application, according to court documents.\
Prosecutors said the marriages were part of an immigration scam.\
On Friday, she pleaded not guilty at State Supreme Court in the Bronx, according to her attorney, Christopher Wright, who declined to comment further.\
After leaving court, Barrientos was arrested and charged with theft of service and criminal trespass for allegedly sneaking into the New York subway through an emergency exit, said Detective\
Annette Markowski, a police spokeswoman. In total, Barrientos has been married 10 times, with nine of her marriages occurring between 1999 and 2002.\
All occurred either in Westchester County, Long Island, New Jersey or the Bronx. She is believed to still be married to four men, and at one time, she was married to eight men at once, prosecutors say.\
Prosecutors said the immigration scam involved some of her husbands, who filed for permanent residence status shortly after the marriages.\
Any divorces happened only after such filings were approved. It was unclear whether any of the men will be prosecuted.\
The case was referred to the Bronx District Attorney\'s Office by Immigration and Customs Enforcement and the Department of Homeland Security\'s\
Investigation Division. Seven of the men are from so-called 'red-flagged' countries, including Egypt, Turkey, Georgia, Pakistan and Mali.\
Her eighth husband, Rashid Rajput, was deported in 2006 to his native Pakistan after an investigation by the Joint Terrorism Task Force.\
If convicted, Barrientos faces up to four years in prison.  Her next court appearance is scheduled for May 18.\
"

const sendMsg = () => {
  console.log('发送消息')
  if (socket.value && socket.value.readyState === WebSocket.OPEN) {
    socket.value.send(data);
  } else {
    console.error('WebSocket连接尚未建立或已关闭');
  }
}

onMounted(()=>{
    created()
})
</script>

<style scoped>

.text {
  width: 702px;
  height: 440px;
  text-align: center;
  margin: 0 auto ;
  padding: 24px;
  border-radius: 16px;
  background: #FFFFFF;
  box-shadow: 0px 3px 8px 0px rgba(0, 0, 0, 0.16);
}

.btn{
  margin-top: 20px;
  width: 100px;
  height: 48px;
  font-size: medium;
  border-radius: 8px;
  background: #FFFFFF;
  box-sizing: border-box;
  /* 主要色/主要色 */
  border: 1px solid #0256FF;
}

</style>
