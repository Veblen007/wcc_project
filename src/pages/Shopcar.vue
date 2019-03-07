<template>
	
	<div class="view">
		
		<div v-show="!$store.state.token" class="tip"> 
			您还没有登录,快去登录吧 <router-link to="/Login?url=/shopcar">login</router-link>
		 </div>

		 <div v-show="$store.state.token">
			
			<div v-if="list.length">
				
				<ul>
					<li v-for="(item,index) in list">
						{{item.title}} <button @click="del(item.Id,index)">X</button>
					</li>
				</ul>

			</div>
			<div class="tip" v-else>
				您的购物车空空如也,快去抢购商品吧  <router-link to="/">首页</router-link>
			</div>

		 </div>

	</div>

</template>
<script>
export default{
	data(){
		return{
			list:[]
		}
	},
	created(){

		if(this.$store.state.token){

			this.$http.get("/api/shoplist",{params:{token:this.$store.state.token}}).then((res)=>{
				this.list=res.data;
			})	

		}

	},
	methods:{
		del(id,index){
			this.$http.get("/api/del",{params:{token:this.$store.state.token,goodId:id}}).then((res)=>{
				if(res.data.code){
					alert("successful delete")

					this.list.splice(index,1)
				}else{
					alert("token expired")
				}
			})	
		}
	}
}
</script>
<style scoped>
	.view{
		width:1200px;
		margin:0 auto;
	}
	.tip{
		height: 500px;
		line-height:500px;
	}
</style>