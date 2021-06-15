<template>
  <nav role="navigation" class="w-full m-2 p-2 inline-block">

    <div class=" ">
      <div v-if="web3Plug.connectedToWeb3() == false" @click="connectToWeb3" class="button text-center bg-blue-500 hover:bg-blue-700 text-white font-bold my-2 py-2 px-4 rounded cursor-pointer">Login with Web3</div>

      <div v-if="web3Plug.connectedToWeb3() "   class="truncate text-center text-gray-800 p-2" style="   ">

      
        <Web3NetButton
           v-bind:providerNetworkID="activeNetworkId"
           v-bind:web3Plug='web3Plug' 
         />


        <span class="  " style=" ">
        <a   v-bind:href="getEtherscanBaseURL()+'/address/'+web3Plug.getActiveAccountAddress()" class="text-gray-800  "   target="_blank">  {{web3Plug.getActiveAccountAddress()}} </a>
       </span>
       </div>
    </div>



    <div class="w-full lg:w-auto block lg:inline-block" v-for="item in navConfig.dropdowns" :key="item.title">


 




  </div>



</nav>
</template>


<script>
import Web3NetButton from './Web3NetButton.vue'
import Config from '../config/UpperNav.js'
export default {
  name: 'AccordionNav',
  props: ['web3Plug'  ],
  components:{Web3NetButton},
  data() {
    return {
      activeAccountAddress:null,
      activeNetworkId: null,
      navConfig: null
    }
  },
  created(){
    this.navConfig = Config;
    this.web3Plug.getPlugEventEmitter().on('stateChanged', function(connectionState) {
          console.log('stateChanged',connectionState);
          this.activeAccountAddress = connectionState.activeAccountAddress
          this.activeNetworkId = connectionState.activeNetworkId
        }.bind(this));
  },
  methods: {
   
    connectToWeb3(){
      this.web3Plug.connectWeb3( )
    },
    getEtherscanBaseURL(){
        if(this.activeNetworkId == 42){
          return  'https://kovan.etherscan.io'
        }
        return 'https://etherscan.io'
    },
  }
}
</script>