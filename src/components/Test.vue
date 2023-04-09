<template>
    <div class="test" v-if="!show_results">
        <div class="progress-bar-full">
            <div class="progress-bar" :style="{ width: progress + '%' }"></div>
        </div>
        <h3> {{ questions[current_question].id }}. {{ questions[current_question].title }}</h3>
        <div v-for="(answer, index) in questions[current_question].answers" :key="index" class="options">
            <input :id="'answer_' + index" v-model="current_answer" :value="answer" type="radio" />
            <label :for="'answer_' + index">{{ answer.title }}</label>
        </div>
        <button v-if="current_question + 1 != questions.length" @click="handleNextQuestion">Далее</button>
        <button v-if="current_question + 1 == questions.length" @click="handleTestFinish">Завершить</button>
        <div v-if="error_message" class="error" style="color: tomato;">{{ error_message }}</div>
    </div>
    <div class="results" v-if="show_results">
        <h2>Рекомендация специалистов:</h2>
        <ul v-for="item in [...new Set(current_specialists)]">{{item}}</ul>
        <p>Полученное заключение не является официальным диагнозом, для уточнения вашего состояния обратитесь к рекомендованным выше специалистам</p>
    </div>

    <button v-if="show_results" type="button" @click="reloadPage">Пройти заново</button>
</template>
  
<script setup>
    import { ref } from 'vue'
    import { computed } from 'vue'

    const emit = defineEmits(['show_results']);

    const current_question = ref(0)
    const current_answer = ref(null)
    const current_specialists = ref([])
    const error_message = ref(null)
    const show_results = ref(false)
    const props = defineProps({
        report: Boolean,
    })

    const specialists = [
        {
            title: 'Нейрохирург'
        },
        {
            title: 'Кардиолог'
        },
        {
            title: 'Эндокринолог'
        },
        {
            title: 'Невропотолог'
        },
        {
            title: 'Гастроэнтеролог'
        },
        {
            title: 'Терапевт'
        },
        {
            title: 'Гинеколог'
        },
    ]

    const questions = [
        {
            id: 1,
            title: 'Слабость',
            answers: [
                { title: 'Усталость', specialists: [specialists[2]] },
                { title: 'Апатия', specialists: [specialists[2]] },
                { title: 'Не характерно', specialists: [] },
            ],
        },
        {
            id: 2,
            title: 'Депресия',
            answers: [
                { title: 'Да', specialists: [specialists[2]] },
                { title: 'Нет', specialists: [] },
            ],
        },
        // {
        //     id: 3,
        //     title: 'Бессоница',
        //     answers: [
        //         { title: 'Да', specialists: [specialists[3]] },
        //         { title: 'Нет', specialists: [] },
        //     ],
        // },
        // {
        //     id: 4,
        //     title: 'Зябкость',
        //     answers: [
        //         { title: 'Утром', specialists: [specialists[2]] },
        //         { title: 'Вечером', specialists: [specialists[1]] },
        //         { title: 'Постоянно', specialists: [specialists[1]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 5,
        //     title: 'Вялость',
        //     answers: [
        //         { title: 'Утром', specialists: [specialists[2]] },
        //         { title: 'Вечером', specialists: [specialists[1]] },
        //         { title: 'Постоянно', specialists: [specialists[3]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 6,
        //     title: 'Сонливость',
        //     answers: [
        //         { title: 'Утром', specialists: [specialists[2]] },
        //         { title: 'Вечером', specialists: [specialists[3]] },
        //         { title: 'Постоянно', specialists: [specialists[2]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 7,
        //     title: 'Прибавка массы тела',
        //     answers: [
        //         { title: 'Плотные отеки по всему телу утром', specialists: [specialists[1], specialists[2]] },
        //         { title: 'Плотные отеки по всему телу вечером', specialists: [specialists[1]] },
        //         { title: 'Отеков нет', specialists: [] },
        //     ],
        // },
        // {
        //     id: 8,
        //     title: 'Онимение пальцев',
        //     answers: [
        //         { title: 'Связанно со стресом', specialists: [specialists[3]] },
        //         { title: 'Независимо от стреса', specialists: [specialists[2]] },
        //         { title: 'Онимения нет', specialists: [] },
        //     ],
        // },
        // {
        //     id: 9,
        //     title: 'Запоры',
        //     answers: [
        //         { title: 'Постоянно', specialists: [specialists[2]] },
        //         { title: 'С приемом пищи', specialists: [specialists[4]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 10,
        //     title: 'Выпадение волос',
        //     answers: [
        //         { title: 'Постоянно', specialists: [specialists[2]] },
        //         { title: 'С приемом препаратов', specialists: [specialists[5]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 11,
        //     title: 'Чувство боли в сердце',
        //     answers: [
        //         { title: 'Учащение ритма', specialists: [specialists[1]] },
        //         { title: 'Замедление ритма', specialists: [specialists[2]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 12,
        //     title: 'Судороги',
        //     answers: [
        //         { title: 'Мышечные', specialists: [specialists[2]] },
        //         { title: 'Эпилептические', specialists: [specialists[3]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 13,
        //     title: 'Нарушение менструального цикла',
        //     answers: [
        //         { title: 'С приемом препаратов', specialists: [specialists[6]] },
        //         { title: 'Не связаны с возрастом', specialists: [specialists[2]] },
        //         { title: 'Переодически', specialists: [specialists[6]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
        // {
        //     id: 14,
        //     title: 'Кожа и кожанные покровы',
        //     answers: [
        //         { title: 'Сухая', specialists: [specialists[2]] },
        //         { title: 'Влажная', specialists: [specialists[3]] },
        //         { title: 'Не характерно', specialists: [] },
        //     ],
        // },
    ]

    const progress = computed(() => {
        const percent = Math.round((current_question.value * 100) / questions.length)
        if (percent === 0) return 1
        return percent
    })
    
    const handleTestFinish = () => {
        if (current_answer.value == null) {
            error_message.value = 'Выбирете один из вариантов ответов!'
            return false
        }
        const selectedAnswer = current_answer.value
        const specialistsForSelectedAnswer = selectedAnswer.specialists.map(s => s.title)
        current_specialists.value.push(...specialistsForSelectedAnswer)
        if (props.report == true) {
            current_specialists.value.push(specialists[0].title);
        }
        error_message.value = null
        show_results.value = true
        emit('show_results', 1)
    }

    const handleNextQuestion = () => {
        if (current_answer.value == null) {
            error_message.value = 'Выбирете один из вариантов ответов!'
            return false
        }
        const selectedAnswer = current_answer.value
        const specialistsForSelectedAnswer = selectedAnswer.specialists.map(s => s.title)
        current_specialists.value.push(...specialistsForSelectedAnswer)
        current_answer.value = null
        error_message.value = null
    
        if (current_question.value + 1 === questions.length) {
            return handleTestFinish()
        }
        current_question.value++
    }

    const reloadPage = () => {
        window.location.reload();
    }
</script> 