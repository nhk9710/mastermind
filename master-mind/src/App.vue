<script setup>
import { ref,reactive,nextTick } from 'vue'
import ConfettiExplosion from 'vue-confetti-explosion'

/* Computer ====================================*/
let computerSelectList = [];
let tryAgain = reactive({chk:false});

/* User =========================================*/
let userSelectedList = reactive([]);
let myColor = '';
let colorIndex = 0;
let countAccept = reactive({count:0});
let userName = '';
let userDialog = reactive({chk: false});
let userScore = reactive([]);


/* 사용자가 색을 선택했을 때 */
function pickColor(color, i){
    if(userName===''){
        userDialog.chk = true;
    }
  myColor = color;
  colorIndex = i;
}
/* 닉네임 입력 */
function inputUserName(){
    userDialog.chk = false;
}


/* 선택한 색을 원하는 위치에 놓았을 때 */
function selectPosition(index){
  if(myColor===''){
    alert('색을 선택해 주세요!');
    return false;
  }

  if(userSelectedList[countAccept.count].colors.includes(myColor)){
    alert('같은색을 사용할 수 없습니다!');
    return false;
  }

  userSelectedList[countAccept.count].colors[index] = myColor;
  myColor = '';
}

/* accept 버튼을 눌렀을때 */
function accept(){
    let congratulation = 0;
  if(userSelectedList[countAccept.count].colors.includes('grey-darken-2')){
    alert('색을 모두 선택해주세요!');
    return false;
  }
    /* 계산식 들어갈 자리 */
    for (let i = 0; i < computerSelectList.length; i++) {
        if (userSelectedList[countAccept.count].colors[i] === computerSelectList[i]) {
            userSelectedList[countAccept.count].checkList[i] = 'red'
            congratulation +=1;
        } else if (computerSelectList.includes(userSelectedList[countAccept.count].colors[i])) {
            userSelectedList[countAccept.count].checkList[i] = 'blue'
        }else{
            userSelectedList[countAccept.count].checkList[i] = 'grey-darken-1'
        }
    }
    if(congratulation===4){
        alert(`You did it the ${countAccept.count+1} time!!!!`);
        userScore.push({ player: userName, tryCount: countAccept.count+1 })
        tryAgain.chk = true
        explode();
        return false;
    }
    console.log(computerSelectList)
    userSelectedList[countAccept.count].checkList.sort()
  /* =====계산식 끝==== */
  countAccept.count++;

  if(countAccept.count===10){
      alert('You failed!!!!!!!');
      tryAgain.chk = true;
      return false;
  }
}

function showHint(){
    alert('One of the Color is' + ' ' + computerSelectList[Math.floor(Math.random() * computerSelectList.length)]);
    return false;
}
const explode = async() => {
    confetti_Visible.value = false;
    await nextTick();
    confetti_Visible.value = true;
}

function newGame(){
    tryAgain.chk = false
    userSelectedList = reactive([]);
    for (let i = 0; i < 10; i++) {
        userSelectedList.push({
            colors: ['grey-darken-2', 'grey-darken-2', 'grey-darken-2', 'grey-darken-2'],
            index: i,
            checkList: ['grey-darken-2', 'grey-darken-2', 'grey-darken-2', 'grey-darken-2']
        })
    }
    colorIndex = 0;
    countAccept = reactive({count:0});
    tryAgain = reactive({chk:false});
    randomColors();
    // console.log(computerSelectList)
}

/* =============================Created Start========================= */
const confetti_Visible = ref(false);
    for (let i = 0; i < 10; i++) {
        userSelectedList.push({
            colors: ['grey-darken-2', 'grey-darken-2', 'grey-darken-2', 'grey-darken-2'],
            index: i,
            checkList: ['grey-darken-2', 'grey-darken-2', 'grey-darken-2', 'grey-darken-2']
        })
    }
    let colors = ['red', 'teal', 'blue', 'yellow', 'orange', 'purple']
    const dialog = reactive({state: false, summary: false})
function three_line_summary(){
        dialog.summary = dialog.summary !== true;
}

    function activeDialog() {
        dialog.state = dialog.state !== true;
    }

    /* 컴퓨터 랜덤 색상 */
    function randomColors() {
        computerSelectList = [];
        for (let i = 0; i < 4; i++) {
            computerSelectList.push(colors[Math.floor(Math.random() * colors.length)])
        }
        let set = new Set(computerSelectList);

        if (4 !== set.size) {
            randomColors();
        }
    }
    randomColors();
// console.log(computerSelectList)
/* ==============================Created End============================ */
</script>

<template>
  <div class="PlayBox d-flex flex-column align-center">
    <ConfettiExplosion v-if="confetti_Visible" :stageHeight="900" />

    <div class="text-h3 GameTitle d-flex justify-center">
      <span>MasterMind</span>
    </div>

    <div class="BoardSet d-flex justify-center mb-lg-10">
      <div class="d-flex flex-column align-center  GameScore mr-10">
        <div class="text-h5 font-weight-bold ma-3">Your Score</div>

        <v-card
          v-for="(score, index) in userScore"
          :key="`userScore-${index}`"
          variant="outlined"
          width="80%"
          class="mb-2"
        >
          <v-card-subtitle> player: {{ score.player }} </v-card-subtitle>
          <v-divider></v-divider>
          <v-card-text>you tried {{ score.tryCount }}!!!!</v-card-text>
        </v-card>
      </div>
      <div class="d-flex flex-column justify-center GameBoard">
        <v-card
          v-for="(card,index) in userSelectedList"
          :key="`card-${index}`"

          :color="index!==countAccept.count ? 'grey-lighten-3' : 'lime-lighten-2'"
          class="mb-3 d-flex justify-space-around pa-1"
        >
          <v-btn v-for="(item, i) in card.colors" @click="selectPosition(i)"  :style="index!==countAccept.count ? 'pointer-events:none;':''" :key="`item-${i}`" :color="item" icon="mdi-plus"></v-btn>
        </v-card>
      </div>
      <div class="d-flex flex-column justify-center ChkBoard ml-10">
        <v-card
          v-for="(item, index) in userSelectedList"
          :key="`item-key-${index}`"
          variant="tonal"
          class="mb-3 pa-1 d-flex justify-space-around"
        >
          <v-btn v-for="(chk, index) in item.checkList" style="pointer-events: none" :key="`chk-${index}`" :color="chk" icon="mdi-plus"></v-btn>
        </v-card>
      </div>
    </div>

    <div class="UserBoard d-flex flex-column align-center">
      <v-card variant="tonal"  class="d-flex justify-space-around UserColor mb-10 pa-1">
        <v-btn v-for="(color,i) in colors" :key="`colors-${i}`" @click="pickColor(color, i)" :color="color" icon="mid-plus"></v-btn>
      </v-card>

      <div class="d-flex justify-space-around">
      <v-btn class="CheckBtn" color="blue-accent-3"  @click="accept">accept</v-btn>
      <template v-if="countAccept.count===5">
        <v-btn class="HintBtn" color="blue-grey-lighten-1" @click="showHint">Hint</v-btn>
      </template>
        <template v-if="tryAgain.chk">
          <v-btn class="checkBtn" color="red" @click="newGame">New Game</v-btn>
        </template>


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
          <v-card >
            <v-card-text>
                <div v-if="!dialog.summary">
              - 컴퓨터가 일련의 색상을 선택합니다. 색상 수는 코드 길이입니다. 기본 코드 길이는 4이지만 새 게임을 시작할 때 변경할 수 있습니다.<br><br>

              - 게임의 목적은 컴퓨터의 순서에서 색상의 정확한 위치를 추측하는 것입니다.<br><br>

              - 기본적으로 색상은 코드 시퀀스에서 한 번만 사용할 수 있습니다. '중복 허용'을 선택한 상태에서 새 게임을 시작하면 코드 시퀀스에서 모든 색상을 여러 번 사용할 수 있습니다.<br><br>

              - 당신의 추측으로 한 줄을 채우고 'accept' 버튼을 클릭하면 컴퓨터가 당신의 추측 결과로 응답합니다.<br><br>

              - 코드 시퀀스에서 올바른 색상과 올바른 위치에 있는 추측의 각 색상에 대해 컴퓨터는 현재 추측의 오른쪽에 작은 빨간색을 표시합니다.<br><br>

              - 올바른 색상이지만 코드 시퀀스에서 올바른 위치에 있지 않은 추측의 각 색상에 대해 컴퓨터는 현재 추측의 오른쪽에 작은 파란색을 표시합니다.<br><br>

              - 모든 표시된 색상의 위치와 선택한 색상의 상관관계는 없습니다. 단지 추측이 맞았다 아니다만 알 수 있습니다.<br><br>

              - 코드 시퀀스의 모든 색상을 추측하고 모두 올바른 위치에 있을 때 게임에서 승리합니다.<br><br>

              - 컴퓨터 코드 시퀀스를 추측하지 않고 모든 시도를 사용하면 게임에서 패배합니다.<br><br>

              이 게임을 하는 방법:<br><br>

              - 선 채우기를 시작하려면 먼저 표 하단에서 색상을 클릭하여 선택해야 합니다. 색상을 선택한 후 원하는 위치를 클릭하여 위의 현재 추측 라인에 넣을 수 있습니다.<br><br>

              - 색상 선택을 변경하려면 하단의 색상 중 하나에서 새 색상을 클릭하면 됩니다.<br><br>

              - 전체 줄을 채운 후에도 컴퓨터가 추측에 응답하도록 요청하기 전에 선택을 변경할 수 있습니다. 추측에 만족하면 '확인' 버튼을 클릭하고 컴퓨터 응답을 받으세요.
                </div>
              <div v-else>
                - 색상을 선택하고 넣어주세요(중복안됨)<br><br>
                - 한줄 완성하셨으면 accept 버튼을 눌러주세요<br><br>
                - 10번안에 맞추면 성공 아니면 실패입니다.<br><br>
              </div>
            </v-card-text>
            <template class="d-flex justify-end">
              <v-card-actions>
                <v-btn color="red" block @click="three_line_summary" >summary</v-btn>
              </v-card-actions>
              <v-card-actions>
              <v-btn color="primary" block @click="dialog.state = false">close</v-btn>
              </v-card-actions>
            </template>
          </v-card>
        </v-dialog>
        <v-dialog
          v-model="userDialog.chk"
          width="30%"
        >
          <v-card>
          <v-text-field label="NickName" v-model="userName" style></v-text-field>
            <v-btn color="primary" @click="inputUserName">submit</v-btn>
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
  width: 35%;
}
.UserColor{
  width: 50%;
}
.CheckBtn{
  width: 10%;
}

</style>
