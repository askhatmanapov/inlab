<template>
    <div class="pdf">
        <table v-if="analyse">
            <caption> Анализ крови (содержание гормонов и медиаторов)</caption>
            <tbody>
                <tr>
                    <th>Компонент</th>
                    <th>Результат</th>
                    <th>Норма</th>
                    <th>Комментарий</th>
                </tr>
                <tr v-if="ttg">
                    <td>ТТГ</td>
                    <td @click="editTtg" :class="bold[0]">{{ ttg }} мЕд/л</td>
                    <td>{{ ttgl }} - {{ ttgu }}</td>
                    <td :class="color[0]">{{ parseFloat(ttgl.replace(',', '.')) > parseFloat(ttg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(ttg.replace(',', '.')) > parseFloat(ttgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t3">
                    <td>Свободный Т3</td>
                    <td :class="bold[1]">{{ t3 }} пмоль/л</td>
                    <td>{{ t3l }} - {{ t3u }}</td>
                    <td :class="color[1]">{{ parseFloat(t3l.replace(',', '.')) > parseFloat(t3.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3.replace(',', '.')) > parseFloat(t3u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t4">
                    <td>Свободный Т4</td>
                    <td @click="editT4" :class="bold[2]">{{t4}} пмоль/л</td>
                    <td>{{ t4l }} - {{ t4u }}</td>
                    <td :class="color[2]">{{ parseFloat(t4l.replace(',', '.')) > parseFloat(t4.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4.replace(',', '.')) > parseFloat(t4u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="tpo">
                    <td>Анти ТПО</td>
                    <td :class="bold[3]">{{tpo}} МЕ/мл</td>
                    <td>{{ tpol }} - {{ tpou }}</td>
                    <td :class="color[3]">{{ parseFloat(tpol.replace(',', '.')) > parseFloat(tpo.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tpo.replace(',', '.')) > parseFloat(tpou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
            </tbody>
        </table>
    </div>

    <div class="report">
        <Report :report="report" :show_results="show_results"></Report>
        <div v-if="report[7]" class="abnormal">
            <h3>(!) Следующие параметры не учитывались при интерпретации, так как не относятся к гормонам щитовидной железы:</h3>
            <ul v-for="item in abnormal">{{ item }}</ul>
        </div>
    </div>

    <div class="congrats">
        <div v-if="report[6]">
            <h4>Поздравляем! Все анализы в норме!</h4>
        </div>
    </div>

    <div class="survey" v-if="status==1">
        <test @show_results="passdata" :report="report[3]"></test>
    </div>

    <div v-if="report[4] || report[5] || report[6] || report[7]">
        <button type="button" @click="reloadPage">Пройти заново</button>
    </div>
</template>
<script>
    import Swal from 'sweetalert2';
    import { ref } from 'vue';
    import Test from './Test.vue'
    import Report from './Report.vue'
    export default {
        components: {
            Test,
            Report
        },
        props: ['extractedText'],
        data(){
            return {
                ttg: '', ttgl: '', ttgu: '',
                t3: '', t3l: '', t3u: '',
                t4: '', t4l: '', t4u: '',
                tpo: '', tpol: '', tpou: '',
                report: [],
                color: [],
                bold: [],
                options: [],
                status: 0,
                show_results: ref(''),
                analyse: ref(''),
                abnormal: ref([]),
                results: ref([])
            }
        },
        methods: {
            passdata(finish){
                this.show_results = finish
            },
            reloadPage(){
                window.location.reload();
            },
            async editTtg() {
                this.status = 0
                const { value: input } = await Swal.fire({
                    title: 'Введите новое значение "ТТГ"',
                    input: 'text',
                    inputValue: this.ttg,
                    inputValidator: (value) => {
                        if (!value) {
                            return 'Введите новое значение "ТТГ"';
                        } else if (isNaN(value.replace(',', '.'))) {
                            return 'Значение должно быть числом';
                        }
                    },
                });

                if (input) {
                    this.ttg = input;
                    this.color[0] = '';
                    this.bold[0] = '';
                    this.report = [];
                    this.show_results = '';
                    const ttgValue = parseFloat(this.ttg.replace(',', '.'));
                    const ttglValue = parseFloat(this.ttgl.replace(',', '.'));
                    const ttguValue = parseFloat(this.ttgu.replace(',', '.'));
                    if (ttglValue <= ttgValue && ttgValue <= ttguValue) {
                        this.color[0] = 'n'
                    } else if (ttgValue <= ttglValue) {
                        this.color[0] = 'bn'
                        this.bold[0] = 'nenorm'
                    } else if (ttgValue >= ttguValue) {
                        this.color[0] = 'an'
                        this.bold[0] = 'nenorm'
                    }
                }
                await this.colorcheck();
            },

            async editT4() {
                this.status = 0
                const { value: input } = await Swal.fire({
                    title: 'Введите новое значение "Т4"',
                    input: 'text',
                    inputValue: this.t4,
                    inputValidator: (value) => {
                        if (!value) {
                            return 'Введите новое значение "Т4"';
                        } else if (isNaN(value.replace(',', '.'))) {
                            return 'Значение должно быть числом';
                        }
                    },
                });

                if (input) {
                    this.t4 = input;
                    this.color[2] = '';
                    this.bold[2] = '';
                    this.report = [];
                    this.show_results = '';
                    const t4Value = parseFloat(this.t4.replace(',', '.'));
                    const t4lValue = parseFloat(this.t4l.replace(',', '.'));
                    const t4uValue = parseFloat(this.t4u.replace(',', '.'));
                    if (t4lValue <= t4Value && t4Value <= t4uValue) {
                        this.color[2] = 'n'
                    } else if (t4Value <= t4lValue) {
                        this.color[2] = 'bn'
                        this.bold[2] = 'nenorm'
                    } else if (t4Value >= t4uValue) {
                        this.color[2] = 'an'
                        this.bold[2] = 'nenorm'
                    }
                }
                await this.colorcheck();
            },

            async colorcheck(){
                if(this.color[0] == 'bn' && this.color[2] == 'bn'){
                    this.report[2] = true
                    this.status = 1
                }
                else if(this.color[0] == 'bn' && this.color[2] == 'n'){
                    this.report[3] = true
                    this.status = 1
                }
                else if(this.color[0] == 'bn' && this.color[2] == 'an'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'n' && this.color[2] == 'bn'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'n' && this.color[2] == 'n'){
                    this.status = 0;

                    let j = 0;
                    while(j<100){
                        if(this.bold[j] == 'nenorm'){
                            this.abnormal[j] = this.results[j]
                        }
                        j++
                    }
                    if(this.abnormal.length == 0){
                        this.report[6] = true
                    }
                    else{
                        this.report[7] = true
                    }
                }
                else if(this.color[0] == 'n' && this.color[2] == 'an'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'an' && this.color[2] == 'bn'){
                    this.report[0] = true;
                    this.status = 1
                }
                else if(this.color[0] == 'an' && this.color[2] == 'n'){
                    this.report[1] = true;
                    this.status = 1
                }
                else if(this.color[0] == 'an' && this.color[2] == 'an'){
                    this.report[3] = true;
                    this.status = 1
                }
            }
        },
        created(){
            if(this.extractedText.indexOf('Т4 свободный')>0){
                this.analyse = 'Щитовидка и гормоны'

                //this.tpo = '4'

                let ttgrange = this.extractedText.indexOf('ТТГ')
                if (ttgrange>0){
                    for(let i = ttgrange + 20; ttgrange < i; ttgrange++){
                        if(this.extractedText[ttgrange+1] == '/'){
                            let ttgi = ttgrange 
                            let ttgf = ttgrange 
                            while (this.extractedText[ttgi] != " "){
                                ttgi--
                            }
                            ttgi = ttgi - 3
                            while (this.extractedText[ttgi] != " "){
                                ttgi--
                            }
                            ttgi = ttgi + 1
                            while (this.extractedText[ttgi] != " "){
                                this.ttg = this.ttg + this.extractedText[ttgi] 
                                ttgi++
                            }
                            while (this.extractedText[ttgf] != " "){
                                ttgf++
                            } 
                            ttgf = ttgf + 3
                            while (this.extractedText[ttgf] != " "){
                                this.ttgl = this.ttgl + this.extractedText[ttgf]
                                ttgf++
                            }
                            ttgf = ttgf + 3
                            while (this.extractedText[ttgf] != " ") {
                                this.ttgu = this.ttgu + this.extractedText[ttgf]
                                ttgf++
                            }
                        }
                    }
                    if (parseFloat(this.ttgl.replace(',', '.')) <= parseFloat(this.ttg.replace(',', '.')) && parseFloat(this.ttg.replace(',', '.')) <= parseFloat(this.ttgu.replace(',', '.'))) {
                        this.color[0] = 'n'
                    }
                    else if(parseFloat(this.ttg.replace(',', '.')) <= parseFloat(this.ttgl.replace(',', '.'))){
                        this.color[0] = 'bn'
                        this.bold[0] = 'nenorm'
                    }
                    else if(parseFloat(this.ttgu.replace(',', '.')) <= parseFloat(this.ttg.replace(',', '.'))){
                        this.color[0] = 'an'
                        this.bold[0] = 'nenorm'
                    }
                    this.results[0] = 'ТТГ'
                }

                let t3range = this.extractedText.indexOf('T3 свободный')
                if (t3range>0){
                    for(let i = t3range + 30; t3range < i; t3range++){
                        if(this.extractedText[t3range+1] == '/'){
                            let t3i = t3range 
                            let t3f = t3range 
                            while (this.extractedText[t3i] != " "){
                                t3i--
                            }
                            t3i = t3i - 3
                            while (this.extractedText[t3i] != " "){
                                t3i--
                            }
                            t3i = t3i + 1
                            while (this.extractedText[t3i] != " "){
                                this.t3 = this.t3 + this.extractedText[t3i]
                                t3i++
                            }
                            while (this.extractedText[t3f] != " "){
                                t3f++
                            } 
                            t3f = t3f + 3
                            while (this.extractedText[t3f] != " "){
                                this.t3l = this.t3l + this.extractedText[t3f]
                                t3f++
                            } 
                            t3f = t3f + 3
                            while (this.extractedText[t3f] != " ") {
                                this.t3u = this.t3u + this.extractedText[t3f]
                                t3f++
                            }
                        }
                    }
                    if (parseFloat(this.t3l.replace(',', '.')) <= parseFloat(this.t3.replace(',', '.')) && parseFloat(this.t3.replace(',', '.')) <= parseFloat(this.t3u.replace(',', '.'))) {
                        this.color[1] = 'n'
                    }
                    else if(parseFloat(this.t3.replace(',', '.')) <= parseFloat(this.t3l.replace(',', '.'))){
                        this.color[1] = 'bn'
                        this.bold[1] = 'nenorm'
                    }
                    else if(parseFloat(this.t3u.replace(',', '.')) <= parseFloat(this.t3.replace(',', '.'))){
                        this.color[1] = 'an'
                        this.bold[1] = 'nenorm'
                    }
                    this.results[1] = ' Свободный Т3'
                }

                let t4range = this.extractedText.indexOf('Т4 свободный')
                if (t4range>0){
                    for(let i = t4range + 30; t4range < i; t4range++){
                        if(this.extractedText[t4range+1] == '/'){
                            let t4i = t4range 
                            let t4f = t4range 
                            while (this.extractedText[t4i] != " "){
                                t4i--
                            }
                            t4i = t4i - 3
                            while (this.extractedText[t4i] != " "){
                                t4i--
                            }
                            t4i = t4i + 1
                            while (this.extractedText[t4i] != " "){
                                this.t4 = this.t4 + this.extractedText[t4i] 
                                t4i++
                            }
                            while (this.extractedText[t4f] != " "){
                                t4f++
                            } 
                            t4f = t4f + 3
                            while (this.extractedText[t4f] != " "){
                                this.t4l = this.t4l + this.extractedText[t4f]
                                t4f++
                            } 
                            t4f = t4f + 3
                            while (this.extractedText[t4f] != " ") {
                                this.t4u = this.t4u + this.extractedText[t4f]
                                t4f++
                            }
                        }
                    }
                    if (parseFloat(this.t4l.replace(',', '.')) <= parseFloat(this.t4.replace(',', '.')) && parseFloat(this.t4.replace(',', '.')) <= parseFloat(this.t4u.replace(',', '.'))) {
                        this.color[2] = 'n'
                    }
                    else if(parseFloat(this.t4.replace(',', '.')) <= parseFloat(this.t4l.replace(',', '.'))){
                        this.color[2] = 'bn'
                        this.bold[2] = 'nenorm'
                    }
                    else if(parseFloat(this.t4u.replace(',', '.')) <= parseFloat(this.t4.replace(',', '.'))){
                        this.color[2] = 'an'
                        this.bold[2] = 'nenorm'
                    }
                    this.results[2] = 'T4'
                }
    
                if (!this.t4 && !this.ttg) {
                    this.report[5] = true;
                    return
                }
    
                let tporange = this.extractedText.indexOf('АТ-ТПО')
                if (tporange>0){
                    for(let i = tporange + 20; tporange < i; tporange++){
                        if(this.extractedText[tporange+1] == '/'){
                            let tpoi = tporange
                            let tpof = tporange
                            let tpocheck = tporange
                            while (this.extractedText[tpoi] != " "){
                                tpoi--
                            }
                            tpoi = tpoi - 3
                            while (this.extractedText[tpoi] != " "){
                                tpoi--
                            }
                            tpoi = tpoi + 1
                            while (this.extractedText[tpoi] != " "){
                                this.tpo = this.tpo + this.extractedText[tpoi]
                                tpoi++
                            }

                            for(let j = tpocheck + 20; tpocheck<j; tpocheck++){
                                if(this.extractedText[tpocheck] == '<'){
                                    this.tpol = '0'
                                    while (this.extractedText[tpocheck] != " "){
                                        tpocheck++
                                    }
                                    while (this.extractedText[tpocheck] == " "){
                                        tpocheck++
                                    }
                                    while (this.extractedText[tpocheck] != " ") {
                                        this.tpou = this.tpou + this.extractedText[tpocheck]
                                        tpocheck++
                                    }
                                }
                                else if(this.extractedText[tpocheck] == '-'){
                                    while (this.extractedText[tpof] != " "){
                                        tpof++
                                    }
                                    tpof = tpof + 3
                                    while (this.extractedText[tpof] != " "){
                                        this.tpol = this.tpol + this.extractedText[tpof]
                                        tpof++
                                    }
                                    tpof = tpof + 3
                                    while (this.extractedText[tpof] != " ") {
                                        this.tpou = this.tpou + this.extractedText[tpof]
                                        tpof++
                                    }
                                }
                            }

                        }
                    }
                    if (parseFloat(this.tpol.replace(',', '.')) <= parseFloat(this.tpo.replace(',', '.')) && parseFloat(this.tpo.replace(',', '.')) <= parseFloat(this.tpou.replace(',', '.'))) {
                        this.color[3] = 'n'
                    }
                    else if(parseFloat(this.tpo.replace(',', '.')) <= parseFloat(this.tpol.replace(',', '.'))){
                        this.color[3] = 'bn'
                        this.bold[3] = 'nenorm'
                    }
                    else if(parseFloat(this.tpou.replace(',', '.')) <= parseFloat(this.tpo.replace(',', '.'))){
                        this.color[3] = 'an'
                        this.bold[3] = 'nenorm'
                    }
                    this.results[3] = 'Анти ТПО'
                }


                if(this.color[0] == 'bn' && this.color[2] == 'bn'){
                    this.report[2] = true
                    this.status = 1
                }
                else if(this.color[0] == 'bn' && this.color[2] == 'n'){
                    this.report[3] = true
                    this.status = 1
                }
                else if(this.color[0] == 'bn' && this.color[2] == 'an'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'n' && this.color[2] == 'bn'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'n' && this.color[2] == 'n'){
                    this.status = 0;

                    let j = 0;
                    while(j<100){
                        if(this.bold[j] == 'nenorm'){
                            this.abnormal[j] = this.results[j]
                        }
                        j++
                    }
                    if(this.abnormal.length == 0){
                        this.report[6] = true
                    }
                    else{
                        this.report[7] = true
                    }
                }
                else if(this.color[0] == 'n' && this.color[2] == 'an'){
                    this.report[4] = true;
                    this.status = 0
                }
                else if(this.color[0] == 'an' && this.color[2] == 'bn'){
                    this.report[0] = true;
                    this.status = 1
                }
                else if(this.color[0] == 'an' && this.color[2] == 'n'){
                    this.report[1] = true;
                    this.status = 1
                }
                else if(this.color[0] == 'an' && this.color[2] == 'an'){
                    this.report[3] = true;
                    this.status = 1
                }
            }
            else{
                this.report[5] = true
            }
        }
    }
</script>