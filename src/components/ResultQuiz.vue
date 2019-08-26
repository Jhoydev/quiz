<template>
    <div>
        <h1>Resultados</h1>
        <p 
            v-for="(li,i) in listResult"
            :key="i"
            
        ><span v-html="li.question"></span>
            <br>
            <span class="badge badge-success" v-html="li.correct_answer"></span>
        </p>
    </div>
</template>
<script>
export default {
    props: {
        listResult: Array
    }
}
</script>