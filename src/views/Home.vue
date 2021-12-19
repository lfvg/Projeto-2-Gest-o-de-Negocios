<template>
  <div>
    <div style="position: relative">
      <img src="@/assets/nuback.jpg" height="850" />
      <v-btn
        @click="handleOpenNuPay"
        fab
        large
        style="
          position: absolute;
          top: 290px;
          left: 29px;
          background-color: #f0f1f5 !important;
        "
        depressed
      >
        <v-icon color="dark-grey">mdi-cash</v-icon>
      </v-btn>
    </div>
    <!-- scroll-y-reverse-transition dialog-bottom-transition -->
    <!-- DIALOG DE TIMELINE -->
    <v-dialog
      :fullscreen="true"
      v-model="openNuPay"
      transition="scroll-y-reverse-transition"
      scrollable
    >
      <v-card>
        <v-card-title>
          <v-toolbar flat style="padding: 4px 0px !important">
            <div style="display: flex; align-items: center">
              <v-btn icon color="dark-grey" @click="openNuPay = false">
                <v-icon>mdi-chevron-left</v-icon>
              </v-btn>
              <v-text-field
                @click="searchModel = true"
                style="margin-bottom: -24px"
                solo
                dense
                append-icon="mdi-magnify"
              />
            </div>
          </v-toolbar>
        </v-card-title>
        <v-card-text>
          <div class="text-overline">Atividades</div>
          <div class="text-caption">Hoje</div>
          <!-- <div v-for"itemToday in itemsToday" :key="id">
            {{itemToday}}
          </div> -->
          <div
            v-for="itemToday in itemsToday"
            :key="itemToday.id"
            style="padding-top: 24px"
          >
            <!-- {{itemToday}} -->
            <v-row style="flex-wrap: nowrap">
              <div
                style="
                  width: 64px;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                "
              >
                <img :src="itemToday.picture" class="avatar" />
              </div>
              <div class="card-text">
                <div style="font-weight: bold; margin-right: 3px">
                  {{ itemToday.user }}
                </div>
                <div style="margin-right: 3px">
                  {{ itemToday.action }}
                </div>
                <div style="color: blueviolet">
                  {{ itemToday.receiver }}
                </div>
              </div>
            </v-row>
            <v-row style="margin-left: 62px">
              <div v-if="itemToday.points">
                <v-icon v-for="index in itemToday.points" small :key="index">
                  mdi-star
                </v-icon>
              </div>
            </v-row>
            <v-row justify="end">
              <div
                clss="text-caption"
                style="margin-right: 14px; font-size: 0.7rem"
              >
                {{ itemToday.time }}
              </div>
            </v-row>
            <v-divider class="divider-margin"></v-divider>
          </div>

          <div class="text-caption" style="margin-top: 16px">Terça</div>
          <!-- <div v-for"itemToday in itemsToday" :key="id">
            {{itemToday}}
          </div> -->
          <div
            v-for="itemBefore in itemsBefore"
            :key="itemBefore.id"
            style="padding-top: 24px"
          >
            <!-- {{itemToday}} -->
            <v-row style="flex-wrap: nowrap">
              <div
                style="
                  width: 64px;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                "
              >
                <img :src="itemBefore.picture" class="avatar" />
              </div>
              <div class="card-text">
                <div style="font-weight: bold; margin-right: 3px">
                  {{ itemBefore.user }}
                </div>
                <div style="margin-right: 3px">
                  {{ itemBefore.action }}
                </div>
                <div style="color: blueviolet">
                  {{ itemBefore.receiver }}
                </div>
              </div>
            </v-row>
            <v-row style="margin-left: 62px">
              <div v-if="itemBefore.points">
                <v-icon v-for="index in itemBefore.points" small :key="index">
                  mdi-star
                </v-icon>
              </div>
            </v-row>
            <v-row justify="end">
              <div
                clss="text-caption"
                style="margin-right: 14px; font-size: 0.7rem"
              >
                {{ itemBefore.time }}
              </div>
            </v-row>
            <v-divider class="divider-margin"></v-divider>
          </div>
        </v-card-text>
      </v-card>
    </v-dialog>
    <!-- DIALOG DE BUSCA -->
    <v-dialog v-model="searchModel" :fullscreen="true">
      <v-card>
        <!-- searchList -->
        <v-card-title>
          <v-toolbar flat style="padding: 4px 0px !important">
            <div style="display: flex; align-items: center">
              <v-btn icon color="dark-grey" @click="searchModel = false">
                <v-icon>mdi-chevron-left</v-icon>
              </v-btn>
              <v-autocomplete
                v-model="searchAutoCompleteModel"
                style="margin-bottom: -24px"
                solo
                dense
                :items="searchListPos"
                hide-no-data
                hide-selected
                :search-input.sync="search"
              >
                <v-icon slot="append" style="transform: none !important">
                  mdi-magnify
                </v-icon>
              </v-autocomplete>
            </div>
          </v-toolbar>
        </v-card-title>
        <v-card-text v-if="showResults">
          <!-- searchResults -->
          <div
            v-for="searchResult in searchResults"
            :key="searchResult.id"
            style="padding-top: 24px"
            @click="chatModel = true"
          >
            <!-- {{itemToday}} -->
            <v-row style="flex-wrap: nowrap">
              <div
                style="
                  width: 64px;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                "
              >
                <img :src="searchResult.photo" class="avatar" />
              </div>

              <div class="card-text">
                <div style="font-weight: bold; margin-right: 3px; min-width:109px">
                  {{ searchResult.name }}
                </div>
                <div style="margin-left:48px; display:flex">
                  <v-icon small>
                    mdi-star
                  </v-icon>
                  <div style="margin-bottom:-2px">
                  {{searchResult.rate}}
                  </div>
                </div>
              </div>
            </v-row>

            <v-row style="margin-left: 64px">
             
              <div
                style="margin-right: 14px; font-size: 0.7rem; min-width: 42px"
              >
                <v-icon small>mdi-thumb-up</v-icon>
                {{ searchResult.likes }}
              </div>
               <div
                style="font-size: 0.7rem; margin-right: 4px; min-width: 42px"
              >
                <v-icon small style="margin-bottom: -2px">mdi-message</v-icon>
                {{ searchResult.comments }}
              </div>
            </v-row>
            <v-divider class="divider-margin"></v-divider>
          </div>
        </v-card-text>
      </v-card>
    </v-dialog>
    <!-- DIALOG DE CHAT -->
    <v-dialog v-model="chatModel" scrollable :fullscreen="true">
      <v-card>
        <v-card-title>
          <v-toolbar flat>
            <div style="display: flex; align-items: center; width: 100%">
              <v-btn icon color="dark-grey" @click="chatModel = false">
                <v-icon>mdi-chevron-left</v-icon>
              </v-btn>
              <div
                style="width: calc(100% - 96px)"
                class="text-center text-body-1"
              >
                Ana Flávia
              </div>
              <v-btn icon color="dark-grey" @click="handleHome">
                <v-icon>mdi-home</v-icon>
              </v-btn>
            </div>
          </v-toolbar>
        </v-card-title>
        <v-card-text style="padding-top: 32px !important">
          <v-row v-for="(message, index) in chatHistory" :key="message">
            <v-row style="width: 90%; display: flex; justify-content: end">
              <div
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: #c584ff;
                  border-radius: 16px;
                  margin: 16px 32px;
                "
              >
                {{ message }}
              </div>
            </v-row>

            <v-row style="width: 90%" v-if="index === 0">
              <div
                v-if="resp1"
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: gainsboro;
                  border-radius: 16px;
                  margin: 16px 32px;
                  margin-top: -6px;
                "
              >
                Oi Luiz, em que posso ajudá-lo?
              </div>
            </v-row>
            <v-row style="width: 90%" v-if="index === 1">
              <div
                v-if="resp2"
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: gainsboro;
                  border-radius: 16px;
                  margin: 16px 32px;
                  margin-top: -6px;
                "
              >
                Eu tenho um horário livre amanhã as 14:30 e sexta de 17h:00
              </div>
            </v-row>
            <v-row style="width: 90%" v-if="index == 2">
              <div
                v-if="resp3"
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: gainsboro;
                  border-radius: 16px;
                  margin: 16px 32px;
                  margin-top: -6px;
                "
              >
                Fica por R$76.00
              </div>
            </v-row>
            <v-row style="width: 90%" v-if="index == 3">
              <div
                v-if="resp4"
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: gainsboro;
                  border-radius: 16px;
                  margin: 16px 32px;
                  margin-top: -6px;
                "
              >
                <v-card flat style="background-color: gainsboro">
                  <v-card-text>
                    Ana Flávia lhe enviou uma solicitção de transferencia. Para
                    enviar $76.00 para Ana Flávia clique no botão transferir.
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn
                      :disabled="disablePay"
                      @click="handlePay"
                      small
                      style="background-color: blueviolet; color: white"
                    >
                      Transferir
                      <v-icon> mdi-currency-usd </v-icon>
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </div>
            </v-row>
            <v-row style="width: 90%">
              <div
                v-if="resp5"
                style="
                  min-height: 48px;
                  padding: 16px;
                  background-color: gainsboro;
                  border-radius: 16px;
                  margin: 16px 32px;
                  margin-top: -6px;
                "
              >
                Te vejo sexta
              </div>
            </v-row>
          </v-row>
          <v-row v-if="typingModel">
            <!-- balaozinho -->
            <div class="typingBaloom">
              <div class="dots dots-1"></div>
              <div class="dots dots-2"></div>
              <div class="dots dots-3"></div>
            </div>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-text-field
            color="blueviolet"
            v-model="chatMessage"
            solo
            dense
            @click:append="handleSendMessage"
            label="Mensagem"
          >
            <v-icon slot="append" color="#830ad1" @click="handleSendMessage">
              mdi-send
            </v-icon>
          </v-text-field>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- sucess dialog -->
    <v-dialog style="box-shadow: unset; border-radius:50%"  content-class="success-dialog" v-model="successDialogControl" persistent   width="160"> 
       <v-card style="box-shadow: unset; border-radius:50%" width="300px">
          <div class="success-dialog-image">
            <img src="@/assets/check.svg" width="160px"/>
          </div>
          
        </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "Home",
  watch: {
    search(val) {
      console.log(val)
      this.querySelections(val);
      
      //if (val.length > 0) this.searchListPos = this.searchList
      //else this.searchListPos = []
    },
  },
  data() {
    return {
      search: null,
      openNuPay: false,
      chatModel: false,
      searchModel: false,
      showResults: false,
      successDialogControl: false,
      disablePay: false,
      chatMessage: "",
      chatHistory: [],
      typingModel: false,
      searchAutoCompleteModel: "",
      resp1: false,
      resp2: false,
      resp3: false,
      resp4: false,
      resp5: false,
      itemsToday: [
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/92.jpg",
          id: 2,
          user: "Paulo",
          action: "recebeu um pagamento de ",
          receiver: "Christina Santos",
          time: "10:28",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/4.jpg",
          id: 3,
          user: "César Bulhões",
          action: "pagou ",
          receiver: "Liz",
          time: "09:35",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 4,
          user: "Nathália França",
          action: "contratou ",
          receiver: "Walber Rodrigues",
          time: "09:00",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 1,
           user: "Nathália França",
          action: "avaliou ",
          receiver: "Júlio Venancio",
          points: 4,
          time: "08:55",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 5,
           user: "Nathália França",
          action: "pagou ",
          receiver: "Walber Rodrigues",
          time: "08:26",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/92.jpg",
          id: 6,
          user: "Paulo",
          action: "recebeu um pagamento de ",
          receiver: "Christina Santos",
          time: "08:15",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/4.jpg",
          id: 7,
          user: "César Bulhões",
          action: "pagou ",
          receiver: "Liz",
          time: "07:02",
        },
      ],
      itemsBefore: [
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 8,
           user: "Nathália França",
          action: "contratou ",
          receiver: "Walber Rodrigues",
          time: "22:28",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 9,
           user: "Nathália França",
          action: "avaliou ",
          receiver: "Júlio Venancio",
          points: 4,
          time: "21:53",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 10,
           user: "Nathália França",
          action: "pagou ",
          receiver: "Walber Rodrigues",
          time: "16:30",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/92.jpg",
          id: 11,
          user: "Paulo",
          action: "recebeu um pagamento de ",
          receiver: "Christina Santos",
          time: "13:43",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/men/4.jpg",
          id: 11,
          user: "César Bulhões",
          action: "pagou ",
          receiver: "Liz",
          time: "13:28",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 12,
           user: "Nathália França",
          action: "contratou ",
          receiver: "Walber Rodrigues",
          time: "12:14",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 13,
           user: "Nathália França",
          action: "avaliou ",
          receiver: "Júlio Venancio",
          points: 4,
          time: "10:28",
        },
        {
          picture: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          id: 14,
           user: "Nathália França",
          action: "pagou ",
          receiver: "Walber Rodrigues",
          time: "08:31",
        },
      ],
      searchList: [
        "Mecânico",
        "Encanador",
        "Guincho",
        "Jardineiro",
        "Diarista",
        "Pintor",
        "Cartomante",
        "Cabeleireiro",
        "Carpinteiro",
      ],
      searchListPos: [],
      searchResults: [
        {
          id: 1,
          photo: "https://randomuser.me/api/portraits/thumb/men/59.jpg",
          name: "Jacob Bergeron",
          rate: 4,
          comments: 32,
          likes: 63,
        },
        {
          id: 2,
          photo: "https://randomuser.me/api/portraits/thumb/men/3.jpg",
          name: "Eduardo Pearson",
          rate: 5,
          comments: 9,
          likes: 82,
        },
        {
          id: 3,
          photo: "https://randomuser.me/api/portraits/thumb/women/42.jpg",
          name: "Bella Nichols",
          rate: 3,
          comments: 15,
          likes: 33,
        },
        {
          id: 4,
          photo: "https://randomuser.me/api/portraits/thumb/women/54.jpg",
          name: "Ana Flávia",
          rate: 5,
          comments: 155,
          likes: 241,
        },
        {
          id: 5,
          name: "Hilla Autio",
          photo: "https://randomuser.me/api/portraits/thumb/women/27.jpg",
          rate: 4,
          comments: 20,
          likes: 44,
        },
        {
          id: 6,
          photo: "https://randomuser.me/api/portraits/thumb/women/37.jpg",
          name: "Aino Lampinen",
          rate: 2,
          comments: 2,
          likes: 10,
        },
      ],
    };
  },
  methods: {
    querySelections(val) {
      console.log(val);
      if (val.length > 0) this.searchListPos = this.searchList;
      else this.searchListPos = [];
      if(val === "Cartomante") this.showResults = true;
    },
    handleOpenNuPay() {
      this.openNuPay = true;
    },
    handleSendMessage() {
      this.chatHistory.push(this.chatMessage);
      this.chatMessage = "";
      setTimeout(() => {
        this.typingModel = true;
      }, 3000);
      setTimeout(() => {
        this.typingModel = false;
        if (this.chatHistory.length === 1) this.resp1 = true;
        else if (this.chatHistory.length === 2) this.resp2 = true;
        else if  (this.chatHistory.length === 3) this.resp3 = true;
        else if  (this.chatHistory.length === 4) this.resp4 = true;
      }, 6500);
    },
    handlePay(){
      this.successDialogControl = true;
      setTimeout(() => {
        this.successDialogControl = false;
        this.typingModel = true;
      }, 3000);
      setTimeout(() => {
        this.typingModel = false;
        this.disablePay = true;
        console.log('entrou')
        this.resp5 = true;
        this.resp5 = true;
      }, 6500);
    },
    handleHome(){
      this.openNuPay= false;
      this.chatModel= false;
      this.searchModel= false;
      this.showResults= false;
      this.successDialogControl= false;
      this.disablePay= false;
      this.resp1= false;
      this.resp2= false;
      this.resp3= false;
      this.resp4= false;
      this.resp5= false;
    }
  },
};
</script>

<style scoped>
.divider-margin {
  margin-top: 24px;
  margin-bottom: 4px;
}
.avatar {
  vertical-align: middle;
  width: 36px;
  height: 36px;
  border-radius: 50%;
}
.card-text {
  display: flex;
  padding: 12px;
  flex-wrap: wrap;
  
}
.card > * {
  flex: 1 1 30%;
}
.dots {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  display: inline-block;
  margin: 0.1rem;
  animation: scaling 2.5s ease-in-out infinite;
}

@keyframes scaling {
  0%,
  100% {
    transform: scale(0.5);
    background-color: darkgray;
  }
  40% {
    transform: scale(1);
    background-color: blueviolet;
  }
  50% {
    transform: scale(1);
    background-color: blueviolet;
  }
}
.typingBaloom {
  background-color: gainsboro;
  border-radius: 16px;
  margin-left: 20px;
  padding: 2px 8px;
}
.dots-1 {
  animation-delay: 0s;
}
.dots-2 {
  animation-delay: 0.2s;
}
.dots-3 {
  animation-delay: 0.4s;
}
.v-icon .notranslate .mdi .mdi-magnify .theme--light {
  transform: none !important;
}
.v-autocomplete.v-select--is-menu-active .v-input__icon--append .v-icon {
  transform: none !important;
}
.v-input
  .v-input--is-focused
  .v-input--dense
  .theme--light
  .v-text-field
  .v-text-field--single-line
  .v-text-field--solo
  .v-text-field--is-booted
  .v-text-field--enclosed
  .v-select
  .v-select--is-menu-active
  .v-autocomplete
  .primary--text {
  transform: none !important;
}

  .success-dialog{
    justify-content: center;
    display: flex;
    box-shadow: unset!important;
    -webkit-box-shadow: unset!important;
  }
  .success-dialog-image{
    width: 100%; 
    display:flex; 
    justify-content: center; 
    height: 160px; 
    align-content:center; 
    background-color: white;
    border-radius: 50%;
  }
  .success-dialog-text{
    justify-content: center;
    
  }
</style>
