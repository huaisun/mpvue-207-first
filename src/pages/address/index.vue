<template>
    <div>
     <div class="div2">
      <van-cell-group v-for="(item, index) in addressData" :key="index">
        <van-cell :title="item.receiverName + ' ' + item.receiverMobile" :label="item.receiverCollegeName +  ' ' + item.receiverAddress">
          <van-icon slot="right-icon" name="edit" class="custom-icon" size="20px"
                    @click="editAddress(item)"></van-icon>
        </van-cell>
      </van-cell-group>
      </div>



        <van-button type="danger" size="large" class="inner-button-class" @click="addsubmit">
          新增收货地址
        </van-button>

    </div>

</template>

<script>
  export default {
    data:()=>{
      return {
        storageAddressKeyName:"address",
        addressData: [/*{
          name: "张三",
          phone: "13855036835",
          city: "浙江省杭州市西湖区",
          address: "文三路138号东方通信大厦7楼501室"
        }, {
          name: "李四",
          phone: "13855036835",
          city: "浙江省杭州市西湖区",
          address: "文三路138号东方通信大厦7楼501室"
        }*/],
        add: {
          userOpenId:"",
          name:"",
          phone:"",
          city:"",
          colleage:"",
          address:""
        }
      }
    },
    onShow(){
      console.log('mine-onshow')
      let newAddress = this.hasNewAddress();
      if(newAddress||this.addressData.length==0){
        this.getAddresslist()
        this.execSetAddressStorageSync(false);
      }

/*      wx.getStorage({
        key:"openid",
        success:(res)=> {
          console.log(res.data);
          //this.add.userOpenId=res.data;
          wx.request({
            url:"http://188.131.244.83/api/v1/service-user/receive-info/list/"+res.data,
            method: "GET",
            success:(res)=>{
              console.log(res.data.data)
              this.addressData = res.data.data
            }
          })
        }
      })*/
   /*   wx.request({
        url:"http://188.131.244.83/api/v1/service-user/receive-info/list/"+,
        method: "GET",
        success(res) {

        }
      })*/
    },
    methods: {
      getAddresslist(){
        wx.getStorage({
          key:"openid",
          success:(res)=> {
            console.log(res.data);
            //this.add.userOpenId=res.data;
            wx.request({
              url:"https://api.ypaot.com/api/v1/service-user/receive-info/list/"+res.data,
              method: "GET",
              success:(res)=>{
                console.log(res.data.data)
                this.addressData = res.data.data
              }
            })
          }
        })
      },
     editAddress(item){
        console.log(item)
        wx.navigateTo({
          url:"/pages/addressedit/main?item="+JSON.stringify(item)
        })
      },
      addsubmit(){
        wx.navigateTo({
          url:"/pages/newaddress/main"
        })
      },
      execSetAddressStorageSync(data){
        wx.setStorageSync(this.storageAddressKeyName,data);
      },
      hasNewAddress(){
        let flag = wx.getStorageSync(this.storageAddressKeyName);
        return flag == true;
      },
      /*下拉刷新页面*/
      onPullDownRefresh(){
        //let that=this;
        this.addressData=[];  //订单初始化

      },
    }

  };
</script>

<style>
  .div2{
    padding-bottom: 50px;
  }
 /*   .div1{
      position: fixed;
      bottom:0;
      width: 100%;
      //height: 50px;
    }*/
  .inner-button-class{
       position: fixed;
       bottom:0;
       width: 100%;
     }
</style>
