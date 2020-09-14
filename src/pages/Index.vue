<template>
  
  <q-page class="flex flex-center">
    <meta charset="utf-8">
    <link type="text/css" rel="stylesheet" href="node_modules/@zoomus/websdk/dist/css/bootstrap.css" />
    <link type="text/css" rel="stylesheet" href="node_modules/@zoomus/websdk/dist/css/react-select.css" /> 
    <meta name="format-detection" content="telephone=no">
    <q-btn color="primary" icon="check" label="OK" @click="sendOk()" />
  </q-page>
</template>

<script>
import { ZoomMtg } from '@zoomus/websdk';
console.log("checkSystemRequirements");
console.log(JSON.stringify(ZoomMtg.checkSystemRequirements()));
ZoomMtg.setZoomJSLib('http://127.0.0.1:9090/node_modules/@zoomus/websdk/dist/lib', '/av');
ZoomMtg.preLoadWasm();
ZoomMtg.prepareJssdk();

var API_KEY = <API_KEY/>;

var API_SECRET = <API_SECRET/>;

export default {
  name: 'PageIndex',
  
  data(){
    return{
      src: "",
      meetConfig: {},
      signature: {}
    }
  },

  created(){
    //configuração da chamada
    this.meetConfig = {
      signatureEndPoint: 'https://github.com/zoom/websdk-sample-signature-node.js',
      apiKey: API_KEY,
      apiSecret: API_SECRET,
      meetingNumber: '79152179692', //change var
      userName: 'lucas', //change var
      passWord: "MU3mDb", //chage var
      leaveUrl: "http://localhost:9999",
      role: 0
    };

    //gerar assinatura
    this.signature = ZoomMtg.generateSignature({
      meetingNumber: this.meetConfig.meetingNumber,
      apiKey: this.meetConfig.apiKey,
      apiSecret: this.meetConfig.apiSecret,
      role: this.meetConfig.role,
      success: function(res) {
        // eslint-disable-next-line
        console.log("success signature: " + res.result);
      }
    });

    ZoomMtg.init({
      leaveUrl: "http://www.zoom.us",
      isSupportAV: true,
      success: () => {
        console.log('meeting: ' + this.meetConfig)
        ZoomMtg.join({
          meetingNumber: this.meetConfig.meetingNumber,
          userName: this.meetConfig.userName,
          signature: this.signature,
          apiKey: this.meetConfig.apiKey,
          userEmail: "Dudu@gmail.com", //change var
          passWord: this.meetConfig.passWord,
          success: function(res) {
            // eslint-disable-next-line
            console.log("join meeting success");
            
          },
          error: function(res) {
            // eslint-disable-next-line
            console.log('deu erro: ' + res);
          }
        });
      },
      error: function(res) {
        // eslint-disable-next-line
        console.log('Deu erro: ' + res);
    }      
    })

  },

  methods:{
  }

}
</script>
