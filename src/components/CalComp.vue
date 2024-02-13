<script>
export default{
    data(){
        return{
            display:'0',
            currentInput: '0',
            previousInput: null,
            operator: null,
            buttons: [
                {label: '1', value: '1'},
                {label: '2', value: '2'},
                {label: '3', value: '3'},
                {label: '+', value: '+'},
                {label: '4', value: '4'},
                {label: '5', value: '5'},
                {label: '6', value: '6'},
                {label: '-', value: '-'},
                {label: '7', value: '7'},
                {label: '8', value: '8'},
                {label: '9', value: '9'},
                {label: '*', value: '*'},
                {label: '0', value: '0'},
                {label: '.', value: '.'},
                {label: '%', value: '%'},
                {label: '÷', value: '÷'},
                {label: '√', value: '√'},
                {label: '=', value: '='},
                {label: 'C', value: 'C'},
            ],
        };
    },
    methods: {
        handleButtonClick(button){
            const { value } = button;

            if(this.isNumber(value)){
                this.handleNumberInput(value);
            } else if (this.isOperator(value)){
                this.handleOperatorInput(value);
            } else if (value === '√'){
                this.handleRoot();
            } else if (value === '='){
                this.handEqual();
            } else if (value === 'C'){
                this.handClear();
            }
        },
        isNumber(input){
            return /^\d+(\.\d+)?$/.test(input);
        },
        isOperator(input){
            return['+', '-', '*', '÷', '%'].includes(input);
        },
        handleNumberInput(number){
            if(this.currentInput === '0'){
                this.currentInput = number;
            } else {
                this.currentInput += number;
            }
            this.display = this.currentInput;
            this.calculate();
        },
        handleOperatorInput(operator){
            if(this.previousInput === null){
                this.previousInput = this.currentInput;
            } else {
                this.handEqual();
            }
            this.operator = operator;
            this.currentInput = '0';
        },
        handleRoot(){
            const number = parseFloat(this.currentInput);
            if(!isNaN(number)){
                this.currentInput = Math.sqrt(number).toString();
                this.display = this.currentInput;
            }
            this.calculate();
        },
        handEqual(){
            const prevNumber = parseFloat(this.previousInput);
            const currentNumber = parseFloat(this.currentInput);

            if(!isNaN(prevNumber) && !isNaN(currentNumber) && this.operator){
                switch(this.operator){
                    case '+':
                        this.currentInput = (prevNumber + currentNumber).toString();
                        break;
                    case '-':
                        this.currentInput = (prevNumber - currentNumber).toString();
                        break;
                    case '*':
                        this.currentInput = (prevNumber * currentNumber).toString();
                        break;
                    case '÷':
                        this.currentInput = (prevNumber / currentNumber).toString();
                        break;
                    case '-':
                        this.currentInput = ((prevNumber / 100) * currentNumber).toString();
                        break;
                }
                this.display = this.currentInput;
                this.previousInput = null;
                this.operator = null;
            }
        },
        handClear(){
            this.display = '0'
            this.currentInput = '0';
            this.previousInput = null;
            this.operator = null;
        },
        handleInputChange() {
            const inputValue = this.display
            if (/^\d+(\.\d+)?$/.test(inputValue)){
                this.currentInput = inputValue;
                this.calculate();
            } else {
                this.currentInput = '0';
                this.display = '0';
            }
        },
        calculate(){
            const prevNumber = parseFloat(this.previousInput);
            const currentNumber = parseFloat(this.currentInput);

            if (!isNaN(prevNumber) && !isNaN(currentNumber) && this.operator){
                switch (this.operator) {
                    case '+':
                        this.currentInput = (prevNumber + currentNumber).toString();
                        break;
                    case '-':
                        this.currentInput = (prevNumber - currentNumber).toString();
                        break;
                    case '*':
                        this.currentInput = (prevNumber * currentNumber).toString();
                        break;
                    case '÷':
                        this.currentInput = (prevNumber / currentNumber).toString();
                        break;
                    case '-':
                        this.currentInput = ((prevNumber / 100) * currentNumber).toString();
                        break;
                }
                this.display = this.currentInput;
                this.previousInput = null;
                this.operator = null;
            }
        },
    },
};
</script>

<template>
    <div class="calculadora">
        <div class="screen">{{ display }}
            <input type="text" v-model="display" @change="handleInputChange">
        </div>
        <div>
        </div>
        <div class="buttons">
            <button v-for="button in buttons" :key="button.value" @click="handleButtonClick(button)">
                {{ button.label }}
            </button>
        </div>
    </div>
</template>

<style>
    .calculadora{
        width:  240px;
        border:  1px solid #ccc;
        border-radius:  5px;
        padding:  10px;
        box-shadow:  opx 2 px 4px rgba(0, 0, 0, 0.1);
    }

    .screen{
        width: 100%;
        height: 40px;
        background-color: #f8f8f8;
        color: black;
        text-align: right;
        padding: 5px;
        font-size: 18px;
        border-radius: 3px;
        margin-bottom: 10px;
        box-shadow: inset 0px 2px 4px rgba(0, 0, 0, 0.1);
    }

    .buttons{
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-gap: 5px;
    }

    .button{
        width: 100%;
        height: 40px;
        background-color: #f0f0f0;
        border: 1px solid #ccc;
        border-radius: 3px;
        font-size: 18px;
        cursor: pointer;
        transition: background-color 0.2 ease;
    }

    button:hover{
        background-color:#e0e0e0;
    }

    button:active{
        background-color: #d0d0d0;
    }
</style>
