<script>
  import KataKit from "./KataKit.svelte";

  import Tone from "tone";
  import chart from "/public/json/kissa-rain.json";

  const header = chart.header;
  const tracks = chart.tracks;

  const chartName = header.name;
  const chartPPQ = header.ppq;
  const chartTempo = header.tempos[0].bpm;
  const tickDuration = (60 * 1000) / (chartTempo * chartPPQ);
  const timeSignature = header.timeSignatures[0].timeSignature;

  let trackObj = {};

  tracks.forEach(track => {
    let notes = track.notes;

    notes.forEach(note => {
      let tick = note.ticks;
      if (trackObj[tick]) {
        trackObj[tick].push([note.name, note.durationTicks]);
      } else {
        trackObj[tick] = [[note.name, note.durationTicks]];
      }
    });
  });

  // TODO: Fix getting EOT marker
  console.log(trackObj);

  //   const lastNoteTicks = parseInt(Object.keys(trackObj).slice(-1)[0]);
  const endOfTrackTicks = 7272;

  // ---------------------

  // Set Tone attributes
  const transport = Tone.Transport;
  transport.bpm.value = chartTempo;
  transport.PPQ = chartPPQ;

  let currentTime = transport.position;
  let currentTick = transport.ticks;

  //   var keys = new Tone.Players(
  //     {
  //       C: "/audio/keysounds/1-kick/01.wav",
  //       "D#": "/audio/keysounds/2-snare/01.wav",
  //       "F#": "/audio/keysounds/3-perc/01.wav",
  //       E: "/audio/keysounds/4-sample/01.wav"
  //     },
  //     {
  //       volume: -10
  //     }
  //   ).toMaster();

  var woodblock = new Tone.Sampler({
    C3: "/audio/woodblock.wav"
  }).toMaster();

  var metronome = new Tone.Loop(function(timeB) {
    woodblock.triggerAttack("C3");
  }, "4n").start(+0.05787);

  //   var song = new Tone.Player("/audio/kissa-rain.mp3")
  //     .toMaster()
  //     .sync()
  //     .start("1m");

  let active = false;
  let currentSixteenth = 0;
  let activeArray = Array.from(Array(16), () => false);

  //   var gameLoop = new Tone.Loop(function(time) {
  //     //instead of scheduling visuals inside of here
  //     //schedule a deferred callback with Tone.Draw

  //     Tone.Draw.schedule(function() {
  //       //this callback is invoked from a requestAnimationFrame
  //       //and will be invoked close to AudioContext time

  //       currentSixteenth = Number(transport.position.split(":")[2]).toFixed(0) - 1;

  //       activeArray[currentSixteenth] = true;

  //       setTimeout(() => {
  //         activeArray[currentSixteenth] = false;
  //       }, 100);
  //     }, time); //use AudioContext time of the event
  //   }, "16n").start(0);

  Tone.Transport.lookAhead = 0.5;

  //--------------------------------------------

  Object.keys(trackObj).forEach(tick => {
    trackObj[tick].forEach(noteObj => {
      Tone.Transport.schedule(function(time) {
        Tone.Draw.schedule(function() {
          //   activeArray[currentSixteenth] = true;
          switch (noteObj[0]) {
            case "C2":
              activeArray[0] = true;
              setTimeout(() => {
                activeArray[0] = false;
              }, 1111);
              break;
            case "C#2":
              activeArray[1] = true;
              setTimeout(() => {
                activeArray[1] = false;
              }, 1111);
              break;
            case "D2":
              activeArray[2] = true;
              setTimeout(() => {
                activeArray[2] = false;
              }, 1111);
              break;
            case "D#2":
              activeArray[3] = true;
              setTimeout(() => {
                activeArray[3] = false;
              }, 1111);
              break;
            // Snare
            case "E2":
              activeArray[4] = true;
              setTimeout(() => {
                activeArray[4] = false;
              }, 1111);
              break;
            case "F2":
              activeArray[5] = true;
              setTimeout(() => {
                activeArray[5] = false;
              }, 1111);
              break;
            case "F#2":
              activeArray[6] = true;
              setTimeout(() => {
                activeArray[6] = false;
              }, 1111);
              break;
            case "G2":
              activeArray[7] = true;
              setTimeout(() => {
                activeArray[7] = false;
              }, 1111);
              break;
            // Perc
            case "G#2":
              activeArray[8] = true;
              setTimeout(() => {
                activeArray[8] = false;
              }, 1111);
              break;
            case "A2":
              activeArray[9] = true;
              setTimeout(() => {
                activeArray[9] = false;
              }, 1111);
              break;
            case "A#2":
              activeArray[10] = true;
              setTimeout(() => {
                activeArray[10] = false;
              }, 1111);
              break;
            case "B2":
              activeArray[11] = true;
              setTimeout(() => {
                activeArray[11] = false;
              }, 1111);
              break;
            // Sample
            case "C3":
              activeArray[12] = true;
              setTimeout(() => {
                activeArray[12] = false;
              }, 1111);
              break;
            case "C#3":
              activeArray[13] = true;
              setTimeout(() => {
                activeArray[13] = false;
              }, 1111);
              break;
            case "D3":
              activeArray[14] = true;
              setTimeout(() => {
                activeArray[14] = false;
              }, 1111);
              break;
            case "D#3":
              activeArray[15] = true;
              setTimeout(() => {
                activeArray[15] = false;
              }, 1111);
              break;
            default:
              break;
          }
        }, time); //use AudioContext time of the event

        console.log(noteObj[0] + " played");
      }, tick - 192 + "i");
    });
  });

  function start() {
    transport.toggle();

    setInterval(function() {
      //   if (transport.state === "started") {
      //   }
      if (currentTick > endOfTrackTicks) {
        transport.stop();
      } else if (Object.keys(trackObj).includes(currentTick.toString())) {
        // console.log(currentTick);
      }
    }, tickDuration);
  }

  function updateTime() {
    // Replace this with Tone transport?
    requestAnimationFrame(updateTime);
    currentTime = transport.position;
    currentTick = transport.ticks;
  }

  updateTime();
</script>

<style>
    span {
        position: absolute;
        margin-left: 8px;
        font-variant-numeric: tabular-nums;
    }

    button {
        font-size: 72px;
        /* font-family: "Maison Neue"; */
        /* padding: 24px; */
        cursor: pointer;
        background: none;
        border: 0;
        outline: none;
    }

    button:hover {
        transform: scale(1.05);
    }

    button:active {
        transform: scale(0.95);
    }

    p {
        text-align: left;
    }

    .square-grid {
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        max-width: 500px;
    }

    .square {
        flex: 0 23%;
        height: 100px;
        margin-bottom: 2%; /* (100-32*3)/2 */

        border: 1px solid black;
        border-radius: 8px;
    }

    .square-0,
    .square-1,
    .square-2,
    .square-3  {
        background: red;
    }

    .square-4,
    .square-5,
    .square-6,
    .square-7  {
        background: blue;
    }

    .square-8,
    .square-9,
    .square-10,
    .square-11  {
        background: orange;
    }

    .square-12,
    .square-13,
    .square-14,
    .square-15  {
        background: greenyellow;
    }

    .inner {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 64px;
        font-family: 'Maison Neue ExtraThin';
        font-weight: 100;
        width: 100%;
        height: 100%;
        background: orange;

        /* y tho */
        border-radius: 8px;
    }

    .square .inner {
        background: white;
    }

        /* need to use transform instead for performance */
    .square.active .inner {
        animation: iris 1.111s linear;
        transition-delay: 0, 1.111s;
    }

    @keyframes iris {
        0% { clip-path: circle(100%); }
        100% { clip-path: circle(0%); }
    }
</style>

<p>Name: <span>{chartName}</span></p>
<p>Tempo: <span>{Math.round(transport.bpm.value)}</span></p>
<p>PPQ: <span>{transport.PPQ}</span></p>
<p>Tick Duration: <span>{tickDuration.toFixed(3)} ms</span></p>
<p>Current Time: <span>{currentTime}</span></p>
<p>Current Tick: <span>{currentTick}/{endOfTrackTicks}</span></p>

<button on:click={start}>⏯</button>

<div class="square-grid">
    {#each [...Array(16).keys()] as index}
        <div class="square square-{index}" id="{index}" class:active="{activeArray[index] === true}">
            <div class="inner">{index}</div>
        </div>
    {/each}
</div>

    <!-- <section class="kit">
        <KataKit />
    </section> -->

<!-- <Katakit /> -->



