<template>
    <el-row class="row">
        <table width="100%">
            <template v-for="(item,index) in arr">
                <tr style="text-align: center;vertical-align: middle">
                    <td style="width: 20%;height: 50px">
                        <el-autocomplete style="width: 90%" class="inline-input" placeholder="请填写header" v-model="item.name" :fetch-suggestions="querySearchKey"></el-autocomplete>
                    </td>
                    <td style="width: 30%">
                        <el-autocomplete style="width: 90%;" class="inline-input" placeholder="请填写value" v-model="item.value" :fetch-suggestions="querySearchValue" @mouseenter.native="focus(item)"></el-autocomplete>
                    </td>
                    <td style="width: 40%">
                        <el-input style="width: 90%;font-size: 15px" placeholder="请填写备注" v-model="item.remark"></el-input>
                    </td>
                    <td style="width: 5%">
                        <el-button type="text" icon="close" size="small" style="color: red;font-size: 15px" @click="remove(index)"></el-button>
                    </td>
                    <td style="width: 5%">
                        <el-button type="text" size="small" v-if="index==arr.length-1" @click="add" icon="plus" style="font-size: 15px"></el-button>
                    </td>
                </tr>
            </template>
        </table>

    </el-row>
</template>
<script>
    var headerData=require("./inparamHeader")
    module.exports={
        data:function () {
            return {
                keys:Object.keys(headerData).map(function (obj) {
                    return {value:obj}
                }),
                itemSel:null
            }
        },
        computed:{
            arr:function () {
                return this.$store.state.header
            }
        },
        methods:{
            querySearchKey:function(queryString, cb) {
                var results;
                if(queryString)
                {
                    results=this.keys.filter(function (obj) {
                        return obj.value.toLowerCase().indexOf(queryString.toLowerCase()) > -1
                    })
                }
                else
                {
                    results=this.keys;
                }
                cb(results);
            },
            querySearchValue:function(queryString, cb) {
                var results;
                if(headerData[this.itemSel.name])
                {
                    results=headerData[this.itemSel.name].map(function (obj) {
                        return {value:obj}
                    })
                }
                else
                {
                    cb([]);
                    return;
                }
                if(queryString)
                {
                    results=results.filter(function (obj) {
                        return obj.value.toLowerCase().indexOf(queryString.toLowerCase()) > -1
                    })
                }
                cb(results);
            },
            focus:function (item) {
                  this.itemSel=item;
            },
            add:function () {
                this.arr.push({name:'',value:'',remark:''});
            },
            remove:function (index) {
                if(this.arr.length>1)
                {
                    this.arr.splice(index,1)
                }
                else
                {
                    this.arr[0].name="";
                    this.arr[0].value="";
                    this.arr[0].remark="";
                }
            }
        },
        created:function () {

        }
    }
</script>
