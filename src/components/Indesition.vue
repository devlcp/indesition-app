<template>
    <h1 class="title">Indesition App</h1>

    <img class="animate__fadeIn" v-if="image" :src="setImage()" alt="imagen de respuesta">
    <div class="bg-dark"></div>

    <section class="indesition-container">
        <input type="text" v-model="question" placeholder="Hazle una pregunta...">
        <p>Recuerda de terminar tu pregunta con un simbolo de interrogacion (?)</p>

        <div v-if="isValidQuestion">
            <h2>{{ question }}</h2>
            <h1>{{ answer }}</h1>
        </div>
    </section>
</template>

<script>
import bgImage from '../assets/images/imagedefault.jpg'

export default {
    name: 'Indesition',
    data() {
        return {
            question: null,
            answer: null,
            image: null,
            isValidQuestion: null,
            api: "https://yesno.wtf/api"
        }
    },
    methods: {
        async getAnswer() {
            this.answer = "Analizando tu pregunta...."

            const { answer, image } = await fetch(this.api).then(r => r.json())

            this.answer = answer === 'yes' ? "Sii!": "No!"
            this.image = image
        },
        
        setImage() {
            if(!this.question) return this.image = bgImage

            return this.image
        }
    },
    watch: {
        question(value){
            this.isValidQuestion = false

            if(!value.includes('?')) return;

            this.isValidQuestion = true

            //TODO Realizar peticion https

            this.getAnswer()
        }
    },
}
</script>

<style scoped>
    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .title {
        padding: 20px 0;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indesition-container {
        text-align: center;
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 500px;
        padding: 10px 15px;
        border-radius: 5px;
        text-align: center;
        border: none;
        font-size: 24px;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 14px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }
</style>
