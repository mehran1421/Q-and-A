<template>
<div>
    <b-jumbotron>

        <template>
            {{cquestion.question}}
        </template>

        <hr class="my-4">
        <b-list-group>
            <b-list-group-item v-for="(ans,i) in shfelAnswers" :key="i" @click="ChangeIndex(i)" 
                :class="[!answeredUser && i===cindex ? 'selected' 
                    :answeredUser && i==corectAns ? 'corect'
                        :answeredUser && i !== corectAns ? 'incorect':'']">
                    {{ans}}
            </b-list-group-item>
        </b-list-group>

        <b-button variant="primary" :disabled="cindex === null || answeredUser" href="#" @click="submitAnswer">Submit</b-button>
        <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash';
export default {
    props: {
        cquestion: Object,
        next: Function,
        increment: Function,
    },
    data() {
        return {
            cindex: null,
            shfelAnswers: [],
            corectAns: null,
            answeredUser: false,
        }

    },
    computed: {
        answers() {
            let answers = [...this.cquestion.incorrect_answers];
            answers.push(this.cquestion.correct_answer);
            return answers;
        },
    },
    watch: {
        cquestion: {
            immediate: true,
            handler() {
                this.cindex = null;
                this.answeredUser = false;
                this.randomAnswers();
            }

        },

    },
    methods: {
        ChangeIndex(i) {
            this.cindex = i;
        },
        randomAnswers() {
            let answers = [...this.cquestion.incorrect_answers, this.cquestion.correct_answer];
            this.shfelAnswers = _.shuffle(answers);
            this.corectAns = this.shfelAnswers.indexOf(this.cquestion.correct_answer);

        },
        submitAnswer() {
            let is_corect = false;
            if (this.cindex === this.corectAns) {
                is_corect = true;
            }
            this.answeredUser = true;
            this.increment(is_corect);
        },

    },
}
</script>

<style scoped>
.list-group-item:hover {
    background-color: #eee;
    cursor: pointer;
}

.btn {
    margin: 10px 2px;
}

.selected {
    background-color: blue;
    color: white;
}

.corect {
    background-color: green;
}

.incorect {
    background-color: red;
}
</style>>
