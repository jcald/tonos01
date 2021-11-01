<script>
  import Conductor from "./Conductor.svelte";
</script>

<style>
    :global(html) {
        box-sizing: border-box;
    }

    :global(*, *:before, *:after) {
        box-sizing: inherit;
    }

    .app {
        font-family: 'Maison Neue';
        text-align: center;
    }

    .title {
        font-size: 48px;
        letter-spacing: -1px;
    }

    /* .kit {
        display: flex;
        justify-content: center;
    } */

    /* button {
        line-height: 48px;
        font-size: 48px;
        padding: 8px;
    }

    h2, p {
        font-variant-numeric: tabular-nums;
    } */
</style>

<main class="app">
    <Conductor />

    <h1 class="title">Kata-kit</h1>

    <!-- <section class="kit">
        <KataKit />
    </section> -->
</main>