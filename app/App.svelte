<page class="page">
    <actionBar title="CSS Colors Game!" class="action-bar">
    </actionBar>
    <stackLayout>
        <label id="streak" text="Streak: {streak}" />
        <label id="timer" text="Timer: {time}" />
        <label id="question-circle" alignSelf="center" text="What color is {targetColor}?" />
        <gridLayout alignSelf="center" columns="*, *" rows="*, *">
            <label on:tap={() => choose(colors[0])} row="0" col="0" backgroundColor={colors[0]}/>
            <label on:tap={() => choose(colors[1])} row="0" col="1" backgroundColor={colors[1]}/> 
            <label on:tap={() => choose(colors[2])} row="1" col="0" backgroundColor={colors[2]}/> 
            <label on:tap={() => choose(colors[3])} row="1" col="1" backgroundColor={colors[3]}/>       
        </gridLayout> 
    </stackLayout>
</page>

<style>
    gridLayout {
        width: 100%;
    }
    gridLayout > label {
        margin: 10;
        font-size: 20;
        font-weight: bold;
        text-align: center;
        border-color: black;
        border-style: solid;
        border-width: 2; 
    }

    #streak, #timer {
        font-size: 20;
        text-align: center;
        background-color: yellow;
    }
    #question-circle {
        border-radius: 100%;
        height: 200;
        padding: 80 0;
        font-size: 20;
        text-align: center;

    }
</style>

<script>
    export let selectedTab
    import { showModal } from 'svelte-native';
    import LosePage from './LosePage.svelte';
    let allColors = [
        'IndianRed','LightCoral','Salmon','DarkSalmon','LightSalmon','Crimson','Red','FireBrick','DarkRed','LightGoldenrodYellow','PapayaWhip','Moccasin','PeachPuff','PaleGoldenrod','Khaki','DarkKhaki','MediumSeaGreen','SeaGreen','ForestGreen','Green','DarkGreen','YellowGreen','DliveDrab','PaleTurquoise','Aquamarine','Turquoise','MediumTurquoise','DarkTurquoise','CadetBlue','Goldenrod','DarkGoldenrod','Paru','Chocolate','SaddleBrown','Sienne','Brown','Maroon','White','Snow','Honeydew','MintCream','Azure','AliceBlue','GhostWhite'
        ]

    let streak = 0;
    let time = 10;

    function setTimerInterval() {
        return setInterval(() => {
            time--;
            if (time === 0) {
                lose();
            }
        }, 1000);
    }

    let intervalId = setTimerInterval();

    let colors = [
        allColors[randomInt(allColors.length)],
        allColors[randomInt(allColors.length)],
        allColors[randomInt(allColors.length)],
        allColors[randomInt(allColors.length)]
    ];

    let targetColor = colors[randomInt(4)];

    function randomInt(n) {
        return Math.floor(Math.random() * n)
    }

    async function lose() {
        time = 10;
        streak = 0;
        clearInterval(intervalId);
        await showModal({page: LosePage});
        randomize();
        intervalId = setTimerInterval();
    }

    function choose(color) {
        if (color === targetColor) {
            streak += 1;
            time += 1;
            randomize();
        } else {
            lose();
        }
    }

    function randomize() {
        colors = [
            allColors[randomInt(allColors.length)],
            allColors[randomInt(allColors.length)],
            allColors[randomInt(allColors.length)],
            allColors[randomInt(allColors.length)]
        ];
        
        targetColor = colors[randomInt(4)];
    }

</script>