<template>
	
	<div class="view">
		
		<div class="btn">
			<div class="btn_c" :class="{con:flag}" @click="flag=true">login</div>
			<div class="btn_c" :class="{con:!flag}" @click="flag=false">register</div>
		</div>

		<!-- 登录 -->
		<div class="group" v-show="flag">
			<p>
				<input type="text" v-model="login_user" placeholder="userName">
			</p>
			<p>
				<input type="password" v-model="login_pwd" placeholder="password">
			</p>
			<p>
				<button @click="login">login</button>
			</p>
		</div>
		
		<!-- 注册 -->
		<div class="group" v-show="!flag">
			
			<p>
				<input type="text" v-model="reg_user" placeholder="userName">
			</p>
			<p>
				<input type="password" v-model="reg_pwd" placeholder="password">
			</p>
			<p>
				<button @click="reg">register</button>
			</p>

		</div>

	</div>

</template>
<script>
export default{
	data(){
		return{
			flag:true,
			reg_user:"",
			reg_pwd:"",
			login_user:"",
			login_pwd:""
		}
	},
	methods:{
		login(){

			if(!(/^.{6,12}$/.test(this.login_user))){

				this.$alert("userName is wrong","check",{
					confirmButtonText: 'I see'
				})

				return false;

			}

			if(!(/^\w{6,12}$/.test(this.login_pwd))){

				this.$alert("password is wrong","check",{
					confirmButtonText: 'I see'
				})

				return false;

			}
			
			this.$store.commit("loading_show");

			this.$http.get("/api/login",{params:{userName:this.login_user,password:this.login_pwd}}).then((res)=>{

				this.$store.commit("loading_hide");

				console.log(res.data)

				if(res.data.code){
					// 登录成功 在vuex内部存储token以便被其他组件访问
					this.$store.commit("update_token",res.data.token)

					this.$alert("very good","response",{
						confirmButtonText: 'I see',
						callback:()=>{
							if(this.$route.query.url){
								this.$router.push(this.$route.query.url)
							}else{
								this.$router.push("/")
							}
						}
					})

				}else{

					this.$alert("sorry,err","response",{
						confirmButtonText: 'I see'
					})

				}

			})


		},
		reg(){

			if(!(/^.{6,12}$/.test(this.reg_user))){

				this.$alert("userName is wrong","check",{
					confirmButtonText: 'I see'
				})

				return false;

			}

			if(!(/^\w{6,12}$/.test(this.reg_pwd))){

				this.$alert("password is wrong","check",{
					confirmButtonText: 'I see'
				})

				return false;

			}
			
			this.$store.commit("loading_show");

			this.$http.get("/api/register",{params:{userName:this.reg_user,password:this.reg_pwd}}).then((res)=>{

				this.$store.commit("loading_hide");

				if(res.data.code){
					// code值为1,证明注册成功,为0注册失败
					this.$alert("very good,go to login","response",{
						confirmButtonText: 'I see',
						callback:()=>{
							this.flag=true
						}
					})

				}else{

					this.$alert("sorry,we had","response",{
						confirmButtonText: 'I see'
					})

				}

			})

		}
	}
}
</script>
<style scoped>
	.group button{
		width: 300px;
		height: 50px;
		background: red;
		font-size:16px;
		color:white;
		outline: 0;
	}

	

	.group input{
		width:500px;
		height:50px;
		text-indent: .5em;
		border-radius: 25px;
		border:2px solid #999;
		outline: 0
	}

	.group input:focus{
		border:2px solid orange;
	}
	.group{
		margin-top:100px;
	}
	.group p{
		text-align: center;
		margin-top:20px;
	}
	.btn .btn_c{
		width:400px;
		height:50px;
		float:left;
		line-height: 50px;
		background: #999;
		cursor: pointer;
		color:white;
	}
	.btn .con{
		background:white;
		color:black;
	}
	.btn{
		overflow: hidden;
	}
	.view{
		width:800px;
		height: 500px;
		background: white;
		margin:100px auto;
	}

</style>