<template>
    <header>        
        <h1>{{ headerTitle }}</h1>
        <div class="buttonDiv">
            <button v-on:click="goToPreviousPage" :disabled="pageNumber === 1" id="previousBtn">{{ previousPage }}</button>
            <span>{{ pageNumber }}</span>
            <button v-on:click="goToNextPage">{{ nextPage }}</button>
        </div>                    
    </header>  
</template>

<script>

import axios from 'axios';
import { bus } from '../main';

export default {
    name: "headerDiv",

    data: () => {
        return {
            headerTitle: 'Images List',
            pageNumber: 1,
            nextPage: 'Next',
            previousPage: 'Previous'
        }
        
    },

    props: {
    
    },

    methods: {
        goToNextPage() {                        
            this.pageNumber++;            

            bus.$emit('pageNumberChanged', this.pageNumber);
        },

        goToPreviousPage() {
            if(this.pageNumber > 1) {
                this.pageNumber--;

                bus.$emit('pageNumberChanged', this.pageNumber);
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    header {
        width: 100%;
        height: 110vh;
        margin: 0 auto;
        position: relative;

        background: url(../assets/bg.jpg) no-repeat center center; 
        -webkit-background-size: cover;
        -moz-background-size: cover;
        -o-background-size: cover;
        background-size: cover;
    }   

    h1 {
        position: absolute;
        top: 20%;
        left: 50%;
        font-size: 4.5em;
        text-decoration: underline;
        border: 8px solid #FFBF00;        
        padding: 10px 120px 10px 20px;
    }

    .buttonDiv {
        position: absolute;
        bottom: 0;
        left: 25%;        
        background-color:rgba(255, 0, 0, 0);
        color: #2C3E50;
        width: 50%;
        padding: 10px;
    }

    span {
        width: 400px;
        margin: 0 40px;
        padding: 10px 20px;        
        font-size: 1.1em;
        color: #FFBF00;
    }

    button {
        padding: 10px 20px;
        width: 100px;
        border: none;
        border-bottom: 1.5px double #FFBF00;
        background-color: rgba(255, 0, 0, 0);      
        font-size: 1em;
        color: #FFBF00;
        cursor: pointer;
    }

    button:focus { 
        outline: none; 
    }

    button:hover {
        color: #DB7B51;
        border-bottom: 1.5px double #DB7B51;
    }

    button:disabled {
        color: rgb(70, 92, 114);
        border-bottom: 1.5px double rgb(70, 92, 114);
    }


    @media (max-height:769px) and (max-width:1024px){
        h1 {        
            font-size: 3em;            
        }
    }

    @media (max-width:768px) and (max-height:1024px){
        h1 {        
            font-size: 2.4em; 
            left: 40%;           
        }
    }

    @media (max-width:360px) and (max-height:720px){
        h1 {
            font-size: 1.5em;
            top: 20%;
            left: 30%;
            border: 4px solid #FFBF00;  
            padding: 10px 40px 10px 20px;  
        }

        .buttonDiv {
            left: 0;        
            width: 100%;
        }        

        span {
            width: 50px;
            margin: 0 20px;
            padding: 10px 20px;        
            font-size: 0.6em;
        }

        button {
            width: 80px;
            border: none;
            border-bottom: 1.5px double #FFBF00;
            background-color: rgba(255, 0, 0, 0);      
            font-size: 0.6em;
            color: #FFBF00;
            cursor: pointer;
        }
    }

</style>
