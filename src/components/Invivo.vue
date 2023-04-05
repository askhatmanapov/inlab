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
                    <td @click="editTtg" :class="bold[0]">{{ ttg }} mIU/mL</td>
                    <td>{{ ttgl }} - {{ ttgu }}</td>
                    <td :class="color[0]">{{ parseFloat(ttgl.replace(',', '.')) > parseFloat(ttg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(ttg.replace(',', '.')) > parseFloat(ttgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t3">
                    <td>Свободный FТ3</td>
                    <td :class="bold[1]">{{ t3 }} pmol/L</td>
                    <td>{{ t3l }} - {{ t3u }}</td>
                    <td :class="color[1]">{{ parseFloat(t3l.replace(',', '.')) > parseFloat(t3.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3.replace(',', '.')) > parseFloat(t3u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="t4">
                    <td>Свободный FТ4</td>
                    <td @click="editT4" :class="bold[2]">{{t4}} pmol/L</td>
                    <td>{{ t4l }} - {{ t4u }}</td>
                    <td :class="color[2]">{{ parseFloat(t4l.replace(',', '.')) > parseFloat(t4.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4.replace(',', '.')) > parseFloat(t4u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="tpo">
                    <td>Анти ТПО</td>
                    <td :class="bold[3]">{{tpo}} IU/mL</td>
                    <td>{{ tpol }} - {{ tpou }}</td>
                    <td :class="color[3]">{{ parseFloat(tpol.replace(',', '.')) > parseFloat(tpo.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tpo.replace(',', '.')) > parseFloat(tpou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr v-if="tg">
                    <td>Анти ТГ</td>
                    <td :class="bold[4]">{{tg}} IU/mL</td>
                    <td>{{ tgl }} - {{ tgu }}</td>
                    <td :class="color[4]">{{ parseFloat(tgl.replace(',', '.')) > parseFloat(tg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tg.replace(',', '.')) > parseFloat(tgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
            </tbody>
        </table>
    </div>

    <div class="report">
        <div v-if="report[0]">
            <h3>У Вас выявлены подозрения на Первичный гипотериоз</h3>
            <h4 style="color: tomato;">Проконсультируйтесь с рекомендованными специалистами предварительно сдав необходимые анализы.</h4>
            <p v-if="!show_results">Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
            <h4 v-if="show_results" style="color: lightcoral;">РЕКОМЕНДУЕМЫЕ АНАЛИЗЫ</h4>
            <p v-if="show_results" style="color: lightcoral;">УЗИ щитовидной железы,  ЭКГ, ЭхоКГ</p>
            <h4 v-if="show_results" style="color: lightcoral;">Дополнительные анализы:</h4>
            <p v-if="show_results" style="color: lightcoral;">ОАК, ЛПНП</p>
        </div>
        <div v-if="report[1]">
            <h3>У Вас выявлены подозрения на Субклинический гипотериоз</h3>
            <h4 style="color: tomato;">Проконсультируйтесь с рекомендованными специалистами предварительно сдав необходимые анализы.</h4>
            <p v-if="!show_results">Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
            <h4 v-if="show_results" style="color: lightcoral;">РЕКОМЕНДУЕМЫЕ АНАЛИЗЫ</h4>
            <p v-if="show_results" style="color: lightcoral;">УЗИ щитовидной железы</p>
            <h4 v-if="show_results" style="color: lightcoral;">Дополнительные анализы:</h4>
            <p v-if="show_results" style="color: lightcoral;">ОАК</p>
        </div>
        <div v-if="report[2]">
            <h3>У Вас выявлены подозрения на Центральный гипотериоз</h3>
            <h4 style="color: tomato;">Проконсультируйтесь с рекомендованными специалистами предварительно сдав необходимые анализы.</h4>
            <p v-if="!show_results">Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
            <h4 v-if="show_results" style="color: lightcoral;">РЕКОМЕНДУЕМЫЕ АНАЛИЗЫ</h4>
            <p v-if="show_results" style="color: lightcoral;">УЗИ щитовидной железы,  ЭКГ, ЭхоКГ, МРТ/КТ гипофеза головного мозга с контрастированием</p>
            <h4 v-if="show_results" style="color: lightcoral;">Дополнительные анализы:</h4>
            <p v-if="show_results" style="color: lightcoral;">ОАК, ЛПНП</p>
        </div>
        <div v-if="report[3]">
            <h3>Необходима консультация нейрохирурга</h3>
            <h4 style="color: tomato;">Проконсультируйтесь с рекомендованными специалистами предварительно сдав необходимые анализы.</h4>
            <p v-if="show_results">Ответьте на следующие вопросы чтобы узнать к каким специалистам обратиться! Займет не более 2-х минут.</p>
            <h4 v-if="show_results" style="color: lightcoral;">РЕКОМЕНДУЕМЫЕ АНАЛИЗЫ</h4>
            <p v-if="show_results" style="color: lightcoral;">МРТ/КТ гипофеза головного мозга с контрастированием</p>
        </div>
        <div v-if="report[4]">
            <h3>Возможна погрешность анализов</h3>
            <p>Необходимо повторно сдать анализ гормонов щитовидной железы</p>
        </div>
        <div v-if="report[5]">
            <h3>Ошибка считывания</h3>
            <p>Разбор вашего анализа недоступен в базе, просим извинения</p>
        </div>
    </div>
    <div v-if="report[7]">
            <!-- <h3>Error</h3> -->
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
    export default {
        components: {
            Test
        },
        props: ['extractedText'],
        data(){
            return {
                ttg: '', ttgl: '', ttgu: '',
                t3: '', t3l: '', t3u: '',
                t4: '', t4l: '', t4u: '',
                tpo: '', tpol: '', tpou: '',
                tg: '', tgl: '', tgu: '',
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
            if(this.extractedText.indexOf('тиреотропный гормон')>0){
                this.analyse = 'Щитовидка и гормоны'

                let ttgrange = this.extractedText.indexOf('ТТГ') - 3
                if (ttgrange>0){
                    while (this.extractedText[ttgrange] != " ") {
                        ttgrange--
                    }
                    while (this.extractedText[ttgrange + 1] != " ") {
                        this.ttg = this.ttg + this.extractedText[ttgrange + 1]
                        ttgrange++
                    }
                    let ttgi = this.extractedText.indexOf('ТТГ')
                    while (this.extractedText[ttgi+2] != "-") {
                        ttgi--
                    }
                    let ttgf = ttgi + 2
                    while (this.extractedText[ttgi] != " ") {
                        ttgi--
                    }
                    while (this.extractedText[ttgi + 1] != " ") {
                        this.ttgl = this.ttgl + this.extractedText[ttgi + 1]
                        ttgi++
                    }
                    while (this.extractedText[ttgf + 2] != " ") {
                        this.ttgu = this.ttgu + this.extractedText[ttgf + 2]
                        ttgf++
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

                let t3range = this.extractedText.indexOf('Трийодтиронин') - 3
                if (t3range>0){
                    while (this.extractedText[t3range] != " ") {
                        t3range--
                    }
                    while (this.extractedText[t3range + 1] != " ") {
                        this.t3 = this.t3 + this.extractedText[t3range + 1]
                        t3range++
                    }
                    let t3i = this.extractedText.indexOf('Трийодтиронин')
                    while (this.extractedText[t3i+2] != "-") {
                        t3i--
                    }
                    let t3f = t3i + 2
                    while (this.extractedText[t3i] != " ") {
                        t3i--
                    }
                    while (this.extractedText[t3i + 1] != " ") {
                        this.t3l = this.t3l + this.extractedText[t3i + 1]
                        t3i++
                    }
                    while (this.extractedText[t3f + 2] != " ") {
                        this.t3u = this.t3u + this.extractedText[t3f + 2]
                        t3f++
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
                    this.results[1] = 'Т3'
                }

                let t4range = this.extractedText.indexOf('Тироксин ') - 3
                if (t4range>0){
                    while (this.extractedText[t4range] != " ") {
                        t4range--
                    }
                    while (this.extractedText[t4range + 1] != " ") {
                        this.t4 = this.t4 + this.extractedText[t4range + 1]
                        t4range++
                    }
                    let t4i = this.extractedText.indexOf('Тироксин ')
                    while (this.extractedText[t4i+2] != "-") {
                        t4i--
                    }
                    let t4f = t4i + 2
                    while (this.extractedText[t4i] != " ") {
                        t4i--
                    }
                    while (this.extractedText[t4i + 1] != " ") {
                        this.t4l = this.t4l + this.extractedText[t4i + 1]
                        t4i++
                    }
                    while (this.extractedText[t4f + 2] != " ") {
                        this.t4u = this.t4u + this.extractedText[t4f + 2]
                        t4f++
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
                    this.results[2] = 'Т4'
                }
                
    
                if (!this.t4 && !this.ttg) {
                    this.report[5] = true;
                    return
                }

                let tporange = this.extractedText.indexOf('Антитела к тиреопероксидазе') - 3
                if (tporange>0){
                    while (this.extractedText[tporange] != " ") {
                        tporange--
                    }
                    while (this.extractedText[tporange + 1] != " ") {
                        this.tpo = this.tpo + this.extractedText[tporange + 1]
                        tporange++
                    }
                    let tpoi = this.extractedText.indexOf('Антитела к тиреопероксидазе')
                    while (this.extractedText[tpoi+2] != "-") {
                        tpoi--
                    }
                    let tpof = tpoi + 2
                    while (this.extractedText[tpoi] != " ") {
                        tpoi--
                    }
                    while (this.extractedText[tpoi + 1] != " ") {
                        this.tpol = this.tpol + this.extractedText[tpoi + 1]
                        tpoi++
                    }
                    while (this.extractedText[tpof + 2] != " ") {
                        this.tpou = this.tpou + this.extractedText[tpof + 2]
                        tpof++
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

                let tgrange = this.extractedText.indexOf('Антитела к тиреоглобулину') - 3
                if (tgrange>0){
                    while (this.extractedText[tgrange] != " ") {
                        tgrange--
                    }
                    while (this.extractedText[tgrange + 1] != " ") {
                        this.tg = this.tg + this.extractedText[tgrange + 1]
                        tgrange++
                    }
                    let tgi = this.extractedText.indexOf('Антитела к тиреоглобулину')
                    while (this.extractedText[tgi+2] != "-") {
                        tgi--
                    }
                    let tgf = tgi + 2
                    while (this.extractedText[tgi] != " ") {
                        tgi--
                    }
                    while (this.extractedText[tgi + 1] != " ") {
                        this.tgl = this.tgl + this.extractedText[tgi + 1]
                        tgi++
                    }
                    while (this.extractedText[tgf + 2] != " ") {
                        this.tgu = this.tgu + this.extractedText[tgf + 2]
                        tgf++
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