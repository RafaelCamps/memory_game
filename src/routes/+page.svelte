<script lang="ts">
    import { IconBrain, IconHeart } from '@tabler/icons-svelte';
    import { emojiFood } from '$lib/emoji-food.ts'

    type State = 'start' | 'playing' | 'paused' | 'won' | 'lost'

    let state:State = 'start'
    let size = 20
    let grid = createGrid()

    const createGrid = () => {
        let cards = new Set<string>()
        let maxSize = size / 2
    }

</script>


<svelte:head>
    <title>Memory game</title>
</svelte:head>


<main class="flex flex-col justify-center items-center h-screen bg-slate-200">
    <div class="flex gap-x-3">
        <IconBrain size={54} class={'text-pink-300'} />
        <h1 class="text-5xl text-green-500">Memory Game</h1>
    </div>
    <h2 class="text-2xl text-gray-700">Cooming soon {emojiFood[10]}</h2>
    <footer>
        by @Rafactorizando
    </footer>
</main>


