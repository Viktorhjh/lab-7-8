<template>
  <ion-page>    
    <ion-content :fullscreen="true">      
      <div class="content">
        <ion-grid class="mainGrid">
          <ion-row>
            <ion-col class="gridText">Wiek</ion-col>
            <ion-col>
              <ion-input class="custom" placeholder="wiek" type="number"
              :value="wiek"
              @ionInput="wiek = $event.target.value;"> </ion-input>              
              </ion-col>         
          </ion-row>
            
          <ion-row>
           <ion-col class="gridText">Płeć</ion-col>
           <ion-col> 
             <ion-select placeholder="Płeć" v-model="plec">                
              <ion-select-option value="M">Mężczyzna</ion-select-option>
              <ion-select-option value="K">Kobieta</ion-select-option>
             </ion-select>
           </ion-col>         
          </ion-row>         

          <ion-row>
            <ion-col class="gridText">Ciśnienia tętnicze</ion-col>                        
            <ion-col><ion-input class="custom" type="number"
                :value="cisnienia"
                @ionInput="cisnienia = $event.target.value;">  </ion-input></ion-col>         
          </ion-row>

          <ion-row>
            <ion-col class="gridText">Cholesterol mmol/l</ion-col>
            <ion-col><ion-input class="custom" type="number"
                :value="mmol"
                @ionInput="mmol = $event.target.value;">  </ion-input></ion-col>                            
          </ion-row>

          <ion-row>
            <ion-col class="gridText">cholesterol mg/dl</ion-col>
            <ion-col><ion-input class="custom" type="number"
                :value="mg"
                @ionInput="mg = $event.target.value;">  </ion-input></ion-col>                
          </ion-row>

          <ion-row>
            <ion-col class="gridText"> nie palący </ion-col>
            <ion-col> <ion-toggle @click="setSmoke" v-model="smoke"></ion-toggle> </ion-col>         
            <ion-col class="gridText"> palący </ion-col>
          </ion-row>

          <ion-row>         
            <ion-col class="gridText"> 
              <ion-button class="buttonOblicz" v-on:click="nextPage" > Tablica </ion-button>   
              <ion-button class="buttonOblicz" v-on:click="result" v-if="check"> Oblicz </ion-button>             
              </ion-col>
          </ion-row>
        </ion-grid>       
        <div class="information">
          Miażdżyca jest chorobą wieloczynnikową, przez co należy rozumieć, że przyczyny mogą być różne. Nazywamy je czynnikami ryzyka. Do najważniejszych należą: wysoki poziom złego cholesterolu w surowicy, palenie tytoniu, nadciśnienie tętnicze, cukrzyca, otyłość brzuszna.
        </div>                        
       </div>
       <div class="information">
        Wynik: {{ res }}
       </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">  
  import { IonPage, IonContent, IonSelect, IonSelectOption, IonButton, IonGrid } from '@ionic/vue';
  import router from '@/router';
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    components: { IonContent, IonPage, IonSelect, IonSelectOption, IonButton, IonGrid },
    name: 'Tab1Page',    
    data(){
      return{
        wiek: '',
        plec: '',
        cisnienia: '',
        mmol: 0,
        mg: 0,
        res: 0,
        smoke: false,

        man40:[[1, 1, 1, 1, 1,],[1, 1, 1, 2, 2,],[1, 2, 2, 2, 3,],[2, 2, 3, 3, 4]],              
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
      }
    },

    methods:{

      nextPage(){                
        if(this.check()){          
          router.push('/tabs/tab2/' + this.wiek + '/' + this.plec + '/' + this.smoke + '/' + this.cisnienia + '/' + this.mmol + '/' + this.mg)              
        }
      },
      
      setSmoke(){        
        this.smoke = !this.smoke
      },

      result(){
        let i = parseFloat(this.cisnienia)
        let j = this.mmol-4        
        
        if(parseFloat(this.cisnienia) <= 120){
          i = 0
        }
        
        if(parseFloat(this.cisnienia) <= 140 && parseFloat(this.cisnienia) > 120){
          i = 1
        }        
        
        if(parseFloat(this.cisnienia) <= 160  && parseFloat(this.cisnienia) > 140){
          i = 2
        }
                
        if(parseFloat(this.cisnienia) > 160){
          i = 3
        }                                    
        console.log(this.plec)
        let table = this.searchTable()

        this.res = table[i][j]                  
      },

      searchTable(){        
        let table = this.man40
        if(this.plec == 'M'){
          if(parseInt(this.wiek) <= 40 && this.smoke){
          table = this.man40s
          }
          
          if(parseInt(this.wiek) <= 40 && !this.smoke){
            table = this.man40
          }
                  
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && this.smoke){
            table = this.man50s
          }
        
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && !this.smoke){
            table = this.man50
          }

          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && this.smoke){          
            table = this.man55s
          }
          
          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && !this.smoke){      
            table = this.man55
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && this.smoke){
            table = this.man60s         
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && !this.smoke){
            table = this.man60     
          }

          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && this.smoke){
            table = this.man65s          
          }        
                  
          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && !this.smoke){
            table = this.man65          
          }        

          if(parseInt(this.wiek) >= 70 && this.smoke){
            table = this.man70s         
          }
          
          if(parseInt(this.wiek) >= 70 && !this.smoke){
            table = this.man70
          }
        }

        if(this.plec == 'K'){
          if(parseInt(this.wiek) <= 40 && this.smoke){
          table = this.w40s
          }
          
          if(parseInt(this.wiek) <= 40 && !this.smoke){
            table = this.w40
          }
                  
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && this.smoke){
            table = this.w50s
          }
        
          if(parseInt(this.wiek) <= 50 && parseInt(this.wiek) > 40 && !this.smoke){
            table = this.w50
          }

          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && this.smoke){          
            table = this.w55s
          }
          
          if(parseInt(this.wiek) <= 55 && parseInt(this.wiek) > 50 && !this.smoke){      
            table = this.w55
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && this.smoke){
            table = this.w60s         
          }

          if(parseInt(this.wiek) <= 60 && parseInt(this.wiek) > 55 && !this.smoke){
            table = this.w60     
          }

          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && this.smoke){
            table = this.w65s          
          }        
                  
          if(parseInt(this.wiek) <= 65 && parseInt(this.wiek) > 60 && !this.smoke){
            table = this.w65          
          }        

          if(parseInt(this.wiek) >= 70 && this.smoke){
            table = this.w70s         
          }
          
          if(parseInt(this.wiek) >= 70 && !this.smoke){
            table = this.w70
          }
        }
        
        return table
      },

      check(){
        if(this.wiek == '' || parseInt(this.wiek) < 0){
          alert('Blądny wiek!')          
        }
        else
        if(this.plec == ''){
          alert('Prosze obrać swoją płeć')
        }
        else
        if(parseFloat(this.cisnienia) < 100 || this.mmol < 2 || this.mg < 20){
          alert('Bląd dannych!')
        } 
        else{
          return true
        }
      }
    },
    watch: {
      mmol(){
        this.mg = 18*this.mmol
      },
      mg(){
        this.mmol = this.mg/18
      }
    }
  })
  

</script>

<style scoped>
  ion-input.custom {    
    color: #fff;        
    padding: 10px;    
    margin: 5px;    
    border-bottom: 2px solid white;    
    background: -webkit-linear-gradient(#121212 70%, #333333 );
  }

  ion-grid.mainGrid{   
    display: flexbox;     
    margin: 0px;
    padding: 0px;
    width: 30%;
    text-align: left;
    justify-content: left; 
  }  

  ion-col.gridText{    
    display: flex;
    align-items: center; 
  }

  .content{
    display: flex;
    margin: 10px;
    margin-top: 5%;
    padding: 10px;
    padding-left: 10%;
    justify-content: left;
  }

  .information{
    border: 1px solid white;
    display: flex;
    width: 30%;
    margin: 5px;
    margin-left: 10%;
    margin-right: 20%;
    height: max-content;
    padding: 15px;
    border-radius: 3px; 
  }

  .buttonOblicz{
    margin: 10px;
    margin-left: 0px;
    border: 1px solid white;
    border-radius: 5px;
    --background: transparent;
  }

  .buttonOblicz:hover{
    background-color: #222222;
  }
</style>