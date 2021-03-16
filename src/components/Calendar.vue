<template>
    <div>
        <div id="cal-header">
            <span class="header-arrow" @click="setLastMonth">＜</span>
            <span class="selected-month">{{year}}年{{month}}月</span>
            <span class="header-arrow" @click="setNextMonth">＞</span>
        </div>
        <table id="cal-main">
            <thead>
                <th 
                    v-for="(dayname,index) in weekdays" 
                    :key="index"
                >{{dayname}}</th>
            </thead>
            <tbody>
                <tr v-for="(weekData,index) in numData" 
                    :key="index">
                    <td class="cal-day" 
                        v-for="(dayNum,index) in weekData" 
                        :key="index"
                        @click="dateClick(dayNum)"
                        :class="{'cal-today':isToday(dayNum),active:day === dayNum}"
                    >
                        <span v-if="isToday(dayNum)">今日</span>
                        <span v-else>{{dayNum}}</span>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                weekdays:['日','月','火','水','木','金','土'],
                year:2020,
                month:3,
                today:'',
                day:this.dayNum
            }
        },
        computed:{
            numData:function(){
                let number = []
                let firstWeekDay = new Date(this.year,this.month-1,1).getDay();
                let lastDay = new Date(this.year,this.month,0).getDate();
                let dayNum = 1
                while(dayNum <= lastDay){
                    let weekData = [];
                    for(let i = 0;i <= 6;i++){
                        if(number.length === 0 && i < firstWeekDay){
                            weekData[i] = '';
                        }else if(lastDay < dayNum){
                            weekData[i] = '';
                        }else{
                            weekData[i] = dayNum;
                            dayNum++;
                        }
                    }
                    number.push(weekData);
                }
                return number
            }
        },
        methods:{
            setLastMonth:function(){
                if(this.month === 1){
                    this.year--;
                    this.month = 12;
                }else{
                    this.month--;
                }
            },
            setNextMonth:function(){
                if(this.month === 12){
                    this.year++;
                    this.month = 1;
                }else{
                    this.month++;
                }
            },
            dateClick:function(dayNum){
                if(dayNum != ''){
                    this.day = dayNum
                }
            },
            isToday:function(day){
                let date = this.year + "-" + this.month + "-" +day
                if(this.today == date){
                    return true;
                }
                return false;
            }
        },
        mounted(){
            let date = new Date();
            let y = date.getFullYear();
            let m = ('0' + (date.getMonth() +1)).slice(-2);
            let d = ('0' + date.getDate()).slice(-2);
            this.year = y;
            this.month = m;
            this.today = y + '-' + m + '-' + d;
        }
    }
</script>


<style scoped>
    #cal-main {
    font-size:14px;
    line-height:20px;
    table-layout: fixed;
    width: 100%;
    margin-bottom: 1rem;
    color: #212529;
    border-bottom: 1px solid #ddd;
    border-collapse: collapse;
    }
    #cal-main th {
    padding: 0;
    text-align: center;
    vertical-align: middle;
    font-weight: normal;
    color: #999;
    }
    #cal-main td {
    padding: 8px;
    text-align: center;
    vertical-align: middle;
    border-top: 1px solid #ddd;
    }
    .cal-today {
    background-color: #fcf8e3;
    }
    .cal-day.active {
    background-color: #ffc9d7;
    }
    #cal-header {
        font-size: 24px;
        padding: 0;
        text-align: center;
        margin-bottom: 10px; 
        background-color: darkorange;
        border-bottom: 1px solid #ddd;
        display:flex;
        justify-content: space-between;
    }
    #cal-header span{
        padding:15px 20px;
        color: white;
        display: inline-block;
    }
</style>