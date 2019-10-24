<template>
    <div class="h-75-screen w-full tablet:w-3/5 desktop:w-1/4 mx-4 desktop:mx-0 bg-gray-300 rounded-lg shadow-md flex flex-col items-center">
        <div class="w-4/5 h-48 tablet:h-56 rounded-lg shadow-xl mt-10 font-mono flex flex-col px-6 py-4"
             style="background: linear-gradient(45deg, #421579, #1cb5e0)">
            <div class="flex w-full items-center justify-between">
                <span class="font-semibold text-gray-100 text-lg">{{cardIssuer}}</span>
                <span class="font-sans text-gray-100 text-sm">payWave</span>
            </div>
            <div style="height: 20%"></div>
            <div class=" w-10 h-8 bg-orange-100 rounded"></div>
            <div class="flex w-full justify-around mt-4">
                <template v-for="(chunk, index) in cardChars">
                    <span :key="index" class="text-lg text-gray-100" v-html="chunk"></span>
                </template>
            </div>
            <div class="flex w-full items-start justify-between mt-2">
                <div class="flex flex-col text-left ">
                    <span class="text-gray-400 text-sm">CARD HOLDER</span>
                    <span class="text-gray-200 ">{{cardHolderName}}</span>
                </div>
                <div class="flex flex-col text-right">
                    <span class="text-gray-400 text-sm">VALID THRU</span>
                    <span class="text-gray-200 ">{{month}}/{{year}}</span>
                </div>
            </div>
        </div>
        <div class=" mt-10 w-4/5">
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="card-number">Card Number</label>
                <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        id="card-number"
                        type="number"
                        :value="cardInput"
                        @input="onNumberInput"
                >
            </div>
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="card-holder">Card Holder</label>
                <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        id="card-holder"
                        type="text"
                        v-model="cardHolder"
                >
            </div>
            <div class="flex items-center justify-between w-full">
                <div>
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="card-expiration-date">
                        Expiration Date
                    </label>
                    <div class="flex items-center" id="card-expiration-date">
                        <select v-model="month"
                                class="shadow  border rounded  py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline mr-3"
                        >
                            <option disabled value="MM">MM</option>
                            <option v-for="option in months" :value="option" :key="option">{{option}}</option>
                        </select>
                        <select v-model="year"
                                class="shadow  border rounded  py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        >
                            <option disabled value="YY">YY</option>
                            <option v-for="option in years" :value="option" :key="option">{{option + 2000}}</option>
                        </select>
                    </div>
                </div>
                <div class="w-1/4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="card-cvv">
                        CVV
                    </label>
                    <input
                            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                            id="card-cvv"
                            type="text"
                            v-model="cardCvv"
                    >
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Card",
        data() {
            return {
                cardIssuer: 'VISA',
                cardNumber: '################',
                cardInput: '',
                cardHolder: '',
                cardCvv: '',
                month: "MM",
                year: "YY",
                months: ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12'],
                years: Array.from(Array(10), (_, i) => i + 19)
            };
        },
        computed: {
            cardChars() {
                let charChunks = [];
                for (let index of [0, 4, 8, 12]) {
                    let chars = '';
                    for (let charIndex of Array.from(Array(4), (_, i) => index + i)) {
                        chars += this.cardNumber[charIndex];
                    }
                    charChunks.push(chars);
                }

                return charChunks
            },
            cardHolderName() {
                return this.cardHolder.split(' ').map(name => name.charAt(0).toUpperCase() + name.slice(1)).join(' ');
            }
        },
        methods: {
            onNumberInput(event) {
                const value = event.target.value;
                if (value.length <= 16) {
                    this.cardInput = value;
                }
                const lastChar = this.cardInput.length > 0 ? this.cardInput.length - 1 : 0;
                this.cardNumber = this.cardInput;
                for (let i = lastChar; i < 16; i++) {
                    this.cardNumber += '#';
                }

                this.$forceUpdate();

            },
        }

    }
</script>

<style scoped>

</style>