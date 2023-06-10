<script lang="ts">

    let tipPercentage;
    let billAmount;
    let tipAmount;
    let totalPerPerson;
    let amountOfPeople;
    let validAmountOfPeople = false;
    
    $: {
        if (amountOfPeople != 0){
            validAmountOfPeople = true;
            tipAmount = ((tipPercentage / 100) * billAmount) / amountOfPeople;
            totalPerPerson = (billAmount + tipAmount) / amountOfPeople;
        }else {
            validAmountOfPeople = false;
        }
    }

    let setTipPercentage = amount => tipPercentage = amount;

    let reset = () => {
        tipPercentage = undefined;
        billAmount = undefined;
        totalPerPerson = undefined;
        amountOfPeople = undefined;
    };
</script>

<div class="header">
    <img src="logo.svg" alt="">
</div>
<div class="container">
    <div class="left">
        <div class="bill">
            <p>Bill</p>
            <input 
                type="number" 
                pattern="\d*" 
                bind:value={billAmount}
            >
        </div>
        <div class="tip">
            <p>Select tip %</p>
            <div class="tipOptions">
                {#each [5, 10, 15, 20, 50] as percentage}
                    <button 
                        on:click={() => setTipPercentage(percentage)} 
                        class="percentage 
                        {tipPercentage == percentage ? "active" : ""}"
                        >{percentage}
                    </button>
                {/each}
                <input 
                    bind:value={tipPercentage}
                    type="number" 
                    pattern="\d*"
                    class="customAmount"
                    placeholder="custom"
                >
            </div>
        </div>
        <div class="people">
            <p>Number of people</p>
            <input
                type="number" 
                pattern="\d*" 
                bind:value={amountOfPeople}
            >
        </div>
    </div>
    <div class="right">
        <div class="amount">
            <div class="text">
                <p class="title"><strong>Tip Amount</strong></p>
                <p class="small">/ person</p>
            </div>
            <div class="number">
                <h2>${tipAmount ? tipAmount.toFixed(2) : 0}</h2>
            </div>
        </div>
        <div class="total">
            <div class="text">
                <p class="title"><strong>Total</strong></p>
                <p class="small">/ person</p>
            </div>
            <div class="number">
                <h2>${totalPerPerson ? totalPerPerson.toFixed(2) : 0}</h2>
            </div>
        </div>
        <button class="reset" on:click={reset}>Reset</button>
    </div>
</div>

<style>
    div {
        margin: 1rem 0;
        touch-action: pan-y;
    }
    .header {
        display: flex;
        justify-content: center;
        margin: 20px 0;
    }
    .container {
        background-color: white;
        border-radius: 15px 15px 0 0;
        display: flex;
        flex-direction: column;
        align-content: center;
        align-items: center;
        text-align: left;
        margin: 0;
    }
    .left {
        width: 90vw;
    }
    .right {
        width: 90vw;
        background-color: hsl(183, 100%, 15%);
        border-radius: 20px;
        text-align: center;
    }
    p {
        color: hsl(184, 14%, 56%);
    }
    h2 {
        color: hsl(172, 67%, 45%);
    }
    input {
        width: 100%;
        text-align: right;
        border: none;
        border-radius: 10px;
        background-color: hsl(189, 41%, 97%);
        padding: 10px 5px;
    }
    .tipOptions {
        display: grid;
        grid-template-columns: 50% 50%;
        gap: 10px;
    }
    .customAmount {
        width: auto;
    }
    button {
        background-color: hsl(183, 100%, 15%);
        color: white;
        border: none;
        border-radius: 5px;
        padding: .8rem 0;;
    }
    button.active {
        background-color: hsl(172, 67%, 45%);
        color: hsl(183, 100%, 15%);
    }
    .amount , .total{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 0 30px;
        text-align: left;
    }
    p.small {
        color: hsl(184, 14%, 56%);
        font-size: small;
    }
    p.title {
        color: white;
        font-size: large;
    }
    button.reset{
        background-color: hsl(172, 67%, 45%);
        color: hsl(183, 100%, 15%);
        width: calc(100% - 60px);
        margin-bottom: 2.6rem;
    }
    @media (min-width: 500px) {
        .container {
            display: grid;
            justify-content: center;
            align-items: center;
            grid-auto-flow: column;
            grid-template-columns: 50% 50%;
            gap: 2rem;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            border-radius: 30px;
        }
        .left {
            width: 90%;
            justify-self: end;
        }
        .right {
            width: 90%;
            height: 90%;
            display: flex;
            flex-direction: column;
            justify-content: space-around;
            align-items: center;
        }
        .amount , .total {
            width: 80%;
            margin: 0;
        }
    }
</style>