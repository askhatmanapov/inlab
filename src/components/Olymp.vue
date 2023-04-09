<template> <!--Olymp-->
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
                    <td @click="editTtg" :class="bold[0]">{{ ttg }} мкМЕ/мл</td>
                    <td>{{ ttgl }} - {{ ttgu }}</td>
                    <td :class="color[0]">{{ parseFloat(ttgl.replace(',', '.')) > parseFloat(ttg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(ttg.replace(',', '.')) > parseFloat(ttgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t3">
                    <td>Свободный Т3</td>
                    <td  :class="bold[1]">{{ t3 }} пг/мл</td>
                    <td>{{ t3l }} - {{ t3u }}</td>
                    <td :class="color[1]">{{ parseFloat(t3l.replace(',', '.')) > parseFloat(t3.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3.replace(',', '.')) > parseFloat(t3u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t4">
                    <td>Свободный Т4</td>
                    <td @click="editT4" :class="bold[2]">{{t4}} нг/дл</td>
                    <td>{{ t4l }} - {{ t4u }}</td>
                    <td :class="color[2]">{{ parseFloat(t4l.replace(',', '.')) > parseFloat(t4.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4.replace(',', '.')) > parseFloat(t4u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t3o">
                    <td>Общий Т3</td>
                    <td  :class="bold[9]">{{ t3o }} нг/мл</td>
                    <td>{{ t3ol }} - {{ t3ou }}</td>
                    <td :class="color[9]">{{ parseFloat(t3ol.replace(',', '.')) > parseFloat(t3o.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3o.replace(',', '.')) > parseFloat(t3ou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t4o">
                    <td>Общий Т4</td>
                    <td  :class="bold[10]">{{ t4o }} мкг/дл</td>
                    <td>{{ t4ol }} - {{ t4ou }}</td>
                    <td :class="color[10]">{{ parseFloat(t4ol.replace(',', '.')) > parseFloat(t4o.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4o.replace(',', '.')) > parseFloat(t4ou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="tpo">
                    <td>Анти ТПО</td>
                    <td :class="bold[3]">{{tpo}} МЕ/мл</td>
                    <td>{{ tpol }} - {{ tpou }}</td>
                    <td :class="color[3]">{{ parseFloat(tpol.replace(',', '.')) > parseFloat(tpo.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tpo.replace(',', '.')) > parseFloat(tpou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="tg">
                    <td>Анти ТГ</td>
                    <td :class="bold[4]">{{tg}} МЕ/мл</td>
                    <td>{{ tgl }} - {{ tgu }}</td>
                    <td :class="color[4]">{{ parseFloat(tgl.replace(',', '.')) > parseFloat(tg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tg.replace(',', '.')) > parseFloat(tgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="psao">
                    <td>ПСА общий</td>
                    <td :class="bold[5]">{{psao}} нг/мл</td>
                    <td>{{ psaol }} - {{ psaou }}</td>
                    <td :class="color[5]">{{ parseFloat(psaol.replace(',', '.')) > parseFloat(psao.replace(',', '.')) ? 'Ниже нормы' : parseFloat(psao.replace(',', '.')) > parseFloat(psaou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="ispsa">
                    <td>Индекс свободного ПСА</td>
                    <td :class="bold[6]">{{ispsa}} %</td>
                    <td>Выше {{ ispsal }}</td>
                    <td :class="color[6]">{{ parseFloat(ispsal.replace(',', '.')) > parseFloat(ispsa.replace(',', '.')) ? 'Ниже нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="psas">
                    <td>ПСА свободный</td>
                    <td :class="bold[7]">{{psas}} нг/мл</td>
                    <td></td>
                    <td :class="color[7]">В норме</td>
                </tr>
                <tr v-if="tireoglubin">
                    <td>Тиреоглобулин</td>
                    <td :class="bold[8]">{{tireoglubin}} нг/мл</td>
                    <td>{{ tireoglubinl }} - {{ tireoglubinu }}</td>
                    <td :class="color[8]">{{ parseFloat(tireoglubinl.replace(',', '.')) > parseFloat(tireoglubin.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tireoglubin.replace(',', '.')) > parseFloat(tireoglubinu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="vitamin">
                    <td>25-OH витамин D</td>
                    <td :class="bold[11]">{{vitamin}} нг/мл</td>
                    <td>{{ vitaminl }} - {{ vitaminu }}</td>
                    <td :class="color[11]">{{ parseFloat(vitaminl.replace(',', '.')) > parseFloat(vitamin.replace(',', '.')) ? 'Ниже нормы' : parseFloat(vitamin.replace(',', '.')) > parseFloat(vitaminu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
            </tbody>
        </table>
    </div>
    
    <div class="report">
        <Report :report="report" :show_results="show_results"></Report>
    </div>

    <div v-if="report[7]">
        <p>(!) Следующие параметры не учитывались при интерпретации, так как не относятся к гормонам щитовидной железы:</p>
        <p v-for="item in abnormal" style="font-weight: bold;"> {{ item }}</p>
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
                t3o: '', t3ol: '', t3ou: '',
                t4: '', t4l: '', t4u: '',
                t4o: '', t4ol: '', t4ou: '',
                tpo: '', tpol: '', tpou: '',
                tg: '', tgl: '', tgu: '',
                psao: '', psaol: '', psaou: '',
                ispsa: '', ispsal: '',
                psas: '',
                tireoglubin: '', tireoglubinl: '', tireoglubinu: '',
                vitamin: '', vitaminl: '', vitaminu: '',
                report: [],
                color: ref([]),
                bold: [],
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
            if(this.extractedText.indexOf('содержание гормонов и медиаторов')>0){
                this.analyse = 'Щитовидка и гормоны'

                let ttgrange = this.extractedText.indexOf('ТТГ')
                if (ttgrange>0){
                    for(let i = ttgrange + 20; ttgrange < i; ttgrange++){
                        if(this.extractedText[ttgrange+1] == '/'){
                            let ttgi = ttgrange 
                            let ttgf = ttgrange 
                            while (this.extractedText[ttgi] != " "){
                                ttgi--
                            }
                            ttgi = ttgi - 2
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

                let t3range = this.extractedText.indexOf('Свободный Т3')
                if (t3range>0){
                    for(let i = t3range + 30; t3range < i; t3range++){
                        if(this.extractedText[t3range+1] == '/'){
                            let t3i = t3range 
                            let t3f = t3range 
                            while (this.extractedText[t3i] != " "){
                                t3i--
                            }
                            t3i = t3i - 2
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

                let t4range = this.extractedText.indexOf('Свободный Т4')
                if (t4range>0){
                    for(let i = t4range + 30; t4range < i; t4range++){
                        if(this.extractedText[t4range+1] == '/'){
                            let t4i = t4range 
                            let t4f = t4range 
                            while (this.extractedText[t4i] != " "){
                                t4i--
                            }
                            t4i = t4i - 2
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
    
                let tporange = this.extractedText.indexOf('ТПО')
                if (tporange>0){
                    for(let i = tporange + 20; tporange < i; tporange++){
                        if(this.extractedText[tporange+1] == '/'){
                            let tpoi = tporange
                            let tpof = tporange
                            let tpocheck = tporange
                            while (this.extractedText[tpoi] != " "){
                                tpoi--
                            }
                            tpoi = tpoi - 2
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
    
                let tgrange = this.extractedText.indexOf(' ТГ ')
                if (tgrange>0){
                    for(let i = tgrange + 20; tgrange < i; tgrange++){
                        if(this.extractedText[tgrange+1] == '/'){
                            let tgi = tgrange 
                            let tgf = tgrange
                            let tgcheck = tgrange
                            while (this.extractedText[tgi] != " "){
                                tgi--
                            }
                            tgi = tgi - 2
                            while (this.extractedText[tgi] != " "){
                                tgi--
                            }
                            tgi = tgi + 1
                            while (this.extractedText[tgi] != " "){
                                this.tg = this.tg + this.extractedText[tgi]
                                tgi++
                            }

                            for(let j = tgcheck + 20; tgcheck<j; tgcheck++){
                                if(this.extractedText[tgcheck] == '<'){
                                    this.tgl = '0'
                                    while (this.extractedText[tgcheck] != " "){
                                        tgcheck++
                                    }
                                    while (this.extractedText[tgcheck] == " "){
                                        tgcheck++
                                    }
                                    while (this.extractedText[tgcheck] != " ") {
                                        this.tgu = this.tgu + this.extractedText[tgcheck]
                                        tgcheck++
                                    }
                                } 
                                else if(this.extractedText[tgcheck] == '-'){
                                    while (this.extractedText[tgf] != " "){
                                        tgf++
                                    }
                                    tgf = tgf + 3
                                    while (this.extractedText[tgf] != " "){
                                        this.tgl = this.tgl + this.extractedText[tgf]
                                        tgf++
                                    }
                                    tgf = tgf + 3
                                    while (this.extractedText[tgf] != " ") {
                                        this.tgu = this.tgu + this.extractedText[tgf]
                                        tgf++
                                    }
                                }
                            }
                        }
                    }
                    if (parseFloat(this.tgl.replace(',', '.')) <= parseFloat(this.tg.replace(',', '.')) && parseFloat(this.tg.replace(',', '.')) <= parseFloat(this.tgu.replace(',', '.'))) {
                        this.color[4] = 'n'
                    }
                    else if(parseFloat(this.tg.replace(',', '.')) <= parseFloat(this.tgl.replace(',', '.'))){
                        this.color[4] = 'bn'
                        this.bold[4] = 'nenorm'
                    }
                    else if(parseFloat(this.tgu.replace(',', '.')) <= parseFloat(this.tg.replace(',', '.'))){
                        this.color[4] = 'an'
                        this.bold[4] = 'nenorm'
                    }
                    this.results[4] = 'Анти ТГ'
                }
    
                let psaorange = this.extractedText.indexOf('ПСА общий')
                if (psaorange>0){
                    for(let i = psaorange + 30; psaorange < i; psaorange++){
                        if(this.extractedText[psaorange+1] == '/'){
                            let psaoi = psaorange
                            let psaof = psaorange
                            while (this.extractedText[psaoi] != " "){
                                psaoi--
                            }
                            psaoi = psaoi - 2
                            while (this.extractedText[psaoi] != " "){
                                psaoi--
                            }
                            psaoi = psaoi + 1
                            while (this.extractedText[psaoi] != " "){
                                this.psao = this.psao + this.extractedText[psaoi]
                                psaoi++
                            }
                            while (this.extractedText[psaof] != " "){
                                psaof++
                            }
                            psaof = psaof + 3
                            while (this.extractedText[psaof] != " "){
                                this.psaol = this.psaol + this.extractedText[psaof]
                                psaof++
                            }
                            psaof = psaof + 3
                            while (this.extractedText[psaof] != " ") {
                                this.psaou = this.psaou + this.extractedText[psaof]
                                psaof++
                            }
                        }
                    }
                    if (parseFloat(this.psaol.replace(',', '.')) <= parseFloat(this.psao.replace(',', '.')) && parseFloat(this.psao.replace(',', '.')) <= parseFloat(this.psaou.replace(',', '.'))) {
                        this.color[5] = 'n'
                    }
                    else if(parseFloat(this.psao.replace(',', '.')) <= parseFloat(this.psaol.replace(',', '.'))){
                        this.color[5] = 'bn'
                        this.bold[5] = 'nenorm'
                    }
                    else if(parseFloat(this.psaou.replace(',', '.')) <= parseFloat(this.psao.replace(',', '.'))){
                        this.color[5] = 'an'
                        this.bold[5] = 'nenorm'
                    }
                    this.results[5] = 'ПСА общий'
                }
    
                let ispsarange = this.extractedText.indexOf('Индекс')
                if (ispsarange>0){
                    for(let i = ispsarange + 40; ispsarange < i; ispsarange++){
                        if(this.extractedText[ispsarange] == '%'){
                            let ispsai = ispsarange
                            let ispsaf = ispsarange + 9
                            while (this.extractedText[ispsai] != " "){
                                ispsai--
                            }
                            ispsai = ispsai - 2
                            while (this.extractedText[ispsai] != " "){
                                ispsai--
                            }
                            ispsai = ispsai + 1
                            while (this.extractedText[ispsai] != " "){
                                this.ispsa = this.ispsa + this.extractedText[ispsai]
                                ispsai++
                            }
                            while (this.extractedText[ispsaf] != " "){
                                this.ispsal = this.ispsal + this.extractedText[ispsaf]
                                ispsaf++
                            }
                        }
                    }
                    if (parseFloat(this.ispsal.replace(',', '.')) <= parseFloat(this.ispsa.replace(',', '.'))) {
                        this.color[6] = 'n'
                    }
                    else if(parseFloat(this.ispsa.replace(',', '.')) <= parseFloat(this.ispsal.replace(',', '.'))){
                        this.color[6] = 'bn'
                        this.bold[6] = 'nenorm'
                    }
                    this.results[6] = 'Индекс свободного ПСА'
                }
    
                let psasrange = this.extractedText.indexOf('ПСА свободный')
                if (psasrange>0){
                    for(let i = psasrange + 30; psasrange < i; psasrange++){
                        if(this.extractedText[psasrange+1] == '/'){
                            let psasi = psasrange
                            while (this.extractedText[psasi] != " "){
                                psasi--
                            }
                            psasi = psasi - 2
                            while (this.extractedText[psasi] != " "){
                                psasi--
                            }
                            psasi = psasi + 1
                            while (this.extractedText[psasi] != " "){
                                this.psas = this.psas + this.extractedText[psasi]
                                psasi++
                            }
                        }
                    }
                    this.color[7] = 'n'
                    this.results[7] = 'ПСА свободный'
                }
                    
                let tireoglubinrange = this.extractedText.indexOf('Тиреоглобулин')
                if (tireoglubinrange>0){
                    for(let i = tireoglubinrange + 30; tireoglubinrange < i; tireoglubinrange++){
                        if(this.extractedText[tireoglubinrange+1] == '/'){
                            let tireoglubini = tireoglubinrange
                            let tireoglubinf = tireoglubinrange
                            while (this.extractedText[tireoglubini] != " "){
                                tireoglubini--
                            }
                            tireoglubini = tireoglubini - 2
                            while (this.extractedText[tireoglubini] != " "){
                                tireoglubini--
                            }
                            tireoglubini = tireoglubini + 1
                            while (this.extractedText[tireoglubini] != " "){
                                this.tireoglubin = this.tireoglubin + this.extractedText[tireoglubini]
                                tireoglubini++
                            }
                            while (this.extractedText[tireoglubinf] != " "){
                                tireoglubinf++
                            }
                            tireoglubinf = tireoglubinf + 3
                            while (this.extractedText[tireoglubinf] != " "){
                                this.tireoglubinl = this.tireoglubinl + this.extractedText[tireoglubinf]
                                tireoglubinf++
                            }
                            tireoglubinf = tireoglubinf + 3
                            while (this.extractedText[tireoglubinf] != " ") {
                                this.tireoglubinu = this.tireoglubinu + this.extractedText[tireoglubinf]
                                tireoglubinf++
                            }
                        }
                    }
                    if (parseFloat(this.tireoglubinl.replace(',', '.')) <= parseFloat(this.tireoglubin.replace(',', '.')) && parseFloat(this.tireoglubin.replace(',', '.')) <= parseFloat(this.tireoglubinu.replace(',', '.'))) {
                        this.color[8] = 'n'
                    }
                    else if(parseFloat(this.tireoglubin.replace(',', '.')) <= parseFloat(this.tireoglubinl.replace(',', '.'))){
                        this.color[8] = 'bn'
                        this.bold[8] = 'nenorm'
                    }
                    else if(parseFloat(this.tireoglubinu.replace(',', '.')) <= parseFloat(this.tireoglubin.replace(',', '.'))){
                        this.color[8] = 'an'
                        this.bold[8] = 'nenorm'
                    }
                    this.results[8] = 'Тиреоглобулин'
                }

                let t3orange = this.extractedText.indexOf('Общий Т3')
                if (t3orange>0){
                    for(let i = t3orange + 30; t3orange < i; t3orange++){
                        if(this.extractedText[t3orange+1] == '/'){
                            let t3oi = t3orange 
                            let t3of = t3orange 
                            while (this.extractedText[t3oi] != " "){
                                t3oi--
                            }
                            t3oi = t3oi - 2
                            while (this.extractedText[t3oi] != " "){
                                t3oi--
                            }
                            t3oi = t3oi + 1
                            while (this.extractedText[t3oi] != " "){
                                this.t3o = this.t3o + this.extractedText[t3oi]
                                t3oi++
                            }
                            while (this.extractedText[t3of] != " "){
                                t3of++
                            } 
                            t3of = t3of + 3
                            while (this.extractedText[t3of] != " "){
                                this.t3ol = this.t3ol + this.extractedText[t3of]
                                t3of++
                            } 
                            t3of = t3of + 3
                            while (this.extractedText[t3of] != " ") {
                                this.t3ou = this.t3ou + this.extractedText[t3of]
                                t3of++
                            }
                        }
                    }
                    if (parseFloat(this.t3ol.replace(',', '.')) <= parseFloat(this.t3o.replace(',', '.')) && parseFloat(this.t3o.replace(',', '.')) <= parseFloat(this.t3ou.replace(',', '.'))) {
                        this.color[9] = 'n'
                    }
                    else if(parseFloat(this.t3o.replace(',', '.')) <= parseFloat(this.t3ol.replace(',', '.'))){
                        this.color[9] = 'bn'
                        this.bold[9] = 'nenorm'
                    }
                    else if(parseFloat(this.t3ou.replace(',', '.')) <= parseFloat(this.t3o.replace(',', '.'))){
                        this.color[9] = 'an'
                        this.bold[9] = 'nenorm'
                    }
                    this.results[9] = 'Общий Т3'
                }

                let t4orange = this.extractedText.indexOf('Общий Т4')
                if (t4orange>0){
                    for(let i = t4orange + 30; t4orange < i; t4orange++){
                        if(this.extractedText[t4orange+1] == '/'){
                            let t4oi = t4orange 
                            let t4of = t4orange 
                            while (this.extractedText[t4oi] != " "){
                                t4oi--
                            }
                            t4oi = t4oi - 2
                            while (this.extractedText[t4oi] != " "){
                                t4oi--
                            }
                            t4oi = t4oi + 1
                            while (this.extractedText[t4oi] != " "){
                                this.t4o = this.t4o + this.extractedText[t4oi]
                                t4oi++
                            }
                            while (this.extractedText[t4of] != " "){
                                t4of++
                            } 
                            t4of = t4of + 3
                            while (this.extractedText[t4of] != " "){
                                this.t4ol = this.t4ol + this.extractedText[t4of]
                                t4of++
                            } 
                            t4of = t4of + 3
                            while (this.extractedText[t4of] != " ") {
                                this.t4ou = this.t4ou + this.extractedText[t4of]
                                t4of++
                            }
                        }
                    }
                    if (parseFloat(this.t4ol.replace(',', '.')) <= parseFloat(this.t4o.replace(',', '.')) && parseFloat(this.t4o.replace(',', '.')) <= parseFloat(this.t4ou.replace(',', '.'))) {
                        this.color[10] = 'n'
                    }
                    else if(parseFloat(this.t4o.replace(',', '.')) <= parseFloat(this.t4ol.replace(',', '.'))){
                        this.color[10] = 'bn'
                        this.bold[10] = 'nenorm'
                    }
                    else if(parseFloat(this.t4ou.replace(',', '.')) <= parseFloat(this.t4o.replace(',', '.'))){
                        this.color[10] = 'an'
                        this.bold[10] = 'nenorm'
                    }
                    this.results[10] = 'Общий Т4'
                }

                let vitaminrange = this.extractedText.indexOf('25-OH витамин D')
                if (vitaminrange>0){
                    for(let i = vitaminrange + 30; vitaminrange < i; vitaminrange++){
                        if(this.extractedText[vitaminrange+1] == '/'){
                            let vitamini = vitaminrange
                            let vitaminf = vitaminrange
                            while (this.extractedText[vitamini] != " "){
                                vitamini--
                            }
                            vitamini = vitamini - 2
                            while (this.extractedText[vitamini] != " "){
                                vitamini--
                            }
                            vitamini = vitamini + 1
                            while (this.extractedText[vitamini] != " "){
                                this.vitamin = this.vitamin + this.extractedText[vitamini]
                                vitamini++
                            }
                            while (this.extractedText[vitaminf] != " "){
                                vitaminf++
                            }
                            vitaminf = vitaminf + 3
                            while (this.extractedText[vitaminf] != " "){
                                this.vitaminl = this.vitaminl + this.extractedText[vitaminf]
                                vitaminf++
                            }
                            vitaminf = vitaminf + 3
                            while (this.extractedText[vitaminf] != " ") {
                                this.vitaminu = this.vitaminu + this.extractedText[vitaminf]
                                vitaminf++
                            }
                        }
                    }
                    if (parseFloat(this.vitaminl.replace(',', '.')) <= parseFloat(this.vitamin.replace(',', '.')) && parseFloat(this.vitamin.replace(',', '.')) <= parseFloat(this.vitaminu.replace(',', '.'))) {
                        this.color[11] = 'n'
                    }
                    else if(parseFloat(this.vitamin.replace(',', '.')) <= parseFloat(this.vitaminl.replace(',', '.'))){
                        this.color[11] = 'bn'
                        this.bold[11] = 'nenorm'
                    }
                    else if(parseFloat(this.vitaminu.replace(',', '.')) <= parseFloat(this.vitamin.replace(',', '.'))){
                        this.color[11] = 'an'
                        this.bold[11] = 'nenorm'
                    }
                    this.results[11] = '25-OH витамин D'
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