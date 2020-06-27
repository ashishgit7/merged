<template lang="html">
  <div>
    <div id ="referAndEarnPhoto" >
      <img src="../assets/Refer@300x-100.jpg" class="rounded mx-auto mt-3" style="box-shadow:-6px 1px 21px -12px rgb(0, 0, 0);width:400px" @click="refer">
    </div>
    <div id="referAndEarn" style="display:none">
    <vs-tabs :color="colorx" alignment="center">
      <vs-tab @click="colorx = 'success'" label="Tenant">
        <div class=" text-center mx-auto ">
            
         <div class="text-left mx-auto" style="width: fit-content;background-color: #dbdbdb;">  
             <div>
                 <div class="row p-3"> 
                     <div class="col-1">
                         <div class="vl"><div class="sl"></div><div class="dl"></div></div>
                         
                         </div>
                    <div class="col-11"> 
         <h4 class="text-left">House tenant Detail</h4>
         <h5>The tenant will be contacted in this detail</h5>
     
       <vs-input  label-placeholder="Tenant name" class="mt-4"  v-model="tenantName"/>
       <vs-input  label-placeholder="+91| Tenant Phone number" class="mt-4" v-model="tenantPhone" />
       <vs-input  label-placeholder="House location or city" class="mt-4"  v-model="tenantCity" />
       <h4 class="text-left mt-3">Your Detail</h4>
         <h5>you will be contacted for UPI verification</h5>
     <vs-input  label-placeholder=" Name" class="mt-4" v-model="userName"/>
       <vs-input type="tel"  label-placeholder="+91|  Phone number" class="mt-4 mb-2" pattern="[789][0-9]{9}" required v-model="userPhone"/>
      <small class="text-danger"  v-show="checkppbox1"> Check the Privacy box first
           </small>
       <p class="text-muted mt-3">
           <input type = "checkbox" class="bg-success" v-model="checkPPTenant">
           I agree to be contacted by Roomlelo as per roomlelo's Privacy policy
       </p>
       <button class="btn btn-primary text-right" @click="tenantSubmit">
              submit
       </button>
        </div>
        </div>
     
        </div>
        </div>
        </div>
      </vs-tab>
      <vs-tab @click="colorx = 'danger'" label="Owner">
         <div class=" text-center mx-auto ">
            
         <div class="text-left mx-auto" style="width: fit-content;background-color: #dbdbdb;">  
             <div>
                 <div class="row p-3"> 
                     <div class="col-1">
                         <div class="vl"><div class="sl"></div><div class="dl"></div></div>
                         </div>
                    <div class="col-11"> 
         <h4 class="text-left">House Owner Detail</h4>
         <h5>The owner will be contacted in this detail</h5>
     
       <vs-input  label-placeholder="Owner name" class="mt-4" v-model="ownerName" />
       <vs-input  label-placeholder="+91| Owner Phone number" class="mt-4" v-model="ownerPhone" />
       <vs-input  label-placeholder="House location or city" class="mt-4" v-model="ownerCity" />
       <h4 class="text-left mt-3">Your Detail</h4>
         <h5>you will be contacted for UPI verification</h5>
     <vs-input  label-placeholder=" Name" class="mt-4" v-model="userName" />
       <vs-input  label-placeholder="+91|  Phone number" class="mt-4 mb-2" v-model="userPhone"/>
       <div>
           <small class="text-danger"  v-show="checkppbox"> Check the Privacy box first
           </small>
       <p class="text-muted mt-0">
           <input type = "checkbox" class="bg-success" v-model="checkPPOwner">
           I agree to be contacted by Roomlelo as per roomlelo's Privacy policy
       </p>
       </div>
       <br>
       <button class="btn btn-primary" style="" @click="ownerSubmit">
              submit
       </button>
        </div>
        </div>
        </div>
        </div>
        </div>
      </vs-tab>
     
      
       
    </vs-tabs>
   </div>
  </div>
</template>

<script>
import db from '../firebase'
export default {
  data:()=>({
    colorx:'success',
    inp:[],
    tenantPhone:'',
    tenantName:'',
    tenantCity:'',
    ownerPhone:'',
    ownerName:'',
    ownerCity:'',
    userName:'',
    userPhone:'',
    checkppbox:false,
    checkPPOwner:false,
    checkppbox1:false,
    checkPPTenant:false,

  }),
  methods:{
      inp1(){
          console.log(this.inp)
      },
      refer(){
        console.log("hello")
        var ele = document.getElementById("referAndEarnPhoto")
        ele.style.display = "none"
        ele = document.getElementById("referAndEarn")
        ele.style.display = "block"
      },
      async tenantSubmit(){
        // console.log(this.tenantCity.length)
        // console.log(this.tenantPhone.length)
        // console.log(this.tenantName.length)
        // console.log(this.userName.length)
        // console.log(this.userPhone.length)
        if(this.checkPPTenant==false)
        {
          this.checkppbox1=true;
          return;
        }
        if(this.tenantPhone.length!=10||this.tenantName.length==0||this.tenantCity.length==0||this.userName.length==0||this.userPhone.length!=10){
          alert("incorrect Detail")
          return;
        }
        if(this.userPhone[0]<6||this.tenantPhone[0]<6)
        {
          alert("incorrect Detail")
          return;
        }
        await db.collection('referedTenant').add({
          tenantName:this.tenantName,
          tenantCity:this.tenantCity,
          tenantPhone:this.tenantPhone,
          userPhone:this.userPhone,
          userName:this.userName,
          seen:false,
        })
        alert("request submitted We will contant you and tenant")
        this.tenantName='',
        this.tenantCity='',
        this.tenantPhone='',
        this.userPhone='',
        this.userName=''

      },
      
     async ownerSubmit(){
        if(this.checkPPOwner==false)
        {
          this.checkppbox=true;
          return;
        }
        if(this.ownerPhone.length!=10||this.ownerName.length==0||this.ownerCity.length==0||this.userName.length==0||this.userPhone.length!=10){
          alert("incorrect Detail")
          return;
        }
        if(this.userPhone[0]<6||this.ownerPhone[0]<6)
        {
          alert("incorrect Detail")
          return;
        }
      await  db.collection('referedOwner').add({
          ownerName:this.ownerName,
          ownerCity:this.ownerCity,
          ownerPhone:this.ownerPhone,
          userPhone:this.userPhone,
          userName:this.userName,
          seen:false,
        })
        alert("request submitted We will contant you and Owner")
        this.ownerName='',
        this.ownerCity='',
        this.ownerPhone='',
        this.userPhone='',
        this.userName=''

      },

  }
}

</script>
<style  scoped>
  pre {
    width: 100%;
    color: rgba(255, 255, 255,.8);
    text-align: center;
  }
  .vl {
  border-left: 6px solid grey;
  height: 550px;
}
  .sl {
  border-left: 6px solid rgb(21, 158, 204);
  height: 10px;
  position: relative;
  top:10px;
  right:6px;


}
  .dl {
  border-left: 6px solid rgb(21, 158, 204);
  height: 10px;
  position: relative;
  top:290px;
  right:6px;
  
}
</style>