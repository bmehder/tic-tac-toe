<script>
  import GameOver from './GameOver.svelte'
  import winningConditions from './winningConditions'
  import {
    pipe,
    map,
    some,
    includes,
    findAllIndexes,
    isEq,
    not,
    getLength,
    intersection,
  } from 'nejquery'

  const isLength3 = pipe(getLength, isEq(3))

  const isWinner = idxs => pipe(map(intersection(idxs)), some(isLength3))

  let player = 'O'
  let isGameOver = false
  let squares = Array(9)
  let winner = null

  const getWinner = idxs => conditions => ({
    X: isWinner(findAllIndexes('X')(idxs))(conditions),
    O: isWinner(findAllIndexes('O')(idxs))(conditions),
  })

  const togglePlayer = str => (player = str === 'O' ? 'X' : 'O')

  const resetGame = () => {
    player = 'O'
    isGameOver = false
    squares = Array(9)
    winner = null
  }

  const handleClick = idx => {
    if (isGameOver) return resetGame()

    togglePlayer(player)
    squares[idx] = player
    winner = getWinner(squares)(winningConditions)
  }

  $: (winner?.X || winner?.O || not(includes(undefined))(squares)) && (isGameOver = true)
</script>

<main class="container">
  <h1>Tic Tac Toe</h1>

  <div>
    {#each squares as _, idx}
      <textarea
        bind:value={squares[idx]}
        on:click={!isGameOver && (() => handleClick(idx))}
        on:keypress={!isGameOver && (() => handleClick(idx))}
        readonly
      />
    {/each}
  </div>

  <GameOver {isGameOver} {winner} on:click={handleClick} />
</main>

<style>
  main {
    max-width: 32rem;
    margin-inline: auto;
  }
  div {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 0.25rem;
  }
  textarea {
    aspect-ratio: 1;
    margin: 0;
    padding: 1rem;
    text-align: center;
    font-size: 9vw;
    resize: none;
    overflow: hidden;
  }
</style>
