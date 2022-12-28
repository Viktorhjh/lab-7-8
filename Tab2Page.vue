<template>
  <ion-page>
    <ion-content :fullscreen="true">   
      <div class="content">

        <ion-grid class="mainGrid">                                    
          
          <ion-row  v-for="j in 4" :key="j"  class="tableContent">             
            <ion-col>
              {{ tableCisnenie[j-1] }}
            </ion-col>          
           
            <ion-col  v-for="i in table[j-1]" :key="i" class="tableContent"> 
              <div v-if="i==0" class="green">
                {{ i }} 
              </div>   
              <div v-if="i==1" class="lime">
                {{ i }} 
              </div> 
              <div v-if="i==2" class="yellow">
                {{ i }} 
              </div> 
              <div v-if="i >= 3 && i <= 4" class="orange">
                {{ i }} 
              </div>  
              <div v-if="i >= 5 && i <= 10" class="red">
                {{ i }} 
              </div>  
              <div v-if="i > 10" class="maroon">
                {{ i }} 
              </div>                                                                                                                                 
            </ion-col>                         
          </ion-row>  

          <ion-col>
              <ion-row class="tableContent">
                cis/mmol
              </ion-row>
              <ion-row v-for="k in 5" :key="k" class="tableContent">
                {{ k+3 }}
              </ion-row>
            </ion-col>   

        </ion-grid>                                 
        <div class="data">
          <ion-grid>
            <ion-row>
              <ion-col>
                Wiek: 
              </ion-col>
              <ion-col>
                {{wiek}}                
              </ion-col>
            </ion-row> 
            
            <ion-row>
              <ion-col>
                Płeć: 
              </ion-col>
              <ion-col>
                {{ plec }}
              </ion-col>
            </ion-row>  
            
            <ion-row>
              <ion-col>
                Ciśnienia tętnicze: 
              </ion-col>
              <ion-col>                
                <ion-input class="custom" type="number"
                :value="cisnienia"
                @ionInput="cisnienia = $event.target.value;"> </ion-input>              
              </ion-col>
            </ion-row>  
            <ion-row>
              <ion-col>
                Cholesterol mmol/l:
              </ion-col>
              <ion-col>
                <ion-input class="custom"
                :value="mmol"
                @ionInput="mmol = $event.target.value;"> </ion-input>                
                </ion-col>
            </ion-row>  
            <ion-row>
              <ion-col>
                Cholesterol mg/dl: 
              </ion-col>
              <ion-col>
                <ion-input class="custom"
                :value="mg"
                @ionInput="mg = $event.target.value;"> </ion-input>                
                </ion-col>
              </ion-row>  
              <ion-row>
                <ion-col>
                  palenia: 
                </ion-col>
                <ion-col>
                  <ion-toggle @click="setSmoke" v-model="smokeCur" v-if="smoke"></ion-toggle>
                </ion-col>
              </ion-row>  
                            
              <ion-button class="buttonOblicz" v-on:click="nextPage" > Powróć </ion-button>
              <ion-button class="buttonOblicz" v-on:click="searchTable" > Refresh </ion-button>              
          </ion-grid>
        </div>
      </div>        
    </ion-content>
  </ion-page>
</template>


<script lang="ts">  
  import { IonPage, IonContent, IonGrid } from '@ionic/vue';
  import router from '@/router';
  import { useRoute } from 'vue-router';
  import { defineComponent } from '@vue/runtime-core';

  export default defineComponent({    
    setup(){
      const route = useRoute();
      const { wiek, plec, smoke, cisnienia, mmol, mg } = route.params;      
      return { wiek, plec, smoke, cisnienia, mmol, mg };            
    },
    data(){          
      return{       
        smokeCur: false,                     
        name: 'Tab2Page',
        iClass: 'tableContent',        
        tableCisnenie: [120, 140, 160, 180],        
        man40:[[1, 1, 1, 1, 1],[1,1,1,2,2],[1,2,2,2,3],[2, 2, 3, 3, 4]],              
        man50:[[2, 2, 2, 3, 3],[2, 3, 3, 4, 5],[3, 4, 5, 6, 7],[5, 6, 7, 8, 9]],              
        man55:[[2, 3, 3, 4, 5],[3, 4, 5, 6, 7],[5, 6, 7, 8, 10],[7, 8, 10, 12, 14]],              
        man60:[[4, 4, 5, 6, 8],[5, 6, 8, 9, 11],[8, 9, 11, 13, 15],[11, 13, 15, 18, 21]],
        man65:[[6, 7, 9, 10, 12],[9, 10, 12, 14, 17],[12, 14, 17, 20, 24],[17, 20, 24, 28, 32]],              
        man70:[[10, 12, 14, 17, 20],[14, 17, 20, 24, 28],[20, 24, 28, 32, 38],[28, 33, 38, 43, 50]],

        man40s:[[1,1,2,2,3],[2,2,3,3,4],[3,3,4,4,5],[4,4,5,6,8]],
        man50s:[[3,4,5,5,6],[5,5,6,8,9],[6,8,9,11,13],[9,11,13,16,18]],
        man55s:[[5,6,7,8,10],[7,8,10,12,14],[10,12,14,17,20],[14,17,20,23,27]],
        man60s:[[7,9,11,13,15],[11,13,15,18,21],[15,18,21,25,29],[21,25,29,34,39]],
        man65s:[[12,14,17,20,23],[17,20,23,27,32],[23,27,32,37,43],[32,37,43,49,55]],
        man70s:[[20,23,27,32,37],[27,32,37,43,49],[37,43,49,55,62],[49,56,62,69,76]],    
        
        w40:[[0,0,0,0,0],[0,0,0,1,1],[0,1,1,1,1],[1,1,1,1,1]],
        w50:[[1,1,1,1,1],[1,1,1,1,2],[1,1,2,2,2],[2,2,2,3,3]],
        w55:[[1,1,1,1,2],[1,1,2,2,3],[2,2,3,3,4],[3,3,4,4,5]],
        w60:[[2,2,2,3,3],[2,3,3,4,5],[3,4,5,5,7],[5,6,7,8,9]],
        w65:[[3,3,4,5,6],[4,5,6,7,9],[6,7,9,10,12],[9,10,12,15,17]],
        w70:[[6,7,8,10,12],[8,10,12,14,17],[12,14,17,20,24],[17,20,24,28,32]],

        w40s:[[0,1,1,1,1],[1,1,1,1,1],[1,1,1,2,2],[1,2,2,2,3]],
        w50s:[[1,1,2,2,2],[2,2,2,3,3],[2,3,3,4,5],[3,4,5,5,7]],
        w55s:[[2,2,3,3,4],[3,3,4,4,5],[4,4,5,6,7],[5,6,8,9,11]],
        w60s:[[3,4,4,5,6],[5,5,6,8,9],[6,8,9,11,13],[9,11,13,15,18]],
        w65s:[[6,7,8,10,12],[8,10,12,14,17],[12,14,17,20,23],[17,20,24,28,32]],
        w70s:[[12,14,17,20,23],[17,20,23,27,32],[23,27,32,37,42],[32,37,43,49,55]],
        table: [[0,0,0,0]]   
      }
    },
    methods:{      
      nextPage(){        
        router.push('/tabs/tab1')                
      },  
      searchTable(){                            
        this.wiek = this.wiek.toString()        
        if(this.plec == 'M'){
          if(parseInt(this.wiek) <= 40 && this.smokeCur){
            this.table = this.man40s            
          }
          
          if(parseInt(this.wiek) <= 40 && !this.smokeCur){
            this.table = this.man40
          }
                  
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && this.smokeCur){
            this.table = this.man50s
          }
        
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && !this.smokeCur){
            this.table = this.man50
          }

          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && this.smokeCur){          
            this.table = this.man55s
          }
          
          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && !this.smokeCur){      
            this.table = this.man55
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && this.smokeCur){
            this.table = this.man60s         
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && !this.smokeCur){
            this.table = this.man60     
          }

          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && this.smokeCur){
            this.table = this.man65s          
          }        
                  
          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && !this.smokeCur){
            this.table = this.man65          
          }        

          if(parseInt(this.wiek) >= 70 && this.smokeCur){
            this.table = this.man70s         
          }
          
          if(parseInt(this.wiek) >= 70 && !this.smokeCur){
            this.table = this.man70
          }
        }

        if(this.plec == 'K'){
          if(parseInt(this.wiek) <= 40 && this.smokeCur){
          this.table = this.w40s
          }
          
          if(parseInt(this.wiek) <= 40 && !this.smokeCur){
            this.table = this.w40
          }
                  
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && this.smokeCur){
            this.table = this.w50s
          }
        
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && !this.smokeCur){
            this.table = this.w50
          }

          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && this.smokeCur){          
            this.table = this.w55s
          }
          
          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && !this.smokeCur){      
            this.table = this.w55
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && this.smokeCur){
            this.table = this.w60s         
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && !this.smokeCur){
            this.table = this.w60     
          }

          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && this.smokeCur){
            this.table = this.w65s          
          }        
                  
          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && !this.smokeCur){
            this.table = this.w65          
          }        

          if(parseInt(this.wiek) >= 70 && this.smokeCur){
            this.table = this.w70s         
          }
          
          if(parseInt(this.wiek) >= 70 && !this.smokeCur){
            this.table = this.w70
          }
        }
        
      },
      setSmoke(){    
        this.table = [[]]        
        this.smokeCur = !this.smokeCur                                
      },
      
    },
    watch: {
      mmol(){
        this.mg = (18*parseFloat(this.mmol.toString())).toString()
      },
      mg(){
        this.mmol = (parseFloat(this.mg.toString())/18).toString()
      }
    },
    components: { IonContent, IonPage, IonGrid }
  })

</script>

<style scoped>
  ion-grid.mainGrid{
    border: 2px white solid;             
    text-align: center;
    justify-content: left; 
    display: grid;
    height: 330px; 
    grid-template-columns: repeat(5, auto);  
    grid-template-rows: repeat(6, auto);
    font-size: 120%;
    margin: 10px;
    padding: 10px;
  }

  .tableContent{      
    text-align: center;
    margin: 10px;      
    margin-bottom: 0px; 
    padding-bottom: 0px;
    width: 60px;
    height: 40px;
  }   

  .content{
    display: flex;
    margin: 10px;
    margin-top: 5%;
    padding: 10px;
    padding-left: 10%;
    justify-content: left;
  }

  .data{
    justify-content: left;
    margin-left: 10%;
    width: 50%;
  }

  .green{
    background-color: rgb(0, 114, 0);
  }

  .lime{
    background-color: rgb(87, 180, 0);
  }

  .yellow{
    background-color: rgb(255, 233, 32);
  }

  .orange{
    background-color: orange;
  }

  .red{
    background-color: red;
  }

  .maroon{
    background-color: maroon;
  }

  ion-input.custom {    
    color: #fff;        
    padding: 10px;    
    margin: 5px;    
    border-bottom: 2px solid white;    
    background: -webkit-linear-gradient(#121212 70%, #333333 );
  }

  .buttonOblicz{
    margin: 10px;
    margin-left: 0px;
    border: 1px solid white;
    border-radius: 5px;
    --background: transparent;
  }

</style>
