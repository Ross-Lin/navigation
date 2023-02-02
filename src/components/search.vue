<template>
    <!-- form表单中action属性的作用是，当提交表单时向何处发送表单数据。 -->
    <el-form method="get" target="_blank" :action="scdata.url" id="search-component">
        <el-form-item>
            <el-input placeholder="请输入搜索内容" :name="scdata.key" v-model="sctext" clearable>
                <el-dropdown
                    trigger="click"
                    slot="prepend"
                    placement="bottom"
                    @command="changedata"
                >
                    <span class="el-dropdown-link">
                        <img :src="scdata.icon" alt="scdata.title" />
                        <i class="el-icon-arrow-down el-icon--right"></i>
                    </span>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item
                            v-for="each_type in sctypelist"
                            :key="each_type"
                            :command="each_type"
                        >
                            <img :src="$store.state.searchList[each_type].icon" :alt="each_type" />
                            {{ $store.state.searchList[each_type].title }}
                        </el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
                <el-button slot="append" icon="el-icon-search" native-type="submit"></el-button>
            </el-input>
        </el-form-item>
    </el-form>
</template>

<script>
export default {
    data() {
        return {
            // 从store里面读取数据并使用
            sctype: "",
            scdata: {},
            sctypelist: [],
            sctext: ""
        };
    },
    created() {
        // 设置默认值，sessionStorage 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。
        // 如果你想在浏览器窗口关闭后还保留数据，可以使用 localStorage 属性， 改数据对象没有过期时间，今天、下周、明年都能用，除非你手动去删除。
        var last_type = sessionStorage.getItem("last_type");
        var default_type = last_type
            ? last_type
            : this.$store.getters.searchTypes[0];
        this.sctype = default_type;
        this.sctypelist = this.$store.getters.searchTypes;
        this.scdata = this.$store.state.searchList[default_type];
    },
    methods: {
        // 选择表单变化的时候同步数据，同时添加到session
        changedata: function(command) {
            this.sctype = command;
            sessionStorage.setItem("last_type", command);
            this.scdata = this.$store.state.searchList[command];
        }
    }
};
</script>

<style scoped>
img {
    width: 1.2rem;
    margin: 0 0.2rem -0.2rem 0;
}
</style>
