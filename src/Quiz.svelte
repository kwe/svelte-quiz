<script>
    import {blur} from "svelte/transition";
    import Question from "./Question.svelte"
    import { onMount} from 'svelte'

    let activeQuestion = 0;
    let score = 0;
	
	let quiz = getQuiz();
	async function getQuiz(){
		const res = await fetch('https://opentdb.com/api.php?amount=10')
		const quiz = await res.json();
		return quiz

	}

    function nextQuestion(){
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz(){
        score = 0;
        activeQuestion = 0;
        quiz = getQuiz()
    }

    function addToScore(){
        score = score + 1;
    }

    $: actualNumber = activeQuestion +1;

    $: if (score > 7){
        alert("You won!");
        resetQuiz();
    }

    $: if (activeQuestion > 10){
        resetQuiz();
    }

</script>
<style>
    .fade-wrapper {
        position: absolute;
    }
</style>

<div>
    <button on:click={resetQuiz}>Start New Quiz</button>
    <h3>My Score: {score}</h3>
    <h4>Question #{actualNumber}</h4>

	{#await quiz}
		loading...
	{:then data}

    {#each data.results as question, index}
    {#if index == activeQuestion}
        <div transition:blur={{amount:10}} class="fade-wrapper">
            <Question {nextQuestion} {addToScore} {question}/>
        </div>
    {/if}
		
	{/each}
	{/await}

</div>
