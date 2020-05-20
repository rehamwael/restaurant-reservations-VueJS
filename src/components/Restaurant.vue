<template>
    <div class="restaurant-venue">
        <div class="restaurant-table">
            <div class="table-content" v-bind:key="table.id" v-for="table in tables">
                <XTable v-bind:table="table"/>
            </div> 
        </div>
    </div>
</template>

<script>
import XTable from './table'
let allTables;
export default {
    name: 'x-restaurant',
    props: ["tables"],
    components: {
      XTable
    },
    created() {
       allTables = this.tables;
    },
    methods: {
        tableOrder: function(partySize){
            let tablesCapacities = allTables.map(function(a) {return a.capacity;});
            const output = tablesCapacities.reduce((prev, curr) => Math.abs(curr - partySize) < Math.abs(prev - partySize) ? curr : prev);
            return output;
        },
    }
}
</script>
<style scoped>
.restaurant-table .table-content:nth-child(even){
        background: #35495e;
        color: #fff;
    }

    .restaurant-table .table-content:nth-child(odd){
        background: #85ebcd;
        color: #000;
    }

    .restaurant-table{
        display: grid;
        grid-template-columns: repeat(auto-fill , minmax(200px, 2fr));
        grid-column-gap: 5px;
        grid-row-gap: 5px;
        border: 1px solid #e2e2e2;
        width: 70%;
        margin: 0 auto;
        margin-top: 4.4em;
    }

    .restaurant-table .table-content{
        display:flex;
        justify-content:center;
        align-items: center;
        height: 100px;
    }
        .table-item {
        text-align: center;
    }
    .btn {
        display: flex;
        justify-content: center;
        align-items: center;
        background: #85ebcd;
        padding: 10px 30px;
        border: 1px solid #dbfef4;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin: 10px 0;
        outline: 0;
        cursor: pointer;
        color: #1e312b;
    }
    @media only screen and (max-width: 990px){
        div.restaurant-table{
            width: 100%;
        }
    }
</style>