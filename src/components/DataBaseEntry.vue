<template>
    <div class="col dataEntry p-2">
        <div class="innerContainer ">
            <div class="content">
                <div class="row align-items-center"> <!-- Zentriert die Inhalte vertikal -->
                    <div class="col-8" style="text-align: left;">
                        <div>
                            <p style="margin-bottom: 0;"><strong>{{ title }}</strong></p> <!-- Reduziert den Abstand -->
                            <p class="text-muted" style="font-size: 0.8em; margin-bottom: 0;">{{ addDate }}</p> <!-- Reduziert den Abstand -->
                            <p style="font-size: 0.8em;">Times Watched: {{ watchCounter }}</p>
                        </div>
                    </div>
                    <div class="col-4 d-flex justify-content-center">
                        <PercentageCircle :percentage="20"></PercentageCircle>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import PercentageCircle from '@/components/PercentageCircle.vue'

export default {
    props: ["title", "addDate", "watchCounter"],
    components : {
        PercentageCircle
    },
    methods: {
        increaseWatchCounter() {
        // Implementieren Sie die Logik zum Erhöhen des Watchcounters
        }
    }
}
</script>

<style>
    .dataEntry {
        background-color: rgb(22, 21, 21);
        border-radius: 10px
    }
    
    .innerContainer {
        border: 3px solid #ffffffad; 
        height: 100%; 
        border-radius: 8px;
        animation: ease-in;
        padding-left: 30px;
        padding-right: 20px;
        padding-top: 10px;
    }

    .innerContainer:hover {
        border-color: #f5c014;
    }

    .entryButton{
        background-color: transparent;
        border: 1.5px solid white;
        font-size: 10px;
        border-radius: 5px;
    }
    .entryButton:active{
        border-color: #f5c014;
        color:#f5c014;
    }
</style>