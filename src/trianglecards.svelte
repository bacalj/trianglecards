<script>
    let ci = 2;
    let pairs = [
        [2,2],[2,3],[2,4],[2,5],[2,6],[2,7],[2,8],[2,9],
        [3,3],[3,4],[3,5],[3,6],[3,7],[3,8],[3,9],
        [4,4],[4,5],[4,6],[4,7],[4,8],[4,9],
        [5,5],[5,6],[5,7],[5,8],[5,9],
        [6,6],[6,7],[6,8],[6,9],
        [7,7],[7,8],[7,9],
        [8,8],[8,9],
        [9,9]
    ];

    let guess = '';
    let mode = 'cover-product';
    let correctStatus = "incorrect"

    $: f1 = pairs[ci][0];
    $: f2 = pairs[ci][1];
    $: product = f1 * f2;

    function advance(){
        ci++;
        ci > 35 ? ci = 0 : ci == ci;
    }

    function rewind(){
        ci--;
        ci < 0 ? ci = 35 : ci == ci;
    }

    function testGuess(e){
        if ( mode == 'cover-product'){
            if ( product == guess ){
                correctStatus = 'correct';
            } else {
                correctStatus = 'incorrect';
            }
        } else {
            if (product/guess !== f1 && product/guess !== f2){
                correctStatus = 'incorrect';
            } else {
                correctStatus = 'correct';
            }
        }
    }
</script>

<main class="main">
    <div class="{ mode ==='cover-none' ? 'the-card-wrap cover-none' : 'the-card-wrap playing'}">
        <div class="{ correctStatus === 'correct' ? 'correct card' : 'incorrect card'  }">
            {#if mode == "cover-product"}
                <div class="product">{guess}</div>
            {:else}
                <div class="product">{product}</div>
            {/if}

            {#if mode == "cover-left"}
                <div class="factor factor-one">{guess}</div>
            {:else}
                <div class="factor factor-one">{pairs[ci][0]}</div>
            {/if}

            {#if mode == "cover-right"}
                <div class="factor factor-two">{guess}</div>
            {:else}
                <div class="factor factor-two">{pairs[ci][1]}</div>
            {/if}
        </div>	
        Guess: <input type="text" bind:value={guess} on:keyup={testGuess}>
    </div>

    <div class="control-area">
        <button on:click="{rewind}">⏪</button>
        <button on:click="{advance}">⏩</button>
        

        <div class="mode-choices">
            <label>
                <input type=radio bind:group={mode} value={'cover-product'}>
                Cover product
            </label>

            <label>
                <input type=radio bind:group={mode} value={'cover-left'}>
                Cover Left
            </label>

            <label>
                <input type=radio bind:group={mode} value={'cover-right'}>
                Cover Right
            </label>

            <label>
                <input type=radio bind:group={mode} value={'cover-none'}>
                Cover None
            </label>
        </div>    
    </div>

</main>



<style>
    main {
        text-align: center;
    }

    .the-card-wrap {
        width:90%;
        margin: 0 auto;
    }

    .card {
        width: 0;
        height: 0;
        border-style: solid;
        border-width: 0 200px 200px 200px;
        margin:0 auto;
        position: relative;
    }

    .cover-none .card {
        border-color: transparent transparent lightgray transparent;
    }
    .playing .correct {
        border-color: transparent transparent lightgreen transparent;
    }

    .playing .incorrect {
        border-color: transparent transparent lightcoral transparent;
    }

    .factor {
        height:20px;
        width:20px;
        position: absolute;
        top:157px;
        font-size:32px;
    }

    .factor-one {
        right:130px;
    }

    .factor-two {
        left:130px;
    }

    .product {
        position: absolute;
        font-size:32px;
        left:-14px;
        top:10px;
    }
</style>