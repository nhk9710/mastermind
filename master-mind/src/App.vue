<script setup>
import { reactive } from 'vue'

/* User */
let userSelectedList = [];
let selectColor = ['grey-darken-2','grey-darken-2','grey-darken-2','grey-darken-2'];

/* Computer */
let computerSelectList = [];

/* Created */
for(let i=0; i<10; i++){
  userSelectedList.push({ colors: ['grey-darken-2','grey-darken-2','grey-darken-2','grey-darken-2'], index: i, checkList: ['', '', '', ''] })
}
let colors = ['red', 'teal', 'blue', 'yellow']
const dialog = reactive({state:false})

function activeDialog() {
  console.log(dialog.state + '4')
  if(dialog.state===true){
    console.log(dialog.state + '1')
    dialog.state = false
  }else{
    console.log(dialog.state + '2')
    dialog.state = true;
  }
  console.log(dialog.state + '3')
}
</script>

<template>
  <div class="PlayBox d-flex flex-column align-center">


    <div class="text-h3 GameTitle d-flex justify-center">
      <span>MasterMind</span>
    </div>

    <div class="BoardSet d-flex justify-center mb-lg-10">
      <div class="d-flex flex-column align-center  GameScore mr-10">
        <div class="text-h5 font-weight-bold ma-3">Your Score</div>
      </div>
      <div class="d-flex flex-column justify-center GameBoard">
        <v-card
          v-for="(card,index) in userSelectedList"
          :key="`card-${index}`"
          variant="tonal"
          class="mb-3 d-flex justify-space-around pa-1"
        >
          <v-btn v-for="(item, i) in card.colors" :key="`item-${i}`" :color="item" icon="mdi-plus"></v-btn>
        </v-card>
      </div>
      <div class="d-flex flex-column justify-center ChkBoard ml-10">
        <v-card
          v-for="(item, index) in userSelectedList"
          :key="`item-key-${index}`"
          variant="tonal"
          class="mb-3 pa-1 d-flex justify-space-around"
        >
          <v-btn v-for="(chk, index) in item.checkList" :key="`chk-${index}`" :color="chk==='' ? 'grey' : chk==='color' ? 'white' : 'red'" icon="mdi-plus"></v-btn>
        </v-card>
      </div>
    </div>


    <div class="UserBoard d-flex flex-column align-center">
      <v-card variant="tonal"  class="d-flex justify-space-around UserColor mb-10 pa-1">
        <v-btn v-for="(color,i) in colors" :key="`colors-${i}`" :color="color" icon="mid-plus"></v-btn>
      </v-card>

      <div class="d-flex justify-space-around">
      <v-btn class="CheckBtn" color="blue-accent-3">accept</v-btn>

        <v-btn
            color="yellow-lighten-1"
            @click="activeDialog"
        >
          Rule
        </v-btn>

        <v-dialog
          v-model="dialog.state"
          width="50%"
        >
          <v-card>
            <v-card-text>
              - 컴퓨터가 일련의 색상을 선택합니다. 색상 수는 코드 길이입니다. 기본 코드 길이는 4이지만 새 게임을 시작할 때 변경할 수 있습니다.

              - 게임의 목적은 컴퓨터의 순서에서 색상의 정확한 위치를 추측하는 것입니다.

              - 기본적으로 색상은 코드 시퀀스에서 한 번만 사용할 수 있습니다. '중복 허용'을 선택한 상태에서 새 게임을 시작하면 코드 시퀀스에서 모든 색상을 여러 번 사용할 수 있습니다.

              - 당신의 추측으로 한 줄을 채우고 '확인' 버튼을 클릭하면 컴퓨터가 당신의 추측 결과로 응답합니다.

              - 코드 시퀀스에서 올바른 색상과 올바른 위치에 있는 추측의 각 색상에 대해 컴퓨터는 현재 추측의 오른쪽에 작은 빨간색을 표시합니다.

              - 올바른 색상이지만 코드 시퀀스에서 올바른 위치에 있지 않은 추측의 각 색상에 대해 컴퓨터는 현재 추측의 오른쪽에 작은 흰색을 표시합니다.

              - 코드 시퀀스의 모든 색상을 추측하고 모두 올바른 위치에 있을 때 게임에서 승리합니다.

              - 컴퓨터 코드 시퀀스를 추측하지 않고 모든 시도를 사용하면 게임에서 패배합니다.

              이 게임을 하는 방법:

              - '새 게임 시작' 버튼을 클릭하여 새 게임을 시작합니다. 기본 게임 매개변수를 변경하려면 '새 게임 시작' 버튼을 클릭하기 전에 '코드 길이' 및/또는 '중복 허용' 필드를 변경할 수 있습니다.

              - 선 채우기를 시작하려면 먼저 표 하단에서 색상을 클릭하여 선택해야 합니다. 색상을 선택한 후 원하는 위치를 클릭하여 위의 현재 추측 라인에 넣을 수 있습니다.

              - 색상 선택을 변경하려면 두 가지 방법이 있습니다. 하나는 하단의 색상 중 하나에서 새 색상을 클릭하는 것이고 다른 하나는 마우스 휠을 스크롤하는 것입니다.

              - 전체 줄을 채운 후에도 컴퓨터가 추측에 응답하도록 요청하기 전에 선택을 변경할 수 있습니다. 추측에 만족하면 '확인' 버튼을 클릭하고 컴퓨터 응답을 받으세요.
            </v-card-text>
            <v-card-actions>
              <v-btn color="primary" block @click="dialog.state = false">close</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </div>
  </div>
</template>

<style scoped>
.PlayBox {
  width: 100vw;
  height: 100vh;
}

.GameTitle{
  padding: 10px;
}

/* BoardSet Start */
.BoardSet{
  width: 100%;
}

.GameScore{
  width: 15%;
  border: 1px solid grey;
  border-radius: 10px;
}

.GameBoard{
  width: 15%;
}
.ChkBoard{
  width: 15%;
}
/* BoardSet End */
.UserBoard{
  width: 30%;
}
.UserColor{
  width: 50%;
}
.CheckBtn{
  width: 10%;
}

</style>
