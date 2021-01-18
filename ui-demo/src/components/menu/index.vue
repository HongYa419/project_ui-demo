<template>
    <div>
        <el-breadcrumb separator=">">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>菜单管理</el-breadcrumb-item>
        </el-breadcrumb>
        <el-button type="primary" @click="$router.push('/menu/add')">添加</el-button>
        <el-table :data="arr" row-key="id" border stripe>
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="菜单名称" prop="title"></el-table-column>
            <el-table-column label="菜单类型">
                <template slot-scope="item">
                    <el-tag v-if="item.row.type == 1">目录</el-tag>
                    <el-tag v-else>菜单</el-tag>
                </template>
            </el-table-column>
            <el-table-column label="菜单图标" prop="icon">
                <template slot-scope="item">
                    <i :class="item.row.icon"></i>
                </template>
            </el-table-column>
            <el-table-column label="菜单地址" prop="url"></el-table-column>
            <el-table-column label="状态">
                <template slot-scope="item">
                    <el-tag v-if="item.row.status == 1">启用</el-tag>
                    <el-tag v-else>禁用</el-tag>
                </template>
            </el-table-column>
            <el-table-column label="操作" width="150px">
                <template slot-scope="scope">
                    <el-button size="mini" type="primary" @click="$router.push('/menu/'+scope.row.id)">编辑</el-button>
                    <el-button size="mini" type="danger" @click="del(scope.row.id)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            arr:[]
        }
    },
    mounted() {
        this.axios.get('/api/menulist',{ params:{istree:true} }).then(res=>{
            this.arr = res.data.list
        })   
    },
    methods: {
        del(index){
            this.$confirm('此操作将永久删除该菜单, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                this.axios.post('/api/menudelete',{id:index}).then(res=>{
                    if(res.data.code === 200){
                        this.arr = res.data.list
                        this.$message({
                            type: 'success',
                            message: '删除成功!'
                        });
                    }else {
                        this.$message({
                            type: 'info',
                            message: res.data.msg
                        });
                    }
                });
            }).catch(()=>{})
        }   
    }
}
</script>

<style>

</style>