<style>
    /* 必需 */
    .vue-date-picker-transition {
        transition: all .3s cubic-bezier(.35, 1.43, .56, 1.01);
        transform: scale(1, 1);
        transform-origin: 5% 0%;
    }

    /* .expand-enter 定义进入的开始状态 */
    /* .expand-leave 定义离开的结束状态 */
    .vue-date-picker-enter, .vue-date-picker-leave {
        opacity: 0;
        transform: scale(0.9, 0.9)
    }

    .vue-date-picker {
        text-align: left;
        font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    }

    .vue-date-picker > .picker:after {
        top: -6px;
        left: 12px;
        position: absolute;
        display: block;
        width: 0;
        height: 0;
        content: ' ';
        border: 3px solid transparent;
        border-bottom-color: #fff;
    }

    .vue-date-picker > .picker:before {
        top: -10px;
        left: 10px;
        position: absolute;
        display: block;
        width: 0;
        height: 0;
        content: ' ';
        border: 5px solid transparent;
        border-bottom-color: #000;

    }

    .vue-date-picker > .picker > div {
        width: 100%;
        position: relative;
        height: auto;

    }

    .vue-date-picker > .picker > div > .tile {
        float: left;
        border-radius: 8px;
        border: 2px solid #fff;
        box-sizing: border-box;



    }
    .vue-date-picker > .picker > div > .gray {
        box-sizing: border-box;
        color: #ccc;
    }
    .vue-date-picker > .picker > div > .title {
        width: 100%;
        box-sizing: border-box;

    }


    .vue-date-picker > .picker > div > .tile.selected {
        background: #39f;
        color: #fff;

    }

    .vue-date-picker > .picker > div > .tile.selected:hover {
        background: #39f;
        color: #fff;
    }

    .vue-date-picker > .picker > div > .hover {
        cursor: pointer;
    }

    .vue-date-picker > .picker > div > .hover:hover {
        background: #f0f0f0;
    }

    .vue-date-picker > .picker {
        position: relative;
        text-align: center;
        box-shadow: 0 2px 3px #999;
        box-sizing: content-box;
        height: auto;
        background: #fff;
        border-top: 2px solid #000;
        margin-top: 6px;
        user-select: none;
        -webkit-user-select: none;

    }

    .vue-date-picker > .picker > div > .days_title {
        background: #f0f0f0;
        border-radius: 0px;
        border: 0px;
    }
    .vue-date-picker > .picker > div > .line {
        background: #f0f0f0;
        clear: both;
        width: 100%;
    }

</style>
<template>
    <div class="vue-date-picker" @click="clickInSide" :style="{zIndex:zIndex,fontSize:fontSize+'px'}">
        <div :class="inputClass" v-show="type=='div'" @click="onFocus">{{date}}</div>
        <input :class="inputClass" v-show="type=='input'" @focus="onFocus" v-model="date"/>
        <div class="picker" v-show="show" transition="vue-date-picker" style="position: absolute;">
            <div v-show="selector=='years'">
                <div class="tile title hover" @click="prevYears"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">&lt;
                </div>
                <div class="tile title" :style="{width:70+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">
                    {{years_title}}
                </div>
                <div class="tile title hover" @click="nextYears"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">&gt;
                </div>
                <div class="line" style="height: 1px"></div>
                <div v-for="item in years" class="tile  hover" :class="{selected:item==real_date.year}"
                     @click="selectYear(item)"
                     :style="{width:cellMonthWidth+'px',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">
                    {{item}}
                </div>
            </div>
            <div v-show="selector=='months'">
                <div class="tile title hover" @click="prevYear"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">&lt;
                </div>
                <div class="tile title hover" @click="showSeletor('years')"
                     :style="{width:70+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">{{year_title}}
                </div>
                <div class="tile title hover" @click="nextYear"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">&gt;
                </div>
                <div class="line" style="height: 1px"></div>
                <div v-for="(index,item) in months" class="tile hover"
                     :class="{selected:item==real_date.month&&catch_year==real_date.year}" @click="selectMonth(item)"
                     :style="{width:cellMonthWidth+'px',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">
                    {{lang.monthsShort[index]}}
                </div>
            </div>
            <div v-show="selector=='days'">
                <div class="tile title hover" @click="prevMonth"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">&lt;
                </div>
                <div class="tile title hover" @click="showSeletor('months')"
                     :style="{width:70+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">{{days_title}}
                </div>
                <div class="tile title hover" @click="nextMonth"
                     :style="{width:15+'%',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}" >&gt;
                </div>
                <div class="line" style=""></div>
                <div v-for="(index,item) in lang.daysMin" class="days_title tile "
                     :style="{width:cellDayWidth+'px',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}" style="">
                    {{item}}
                </div>

                <div v-for="(index,item) in days" class="tile"
                     :class="{hover:true,gray:item.month!=catch_month,selected:item.date==real_date.day&&item.month==real_date.month&&catch_year==real_date.year}"
                     @click="selectDay(item)"
                     :style="{width:cellDayWidth+'px',height:cellDayWidth+'px',lineHeight:lineHeight+'px'}">
                    {{item.date}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>


    /*!
     * vue-datepicker v0.0.1 (https://github.com/johnnyGoo/vue-datepicker)
     * Author: Johnny chen
     *
     * Copyright 2016-2017 Johnny chen
     */

    if(!window.Smart){
        throw 'vue-datepicker required smart.js'
    }

    var Smart=window.Smart;
    var Css=Smart.Css;
    var Event=Smart.Event;
    var _=Smart._;
    var documentEvent=Event.documentEvent;

    //正则匹配
    function match(str, matchs) {
        var regStr = '';
        if (_.isArray(matchs)) {
            _.each(matchs, function (v) {
                regStr = regStr + v + '|';
            });
            regStr = regStr.substr(0, regStr.length - 1);
        } else {
            regStr = matchs;
        }
        var reg = new RegExp(regStr);
        return reg.test(str);
    }


    //左边补0
    function pad(num, n) {
        return (Array(n).join(0) + num).slice(-n);
    }

    function stringToDate(dateString) {
        var str = dateString.toString();
        str = str.replace(/-/g, "/");
        var datearr=str.split(' ')[0].split('/');
        var obj = {};
        var date = new Date(str);
        date.setFullYear(datearr[0]);
        if(datearr.length>1){
        date.setMonth(datearr[1]-1);}
        if(datearr.length>2) {
            date.setDate(datearr[2]);
        }
        obj.year = date.getFullYear();
        obj.month = date.getMonth() + 1;
        obj.day = date.getDate();
        obj.hours = date.getHours();
        obj.minutes = date.getMinutes();
        obj.seconds = date.getSeconds();
        return obj;
    }

    /*
     * 替代js本身的 eval,避免编译错误
     * */

    function getDaysInMonth(year, month) {
        var daycount = 30;
        if (_.contains([1, 3, 5, 7, 8, 10, 12], month)) {
            daycount = 31;
        } else if (month == 2) {
            daycount = year % 4 == 0 ? 29 : 28;
        }
        return daycount;
    }

    function buildDays(year, month) {
        year = parseInt(year)
        var daycount = getDaysInMonth(year, month)
        var days = [];
        var days_numbers = _.range(1, daycount + 1);
        var date = new Date();
        date.setFullYear(year);
        date.setMonth(month - 1);
        _.each(days_numbers, function (v, k) {
            date.setDate(v);
            days[k] = ({year: year, day: date.getDay(), date: date.getDate(), month: date.getMonth() + 1});
        });
        var l_date;

        var nowYear = year;
        var nowMonth = month;

        if (month == 1) {
            nowYear = year - 1;
            nowMonth = 12;
        } else {
            nowYear = year;
            nowMonth = month - 1;
        }


        while (days[0].day > 0) {
            if (days[0].date == 1) {
                l_date = getDaysInMonth(year, nowMonth)
            } else {
                l_date = days[0].date - 1;
            }
            date.setFullYear(nowYear, nowMonth - 1, l_date);

            days.unshift({year: nowYear, day: date.getDay(), date: date.getDate(), month: date.getMonth() + 1})
        }
        if (month == 12) {
            nowYear = year + 1;
            nowMonth = 1;
        } else {
            nowYear = year;
            nowMonth = month + 1;
        }

        while (days.length %7!=0) {
            if (days[days.length - 1].date == daycount) {
                l_date = 1
            } else {
                l_date = days[days.length - 1].date + 1;
            }
            date.setFullYear(nowYear, nowMonth - 1, l_date);
            days.push({year: nowYear, day: date.getDay(), date: date.getDate(), month: date.getMonth() + 1})
        }

        return days;

    }

    function buildYears(year) {
        var startYear = parseInt(year) - 5;
        return _.range(startYear, startYear + 12)
    }

    function stringToObj(v) {
        var estr = ('(' + v + ')');
        var Fn = Function;  //一个变量指向Function，防止有些前端编译工具报错
        return new Fn('return ' + estr)();
    }


    export default{
        name: 'vue-datepicker',
        // 声明 props
        props: {
            zIndex: {
                default: 100
            },
            date: {
                default: function () {
                    var date = new Date()
                    return date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate() + ' ' + date.getHours() + ':' + date.getMinutes() + ':' + date.getSeconds();
                }
            },
            lang: {
                default: function () {
                    return {
                        daysMin: ['日', '一', '二', '三', '四', '五', '六'],
                        months: ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月'],
                        monthsShort: ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月'],
                        weekStart: 1,
                        yearFirst: true,
                        yearSuffix: '年'
                    }
                }
            },
            show: {
                default: false
            },
            selector: {
                default: 'days'
            },
            width: {
                default: 240
            },
            autoClose: {
                default: true
            },
            format: {
                default: 'yyyy-mm-dd'
            },
            type: {
                type: String,
                default: 'input'
            },
            inputClass: {
                type: String,
                default: ''
            }
        },
        data: function () {
            return {
                years: [],
                catch_year: 0,
                catch_month: 0,
                months: _.range(1, 13),
                days: [],
                real_date: null,
                min_time: 'day',
                real_date: {},
                click_in_side: false
            };
        },
        // prop 可以用在模板内
        // 可以用 `this.msg` 设置

        methods: {
            clickInSide: function () {
                var me = this;
                me.click_in_side = true;
                setTimeout(function () {
                    me.click_in_side = false;
                }, 10)
            },
            close: function () {
                var me = this;
                if (me.click_in_side == false) {
                    me.show = false;
                }
                return !me.click_in_side;
            },
            onFocus: function () {
                this.show = true;
            },
            onBlur: function () {

            },
            showSeletor: function (type) {
                this.selector = type;
            },
            updateDate: function (date) {
                this.real_date = date;
                this.date = this.dateString;
                this.catch_year = this.real_date.year;
                this.catch_month = this.real_date.month;
                this.catch_day = this.real_date.day;
                this.days = buildDays(this.catch_year, this.catch_month);
                this.$dispatch('datepicker-update', this.date);
            },
            prevYears: function () {
                this.catch_year -= 12;
                this.years = buildYears(this.catch_year);
            },
            nextYears: function () {
                this.catch_year += 12;
                this.years = buildYears(this.catch_year);
            },
            selectYear: function (year) {
                this.real_date.year = (year);
                this.updateDate(this.real_date);
                this.catch_year = this.real_date.year;
                if (this.min_time == 'year' && this.autoClose) {
                    this.show = false;
                    return;
                }
                this.selector = 'months'


            },
            prevYear: function () {
                this.catch_year -= 1;
                this.years = buildYears(this.catch_year);
            },
            nextYear: function () {
                this.catch_year += 1;
                this.years = buildYears(this.catch_year);
            },
            selectMonth: function (month) {
                this.catch_month = month;
                this.real_date.year = this.catch_year;
                this.real_date.month = (month);
                this.updateDate(this.real_date);
                if (this.min_time == 'month' && this.autoClose) {
                    this.show = false;
                    return;
                }

                this.selector = 'days';

            },

            prevMonth: function () {
                this.catch_month -= 1;
                if (this.catch_month < 1) {
                    this.catch_month = 12;
                    this.catch_year--;
                }
                this.days = buildDays(this.catch_year, this.catch_month);
            },
            nextMonth: function () {
                this.catch_month += 1;
                if (this.catch_month > 12) {
                    this.catch_month = 1;
                    this.catch_year++;
                }
                this.days = buildDays(this.catch_year, this.catch_month);
            },
            selectDay: function (day) {
                this.catch_day = day.date;
                this.real_date.day = day.date;
                this.real_date.month = day.month;
                this.real_date.year = day.year;
                this.updateDate(this.real_date);
                if (this.min_time == 'day' && this.autoClose) {
                    this.show = false;
                    return;
                }
            }

        },
        computed: {
            dateString: function () {
                // return this.real_date.year + '/' + (this.real_date.month) + '/' + this.real_date.day
                return this.format.replace('yyyy', this.real_date.year).replace('mm', pad(this.real_date.month, 2)).replace('dd', pad(this.real_date.day, 2)).replace('hh', pad(this.real_date.hours, 2)).replace('ii', pad(this.real_date.minutes, 2)).replace('ss', pad(this.real_date.seconds, 2));

            },
            years_title: function () {
                return this.years[0] + '-' + this.years[this.years.length - 1];
            },
            year_title: function () {
                return this.catch_year + this.lang.yearSuffix + '';
            }, days_title: function () {
                return this.catch_year + this.lang.yearSuffix + ' ' + this.lang.months[this.catch_month - 1];
            },
            cellDayWidth:function () {
                return this.width/7;
            },
            cellMonthWidth:function () {
                return Math.floor(this.width/4);
            },
            fontSize:function () {
                return Math.floor(this.width/20);
            },
            lineHeight:function () {
                return Math.floor(this.width/8);
            }
        },
        compiled: function () {
            var me = this;
            this.updateDate(stringToDate(this.date));
            this.years = buildYears(this.real_date.year);
            Css.smartCss(this.$el.querySelector('.vue-date-picker>.picker'), {width: this.width});
            if (match(this.format, 'yyyy')) {
                this.min_time = 'year';
                this.selector = 'years';
            }
            if (match(this.format, 'mm')) {
                this.min_time = 'month';
                this.selector = 'months';
            }
            if (match(this.format, 'dd')) {
                this.min_time = 'day';
                this.selector = 'days';
            }
            if (match(this.format, 'hh')) {
                this.min_time = 'hours';
                this.selector = 'hours';
            }
            if (match(this.format, 'ii')) {
                this.min_time = 'minutes';
                this.selector = 'minutes';
            }
            if (match(this.format, 'ss')) {
                this.min_time = 'seconds';
                this.selector = 'seconds';
            }
        },
        watch: {
            'show': function (val, oldVal) {
                var me = this;
                if (val == true) {
                    documentEvent('click', me.close)
                }else{
                    this.updateDate(this.real_date);
                }
            },
            'date': function (val, oldVal) {
                if (this.min_time == 'day') {
                    var reg = new RegExp('^(?:(?!0000)[0-9]{1,5}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$')
                    if (reg.test(val)) {

                        this.updateDate(stringToDate(val));
                    }
                }

            }
        }
    }


</script>