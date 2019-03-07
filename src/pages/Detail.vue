<template>
	
	<div>
		
		<h2>{{item.title}}</h2>
		<img :src="item.imageUrl" alt="">
		<p>商品買點{{item.salePoint}}</p>
		<p>商品價格{{item.priceStr}}</p>
		<p>喜歡{{item.nice}}</p>
		
		<button @click="add">立即搶購</button>

	</div>

</template>
<script>
export default{

	data(){
		return{
			item:{}
		}
	},
	created(){

		this.$store.commit("loading_show");

		this.$http.get("/api/detail",{params:{goodId:this.$route.query.goodId}}).then((res)=>{

			this.item=res.data[0];

			this.$store.commit("loading_hide");

		})

	},
	methods:{
		add(){

			if(this.$store.state.token){

				this.$http.get("/api/add",{params:{goodId:this.$route.query.goodId,token:this.$store.state.token}}).then((res)=>{

					if(res.data.code){

						this.$alert("ok","add goods");

					}else{

						this.$store.commit("update_token","");
						localStorage.removeItem("token");

						this.$alert("token expired,we will let you go to login page","add goods",{
							callback:()=>{
								this.$router.push("/login?url="+this.$route.fullPath)
							}
						});

					}

				})

			}else{
				alert("您好没有登录")
			}

		}
	}

}
</script>
<style scoped>
	
	button{
		width:200px;
		height:50px;
		border-radius: 25px;
		background: red;
		color:white;
		margin-top:50px;
		font-size:20px;
		outline: 0;
		box-shadow: 5px 5px 10px rgba(0,0,0,0.6);
		border:none;
		cursor: pointer;
	}
	
	button:active{
		box-shadow:0 0 0;
		transform: translate(5px,5px);
	}
</style>