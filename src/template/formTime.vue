<template>
    <Form ref="formValCount" :model="formValCount" :rules="ruleCustom" :label-width="80">
        <FormItem label="时间计算">
            <Row>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_day.now_day" placeholder="天" disabled></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_hour.now_hour" placeholder="小时" disabled></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_min.now_min" placeholder="分钟" disabled></Input>
                </FormItem>
                </Col>
            </Row>
        </FormItem>
        <FormItem>
            <Row>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_day.add_day" :autofocus="focus == 1" elementId="add_day"
                           placeholder="天" number
                           @keyup.native.enter="enterClick()"></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_hour.add_hour" elementId="add_hour" placeholder="小时"
                           number @keyup.native.enter="enterClick()"></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValidate.date_min.add_min" elementId="add_min" placeholder="分钟"
                           number @keyup.native.enter="enterClick()"></Input>
                </FormItem>
                </Col>
            </Row>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="addDate">确认相加</Button>
            <Button type="primary" @click="clearDate" style="margin-left: 10px">清除</Button>
            <Button type="primary" @click="undo" style="margin-left: 10px">撤销</Button>
            <Button type="primary" @click="add_date_input" style="margin-left: 10px">填入加班时间</Button>
            <Button type="primary" @click="leave_date_input" style="margin-left: 10px">填入请假时间</Button>
        </FormItem>

        <FormItem label="月份">
            <Select v-model="formValCount.month" placeholder="选择月份" style="width: 160px">
                <Option v-for="item in 12" :key="item" :value="item">{{item}}月</Option>
            </Select>
        </FormItem>
        <FormItem label="是否足月">
            <RadioGroup v-model="formValCount.all">
                <Radio label="1">足月</Radio>
                <Radio label="2">不足月</Radio>
            </RadioGroup>
        </FormItem>
        <FormItem v-if="formValCount.all == 2" label="在职天数">
            <Input v-model="formValCount.work_day" placeholder="请输入在职天数" style="width: 160px"></Input>
        </FormItem>
        <FormItem label="基本工资" prop="salary">
            <Input v-model="formValCount.salary" placeholder="请输入基本工资" style="width: 160px"></Input>
        </FormItem>
        <FormItem label="加班时间">
            <Row>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.add_date.day" placeholder="天" number></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.add_date.hour" placeholder="小时" number></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.add_date.min" placeholder="分钟" number></Input>
                </FormItem>
                </Col>
            </Row>
        </FormItem>
        <FormItem label="请假时间">
            <Row>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.leave_date.day" placeholder="天" number></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.leave_date.hour" placeholder="小时" number></Input>
                </FormItem>
                </Col>
                <Col span="1">
                <div class="diving">-</div>
                </Col>
                <Col span="4">
                <FormItem>
                    <Input v-model="formValCount.leave_date.min" placeholder="分钟" number></Input>
                </FormItem>
                </Col>
            </Row>
        </FormItem>
        <FormItem label="迟到扣款">
            <Input v-model="formValCount.late" placeholder="请输入迟到扣款" number style="width: 160px"></Input>
        </FormItem>
        <FormItem label="其他扣款">
            <Input v-model="formValCount.other_del" placeholder="请输入迟到扣款" number style="width: 160px"></Input>
        </FormItem>
        <FormItem label="其他增加">
            <Input v-model="formValCount.other_add" placeholder="请输入迟到扣款" number style="width: 160px"></Input>
        </FormItem>

        <FormItem>
            <Button type="primary" @click="count('formValCount')">计算工资</Button>
        </FormItem>

        <FormItem label="加班工资：">
            <div>{{countResults.add_count}}</div>
        </FormItem>
        <FormItem label="请假工资：">
            <div>{{countResults.off_count}}</div>
        </FormItem>
        <FormItem label="工资：">
            <div>{{countResults.total_count}}</div>
        </FormItem>
    </Form>
</template>

<script>
    export default {
        data() {
            return {
                formValidate: {
                    date_day: {
                        now_day: '',
                        add_day: 0
                    },
                    date_hour: {
                        now_hour: '',
                        add_hour: 0
                    },
                    date_min: {
                        now_min: '',
                        add_min: 0
                    }
                },
                focus: 1,
                formValCount: {
                    month: -1,
                    all: 1,
                    work_day: '',
                    salary: '',
                    add_date: {
                        day: '',
                        hour: '',
                        min: ''
                    },
                    leave_date: {
                        day: '',
                        hour: '',
                        min: ''
                    },
                    late: '',
                    other_del: '',
                    other_add: ''
                },

                countResults: {
                    total_count: '',
                    add_count: '',
                    off_count: ''
                },

                ruleCustom: {
                    salary: [
                        {required: true, message: '请输入基本工资', trigger: 'blur'}
                    ]
                }
            };
        },
        computed: {
            focusSta() {
                return this.focus;
            }
        },
        created() {
            var date = new Date();
            this.formValCount.month = date.getMonth() + 1
        },
        methods: {
            addDate: function () {
                var vm = this;
                vm.dataLog = JSON.parse(JSON.stringify(vm.formValidate))
                if (vm.formValidate.date_day.add_day == '') {
                    vm.formValidate.date_day.add_day = 0;
                }
                if (vm.formValidate.date_day.now_day == '') {
                    vm.formValidate.date_day.now_day = 0;
                }
                vm.formValidate.date_day.now_day = parseInt(vm.formValidate.date_day.now_day) + parseInt(vm.formValidate.date_day.add_day);

                if (vm.formValidate.date_hour.add_hour == '') {
                    vm.formValidate.date_hour.add_hour = 0;
                }
                if (vm.formValidate.date_hour.now_hour == '') {
                    vm.formValidate.date_hour.now_hour = 0;
                }
                vm.formValidate.date_hour.now_hour = parseInt(vm.formValidate.date_hour.now_hour) + parseInt(vm.formValidate.date_hour.add_hour);

                if (vm.formValidate.date_min.add_min == '') {
                    vm.formValidate.date_min.add_min = 0;
                }
                if (vm.formValidate.date_min.now_min == '') {
                    vm.formValidate.date_min.now_min = 0;
                }
                vm.formValidate.date_min.now_min = parseInt(vm.formValidate.date_min.now_min) + parseInt(vm.formValidate.date_min.add_min);

                if (vm.formValidate.date_min.now_min >= 60) {
                    vm.formValidate.date_hour.now_hour = parseInt(vm.formValidate.date_hour.now_hour) + Math.floor(vm.formValidate.date_min.now_min / 60);
                    vm.formValidate.date_min.now_min = parseInt(vm.formValidate.date_min.now_min) - Math.floor(vm.formValidate.date_min.now_min / 60) * 60;
                }
                if (vm.formValidate.date_hour.now_hour >= 8) {
                    vm.formValidate.date_day.now_day = parseInt(vm.formValidate.date_day.now_day) + Math.floor(vm.formValidate.date_hour.now_hour / 8);
                    vm.formValidate.date_hour.now_hour = parseInt(vm.formValidate.date_hour.now_hour) - Math.floor(vm.formValidate.date_hour.now_hour / 8) * 8;
                }
                vm.formValidate.date_day.add_day = 0;
                vm.formValidate.date_hour.add_hour = 0;
                vm.formValidate.date_min.add_min = 0;

                document.getElementById('add_day').focus();
                vm.focus = 1;
            },
            enterClick: function () {
                if (this.focus == 1) {
                    document.getElementById('add_hour').focus();
                    this.focus = 2;
                } else if (this.focus == 2) {
                    document.getElementById('add_min').focus();
                    this.focus = 3;
                } else if (this.focus == 3) {
                    this.addDate();
                }
            },
            clearDate: function () {
                var vm = this;
                if (vm.formValidate.date_day.now_day != '' || vm.formValidate.date_hour.now_hour != '' || vm.formValidate.date_min.now_min != '') {
                    vm.dataLog = JSON.parse(JSON.stringify(vm.formValidate));

                    vm.formValidate.date_day.now_day = '';
                    vm.formValidate.date_hour.now_hour = '';
                    vm.formValidate.date_min.now_min = '';
                }

                vm.formValidate.date_day.add_day = 0;
                vm.formValidate.date_hour.add_hour = 0;
                vm.formValidate.date_min.add_min = 0;

                document.getElementById('add_day').focus();
                vm.focus = 1;
            },
            undo: function () {
                var vm = this;
                vm.formValidate = JSON.parse(JSON.stringify(vm.dataLog));

                document.getElementById('add_day').focus();
                vm.focus = 1;
            },

            count: function (name) {
                var vm = this
                vm.$refs[name].validate((valid) => {
                    if (valid) {
                        var days = vm.getDays(parseInt(vm.formValCount.month));

                        var daySalary = Math.floor((parseInt(vm.formValCount.salary) / days) * 100) / 100;
                        var hourSalary = Math.floor((daySalary / 8) * 100) / 100;
                        var minuteSalary = Math.floor((hourSalary / 60) * 100) / 100;

                        //计算加班工资
                        var add_date = JSON.parse(JSON.stringify(vm.formValCount.add_date))
                        if (add_date.day == '') {
                            add_date.day = 0;
                        }
                        if (add_date.hour == '') {
                            add_date.hour = 0;
                        }
                        if (add_date.min == '') {
                            add_date.min = 0;
                        }
                        var add_count =
                            daySalary * add_date.day +
                            hourSalary * add_date.hour +
                            minuteSalary * add_date.min;
                        add_count = Math.floor(add_count);

                        //计算请假时间
                        var leave_date = JSON.parse(JSON.stringify(vm.formValCount.leave_date))
                        if (leave_date.day == '') {
                            leave_date.day = 0;
                        }
                        if (leave_date.hour == '') {
                            leave_date.hour = 0;
                        }
                        if (leave_date.min == '') {
                            leave_date.min = 0;
                        }
                        var off_count =
                            daySalary * leave_date.day +
                            hourSalary * leave_date.hour +
                            minuteSalary * leave_date.min;
                        off_count = Math.floor(off_count);

                        var total_count;

                        var other_add = vm.formValCount.other_add
                        if (other_add == '') {
                            other_add = 0;
                        }
                        var late = vm.formValCount.late
                        if (late == '') {
                            late = 0;
                        }
                        var other_del = vm.formValCount.other_del
                        if (other_del == '') {
                            other_del = 0;
                        }

                        if (vm.formValCount.all == 2) {
                            if (vm.formValCount.work_day == '') {
                                vm.$Message.error('请输入在职天数');
                                return false;
                            } else {
                                total_count = Math.floor(
                                    daySalary * parseInt(vm.formValCount.work_day) +
                                    add_count +
                                    other_add -
                                    off_count -
                                    late -
                                    other_del);
                                vm.$Message.success('Success!');
                            }
                        } else {
                            total_count = Math.floor(
                                parseInt(vm.formValCount.salary) +
                                add_count +
                                other_add -
                                off_count -
                                late -
                                other_del);
                        }
                        vm.countResults.total_count = total_count
                        vm.countResults.add_count = add_count;
                        vm.countResults.off_count = off_count;

                    } else {
                        vm.$Message.error('计算失败!');
                    }
                });
            },

            add_date_input: function () {
                var vm = this;
                vm.formValCount.add_date.day = vm.formValidate.date_day.now_day;
                vm.formValCount.add_date.hour = vm.formValidate.date_hour.now_hour;
                vm.formValCount.add_date.min = vm.formValidate.date_min.now_min;
            },

            leave_date_input: function () {
                var vm = this;
                vm.formValCount.leave_date.day = vm.formValidate.date_day.now_day;
                vm.formValCount.leave_date.hour = vm.formValidate.date_hour.now_hour;
                vm.formValCount.leave_date.min = vm.formValidate.date_min.now_min;
            },

            getDays: function (month) {
                //构造当前日期对象
                var date = new Date();
                //获取年份
                var year = date.getFullYear();
                //获取月份
                var mouth = month;
                //定义当月的天数；
                var days;
                //当月份为二月时，根据闰年还是非闰年判断天数
                if (mouth == 2) {
                    days = year % 4 == 0 ? 29 : 28;
                }
                else if (mouth == 1 || mouth == 3 || mouth == 5 || mouth == 7 || mouth == 8 || mouth == 10 || mouth == 12) {
                    //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
                    days = 31;
                }
                else {
                    //其他月份，天数为：30.
                    days = 30;
                }
                return days;
            }
        }
    };
</script>