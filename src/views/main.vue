<template>
    <div class="main-box" :class="{'menu-hide': !menuStatus}">
        <header class="header">
            <v-header logo="static/images/logo.png"></v-header>
        </header>
        <aside class="aside clearfix">
            <v-aside></v-aside>
        </aside>
        <div class="common-content">
            <div class="breadcrumb">
                <breadcrumb></breadcrumb>
            </div>
            <div id="contentMain">
                <router-view></router-view>
            </div>
        </div>
        <div class="global-components">
        </div>
    </div>
</template>

<script>
    import breadcrumb from 'components/breadcrumb/index';
    import header from 'components/common/header';
    import aside from 'components/common/aside';
    import systemService from 'services/systemService';

    export default {
        created() {
            let userInfo = {};
            try {
                userInfo = JSON.parse(window.localStorage.getItem('userInfo'));
                this.$store.commit('setUserInfo', userInfo);
            } catch (e) {
                this.$alert('获取用户信息失败');
            }
            // 判断用户是否登录
            systemService.checkLogin().then(({data}) => {
                if (data.data) {
                    console.log('checkLogin success');
                }
            });
        },
        computed: {
            menuStatus() {
                return this.$store.state['config'].menuStatus;
            }
        },
        components: {
            breadcrumb,
            vHeader: header,
            vAside: aside
        }
    };

</script>

<style lang="scss" rel="stylesheet/scss">

</style>
