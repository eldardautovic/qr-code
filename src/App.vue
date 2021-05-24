<template>
  <div id="app">
    <div class="header">
      <div class="content">
        <i class="fas fa-mug-hot"></i>
        <h1>QR Caffe</h1>
      </div>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Pattaya&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: "Noto Sans JP";
}

.header{
  margin-top: 20px;
  padding: 40px;
  text-align: center;
  color: #333;
  background: #f3f3f3;
  max-width: 100%;
  margin: auto;
  font-family: "Pattaya";
}

.content{
 animation: slidein 2s infinite;
}

.header i{
  color: #FF8300;
  font-size: 40px;
  padding-bottom: 6px;
}

.header h1{
  font-size: 25px;
  color: #333;
}

.header img{
  width: 100px;
  height: 100px;
  padding-bottom: 10px;
}

@keyframes slidein {

  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(5px);
  }
}
</style>
