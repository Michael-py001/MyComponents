<template>
  <view class="Form">
    <view class="commom">
      <Check label="类型" :checkList="checkList" @click="checkboxChange"></Check>
      <Form label="姓名" holderPlaceText="请输入您的姓名" v-model="name"></Form>
      <Form label="手机号" type="number" holderPlaceText="请输入您的手机号" v-model="phone"></Form>
      <CityPicker label="站点地址" labelWidth="230" disabled holderPlaceText="请选择城市地区" @comfirm="comfirmCity"></CityPicker>
      <AddressSelect label="详细地址" labelWidth="230" disabled holderPlaceText="请选择详细地址" @comfirm="comfirmAddress"></AddressSelect>
      <ChoosePopup label="物品类型" labelWidth="230" :typeList="typeList" :value.sync="goodsType"></ChoosePopup>
      <ChooseNumber label="预估重量(Kg)" labelWidth="230" :value.sync="weight"></ChooseNumber>
      <ChooseExpressTime shake label="期望上门时间" labelWidth="230" :value="expressTime" @change="expressTimeChange"></ChooseExpressTime>
      <IDCardUpload :imgList.sync="idCardList"  label="请上传您的有效身份证"></IDCardUpload>
      <ChooseImages label="选择图片" labelWidth="230" :imgList.sync="imgList"></ChooseImages>
    </view>
  </view>
</template>

<script>
  import Form from './components/Form.vue'
  import Check from './components/Checkbox/Checkbox.vue'
  import ChooseImages from './components/ChooseImages.vue'
  import AddressSelect from './components/AddressSelect.vue'
  import CityPicker from './components/CityPicker/CityPicker.vue'
  import IDCardUpload from './components/IDCardUpload.vue'
  import ChooseNumber from './components/ChooseNumber.vue'
  import ChooseExpressTime from './components/ChooseExpressTime.vue'
  import ChoosePopup from './components/ChoosePopup.vue'
  export default {
    components:{
      Form,
      IDCardUpload,
      Check,
      CityPicker,
      AddressSelect,
      ChooseImages,
      ChooseNumber,
      ChooseExpressTime,
      ChoosePopup
    },
    data(){
      return {
        checkList:[
          {
            id:0,
            label:'站长',
            value:0,
            selected:true
          },
          {
            id:1,
            label:'快递员',
            value:1,
            selected:false
          },
        ],
        name:'',
        phone:'',
        idCardList:[],
        addressDetail:'',
        typeList: [{
            value: '电子产品',
            label: '电子产品'
          },
          {
            value: '文件',
            label: '文件'
          }
        ],
        goodsType:'',
        weight:'',
        expressTime:''
      }
    },
    computed:{
    },
    methods: {
      checkboxChange(checkbox){
        console.log(checkbox)
        this.checkList.forEach(item=>{
          if(item.id==checkbox.id) {
            item.selected = true
            this.selectCheckboxValue = checkbox.value
            console.log("value:",this.selectCheckboxValue)
          }
          else {
            item.selected = false
          }
        })
      },
      comfirmAddress(address){
         console.log("address:",address)
         this.addressDetail = address
     },
     expressTimeChange(tab,value) {
       let time
       if(tab==0) {
         time = '今天'
       }
       else if(tab==1) {
         time = '明天'
       }
       this.expressTime = `${time} ${value}`
     }
    }
  }
</script>

<style lang="scss" scoped>
  .commom {
    background: #ffffff;
    border-radius: 24rpx;
    padding: 0 30rpx;
    margin-bottom: 24rpx;
  }
  .Form{
    padding: 24rpx;
  }
</style>
