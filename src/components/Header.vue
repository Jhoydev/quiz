<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled>
                <div>Counter: {{ countQuestion }}/{{ totalQuestions }}</div>
            </b-nav-item>
        </b-nav>
    </div>
</template>
<script>
export default {
    props: [
        'correctQuestions',
        'totalQuestions',
        'countQuestion',
        'statusQuiz'
    ]
}
</script>