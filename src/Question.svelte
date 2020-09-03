<script>

    let isCorrect = false;
    let isAnswered = false;

    export let question;
    export let nextQuestion;
    export let addToScore;

    let answers = question.incorrect_answers.map(answer => {
        return {
            answer,
            correct: false
        }
    });

    let allAnswers = [
        ...answers,
        {
            answer: question.correct_answer,
            correct: true 
        }
    ]

    shuffle(allAnswers)

    function shuffle(array) {
        array.sort(()=> Math.random() - 0.5)
    }

    function checkQuestion(correct){
        isCorrect = correct;
        isAnswered = true;
        if (isCorrect) {
            addToScore();
        }
    }

</script>


<h3>{@html question.question}</h3>

{#if isAnswered}
<h5>
    {#if isCorrect }
    You got it right!
    {:else}
    Wrong!
    {/if}
</h5>
{/if}
{#each allAnswers as answer}
    <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}>{@html answer.answer}</button>
{/each}
{#if isAnswered}
<div>
<button on:click={()=>nextQuestion()}>Next Question</button>
</div>
{/if}
