<template>
  <div class="main">	
  		<div class="main_up">
  			这是输入{{initWebSocket(2222,2222)}}
		</div>

	</div>
</template>
<script >
import { ref } from 'vue'
import axios from 'axios'


export default {
	name: 'HelloWorld',
	data(){
		return{
			dialogVisible: false,
			dialogTitle: '',
			loginName: '',
			textarea: "",
			list: [],
			curUserId: "",
			curUserName: "",
			curSessionId: '',
			sessionList_already:[],
			sessionList_not:[],
		}
	},
	methods: {
		//以下是使用客户端使用websocket
		initWebSocket: function (userId,sessionId) {                
					// WebSocket与普通的请求所用协议有所不同，ws等同于http，wss等同于https                
					this.websock = new WebSocket("ws://127.0.0.1:1997/websocket/"+userId+"/"+sessionId);                
					this.websock.onopen = this.websocketonopen;                
					this.websock.onerror = this.websocketonerror;                
					this.websock.onmessage = this.websocketonmessage;                
					this.websock.onclose = this.websocketclose;
				},              
		websocketonopen: function () {                
			console.log("WebSocket连接成功");    
		},              
		websocketonerror: function (e) { 
			console.log("WebSocket连接发生错误",e);              
		},              
		websocketonmessage: function (e) {  
			let data = JSON.parse(e.data);
			if(data instanceof Array){
				// 列表数据
				this.sessionList_already = data
			}else{
				// 消息数据
				this.list.push(data)
			}
		},              
		websocketclose: function (e) {
			if(this.curUserId != null){
				if(this.curSessionId != null){
					this.initWebSocket(this.curUserId, this.curSessionId)
				}else{
					this.initWebSocket(this.curUserId, 99999999)
				}
			}
			console.log("connection closed",e);     
			console.log(e);              
		}
	}//以上是websocket的使用
}

</script>



<style scoped>

</style>
