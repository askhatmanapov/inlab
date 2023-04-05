<template>
  <div class="upload" v-if="!check || !lab">
    <div v-if="!check" class="upload-area">
      <h3>Здравствуйте!</h3>
      <p>Просим вас загрузить анализы!</p>
      <input type="file" @change="handleFileSelect">
    </div>
    <div v-if="check === 1" class="wrong-format">
      <div v-if="!lab">
        <h3 style="color: red">Неправильный формат</h3>
        <p>Пожалуйста, загрузите правильный формат анализа. Или, можете вручную заполнить таблицу</p>
        <button type="button" @click="reloadPage">Пройти заново</button><button v-on:click="manual=true, lab = manual" style="float: right; margin-right: 2%;">Заполнить вручную</button>
      </div>
    </div>
    <div class="img-box">
      <img src="/src/assets/img1.png" alt=""/>
    </div>
  </div>
  <div v-if="check === 1" class="labs">
      <!-- <p>{{ extractedText }}</p> -->
      <h3>{{ lab }}</h3>
      <p>{{ name }} {{ age }}</p>
      <olymp v-if="lab === 'Olymp'" :extractedText="extractedText"></olymp>
      <invivo v-if="lab === 'Invivo'" :extractedText="extractedText"></invivo>
      <sapa v-if="lab === 'Sapa'" :extractedText="extractedText"></sapa>
      <UMC v-if="lab === 'UMC'" :extractedText="extractedText"></UMC>
    </div>
  <div v-if="manual" class="labs">
    <manual v-if="manual == true" @check="passdata"></manual>
  </div>
</template>
<script>

const pdfjsWorker = await new Promise(resolve => {
  import('pdfjs-dist/build/pdf.worker.entry').then(module => {
    resolve(module.default)
  })
})



  import * as pdfjsLib from 'pdfjs-dist';
  import Olymp from './components/Olymp.vue'
  import Invivo from './components/Invivo.vue'
  import Sapa from './components/Sapa.vue'
  import UMC from './components/UMC.vue'
  import Manual from './components/Manual.vue'
  import { ref } from 'vue';
  // const pdfjsWorker = await import('pdfjs-dist/build/pdf.worker.entry');
  export default {
    components: {
      Olymp,
      Invivo,
      Sapa,
      UMC,
      Manual
    },
    data() {
      return {
        extractedText: ref(""),
        lab: ref(""),
        name: '',
        age: '',
        check: ref(""),
        manual: ref("")
      }
    },
    methods: {
      passdata(check){
        this.check = check
      },
      reloadPage(){
        window.location.reload();
      },
      handleFileSelect(event) {
        this.check = 1
        const file = event.target.files[0]
        const reader = new FileReader();
        reader.onload = () => {
          const pdfData = new Uint8Array(reader.result);
          pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker;
          pdfjsLib.getDocument(pdfData).promise.then((pdf) => {
            let pageTextPromises = []
            for (let i = 1; i <= pdf.numPages; i++) {
              pageTextPromises.push(pdf.getPage(i).then((page) => {
                return page.getTextContent()
              }))
            }
            Promise.all(pageTextPromises).then((pages) => {
              let text = ''
              pages.forEach((page) => {
                page.items.forEach((item) => {
                  text += item.str + ' '
                })
              })
              this.extractedText = text

              if(this.extractedText.indexOf('info@kdlolymp.kz')>0) {
                this.lab = 'Olymp'

                let namei = this.extractedText.indexOf('отчёта') 
                while (this.extractedText[namei] != " ") {
                  namei++
                }
                let namef = namei + 2
                while (this.extractedText[namef] != " ") {
                  namef++
                }
                while (this.extractedText[namef + 1] != " ") {
                  namef++
                }
                while (namei<namef + 1){
                  this.name = this.name + this.extractedText[namei]
                  namei++
                }

                let agef = this.extractedText.indexOf('Жынысы') - 3
                while (this.extractedText[agef] != " ") {
                  agef--
                }
                while (this.extractedText[agef + 1] != " ") {
                  this.age = this.age + this.extractedText[agef + 1]
                  agef++
                }
              }


              if(this.extractedText.indexOf('ТОО «INVIVO»')>0) {
                this.lab = 'Invivo'

                let namei = this.extractedText.indexOf('исследования') 
                while (this.extractedText[namei] != " ") {
                  namei++
                }
                let namef = namei + 2
                while (this.extractedText[namef] != " ") {
                  namef++
                }
                while (this.extractedText[namef + 1] != " ") {
                  namef++
                }
                while (namei<namef + 1){
                  this.name = this.name + this.extractedText[namei]
                  namei++
                }

                let agef = this.extractedText.indexOf('Дәрігер') - 3
                while (this.extractedText[agef] != " ") {
                  agef--
                }
                while (this.extractedText[agef + 1] != " ") {
                  this.age = this.age + this.extractedText[agef + 1]
                  agef++
                }
              }


              if(this.extractedText.indexOf('info@sapalab.com')>0){
                this.lab = 'Sapa'

                let i = 0
                let namei = 0
                while(this.extractedText[this.extractedText.indexOf(' ТОО ') - i] != "2" && this.extractedText[this.extractedText.indexOf(' ТОО ') - i + 1] != "0"){
                  i++
                }
                let id = this.extractedText.indexOf(' ТОО') - i
                while (this.extractedText[id] != " ") {
                  id++
                  namei = id
                }
                while (namei<this.extractedText.indexOf(' ТОО')){
                  this.name = this.name + this.extractedText[namei]
                  namei++
                }

                let agei = this.extractedText.indexOf('(Ф.И.О)')
                while (this.extractedText[agei] != " ") {
                  agei++
                }
                while (this.extractedText[agei + 2] != " ") {
                  this.age = this.age + this.extractedText[agei + 2]
                  agei++
                }
              }


              if(this.extractedText.indexOf('University Medical Cente')>0){
                this.lab = 'UMC'

                let namei = this.extractedText.indexOf('NAME):')
                let namef = namei
                while (this.extractedText[namef] != " ") {
                  namef++
                }
                while (namef + 1<this.extractedText.indexOf('Туған')){
                  this.name = this.name + this.extractedText[namef + 1]
                  namef++
                }

                let agef = this.extractedText.indexOf('ЖСН') - 4
                while (this.extractedText[agef] != " ") {
                  agef--
                }
                while (this.extractedText[agef + 1] != " ") {
                  this.age = this.age + this.extractedText[agef + 1]
                  agef++
                }
              }

            });
          });
        }
        reader.readAsArrayBuffer(file);
      }
    }
  }
</script>