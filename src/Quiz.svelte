<script>
    import {blur} from "svelte/transition";
    import Question from "./Question.svelte"
    import Modal from "./Modal.svelte"
    import { onMount} from 'svelte'

    let activeQuestion = 0;
    let score = 0;
    let isModalOpen = false;
	
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
        isModalOpen = false;
    }

    function addToScore(){
        score = score + 1;
    }

    $: actualNumber = activeQuestion +1;

    $: if (score > 1){
        isModalOpen = true;

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
{#if isModalOpen}
<Modal on:close={resetQuiz}>
    <h2>You Won!</h2>
    <p>Congratulations!</p>
    <button on:click={resetQuiz}>Start Over</button>
</Modal>
{/if}
