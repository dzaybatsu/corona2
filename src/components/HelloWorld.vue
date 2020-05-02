<template>
  <div id="app">
    <div v-for="(i,index) in all">
     <img  :src="i.champ" :alt="index" :key="`i-${index}`" />
     <br>
      <div v-if="i.name = true" :alt="i.name">
        <img :src="i.skills[0]" />
        <img :src="i.skills[1]" />
        <img :src="i.skills[2]" />
        <img :src="i.skills[3]" />
      </div>
   </div>
    
  </div>
</template>

<script>
export default {
  data: () => ({
      all:[
        {
				champ: "https://images.contentstack.io/v3/assets/blt731acb42bb3d1659/blt0e3f847946232167/5db05fa889fb926b491ed7ff/RiotX_ChampionList_azir.jpg?quality=90&width=250",
				name: "azir",
				skills:[
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AzirQWrapper.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AzirW.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AzirEWrapper.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AzirR.png",
        ]
			},{
				champ: "https://images.contentstack.io/v3/assets/blt731acb42bb3d1659/blt80ff58fe809777ff/5db05fa8adc8656c7d24e7d6/RiotX_ChampionList_akali.jpg?quality=90&width=250",
				name: "akali",
				skills:[
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AkaliQ.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AkaliW.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AkaliE.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/AkaliR.png",
        ]
			},{
				champ: "https://images.contentstack.io/v3/assets/blt731acb42bb3d1659/blt24f4735ebde3c22b/5db08d642dc72966da74686e/RiotX_ChampionList_senna.jpg?quality=90&width=250",
				name: "senna",
				skills:[
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/SennaQ.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/SennaW.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/SennaE.png",
					"https://ddragon.leagueoflegends.com/cdn/10.9.1/img/spell/SennaR.png",
        ]
			},
        
      ],
      akali: "akali",
      azir: true,
      senna: true,
      }),
      methods:{
        
      }
    
}







</script>
