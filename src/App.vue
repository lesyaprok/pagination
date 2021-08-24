<template>
    <div class="container">
        <div class="countries">
            <div v-for="item of newList" :key="item.id" class="countries-wrapper">
                <Countries :inputData="item" @country="gotClickCountry($event)"/>
            </div>
        </div>

        <div  class="paginationButtonWrapper">
            <div class="paginationButton" @click="changePage(back)"> {{ back }} </div>
            <div v-for="i of pageList" :key="i.id">       
                <PaginationButton :value="i" @change="changePage(i)"/>
            </div>
            <div class="paginationButton" @click="changePage(forward)"> {{ forward }} </div>
        </div>

            <!-- <div class="paginationButton" @click="changePage(back)"> {{ back }} </div>
            <div class="paginationButton" @click="changePage(1)">1</div>
            <div class="paginationButton" @click="changePage(2)">2</div>
            <div class="paginationButton" @click="changePage(3)">3</div>         
            <div class="paginationButton" @click="changePage(forward)"> {{ forward }} </div> -->
                 
    </div>
    
</template>

<script>
import { countryList } from './services/countries';
import Countries from './components/Countries';
import PaginationButton from './components/PaginationButton'

export default {
    name: 'App',
    data() {
        return {
            countryList,
            back: '<',
            forward: '>',
            newList: '',
            page: 1,
            pageList: [1, 2, 3, 4, 5, 6], 
            params: {
                offset: 0,
                limit: 5,
            },
        }
    },
    components: {
        Countries, PaginationButton
    },
    mounted() {
        this.newList = this.getCountriesFromBackend(this.params);
        [...document.querySelectorAll(".paginationButton")][1].classList.add('active');
    },
    methods: {
        getCountriesFromBackend(params) {
            return this.countryList.slice(params.offset, (params.offset + params.limit));
        },
        changePage(page) {
          const button = document.querySelectorAll(".paginationButton");
    
            switch (page){
                case this.forward:
                  // console.log(this.countryList.length)
                  if (this.page !== this.countryList.length / this.params.limit) {
                    this.params.offset = this.page * this.params.limit;
                    this.page = this.page + 1;
                    this.addActive(button);               
                  }  
                    break;
                case this.back:
                  if (this.page !== 1) { 
                    this.page = this.page - 1;   
                    this.params.offset = this.page * this.params.limit - this.params.limit;  
                    this.addActive(button);
                  }
                  break;           
                default:
                  this.page = page;
                  this.params.offset = (this.page - 1) * this.params.limit;
                  this.addActive(button);                  
            }
            this.newList = this.getCountriesFromBackend(this.params);
        },
        addActive(button) {
            [...button].map(e => e.classList.remove('active'));
            button[this.page].classList.add('active');
        },
        gotClickCountry(event) {
            console.log('CLICKED!', event);
        },
    }
}

</script>

<style scoped>
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin: 0 auto;
        /* max-width: 30%; */
        height: 90vh;
        font-family: Arial, Helvetica, sans-serif;  
        color: #2c3e50;    
    }

 

    .paginationButtonWrapper {
        display: flex;
        justify-content: space-between;
        margin-top: 30px;
        user-select: none;
    }

    .paginationButton {
        border: 1px solid steelblue;
        color: steelblue;
        padding: 5px;
        width: 15px;
        height: 15px;
        margin: 0 5px;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s;
    }

    .paginationButton:hover,
    .active {
        background: steelblue;
        color: #fff;
    }

</style>
