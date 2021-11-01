<script>
    export let label = "";
</script>

<style>
    .pad {
        width: 128px;
        height: 128px;
        line-height: 128px;
        cursor: pointer;
        border: 2px solid black;
        border-radius: 8px;

        display: flex;
        justify-content: center;
        align-items: center;
        background: orange;
    }

    .inner {
        width: 100%;
        height: 100%;
        background: white;

        /* y tho */
        border-radius: 6px;
    }

    .pad:hover .inner {
        background: orange;
    }

    /* need to use transform instead for performance */
    /* .pad:hover .inner {
        animation: iris 2s infinite linear alternate;
    }

    @keyframes iris {
        0% { clip-path: circle(100%); }
        100% { clip-path: circle(0%); }
    } */
</style>

<div class="pad">
    <div class="inner">
        {label}
    </div>
</div>