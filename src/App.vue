<template>
<div id="app">
    <Header :totalCount="totalCount" :corectCount="corectCount" />
    <b-container class="bv-example-row">
        <b-row>

            <b-col sm="6" offset="3">
                <contact v-if="questionList.length" :cquestion="questionList[index]" :next="next" :increment="increment" />
            </b-col>

        </b-row>
    </b-container>

</div>
</template>

<script>
import Header from '@/components/Header'
import Contact from '@/components/Contact'

export default {
    name: 'App',
    components: {
        Header,
        Contact,
    },
    data() {
        return {
            questionList: [],
            index: 0,
            corectCount: 0,
            totalCount: 0,
        }

    },
    methods: {
        next() {
            if (this.index < 9) {
                this.index++
            }
        },
        increment(is_corect) {
            if (is_corect) {
                this.corectCount = this.corectCount + 1;
            }
            this.totalCount = this.totalCount + 1;
        }

    },
    mounted: function () {
        fetch("https://opentdb.com/api.php?amount=10&category=19&type=multiple", {
            method: "get"
        }).then((response) => {
            return response.json();

        }).then((result) => {
            this.questionList = result.results;
        })

    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
