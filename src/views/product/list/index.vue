<template>
    <div>
        <div class="p-1">
            <a-alert closable>默认新增的产品为关闭状态，需打开启用才可以推广</a-alert>
        </div>

        <div class="ma-content-block lg:flex justify-between p-4">
            <ma-crud :options="options" :columns="columns" ref="crudRef">
                <template #logo_url="{ record }">
                    <a-image :src="record.logo_url || '无预览图'" style="object-fit: cover" alt="logo" width="50px"
                             height="50px"/>
                </template>
                <!-- 排序列 -->
                <template #sort="{ record }">
                    <a-input-number
                            :default-value="record.sort"
                            mode="embed"
                            @change="updateValue($event,record.id,'sort')"
                            :min="0"
                            :max="999999"
                    />
                </template>
                <!-- 状态列 -->
                <template #is_enabled="{ record }">
                    <a-switch
                            type="line"
                            size="small"
                            :checked-value=1
                            unchecked-value=0
                            @change="updateValue($event, record.id,'is_enabled')"
                            :default-checked="parseInt(record.is_enabled) === 1"
                    />
                </template>
            </ma-crud>
        </div>
    </div>
</template>
<script setup>
import {reactive, ref} from 'vue'
import productList from '@/api/product/productList'
import {Message} from '@arco-design/web-vue'

const crudRef = ref()

const updateValue = (statusValue, id, statusName) => {
    productList.changeStatus({id, statusName, statusValue}).then(res => {
        res.success && Message.success(res.message)
        crudRef.value.refresh()
    }).catch(e => {
        console.log(e)
    })
}

const options = reactive({
    beforeRequest: (params) => {
        params.orderBy = 'id'
        params.orderType = 'desc'
    },
    id: 'vcm_product_list',
    rowSelection: {
        showCheckedAll: false
    },
    pk: 'id',
    operationColumn: true,
    operationWidth: 160,
    formOption: {
        viewType: 'modal',
        width: 650
    },
    api: productList.getList,
    recycleApi: productList.getRecycleList,
    add: {
        show: true,
        api: productList.save,
        auth: ['product:list:save']
    },
    edit: {
        show: true,
        api: productList.update,
        auth: ['product:list:update']
    },
    delete: {
        show: true,
        api: productList.deletes,
        auth: ['product:list:delete'],
        realApi: productList.realDeletes,
        realAuth: ['product:list:realDeletes']
    },
    recovery: {
        show: true,
        api: productList.recoverys,
        auth: ['product:list:recovery']
    },
    import: {
        show: true,
        url: 'product/list/import',
        templateUrl: 'product/list/downloadTemplate',
        auth: ['product:list:import']
    },
    export: {
        show: true,
        url: 'product/list/export',
        auth: ['product:list:export']
    }
})

const columns = reactive([
    {
        title: "ID",
        dataIndex: "id",
        formType: "static-text",
        addDisplay: false,
        editDisplay: true,
        hide: false,
        readonly: true,
        sortable: {
            sortDirections: [
                "ascend",
                "descend"
            ],
            sorter: true
        },
        width: 60
    },
    {
        title: "产品名字",
        dataIndex: "product_name",
        formType: "input",
        search: true,
        commonRules: {
            required: true,
            message: "请输入产品名字"
        },
        width: 100
    },
    {
        title: "LOGO",
        dataIndex: "logo_url",
        formType: "upload",
        commonRules: {
            required: true,
            message: "请上传logo图片一张"
        },
        multiple: false,
        chunk: false,
        onlyData: false,
        type: "image",
        width: 50
    },
    {
        title: "官网地址",
        dataIndex: "official_url",
        formType: "input",
        search: true,
        commonRules: {
            required: true,
            message: "请输入官网地址"
        },
        width: 150
    },
    {
        title: "排序",
        dataIndex: "sort",
        formType: "input-number",
        search: false,
        commonRules: {
            required: false,
            message: "请输入排序"
        },
        addDefaultValue: "0",
        placeholder: "默认为0"
    },
    {
        title: "状态",
        dataIndex: "is_enabled",
        formType: "select",
        search: true,
        commonRules: {
            required: true,
            message: "请输入状态"
        },
        multiple: false,
        dict: {
            name: "is_enabled",
            props: {
                label: "title",
                value: "key"
            },
            translation: true
        },
        width: 40,
        addDefaultValue: 0
    },
    {
        title: "推荐语",
        dataIndex: "remark",
        formType: "textarea",
        commonRules: {
            required: true,
            message: "请输入推荐语"
        },
        width: 180
    },
    {
        title: "创建时间",
        dataIndex: "created_at",
        formType: "range",
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
        showTime: true,
        range: false
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
<script> export default {name: 'product:list'} </script>