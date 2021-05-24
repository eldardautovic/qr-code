<template>
  <div class="stol">
    <div class="heading">
        <h1>ARTIKLI</h1>
        <p>Kliknite na artikal kako bi ga dodali u narudzbu</p>
    </div>
    <ul class="grupa">
        <li class="lista"> <p @click="addToCart('Coca Cola', 2)">Coca Cola</p></li>
        <li class="lista"><p @click="addToCart('Kafa Obicna', 2)">Kafa obicna</p></li>
        <li class="lista"><p @click="addToCart('Kafa Kraca', 1.5)">Kafa kraca</p></li>
        <li class="lista"><p @click="addToCart('Kafa Produzena',1)">Kafa produzena</p></li>
        <li class="lista"><p @click="addToCart('Nes 3u1', 3)">Nes 3u1</p></li>
        <li class="lista"><p @click="addToCart('Kisela', 4)">Kisela</p></li>
        <li class="lista"><p @click="addToCart('Senzacija', 5)">Senzacija</p></li>
        <li class="lista"><p @click="addToCart('Topla Cokolada', 6)">Topla Cokolada</p></li>
        <li class="lista"><p @click="addToCart('Ledeni Caj', 0.5)">Ledeni caj</p></li>
    </ul>
    <div class="form">
    <textarea v-model="napomena" name="note" cols="40" rows="2" placeholder="Napomena.."></textarea>
    </div>

    <button @click="order()">Naruci</button>

    <div class="korpica">
    <h1 class="empty" v-if="!korpa">Vasa korpa je prazna.</h1>
    <div v-else class="cart">
        <h1>Vasa korpa sadrzi: </h1>
        <ul class="korpica2">
            <li class="korpicaItem" v-for="item in korpaItem" :key="item">- {{item}}</li>
        </ul>
        <h1>Napomena: </h1>
        <p class="napomena" v-if="napomena.length > 0">{{napomena}}</p>
        <p class="napomena" v-else> Nema napomene</p>
        <div class="cijena">
        <p  v-if="cijena != null">Ukupna cijena: {{cijena}}KM</p>
        </div>
    </div>
    </div>
  </div>
</template>


<script>
export default {
 data() {
   return {
    korpa: false,
    korpaItem: [],
    napomena: "",
    cijena: null,
    tableId : this.$route.params.stolId
   }
 },
 methods:{
     addToCart(item, cijena){
         if(!this.korpa)
            this.korpa = true;
            
         this.korpaItem.push(item);
         this.cijena += cijena;
     },
     order(){

         if(this.korpaItem.length >= 1){

            var formatedOrder = '';

            for( var i = 0; i<this.korpaItem.length; i++)
            {    
                if(this.korpaItem.length == 1)
                    formatedOrder += `${this.korpaItem[i]}`;
                
                else {

                    if(i+1 >= this.korpaItem.length)
                        formatedOrder += `${this.korpaItem[i]}`;
                    
                    else
                        formatedOrder += `${this.korpaItem[i]}, `;
                }
            }

            if(this.napomena == "")
                this.napomena = "Nema";

            var myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");

            var raw = JSON.stringify({
                "tableId": this.tableId,
                "orderList": `${formatedOrder}`,
                "note": this.napomena
            });

            var requestOptions = {
            method: 'POST',
            headers: myHeaders,
            body: raw,
            redirect: 'follow'
            };

            fetch("https://qr-code-my-project.herokuapp.com/", requestOptions)
            .then(response => response.text())
            .then(result => console.log(result))
            .catch(error => console.log('error', error));

            this.korpaItem = [];
            this.korpa = false;

       }
     }
 }
}
</script>

<style lang="scss">

body{
    color: #333;
}

.heading{
    color: #333;
    padding-top: 50px;
    padding-bottom: 50px;
    text-align: center;
}

.heading p{
    color: rgb(90, 90, 90);
}

.stol{
    max-width: 350px;
    margin: auto;
    border-radius: 10px 10px;
}

.heading h1{
    font-size: 30px;
}
.korpica h1{
    padding-top: 20px;
    font-size: 25px;
    color: #333;
}

.heading p{
    font-size: 15px;
    padding-top: 10px;
    text-align: center;
}
.grupa{
    list-style: none;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 20px;
    margin-bottom: 0px;
}

.lista{
    width: 100px;
    height: 100px;
    background: #e6953f;
    border-radius: 2px;
    margin: auto;
    text-align: center;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 5px 0px, rgba(0, 0, 0, 0.1) 0px 0px 1px 0px;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    font-size: 15px;
    color: #333;
}

textarea {
    margin-top: 30px;
    text-align: center;
    border: none;
    border-radius: 3px;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 5px 0px, rgba(0, 0, 0, 0.1) 0px 0px 1px 0px;
    font-family: "Noto Sans JP";
    transition: .2s ease all;
    margin-bottom: 10px;
}

textarea:focus {
    outline: none !important;
    border:none;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.form{
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

button{
    margin-top: 20px;
    text-align: center;
    display: flex;
    margin: auto;
    font-family: "Noto Sans JP";
    font-size: 18px;
    padding: 5px 30px;
    border-radius: 2px;
    border: none;
    color: #333;
    background: #e6953f;
}
.img{
    width: 50px;
    height: 50px;
    background-image: url("../assets/nes.png");
    background-size:cover;
    background-position: center;
}

.korpica{
    padding-bottom: 20px;
}

.korpica2{
    list-style-type: none;
    padding-top: 10px;
}

.korpicaItem{
    padding-left: 10px;
    font-size: 15px;
}

.napomena{
    font-size: 15px;
    padding-left: 10px;
}

.cijena{
    margin-top: 30px;
    font-size: 15px;
    text-align: center;
    background: rgb(230, 230, 230);
    padding: 10px;
    border-radius: 2px;
}

.cart h1{
    text-decoration: underline;
    font-style: italic;
    font-weight: bold;
}

.empty{
    text-decoration: underline;
    font-style: italic;
    font-weight: bold;
    text-align: center;
}
</style>