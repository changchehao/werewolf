<template lang="pug">
	#app
		.modal(v-show="rolelistModal||numberlistModal == true")
			.wrap
				.rolelist(v-show="rolelistModal == true")
					label(v-for="roletag in roletags")
						input(type="radio" :value="roletag.title" v-model="persons[numberClick].role")
						.roletag.big {{roletag.title}}
				.numberlist(v-show="numberlistModal == true")
					.title 警徽流
					label(v-for="(person,pid) in persons") 
						input(type="checkbox" :value="pid+1" v-model="persons[numberClick].daysinfo[dayfocus-1].wantSeek")
						.numbertag.big {{pid+1}}
					.title 打誰
					label(v-for="(person,pid) in persons") 
						input(type="checkbox" :value="pid+1" v-model="persons[numberClick].daysinfo[dayfocus-1].hit")
						.numbertag.big {{pid+1}}
					.title 保誰
					label(v-for="(person,pid) in persons") 
						input(type="checkbox" :value="pid+1" v-model="persons[numberClick].daysinfo[dayfocus-1].hold")
						.numbertag.big {{pid+1}}
					.title 查殺
					label(v-for="(person,pid) in persons") 
						input(type="checkbox" :value="pid+1" v-model="persons[numberClick].daysinfo[dayfocus-1].getWolf")
						.numbertag.big {{pid+1}}
					.title 金水
					label(v-for="(person,pid) in persons") 
						input(type="checkbox" :value="pid+1" v-model="persons[numberClick].daysinfo[dayfocus-1].getVillager")
						.numbertag.big {{pid+1}}
					.title 其他描述
					textarea.description( v-model="persons[numberClick].daysinfo[dayfocus-1].other")
			.floatbtn
				button(@click="closeModal") 確定

		.contentwrap
			.person(v-for="(person,pid) in persons")
				.left-section
					.id(@click="roleChecked(pid)")
						.number {{pid+1}}
						.roletag {{person.role}}
					.campaign(@click="person.campaign=!person.campaign")
						.campaigntag(v-show="person.campaign == true") ★
						.campaigntag(v-show="person.campaign == false") ☆
				.right-section(@click="numberChecked(pid)")
					.td.wantSeek(v-show="person.daysinfo[dayfocus-1].wantSeek.length") 警徽流:
						.numbertag(v-for="wantSeek in person.daysinfo[dayfocus-1].wantSeek") {{wantSeek}}
					.td.hit(v-show="person.daysinfo[dayfocus-1].hit.length") 打:
						.numbertag(v-for="hit in person.daysinfo[dayfocus-1].hit") {{hit}}
					.td.hold(v-show="person.daysinfo[dayfocus-1].hold.length") 保:
						.numbertag(v-for="hold in person.daysinfo[dayfocus-1].hold") {{hold}}
					.td.getwolf(v-show="person.daysinfo[dayfocus-1].getWolf.length") 查殺:
						.numbertag(v-for="getWolf in person.daysinfo[dayfocus-1].getWolf") {{getWolf}}
					.td.getvillage(v-show="person.daysinfo[dayfocus-1].getVillager.length") 金水:
						.numbertag(v-for="getVillager in person.daysinfo[dayfocus-1].getVillager") {{getVillager}}
					.td.other(v-show="person.daysinfo[dayfocus-1].other.length") 描述:
						.otherword(v-html="person.daysinfo[dayfocus-1].other")
</template>

<script>
var persons = Array.from({length:12} , function(){
	let x = {
    campaign: false,
    role:"",
    daysinfo:[
      {
          wantSeek: [],
          hit:[],
          hold:[],
          getWolf:[],
          getVillager:[],
          other:""
      },
      {
          wantSeek:[],
          hit:[],
          hold:[],
          getWolf:[],
          getVillager:[],
          other:""
      },
      
    ],
	}
	return x;
}	
);
export default {
  name: 'app',
  data () {
    return {
      persons:persons,
      dayfocus:1,
      rolelistModal:false,
      numberlistModal:false,
      days:[
        {title:1},
        {title:2},
        {title:3},
        {title:4},
      ],					
      roletags:[
        {title:'預'},
        {title:'女'},
        {title:'獵'},
        {title:'騎'},
        {title:'守'},
        {title:'混'},
        {title:'好人'},
        {title:'偏好'},
        {title:'偏狼'},
        {title:'狼'},
        {title:'狼王'},
        {title:'金'},
        {title:'銀'},
        {title:'銅'},	
      ],
      roleChoose:"",
      numberChoose:[1,2,3],
      numberClick:0,
    }
  },
  methods:{
    	roleChecked(id){
			this.numberClick=id,
			this.rolelistModal=true;
		},
		numberChecked(id){
			this.numberClick=id,
			this.numberlistModal=true;
		},
		closeModal(){
			this.rolelistModal=false;
			this.numberlistModal=false;
		}
  }
}
</script>

<style lang="scss">
html,body,#app{
	font-family: 微軟正黑體,sans-serif;
	width: 100%;
	height: 100%;
}
.title{
	color: white;
	padding: 10px 0;
}
.description{
	width: 100%;
	height: 100px;
}
label{
	input{
		display: none;
	}
	input:checked + *{
		box-shadow: 0 0px 2px 2px #fff;
	}
}
.floatbtn{
	position: absolute;
	bottom: 0px;
	height: 50px;
	width: 100%;
	padding: 10px;
	box-sizing: border-box;
	background-color:  rgba(black,0.7);
	button{
		color: white;
		background-color: gray;
		width: 100%;
		height: 100%;
	}
}
.rolelist,.numberlist{
	padding: 16px;
}
.modal{
	position: absolute;
	background-color: rgba(black,0.7);
	height: 100%;
	width: 100%;
	z-index: 2;
	.wrap{
		box-sizing: border-box;
		height: 100%;
		width: 100%;
		overflow: auto;
		padding-bottom: 50px;
	}
}
.number{
	text-align: center;
}
.roletag{
	display: inline-block;
	margin: 1px;
	font-size: 13px;
	padding: 2px 4px; 
	border-radius: 10px;
	background-color: brown;
	color: white;
	&.big{
		font-size: 16px;
		padding: 8px;
		margin: 8px;
	}
}
.numbertag{
	display: inline-block;
	margin: 1px;
	font-weight: bold;
	font-size: 13px;
	padding: 2px 6px;
	border-radius: 10px;
	background-color: gray;
	color: white;
	&.big{
		font-size: 16px;
		padding: 8px 12px;
		margin: 8px;
	}
}
.otherword{
	font-size: 14px;
	line-height: 1.2;
}

.headwrap{
	height: 50px;
	border-bottom: 1px solid gray;
	.day{
		display: inline-block;
		padding: 10px;
		&.focus{
			color: blue;
		}
	}
}
.headinfo{
	display: flex;
	.th{
		margin: 2px;
		padding: 10px 6px;
		box-sizing: border-box;
		background-color: #eee;
		flex: 0 0 50px;
	}
}
.contentwrap{
	display: flex;
	flex-direction: column;
	flex-wrap: wrap;
	align-content: stretch;
	height: 100%;
	width: 100%;
}
.person{
	flex: 0 0 calc(100% / 6);
	width: 50%;
	position: relative;
	border: 1px solid #ddd;
	box-sizing: border-box;
	.left-section{
		position: absolute;
		display:flex;
		flex-direction:column;
		top:0;
		left:0;
		width: 30px;
		height: 100%;
	}
	.right-section{
		position: absolute;
		top:0;
		left:30px;
		height: 100%;
		width: calc(100% - 30px);
		height: 100%;
		overflow-y: auto;
		background-color: #eee;
	}
	.id{
		flex: 0 0 40px;
		text-align: center;
	}
	.campaign{
		flex: 1 1 0;
		display: flex;
		justify-content: center;
		align-items: flex-end;
	}
	.campaigntag{
		padding-bottom: 8px;
		font-size: 20px;
		text-align: center;
	}
	.td{
		display: inline;
		margin: 2px;
		padding: 2px;
		box-sizing: border-box;
	}
}

</style>
