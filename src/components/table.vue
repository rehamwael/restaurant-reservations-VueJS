<template>
            <div class="table-item">
                <div v-if="isAvailable()" >
                    <p>Table Available</p>
                    <p>(Capacity= {{this.table.capacity}})</p>
                </div>
                <div v-else>
                    <p>Occupied by {{table.people}}</p>
                    <p>(free in: {{parseInt(table.duration/60)}}:{{table.duration%60}})</p>
                    <button v-on:click="evictTable()" class="btn">Evict</button>
                </div>
            </div>
    
</template>
<script>

export default {
    name : 'TableItem',
    props: ["table"],
    methods: {
        isAvailable() {
            return (this.table.is_available) ? true : false;
        },
        evictTable(){
           this.$parent.$parent.evictTable(this.table.id);
        },
        untilAvailable:async function() {
            return new Promise(resolve => {
                (this.table.is_available) ? resolve(true) : resolve(false);
            });
        }
    }
}
</script>
<style scoped>
    .table-item {
        text-align: center;
    }
    .btn {
        display: flex;
        justify-content: center;
        align-items: center;
        background: #9dcbb1;
        padding: 6px 24px;
        border: 1px solid #85ebcd;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin: 10px 0;
        outline: 0;
        cursor: pointer;
        color: red;
    }
</style>