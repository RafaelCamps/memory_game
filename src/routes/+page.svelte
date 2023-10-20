<script lang="ts">
    import { IconBrain, IconHeart } from '@tabler/icons-svelte';
    import { emojiFood } from '$lib/emoji-food.ts'

    type State = 'start' | 'playing' | 'paused' | 'won' | 'lost'

    let state:State = 'playing'
    let size = 24
    $: maxMatches = grid.length / 2 //esto se podría usar dentro de create grid como maxSize
    let selected:number[] = []
    let matches:string[] = []

    const createGrid = () => {
        let cards = new Set<string>()
        let maxSize = size / 2

        while (cards.size < maxSize) {
            const randomIndex = Math.floor(Math.random() * emojiFood.length)
            cards.add(emojiFood[randomIndex])
        }

        return shuffle([...cards, ...cards])
    }
    
    const shuffle = (array) => {
        return array.sort(() => Math.random() - 0.5)
    }

    let grid = createGrid()

    const selectCard = (cardIndex: number) => {
        selected = [...selected, cardIndex]
    }

    const matchCards = () => {
        const [firstCard, secondCard] = selected
        if(grid[firstCard] === grid[secondCard]){
            matches = [...matches, grid[firstCard]]
        }
        setTimeout(() => {
            selected = []
        }, 500);
        
    }

    const gameWon = () => {
        state = 'won'
        matches = []
    }


    $: selected.length === 2 && matchCards()
    $: maxMatches === matches.length && gameWon()




    $: console.log(state, selected, matches)

</script>


<svelte:head>
    <title>Memory game</title>
</svelte:head>


<main class=" h-screen bg-slate-200">

    {#if state === 'start'}
        <div class="flex flex-col justify-center items-center h-[95vh]">
            <div class="flex gap-x-3">
                <IconBrain size={54} class={'text-pink-300'} />
                <h1 class="text-5xl text-green-500">Memory Game</h1>
            </div>
            <button class="mt-5 border border-gray-600 rounded-xl py-3 px-10 bg-green-400 hover:bg-gray-100 hover:text-green-400 hover:border-green-700
                uppercase font-bold text-gray-100"
                on:click={()=> state = 'playing'}>
                Start
            </button>           
        </div>

    {:else if state === 'playing'}
        <div class="flex gap-x-3 justify-center py-2">
            <IconBrain size={32} class={'text-pink-300'} />
            <h1 class="text-3xl text-green-500">Memory Game</h1>
        </div>
        <div class="h-[85vh] bg-green-600 rounded-xl mx-24 grid grid-cols-6 gap-2 py-2 px-12 place-content-center">
            {#each grid as card, cardIndex}
                {@const isSelected = selected.includes(cardIndex)}
                {@const isSelectedOrMatched = selected.includes(cardIndex) || matches.includes(card)}
                {@const isMatched = matches.includes(card)}
                <button class="bg-slate-100 text-3xl lg:text-5xl w-32 h-32 rounded-xl"
                    class:selected={isSelected} disabled={isSelectedOrMatched}
                    on:click={() => selectCard(cardIndex)}>
                    {#if isSelected || isMatched}
                    <span class:match={isMatched}>
                        {card}
                    </span>
                    {/if}
                </button>
            {/each}
        </div>
        
    {:else if state === 'lost'}
        <div class="h-[95vh] flex flex-col justify-center items-center">
            <h2 class="text-5xl text-center mb-5">You lose! 💩</h2>
            <button class="mt-5 border border-gray-600 rounded-xl py-3 px-10 bg-green-400 hover:bg-gray-100 hover:text-green-400 hover:border-green-700
                uppercase font-bold text-gray-100"
                on:click={()=> state = 'playing'}>
                Play again
            </button>      
        </div>
    {:else if state === 'won'}
        <div class="h-[95vh] flex flex-col justify-center items-center">
            <h2 class="text-5xl text-center mb-5">You win! 🎉</h2>
            <button class="mt-5 border border-gray-600 rounded-xl py-3 px-10 bg-green-400 hover:bg-gray-100 hover:text-green-400 hover:border-green-700
                uppercase font-bold text-gray-100"
                on:click={()=> state = 'playing'}>
                Play again
            </button>        
        </div>
    {/if}
    <footer class="text-center mt-2">
        by @Rafactorizando
    </footer>
</main>

<style>
    .selected{
        border: 2px solid red;
        /* @apply('border-green-400'); */
    }
    .match{
        transition: opacity 0.3s ease-out;
        opacity: 0.4;
    }
</style>
