<template>
    <div class="manual">
        <table>
            <caption>Анализ крови (содержание гормонов и медиаторов)</caption>
            <tbody>
                <tr>
                    <th style="width: 5%;"></th>
                    <th style="width: 20%;">Компонент</th>
                    <th style="width: 20%;">Результат</th>
                    <th style="width: 35%;">Норма</th>
                    <th style="width: 20%;">Комментарий</th>
                </tr>
                <tr :class="checkttg">
                    <td><input type="checkbox" @click="ttgfill($event)"></td>
                    <td>ТТГ</td>
                    <td v-show="checkttg=='solid'" @click="editTtg($event)" :class="bold[0]">{{ ttg }}</td>
                    <td v-show="checkttg=='solid'">{{ ttgl }} - {{ ttgu }}</td>
                    <td v-show="checkttg=='solid'" :class="color[0]">{{ parseFloat(ttgl.replace(',', '.')) > parseFloat(ttg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(ttg.replace(',', '.')) > parseFloat(ttgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checkt3">
                    <td><input type="checkbox" @click="t3fill"></td>
                    <td>Свободный Т3</td>
                    <td v-show="checkt3=='solid'" :class="bold[1]">{{ t3 }}</td>
                    <td v-show="checkt3=='solid'">{{ t3l }} - {{ t3u }}</td>
                    <td v-show="checkt3=='solid'" :class="color[1]">{{ parseFloat(t3l.replace(',', '.')) > parseFloat(t3.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3.replace(',', '.')) > parseFloat(t3u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checkt4">
                    <td><input type="checkbox" @click="t4fill($event)"></td>
                    <td>Свободный Т4</td>
                    <td v-show="checkt4=='solid'" @click="editT4($event)" :class="bold[2]">{{ t4 }}</td>
                    <td v-show="checkt4=='solid'">{{ t4l }} - {{ t4u }}</td>
                    <td v-show="checkt4=='solid'" :class="color[2]">{{ parseFloat(t4l.replace(',', '.')) > parseFloat(t4.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4.replace(',', '.')) > parseFloat(t4u.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checktpo">
                    <td><input type="checkbox" @click="tpofill"></td>
                    <td>Анти ТПО</td>
                    <td v-show="checktpo=='solid'" :class="bold[3]">{{ tpo }}</td>
                    <td v-show="checktpo=='solid'">{{ tpol }} - {{ tpou }}</td>
                    <td v-show="checktpo=='solid'" :class="color[3]">{{ parseFloat(tpol.replace(',', '.')) > parseFloat(tpo.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tpo.replace(',', '.')) > parseFloat(tpou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checktg">
                    <td><input type="checkbox" @click="tgfill"></td>
                    <td>Анти ТГ</td>
                    <td v-show="checktg=='solid'" :class="bold[4]">{{ tg }}</td>
                    <td v-show="checktg=='solid'">{{ tgl }} - {{ tgu }}</td>
                    <td v-show="checktg=='solid'" :class="color[4]">{{ parseFloat(tgl.replace(',', '.')) > parseFloat(tg.replace(',', '.')) ? 'Ниже нормы' : parseFloat(tg.replace(',', '.')) > parseFloat(tgu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checkvitamin"> 
                    <td><input type="checkbox" @click="vitaminfill"></td>
                    <td>25-OH витамин D</td>
                    <td v-show="checkvitamin=='solid'" :class="bold[5]">{{ vitamin }}</td>
                    <td v-show="checkvitamin=='solid'">{{ vitaminl }} - {{ vitaminu }}</td>
                    <td v-show="checkvitamin=='solid'" :class="color[5]">{{ parseFloat(vitaminl.replace(',', '.')) > parseFloat(vitamin.replace(',', '.')) ? 'Ниже нормы' : parseFloat(vitamin.replace(',', '.')) > parseFloat(vitaminu.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checkt3o">
                    <td><input type="checkbox" @click="t3ofill"></td>
                    <td>Общий Т3</td>
                    <td v-show="checkt3o=='solid'" :class="bold[6]">{{ t3o }}</td>
                    <td v-show="checkt3o=='solid'">{{ t3ol }} - {{ t3ou }}</td>
                    <td v-show="checkt3o=='solid'" :class="color[6]">{{ parseFloat(t3ol.replace(',', '.')) > parseFloat(t3o.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t3o.replace(',', '.')) > parseFloat(t3ou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
                </tr>
                <tr :class="checkt4o">
                    <td><input type="checkbox" @click="t4ofill"></td>
                    <td>Общий Т4</td>
                    <td v-show="checkt4o=='solid'" :class="bold[7]">{{ t4o }}</td>
                    <td v-show="checkt4o=='solid'">{{ t4ol }} - {{ t4ou }}</td>
                    <td v-show="checkt4o=='solid'" :class="color[7]">{{ parseFloat(t4ol.replace(',', '.')) > parseFloat(t4o.replace(',', '.')) ? 'Ниже нормы' : parseFloat(t4o.replace(',', '.')) > parseFloat(t4ou.replace(',', '.')) ? 'Выше нормы' : 'В норме' }}</td>
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
        
    </div>
    <div v-if="report[6]">
            <!-- <h3>Error</h3> -->
        <p>(!) Следующие параметры не учитывались при интерпретации, так как не относятся к гормонам щитовидной железы:</p>
        <p v-for="item in abnormal" style="font-weight: bold;"> {{ item }}</p>
    </div>
    <div class="congrats">
        <div v-if="report[5]">
            <h4>Поздравляем! Все анализы в норме!</h4>
        </div>
    </div>
    <div class="survey" v-if="status==1">
        <test @show_results="passdata" :report="report[3]"></test>
    </div>
    <br>
    <div v-if="report[4] || report[5] || report == '' || report[6]">
        <button type="button" @click="reloadPage">Пройти заново</button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import Swal from 'sweetalert2';
    import Test from './Test.vue'

    const emit = defineEmits(['check']);
    emit('check', 2)
    const color = ref([])
    const bold = ref([])
    const report = ref([])
    const status = ref('0')
    const show_results = ref('')
    const results = ref([])
    const abnormal = ref([])
    const reloadPage = () => {
        window.location.reload();
    }
    const passdata = (finish) => {
        show_results.value = finish
    }
    

    const ttg = ref('');
    const ttgl = ref('');
    const ttgu = ref('');
    const checkttg = ref('transparent')
    const ttgfill = async(event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для ТТГ',
                html:
                    '<input id="ttg-input" class="swal2-input" placeholder="результат">' +
                    '<input id="ttgl-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="ttgu-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const ttgInput = document.getElementById('ttg-input').value;
                    const ttglInput = document.getElementById('ttgl-input').value;
                    const ttguInput = document.getElementById('ttgu-input').value;
        
                    if (!ttglInput || !ttguInput || !ttgInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(ttgInput.replace(',', '.')) || isNaN(ttguInput.replace(',', '.')) || isNaN(ttglInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(ttglInput) >= Number(ttguInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    ttg.value = ttgInput;
                    ttgl.value = ttglInput;
                    ttgu.value = ttguInput;
                    checkttg.value = 'solid';

                    if (parseFloat(ttgl.value.replace(',', '.')) <= parseFloat(ttg.value.replace(',', '.')) && parseFloat(ttg.value.replace(',', '.')) <= parseFloat(ttgu.value.replace(',', '.'))) {
                        color.value[0] = 'n'
                    }
                    else if (parseFloat(ttg.value.replace(',', '.')) <= parseFloat(ttgl.value.replace(',', '.'))) {
                        color.value[0] = 'bn'
                        bold.value[0] = 'nenorm'
                    }
                    else if (parseFloat(ttg.value.replace(',', '.')) >= parseFloat(ttgu.value.replace(',', '.'))) {
                        color.value[0] = 'an'
                        bold.value[0] = 'nenorm'
                    }
                    results.value[0] = 'ТТГ'
                    colorcheck();
                },
            });
        }
        else {
            ttgl.value = '';
            ttgu.value = '';
            ttg.value = '';
            checkttg.value = 'transparent';
            color.value[0] = '';
            bold.value[0] = '';
            report.value = ref([]);
            status.value = 0;
            show_results.value = '';
        }
    };
    const editTtg = () => {
        Swal.fire({
            title: 'Редактировать ТТГ',
            input: 'text',
            inputValue: ttg.value,
            showCancelButton: true,
            inputValidator: (value) => {
                if (!value) {
                    return 'Пожалуйста, введите значение';
                }

                if (isNaN(value.replace(',', '.'))) {
                    return 'Значение должно быть числом';
                }

                ttg.value = value;
                color.value[0] = ''
                bold.value[0] = ''
                report.value = ref([]);
                status.value = 0;
                show_results.value = '';
                
                if (parseFloat(ttgl.value.replace(',', '.')) <= parseFloat(ttg.value.replace(',', '.')) && parseFloat(ttg.value.replace(',', '.')) <= parseFloat(ttgu.value.replace(',', '.'))) {
                    color.value[0] = 'n'
                }
                else if (parseFloat(ttg.value.replace(',', '.')) <= parseFloat(ttgl.value.replace(',', '.'))) {
                    color.value[0] = 'bn'
                    bold.value[0] = 'nenorm'
                }
                else if (parseFloat(ttg.value.replace(',', '.')) >= parseFloat(ttgu.value.replace(',', '.'))) {
                    color.value[0] = 'an'
                    bold.value[0] = 'nenorm'
                }
                colorcheck();
            },
        });
        status.value = 0
    };

    const t3 = ref('');
    const t3l = ref(''); // t3
    const t3u = ref('');
    const checkt3 = ref('transparent')
    const t3fill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Т3',
                html:
                    '<input id="t3-input" class="swal2-input" placeholder="результат">' +
                    '<input id="t3l-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="t3u-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const t3Input = document.getElementById('t3-input').value;
                    const t3lInput = document.getElementById('t3l-input').value;
                    const t3uInput = document.getElementById('t3u-input').value;
        
                    if (!t3lInput || !t3uInput || !t3Input) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(t3Input.replace(',', '.')) || isNaN(t3uInput.replace(',', '.')) || isNaN(t3lInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(t3lInput) >= Number(t3uInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    t3.value = t3Input;
                    t3l.value = t3lInput;
                    t3u.value = t3uInput;
                    checkt3.value = 'solid';

                    if (parseFloat(t3l.value.replace(',', '.')) <= parseFloat(t3.value.replace(',', '.')) && parseFloat(t3.value.replace(',', '.')) <= parseFloat(t3u.value.replace(',', '.'))) {
                        color.value[1] = 'n'
                    }
                    else if (parseFloat(t3.value.replace(',', '.')) <= parseFloat(t3l.value.replace(',', '.'))) {
                        color.value[1] = 'bn'
                        bold.value[1] = 'nenorm'
                    }
                    else if (parseFloat(t3.value.replace(',', '.')) >= parseFloat(t3u.value.replace(',', '.'))) {
                        color.value[1] = 'an'
                        bold.value[1] = 'nenorm'
                    }
                    results.value[1] = 'Свободный Т3'
                    colorcheck();
                },
            });
        }
        else {
            t3l.value = '';
            t3u.value = '';
            t3.value = '';
            checkt3.value = 'transparent';
            color.value[1] = '';
            bold.value[1] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const t4 = ref('');
    const t4l = ref('');
    const t4u = ref('');
    const checkt4 = ref('transparent')
    const t4fill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Т4',
                html:
                    '<input id="t4-input" class="swal2-input" placeholder="результат">' +
                    '<input id="t4l-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="t4u-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const t4Input = document.getElementById('t4-input').value;
                    const t4lInput = document.getElementById('t4l-input').value;
                    const t4uInput = document.getElementById('t4u-input').value;
        
                    if (!t4lInput || !t4uInput || !t4Input) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(t4Input.replace(',', '.')) || isNaN(t4uInput.replace(',', '.')) || isNaN(t4lInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(t4lInput) >= Number(t4uInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    t4.value = t4Input;
                    t4l.value = t4lInput;
                    t4u.value = t4uInput;
                    checkt4.value = 'solid';

                    if (parseFloat(t4l.value.replace(',', '.')) <= parseFloat(t4.value.replace(',', '.')) && parseFloat(t4.value.replace(',', '.')) <= parseFloat(t4u.value.replace(',', '.'))) {
                        color.value[2] = 'n'
                    }
                    else if (parseFloat(t4.value.replace(',', '.')) <= parseFloat(t4l.value.replace(',', '.'))) {
                        color.value[2] = 'bn'
                        bold.value[2] = 'nenorm'
                    }
                    else if (parseFloat(t4.value.replace(',', '.')) >= parseFloat(t4u.value.replace(',', '.'))) {
                        color.value[2] = 'an'
                        bold.value[2] = 'nenorm'
                    }
                    results.value[2] = 'Свободный Т4'
                    colorcheck();
                },
            });
        }
        else {
            t4l.value = '';
            t4u.value = '';
            t4.value = '';
            checkt4.value = 'transparent';
            color.value[2] = '';
            bold.value[2] = '';
            report.value = ref([]);
            status.value = 0;
            show_results.value = '';
        }
    };
    const editT4 = () => {
        Swal.fire({
            title: 'Редактировать Т4',
            input: 'text',
            inputValue: t4.value,
            showCancelButton: true,
            inputValidator: (value) => {
                if (!value) {
                    return 'Пожалуйста, введите значение';
                }

                if (isNaN(value.replace(',', '.'))) {
                    return 'Значение должно быть числом';
                }

                t4.value = value;
                color.value[2] = ''
                bold.value[2] = ''
                report.value = ref([]);
                status.value = 0;
                show_results.value = '';
                
                if (parseFloat(t4l.value.replace(',', '.')) <= parseFloat(t4.value.replace(',', '.')) && parseFloat(t4.value.replace(',', '.')) <= parseFloat(t4u.value.replace(',', '.'))) {
                    color.value[2] = 'n'
                }
                else if (parseFloat(t4.value.replace(',', '.')) <= parseFloat(t4l.value.replace(',', '.'))) {
                    color.value[2] = 'bn'
                    bold.value[2] = 'nenorm'
                }
                else if (parseFloat(t4.value.replace(',', '.')) >= parseFloat(t4u.value.replace(',', '.'))) {
                    color.value[2] = 'an'
                    bold.value[2] = 'nenorm'
                }
                colorcheck();
            },
        });
        status.value = 0
    };

    const tpo = ref('');
    const tpol = ref(''); 
    const tpou = ref('');
    const checktpo = ref('transparent')
    const tpofill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Анти ТПО',
                html:
                    '<input id="tpo-input" class="swal2-input" placeholder="результат">' +
                    '<input id="tpol-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="tpou-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const tpoInput = document.getElementById('tpo-input').value;
                    const tpolInput = document.getElementById('tpol-input').value;
                    const tpouInput = document.getElementById('tpou-input').value;
        
                    if (!tpolInput || !tpouInput || !tpoInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(tpoInput.replace(',', '.')) || isNaN(tpouInput.replace(',', '.')) || isNaN(tpolInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(tpolInput) >= Number(tpouInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    tpo.value = tpoInput;
                    tpol.value = tpolInput;
                    tpou.value = tpouInput;
                    checktpo.value = 'solid';

                    if (parseFloat(tpol.value.replace(',', '.')) <= parseFloat(tpo.value.replace(',', '.')) && parseFloat(tpo.value.replace(',', '.')) <= parseFloat(tpou.value.replace(',', '.'))) {
                        color.value[3] = 'n'
                    }
                    else if (parseFloat(tpo.value.replace(',', '.')) <= parseFloat(tpol.value.replace(',', '.'))) {
                        color.value[3] = 'bn'
                        bold.value[3] = 'nenorm'
                    }
                    else if (parseFloat(tpo.value.replace(',', '.')) >= parseFloat(tpou.value.replace(',', '.'))) {
                        color.value[3] = 'an'
                        bold.value[3] = 'nenorm'
                    }
                    results.value[3] = 'Анти ТПО'
                    colorcheck();
                },
            });
        }
        else {
            tpol.value = '';
            tpou.value = '';
            tpo.value = '';
            checktpo.value = 'transparent';
            color.value[3] = '';
            bold.value[3] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const tg = ref('');
    const tgl = ref(''); 
    const tgu = ref('');
    const checktg = ref('transparent')
    const tgfill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Анти ТГ',
                html:
                    '<input id="tg-input" class="swal2-input" placeholder="результат">' +
                    '<input id="tgl-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="tgu-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const tgInput = document.getElementById('tg-input').value;
                    const tglInput = document.getElementById('tgl-input').value;
                    const tguInput = document.getElementById('tgu-input').value;
        
                    if (!tglInput || !tguInput || !tgInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(tgInput.replace(',', '.')) || isNaN(tguInput.replace(',', '.')) || isNaN(tglInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(tglInput) >= Number(tguInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    tg.value = tgInput;
                    tgl.value = tglInput;
                    tgu.value = tguInput;
                    checktg.value = 'solid';

                    if (parseFloat(tgl.value.replace(',', '.')) <= parseFloat(tg.value.replace(',', '.')) && parseFloat(tg.value.replace(',', '.')) <= parseFloat(tgu.value.replace(',', '.'))) {
                        color.value[4] = 'n'
                    }
                    else if (parseFloat(tg.value.replace(',', '.')) <= parseFloat(tgl.value.replace(',', '.'))) {
                        color.value[4] = 'bn'
                        bold.value[4] = 'nenorm'
                    }
                    else if (parseFloat(tg.value.replace(',', '.')) >= parseFloat(tgu.value.replace(',', '.'))) {
                        color.value[4] = 'an'
                        bold.value[4] = 'nenorm'
                    }
                    results.value[4] = 'Анти ТГ'
                    colorcheck();
                },
            });
        }
        else {
            tgl.value = '';
            tgu.value = '';
            tg.value = '';
            checktg.value = 'transparent';
            color.value[4] = '';
            bold.value[4] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const vitamin = ref('');
    const vitaminl = ref(''); 
    const vitaminu = ref('');
    const checkvitamin = ref('transparent')
    const vitaminfill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для 25-OH витамин D',
                html:
                    '<input id="vitamin-input" class="swal2-input" placeholder="результат">' +
                    '<input id="vitaminl-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="vitaminu-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const vitaminInput = document.getElementById('vitamin-input').value;
                    const vitaminlInput = document.getElementById('vitaminl-input').value;
                    const vitaminuInput = document.getElementById('vitaminu-input').value;
        
                    if (!vitaminlInput || !vitaminuInput || !vitaminInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(vitaminInput.replace(',', '.')) || isNaN(vitaminuInput.replace(',', '.')) || isNaN(vitaminlInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(vitaminlInput) >= Number(vitaminuInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    vitamin.value = vitaminInput;
                    vitaminl.value = vitaminlInput;
                    vitaminu.value = vitaminuInput;
                    checkvitamin.value = 'solid';

                    if (parseFloat(vitaminl.value.replace(',', '.')) <= parseFloat(vitamin.value.replace(',', '.')) && parseFloat(vitamin.value.replace(',', '.')) <= parseFloat(vitaminu.value.replace(',', '.'))) {
                        color.value[5] = 'n'
                    }
                    else if (parseFloat(vitamin.value.replace(',', '.')) <= parseFloat(vitaminl.value.replace(',', '.'))) {
                        color.value[5] = 'bn'
                        bold.value[5] = 'nenorm'
                    }
                    else if (parseFloat(vitamin.value.replace(',', '.')) >= parseFloat(vitaminu.value.replace(',', '.'))) {
                        color.value[5] = 'an'
                        bold.value[5] = 'nenorm'
                    }
                    results.value[5] = '25-OH витамин D'
                    colorcheck();
                },
            });
        }
        else {
            vitaminl.value = '';
            vitaminu.value = '';
            vitamin.value = '';
            checkvitamin.value = 'transparent';
            color.value[5] = '';
            bold.value[5] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const t3o = ref('');
    const t3ol = ref(''); // t3
    const t3ou = ref('');
    const checkt3o = ref('transparent')
    const t3ofill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Т3',
                html:
                    '<input id="t3o-input" class="swal2-input" placeholder="результат">' +
                    '<input id="t3ol-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="t3ou-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const t3oInput = document.getElementById('t3o-input').value;
                    const t3olInput = document.getElementById('t3ol-input').value;
                    const t3ouInput = document.getElementById('t3ou-input').value;
        
                    if (!t3olInput || !t3ouInput || !t3oInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(t3oInput.replace(',', '.')) || isNaN(t3ouInput.replace(',', '.')) || isNaN(t3olInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(t3olInput) >= Number(t3ouInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    t3o.value = t3oInput;
                    t3ol.value = t3olInput;
                    t3ou.value = t3ouInput;
                    checkt3o.value = 'solid';

                    if (parseFloat(t3ol.value.replace(',', '.')) <= parseFloat(t3o.value.replace(',', '.')) && parseFloat(t3o.value.replace(',', '.')) <= parseFloat(t3ou.value.replace(',', '.'))) {
                        color.value[6] = 'n'
                    }
                    else if (parseFloat(t3o.value.replace(',', '.')) <= parseFloat(t3ol.value.replace(',', '.'))) {
                        color.value[6] = 'bn'
                        bold.value[6] = 'nenorm'
                    }
                    else if (parseFloat(t3o.value.replace(',', '.')) >= parseFloat(t3ou.value.replace(',', '.'))) {
                        color.value[6] = 'an'
                        bold.value[6] = 'nenorm'
                    }
                    results.value[6] = 'Общий Т3'
                    colorcheck();
                },
            });
        }
        else {
            t3ol.value = '';
            t3ou.value = '';
            t3o.value = '';
            checkt3o.value = 'transparent';
            color.value[6] = '';
            bold.value[6] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const t4o = ref('');
    const t4ol = ref(''); // t3
    const t4ou = ref('');
    const checkt4o = ref('transparent')
    const t4ofill = (event) => {
        if (event.target.checked) {
            Swal.fire({
                title: 'Введите значения для Т4',
                html:
                    '<input id="t4o-input" class="swal2-input" placeholder="результат">' +
                    '<input id="t4ol-input" class="swal2-input" placeholder="нижняя граница">' +
                    '<input id="t4ou-input" class="swal2-input" placeholder="верхняя граница">',
                focusConfirm: false,
                preConfirm: () => {
                    const t4oInput = document.getElementById('t4o-input').value;
                    const t4olInput = document.getElementById('t4ol-input').value;
                    const t4ouInput = document.getElementById('t4ou-input').value;
        
                    if (!t4olInput || !t4ouInput || !t4oInput) {
                        Swal.showValidationMessage('Пожалуйста, введите все значения. (К примеру, если, у вас интервал "< 45", то введите 0 и 45)');
                        return;
                    }
        
                    if (isNaN(t4oInput.replace(',', '.')) || isNaN(t4ouInput.replace(',', '.')) || isNaN(t4olInput.replace(',', '.'))) {
                        Swal.showValidationMessage('Значения должны быть числами');
                        return;
                    }

                    if (Number(t4olInput) >= Number(t4ouInput)) {
                        Swal.showValidationMessage('Нижняя граница должна быть меньше верхней');
                        return;
                    }
        
                    t4o.value = t4oInput;
                    t4ol.value = t4olInput;
                    t4ou.value = t4ouInput;
                    checkt4o.value = 'solid';

                    if (parseFloat(t4ol.value.replace(',', '.')) <= parseFloat(t4o.value.replace(',', '.')) && parseFloat(t4o.value.replace(',', '.')) <= parseFloat(t4ou.value.replace(',', '.'))) {
                        color.value[7] = 'n'
                    }
                    else if (parseFloat(t4o.value.replace(',', '.')) <= parseFloat(t4ol.value.replace(',', '.'))) {
                        color.value[7] = 'bn'
                        bold.value[7] = 'nenorm'
                    }
                    else if (parseFloat(t4o.value.replace(',', '.')) >= parseFloat(t4ou.value.replace(',', '.'))) {
                        color.value[7] = 'an'
                        bold.value[7] = 'nenorm'
                    }
                    results.value[7] = 'Общий Т4'
                    colorcheck();
                },
            });
        }
        else {
            t4ol.value = '';
            t4ou.value = '';
            t4o.value = '';
            checkt4o.value = 'transparent';
            color.value[7] = '';
            bold.value[7] = '';
            report.value = [];
            abnormal.value = [];
            colorcheck();
        }
    };

    const colorcheck = async () => {
        report.value = [];
        if (color.value[0] == 'bn' && color.value[2] == 'bn') {
            report.value[2] = true;
            status.value = 1;
        } else if (color.value[0] == 'bn' && color.value[2] == 'n') {
            report.value[3] = true;
            status.value = 1;
        } else if (color.value[0] == 'bn' && color.value[2] == 'an') {
            report.value[4] = true;
            status.value = 0;
        } else if (color.value[0] == 'n' && color.value[2] == 'bn') {
            report.value[4] = true;
            status.value = 0;
        } else if (color.value[0] == 'n' && color.value[2] == 'n') {
            status.value = 0;

            let k = 0;
            while(k<100){
                if(bold.value[k] == 'nenorm'){
                    abnormal.value[k] = results.value[k];
                }
                k++
            }
            if(abnormal.value.length == 0){
                report.value[5] = true
            }
            else{
                report.value[6] = true
            }

            // report.value[5] = true;
        } else if (color.value[0] == 'n' && color.value[2] == 'an') {
            report.value[4] = true;
            status.value = 0;
        } else if (color.value[0] == 'an' && color.value[2] == 'bn') {
            report.value[0] = true;
            status.value = 1;
        } else if (color.value[0] == 'an' && color.value[2] == 'n') {
            report.value[1] = true;
            status.value = 1;
        } else if (color.value[0] == 'an' && color.value[2] == 'an') {
            report.value[3] = true;
            status.value = 1;
        }
    }
</script>