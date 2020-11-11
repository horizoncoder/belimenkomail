<template>
    <div >
        <h3>Выберите область</h3>
        <select  v-model = "area" v-on:click = "Sort()" @change = "Filter()">
            <option v-for="item in mailarea"  v-bind:key="item.Ref">{{ item.SettlementAreaDescription }}</option>
        </select>
    <div>
        <br>
        <h3>Отделения новой почты</h3>
        <select v-if = "area != false">
            <option v-for="item in MailFilter" v-bind:key="item.Ref">{{ item.Description }} -- {{ item.CityDescription }}</option>
        </select>
    </div>

    </div>

    
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
       data: function() {
           return {
                mail:[],
                mailarea:[],
                area:false,
                MailFilter:[]
           };
        },
        mounted: function(){
             axios.post("https://api.novaposhta.ua/v2.0/json/",{
                    "modelName": "AddressGeneral",
                    "calledMethod": "getWarehouses",
                    "methodProperties": {
                        "CityName": ""
                        },
                    "apiKey": "9a557481f95094531372a9d1b55222c8"
                }).then((response) =>{
                    console.log(response.data);
                    this.mail = response.data.data;
                })
        },
        methods: {
            
            Filter: function(){
                let search = this.area
                this.MailFilter = _.filter(this.mail, function(id) { 
                if(id.SettlementAreaDescription == search){
                        if(id.SettlementTypeDescription == "місто"){
                            return id
                        }
                    } 
                });
                console.log(this.MailFilter);
            },
            Sort: function(){
            var _ = require('lodash');
            this.mailarea = _.uniqBy(this.mail, 'SettlementAreaDescription'); 
            },
        }
    }
</script>

