<template>
  <div class="landingpage">
    <div class="container">
      <div class="subtitle">
        <img style="width:200px" class="icon svg-tiktok" src="../../assets/image/dnu.svg" alt="" /><br>
        DNU is an acronym for the three concepts of DAO, NFT, and UBI.
It has long been paying attention to the development trends in these fields and high grade projects related to investment.<br><br>

        DNUDAO贡献者 银色NFT
      </div>
      <div class="btn-group">
        <button class="btn" v-if="!address || process" style="background-color: gray!important;">{{btnText}}</button>
        <button class="btn" v-if="address && !process" @click="mint()">Mint</button>
      </div>
    </div>
  </div>
</template>
<script>
import testabi from "@/assets/abi/test-dnu.json";
// import abi from "@/assets/abi/dnu.json";
export default {
  data() {
    return {
      address: null,
      chainId: null,
      tokenId: null,
      process:false,
      btnText:"Mint",
      tokenIds: [],
      images: [],
      approved: false,
      dnu_address:"0x01ea69001eded8cd5f9afbc8a0553e358163bfe5",//0x3Ffc39A2962B69De9Be5302fE3FcDBe651F1b3B5,
    };
  },
  props: ['web3'],
  watch: {},
  async mounted() {
    let self = this
    
    this.punk = new this.web3.eth.Contract(
      testabi,
      self.dnu_address
    );
    await this.setAddress()
    // this.checkAllowance()
  },
  methods: {
    setTokenIds(tokenId){
      let self = this
      let old_info = localStorage.getItem('tokenInfo')||"{}"
      old_info = JSON.parse(old_info)
      let address_info = old_info[self.address]||[]
      address_info.push(tokenId)
      old_info[self.address] = address_info
      localStorage.setItem(JSON.stringify(old_info))
    },
    /**
     * @description mint a nft
     */
    mint() {
      this.btnText = "waiting..."
      this.process = true
      let self = this;
        self.punk.methods
          .mint()
          .send({
            from: self.address,
          })
          .on("receipt", function (receipt) {
            self.$message({
              message: "Mint Success",
              type: "success",
            });
            self.btnText = "Success"
            console.log(receipt);
          })
          .on("error", function (error, receipt) {
            self.$message({
              message: "mint error",
              type: "error",
            });
             self.process = false
          });
      
    },
    getAddress(address) {
      return `${address.slice(0, 5)}...${address.substr(
        address.length - 5,
        5
      )}`;
    },
    async setAddress() {
      const accounts = await this.web3.eth.getAccounts();
      console.log('address', accounts)
      if (accounts[0]) {
        this.address = accounts[0];
      }
    },
    async disconnect() {},
    async checkChain(id) {
      const chainId = id || (await this.web3.eth.getChainId());
      console.log("chainId", chainId);
      if (chainId !== 1) {
        this.$message({
          message: "Please switch MainNet",
          type: "error",
        });
      }

    },
  },
};
</script>
<style lang='less' scoped>
@import "./style.less";
</style>
