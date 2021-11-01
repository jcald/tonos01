<script>
	import Pad from "./Pad.svelte";

	let kickPlay = false;
	let snarePlay = false;
	let percPlay = false;
	let samplePlay = false;

	const KEYS = {
	  KeyF: "F",
	  KeyJ: "J",
	  KeyD: "D",
	  KeyK: "K",
	  KeyS: "S",
	  KeyL: "L",
	  KeyA: "A",
	  Semicolon: ";"
	};

	let trackKeys = [
	  [KEYS.KeyF, KEYS.KeyJ],
	  [KEYS.KeyD, KEYS.KeyK],
	  [KEYS.KeyS, KEYS.KeyL],
	  [KEYS.KeyA, KEYS.Semicolon]
	];

	window.addEventListener(
	  "keydown",
	  function(event) {
	    switch (event.code) {
	      case "KeyF":
	      case "KeyJ":
	        kickPlay = true;
	        break;
	      case "KeyD":
	      case "KeyK":
	        snarePlay = true;
	        break;
	      case "KeyS":
	      case "KeyL":
	        percPlay = true;
	        break;
	      case "KeyA":
	      case "Semicolon":
	        samplePlay = true;
	        break;
	    }

	    const audio = document.querySelector(
	      'audio[data-key="' + KEYS[event.code] + '"]'
	    );
	    const key = document.querySelector(
	      '.key[data-key="' + KEYS[event.code] + '"]'
	    );

	    console.log(audio);

	    if (!audio) return; // stop function if no audio
	    audio.currentTime = 0;
	    audio.play();
	    // key.classList.add("playing");

	    function removeTransition(event) {
	      if (event.propertyName !== "transform") return; // Skip if it's not transform
	      this.classList.remove("playing");
	    }

	    // const keys = document.querySelectorAll(".key");
	    // keys.forEach(key =>
	    //   key.addEventListener("transitionend", removeTransition)
	    // );

	    // Consume the event so it doesn't get handled twice
	    event.preventDefault();
	  },
	  true
	);
</script>

<style>
    .kata-kit {
        font-size: 64px;
        font-family: 'Maison Neue ExtraThin';

        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-column-gap: 72px;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat(4, 128px);
        grid-gap: 24px;
    }

    kbd {
        font-family: 'Maison Neue ExtraThin';
        height: 128px;
        line-height: 128px;
    }
</style>

<div class="kata-kit">
	<div class="grid">
        {#each Array(8) as pad}
            <Pad />
        {/each}

        <kbd>A</kbd>
        <kbd>S</kbd>
        <kbd>D</kbd>
        <kbd>F</kbd>
	</div>

	<div class="grid">
	    {#each Array(8) as pad}
            <Pad />
        {/each}

        <kbd>J</kbd>
        <kbd>K</kbd>
        <kbd>L</kbd>
        <kbd>;</kbd>
    </div>
</div>

<audio data-key="F" src="/audio/keysounds/1-kick/01.wav"></audio>
<audio data-key="J" src="/audio/keysounds/1-kick/01.wav"></audio>

<audio data-key="D" src="/audio/keysounds/2-snare/01.wav"></audio>
<audio data-key="K" src="/audio/keysounds/2-snare/01.wav"></audio>

<audio data-key="S" src="/audio/keysounds/3-perc/01.wav"></audio>
<audio data-key="L" src="/audio/keysounds/3-perc/01.wav"></audio>

<audio data-key="A" src="/audio/keysounds/4-sample/01.wav"></audio>
<audio data-key=";" src="/audio/keysounds/4-sample/01.wav"></audio>
