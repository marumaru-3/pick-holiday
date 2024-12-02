<script setup>
import { ref } from 'vue'
import LeisureCard from './components/LeisureCard.vue'

let selectStart = ref(false)
let selectedEnd = ref(false)

setTimeout(() => {
  selectStart.value = true
}, 900)

const selectCategory = [
  { name: 'インドア系', imgSrc: '/pick-holiday/images/toshokan_benkyou.webp' },
  { name: 'アウトドア系', imgSrc: '/pick-holiday/images/pool_water_slider.webp' }
]

let select = ref(selectCategory)

const indoor = [
  { name: 'カフェ', imgSrc: '/pick-holiday/images/job_cafe_tenin_woman.webp' },
  { name: 'ショッピングモール', imgSrc: '/pick-holiday/images/shopping_mall_ekinaka.webp' },
  { name: '図書館', imgSrc: '/pick-holiday/images/toshokan_benkyou.webp' },
  { name: '映画館', imgSrc: '/pick-holiday/images/kandou_movie_eigakan.webp' },
  { name: 'ゲームセンター', imgSrc: '/pick-holiday/images/ufo_catcher.webp' },
  { name: '漫画喫茶', imgSrc: '/pick-holiday/images/building_manga_kissa.webp' },
  { name: '美術館', imgSrc: '/pick-holiday/images/bijutsu_kansyo2.webp' },
  { name: 'プラネタリウム', imgSrc: '/pick-holiday/images/planetarium.webp' },
  { name: 'カラオケ', imgSrc: '/pick-holiday/images/karaoke_young.webp' },
  { name: '雀荘', imgSrc: '/pick-holiday/images/ma-jan_woman.webp' },
]
const indoorImgs = indoor.map((obj) => obj.imgSrc)

const outdoor = [
  { name: '公園', imgSrc: '/pick-holiday/images/kouen_oyako.webp' },
  { name: 'キャンプ場', imgSrc: '/pick-holiday/images/yagai_kyoushitsu.webp' },
  { name: '動物園', imgSrc: '/pick-holiday/images/party_animal.webp' },
  { name: '水族館', imgSrc: '/pick-holiday/images/suizokukan_jinbeizame.webp' },
  { name: 'テーマパーク', imgSrc: '/pick-holiday/images/yuenchi.webp' },
  { name: 'プール', imgSrc: '/pick-holiday/images/pool_water_slider.webp' },
  { name: 'ビーチ', imgSrc: '/pick-holiday/images/seisyun_hamabe_oikakekko_couple.webp' },
  { name: 'スキー場', imgSrc: '/pick-holiday/images/ski_snowman.webp' },
  { name: 'アイススケート場', imgSrc: '/pick-holiday/images/skate_man.webp' },
  { name: '温泉', imgSrc: '/pick-holiday/images/onsen_animal.webp' },
]
const outdoorImgs = outdoor.map((obj) => obj.imgSrc)

function preloadImage(path) {
  let imgTag = document.createElement('img');
  imgTag.src = path;
}
indoorImgs.forEach((img) => {
  preloadImage(img)
})
outdoorImgs.forEach((img) => {
  preloadImage(img)
})

const pickedLeisureIndex = ref(0)
const newLeisureIndex = ref(1)
let cardEffectL = ref(false)
let cardEffectR = ref(false)
let cardEffectR02 = ref(false)

function pickLeisure(index) {
  window.scroll({ top: 0 })
  
  if (select.value[index].name === 'インドア系') {
    select.value = indoor
    selectStart.value = false
    setTimeout(() => {
      selectStart.value = true
    }, 700)
  } else if (select.value[index].name === 'アウトドア系') {
    select.value = outdoor
    selectStart.value = false
    setTimeout(() => {
      selectStart.value = true
    }, 700)
  } else {
    if (newLeisureIndex.value < select.value.length) {
      
      if (newLeisureIndex.value + 1 < select.value.length) {
        pickedLeisureIndex.value = index
        newLeisureIndex.value++
        if (newLeisureIndex.value + 1 <= select.value.length) {
          selectStart.value = true
        }
      } else {
        pickedLeisureIndex.value = index
        newLeisureIndex.value++
        if (newLeisureIndex.value + 1 <= select.value.length) {
          selectStart.value = true
        }
      }
    }
    
    if (!(newLeisureIndex.value < select.value.length)) {
      if (selectedEnd.value !== true) {
        selectStart.value = false
        setTimeout(() => {
          selectStart.value = true
        }, 800)
      }
      selectedEnd.value = true
    }
  }
}
function startCard() {
  window.scroll({ top: 0 })
  select.value = selectCategory
  pickedLeisureIndex.value = 0
  newLeisureIndex.value = 1
  selectStart.value = false
  setTimeout(() => {
    selectStart.value = true
  }, 900)
  selectedEnd.value = false
}
</script>

<template>
  <Transition name="progress">
    <div
      v-show="select.length !== 2"
      class="progress-bar"
      :style="{
        width: `${(newLeisureIndex / select.length) * 100}%`
      }"
    ></div>
  </Transition>
  <main :class="{ 'selected-end': selectedEnd }">
    <h2 v-if="select.length === 2" class="title typing01">お出かけするならどっち？</h2>
    <h2 v-else-if="newLeisureIndex < select.length" class="title typing02">どこに行く？</h2>
    <h2 v-else class="title typing03">今回はここにしよう！</h2>
      <div class="select-group" :class="{'start': selectStart}">
        <LeisureCard
          :cardClassL="cardEffectL"
          :name="select[pickedLeisureIndex].name"
          :imgSrc="select[pickedLeisureIndex].imgSrc"
          @click="pickLeisure(pickedLeisureIndex)"
        />
        <template v-if="newLeisureIndex < select.length">
          <p class="or">or</p>
          <Transition name="card">
            <LeisureCard
              :cardClassR="cardEffectR"
              :cardClassR02="cardEffectR02"
              :name="select[newLeisureIndex].name"
              :imgSrc="select[newLeisureIndex].imgSrc"
              @click="pickLeisure(newLeisureIndex)"
            />
          </Transition>
        </template>
        <template v-if="newLeisureIndex === select.length">
          <div class="result-check">
            <p>
              <a
                :href="'https://www.google.com/search?q=近くの' + select[pickedLeisureIndex].name"
                target="_blank"
                >近くの{{ select[pickedLeisureIndex].name }}</a
              >について調べる
            </p>
          </div>
          <div class="once-more" @click="startCard()">
            <span>もう一度選び直す</span>
          </div>
        </template>
      </div>
  </main>
</template>
<style>
html,
body {
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Noto Sans JP', sans-serif;
  font-optical-sizing: auto;
  font-style: normal;
}
* {
  box-sizing: border-box;
}
img {
  width: 100%;
}
.in-bl {
  display: inline-block;
}
</style>
<style scoped>
.progress-bar {
  position: fixed;
  top: 0;
  left: 0;
  width: 10%;
  height: 8px;
  background: linear-gradient(45deg, #9844b7, #6ec4e6, #44ea76);
  transition: 0.5s all;
}
.progress-enter-from {
  width: 0 !important;
}
.progress-enter-active {
  transition: 0.5s all;
}
.progress-enter-to {
  width: 10% !important;
}
.progress-leave-from {
  width: 10% !important;
}
.progress-leave-active {
  transition: 0.5s all;
}
.progress-leave-to {
  width: 0 !important;
}
main {
  margin: 100px auto;
  text-align: center;
}
.title {
  margin: 0 auto 60px;
}
.typing01,
.typing02,
.typing03 {
  animation: typing 0.6s steps(12), blink 2s step-end alternate forwards;
  white-space: nowrap;
  overflow: hidden;
  border-right: 3px solid;
  font-size: 2em;
  text-align: left;
}
.typing01 {
  width: calc(20ch + 3px);
  opacity: 1;
}
.typing02 {
  width: calc(10ch + 3px);
  animation: typing 0.4s steps(10), blink 2s step-end alternate forwards;
}
.typing03 {
  width: calc(17ch + 3px);
}

@keyframes typing {
  from {
    width: 0;
  }
}

@keyframes blink {
  0% {
    border-color: #000;
  }
  25% {
    border-color: transparent;
  }
  50% {
    border-color: #000;
  }
  75% {
    border-color: transparent;
  }
  100% {
    border-color: transparent;
  }
}

.or {
  font-size: 28px;
  margin: 0;
}

.select-group {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;

  opacity: 0;
  transform: translateY(20px);
}
/* .select-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.select-enter-active {
  transition: 0.6s all 2s;
}
.select-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.select-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.select-leave-active {
  transition: 0.6s all;
}
.select-leave-to {
  opacity: 0;
  transform: translateY(20px);
} */
.select-group.start {
  opacity: 1;
  transform: translateY(0);
  transition: 0.6s all;
}
.selected-end .select-group {
  flex-direction: column;
}

.result-check {
  position: relative;
  width: 90%;
  max-width: 600px;
  border: solid 3px #61d0c2;
  border-radius: 8px;
  box-sizing: border-box;
  font-size: 18px;
  padding: 20px;
  margin-top: 30px;
}
.result-check::before {
  content: '';
  position: absolute;
  top: -24px;
  left: 50%;
  margin-left: -15px;
  border: 12px solid transparent;
  border-bottom: 12px solid #fff;
  z-index: 2;
}
.result-check::after {
  content: '';
  position: absolute;
  top: -30px;
  left: 50%;
  margin-left: -17px;
  border: 14px solid transparent;
  border-bottom: 14px solid #61d0c2;
  z-index: 1;
}
.result-check p {
  display: inline-block;
  position: relative;
  margin-left: 30px;
}
.result-check p::before {
  content: '';
  position: absolute;
  background: url(../public/images/svg/search.svg) no-repeat;
  top: 2px;
  left: -30px;
  width: 24px;
  height: 24px;
}
.result-check a {
  color: #ff0052;
  margin-right: 5px;
  transition: .5s all;
}
.result-check a:hover {
  color: #ff00ea;
}

.once-more {
  cursor: pointer;
  margin-top: 30px;
  border: 2px solid #9844b7;
  border-radius: 5px;
  padding: 12px 24px;
  transition: .5s all;
}
.once-more:hover {
  background-color: #ff0052;
  border-color: #ff0052;
  color: #fff;
}

@media (max-width: 899px) {
  .title {
    font-size: 28px;
  }
  .or {
    font-size: 24px;
  }
}
@media (max-width: 699px) {
  main {
    margin: 50px auto;
  }
  .title {
    margin-bottom: 40px;
    font-size: min(7vw, 28px);
  }
  .select-group {
    flex-direction: column;
  }
}
</style>
