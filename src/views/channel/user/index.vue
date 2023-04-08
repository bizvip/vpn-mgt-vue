<template>
    <div class="ma-content-block lg:flex justify-between p-4">
        <!-- CRUD 组件 -->
        <ma-crud :options="options" :columns="columns" ref="crudRef">
        </ma-crud>
    </div>
</template>
<script setup>
import {reactive, ref} from 'vue'
import channelUser from '@/api/channel/channelUser'
import {Message} from '@arco-design/web-vue'

const crudRef = ref()

const numberOperation = (newValue, id, numberName) => {
    channelUser.numberOperation({id, numberName, numberValue: newValue}).then(res => {
        res.success && Message.success(res.message)
    }).catch(e => {
        console.log(e)
    })
}

const switchStatus = (statusValue, id, statusName) => {
    channelUser.changeStatus({id, statusName, statusValue}).then(res => {
        res.success && Message.success(res.message)
    }).catch(e => {
        console.log(e)
    })
}


const options = reactive({
    id: 'vcm_channel_user',
    rowSelection: {
        showCheckedAll: true
    },
    pk: 'id',
    operationColumn: true,
    operationWidth: 160,
    formOption: {
        viewType: 'modal',
        width: 600
    },
    api: channelUser.getList,
    recycleApi: channelUser.getRecycleList,
    add: {
        show: true,
        api: channelUser.save,
        auth: ['channel:user:save']
    },
    edit: {
        show: true,
        api: channelUser.update,
        auth: ['channel:user:update']
    },
    delete: {
        show: true,
        api: channelUser.deletes,
        auth: ['channel:user:delete'],
        realApi: channelUser.realDeletes,
        realAuth: ['channel:user:realDeletes']
    },
    recovery: {
        show: true,
        api: channelUser.recoverys,
        auth: ['channel:user:recovery']
    },
    import: {
        show: true,
        url: 'channel/user/import',
        templateUrl: 'channel/user/downloadTemplate',
        auth: ['channel:user:import']
    },
    export: {
        show: true,
        url: 'channel/user/export',
        auth: ['channel:user:export']
    }
})

const columns = reactive([
    {
        title: "ID",
        dataIndex: "id",
        formType: "input-number",
        search: true,
        addDisplay: false,
        editDisplay: false,
        sortable: {
            sortDirections: [
                "ascend",
                "descend"
            ],
            sorter: true
        },
        min: 0,
        max: 4200000000,
        step: 1
    },
    {
        title: "渠道ID",
        dataIndex: "channel_id",
        formType: "select",
        search: true,
        editDisplay: false,
        commonRules: {
            required: true,
            message: "请输入渠道ID"
        }
    },
    {
        title: "渠道名",
        dataIndex: "channel_name",
        formType: "select",
        search: true,
        editDisplay: false,
        commonRules: {
            required: true,
            message: "请输入渠道名"
        },
        multiple: false
    },
    {
        title: "渠道标识",
        dataIndex: "channel_slug",
        formType: "select",
        addDisplay: false,
        editDisplay: false,
        hide: true
    },
    {
        title: "产品ID",
        dataIndex: "product_id",
        formType: "select",
        search: true,
        commonRules: {
            required: true,
            message: "请输入产品ID"
        },
        multiple: false
    },
    {
        title: "用户标识",
        dataIndex: "user_slug",
        formType: "input",
        search: true,
        commonRules: {
            required: true,
            message: "请输入用户标识"
        }
    },
    {
        title: "UUID",
        dataIndex: "uuid",
        formType: "input",
        search: true,
        addDisplay: false,
        editDisplay: false
    },
    {
        title: "邀请人",
        dataIndex: "invited_user",
        formType: "select",
        search: true,
        addDisplay: false,
        editDisplay: false,
        multiple: false
    },
    {
        title: "邀请日期",
        dataIndex: "invited_date",
        formType: "range",
        search: true,
        addDisplay: false,
        editDisplay: false,
        showTime: false
    },
    {
        title: "扣量",
        dataIndex: "deduct_total",
        formType: "select",
        search: true,
        commonRules: {
            required: true,
            message: "请输入扣量"
        },
        multiple: false
    },
    {
        title: "创建时间",
        dataIndex: "created_at",
        formType: "range",
        search: true,
        addDisplay: false,
        editDisplay: false,
        hide: true,
        commonRules: {
            required: true,
            message: "请输入创建时间"
        },
        showTime: true
    },
    {
        title: "更新时间",
        dataIndex: "updated_at",
        formType: "date",
        addDisplay: false,
        editDisplay: false,
        hide: true,
        commonRules: {
            required: true,
            message: "请输入更新时间"
        },
        showTime: true
    },
    {
        title: "删除时间",
        dataIndex: "deleted_at",
        formType: "date",
        addDisplay: false,
        editDisplay: false,
        hide: true,
        showTime: true
    }
])
</script>
<script> export default {name: 'channel:user'} </script>