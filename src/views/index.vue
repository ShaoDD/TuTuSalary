<template>
    <div class="layout" :class="{'layout-hide-text': spanLeft < 4}">
        <Row type="flex">
            <Col :span="spanLeft" class="layout-menu-left">
            <Menu active-name="1" theme="dark" width="auto">
                <div class="layout-logo-left"></div>
                <MenuItem name="1">
                    <Icon type="ios-navigate" :size="iconSize"></Icon>
                    <span class="layout-text">计算工资</span>
                </MenuItem>
            </Menu>
            </Col>
            <Col :span="spanRight">
            <div class="layout-header">
                <Button type="text" @click="toggleClick">
                    <Icon type="navicon" size="32"></Icon>
                </Button>
            </div>
            <div class="layout-breadcrumb">
                <Breadcrumb>
                    <BreadcrumbItem href="#">Index</BreadcrumbItem>
                </Breadcrumb>
            </div>
            <div class="layout-content">
                <div class="layout-content-main">
                    <form-time></form-time>
                </div>
            </div>
            <div class="layout-copy">
                2333-2333 &copy; Dong
            </div>
            </Col>
        </Row>
    </div>
</template>
<script>
    import formTime from '../template/formTime.vue';

    export default {
        data() {
            return {
                spanLeft: 4,
                spanRight: 20
            };
        },
        components: {
            formTime
        },
        computed: {
            iconSize() {
                return this.spanLeft === 4 ? 15 : 25;
            }
        },
        methods: {
            toggleClick() {
                if (this.spanLeft === 4) {
                    this.spanLeft = 1;
                    this.spanRight = 23;
                } else {
                    this.spanLeft = 4;
                    this.spanRight = 20;
                }
            }
        }
    };
</script>