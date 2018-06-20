<template>
    <div>
        <el-row>
            <el-col :span="13" :offset="4">
                <el-tabs type="border-card" @tab-click="handleSwitch" class="search-tabs">
                    <el-tab-pane label="api">
                        <el-form :inline="true">
                            <category :items="items" :multiple="false" @update-select="updateSelect"/>
                            <el-form-item label="点值">
                                <el-input v-model.lazy.number="apiSearch.worthy"/>
                            </el-form-item>
                            <el-form-item label="评论">
                                <el-input v-model.lazy.number="apiSearch.comments"/>
                            </el-form-item>
                            <el-form-item label="页码">
                                <el-input v-model.number="apiSearch.pages"/>
                            </el-form-item>
                            <el-form-item label="排序">
                                <el-switch v-model="apiSearch.sortByDate" inactive-color="#ff4949" active-text="时间排序" inactive-text="点值排序"/>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" icon="el-icon-search" @click="search" :loading="loadingStatus">查询</el-button>
                            </el-form-item>
                        </el-form>
                    </el-tab-pane>
                    <el-tab-pane label="db">
                        <el-form :inline="true">
                            <category :items="items" :multiple="true" @update-select="updateSelect"/>
                            <el-form-item label="标题">
                                <el-input v-model="dbSearch.title"/>
                            </el-form-item>
                            <el-form-item label="点值">
                                <el-input v-model.number="dbSearch.worthy"/>
                            </el-form-item>
                            <el-form-item label="评论">
                                <el-input v-model.number="dbSearch.comments"/>
                            </el-form-item>
                            <el-form-item label="时间">
                                <el-date-picker value-format="yyyy-MM-dd" v-model="dbSearch.dateRange" type="daterange" range-separator="至"
                                                start-placeholder="开始日期" end-placeholder="结束日期"/>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" icon="el-icon-search" @click="search" :loading="loadingStatus">查询</el-button>
                            </el-form-item>
                        </el-form>
                    </el-tab-pane>
                </el-tabs>
            </el-col>
        </el-row>
        <el-row style="margin-top: 20px">
            <el-col :span="13" :offset="4" class="z-clearfix" :v-loading="loadingStatus">
                <article-list :items="articleList" v-show="articleList.length >0"/>
                <div v-show="articleList.length === 0" style="padding: 40px">当前暂无记录</div>
            </el-col>
        </el-row>
        <el-row v-show="currentModel === 'db'">
            <el-col :span="6" :offset="8">
                <el-pagination
                        @current-change="handleCurrentChange"
                        :current-page.sync="dbSearch.pageIndex"
                        :page-size="30"
                        layout="total,  prev, pager, next, jumper"
                        :total="total"
                >
                </el-pagination>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import articleList from "./component/article-list.vue";

    import category from "./component/category";
    import moment from "moment";
    import axios from "axios";

    export default {
        data() {
            return {
                currentModel: 'api',
                loadingStatus: false,
                apiSearch: {
                    category: [[], []],
                    worthy: 0,
                    comments: 0,
                    pages: 2,
                    sortByDate: true
                },
                dbSearch: {
                    dateRange: [
                        new moment().subtract(7, 'days').format('YYYY-MM-DD'),
                        new moment().format('YYYY-MM-DD')
                    ],
                    category: [[], []],
                    title: '',
                    worthy: 0,
                    comments: 0,
                    pageIndex: 1,
                },
                total: 500,
                apiList: [],
                dbList: [],
                items: [],
                keywords: ""
            };
        }, components: {
            articleList,
            category
        }, methods: {
            handleCurrentChange(val) {
            }, handleSwitch(tab) {
                this.currentModel = tab.label;
            }, search() {
                let vm = this;
                if (vm.currentModel === 'api') {
                    if (vm.keywords === "") {
                        vm.$message({
                            message: '请选择目录',
                            type: 'warning'
                        });
                        return;
                    }
                    axios.post('/article/proxy-list', {
                        "keyWords": vm.keywords,
                        "page": this.apiSearch.pages,
                        "searchByCategory": true
                    }).then(function (response) {
                        vm.apiList = response.data.data;
                    }).catch(function (error) {
                        console.log(error);
                    });
                } else {
                    console.log(JSON.stringify(this.dbSearch));
                }
            }, updateSelect(selectArr, inputValue) {
                if (this.currentModel === 'api') {
                    this.apiSearch.category = selectArr;
                    this.keywords = inputValue;
                } else {
                    this.dbSearch.category = selectArr;
                }
            }
        }, mounted() {
            let vm = this;
            // 添加请求拦截器
            axios.interceptors.request.use(function (config) {
                // 在发送请求之前做些什么
                vm.loadingStatus = true;
                return config;
            }, function (error) {
                // 对请求错误做些什么
                return Promise.reject(error);
            });
            // 添加响应拦截器
            axios.interceptors.response.use(function (response) {
                // 对响应数据做点什么
                vm.loadingStatus = false;
                return response;
            }, function (error) {
                // 对响应错误做点什么
                vm.loadingStatus = false;
                return Promise.reject(error);
            });
            axios.get(`/category/get-last/${window.localStorage.sha1}`).then(function (response) {
                let data = response.data.data;
                if (Array.isArray(data)) {
                    window.localStorage.sha1 = data[0];
                    window.localStorage.category = data[1];
                }
                vm.items = JSON.parse(window.localStorage.category);
            }).catch(function (error) {
                console.log(error);
            });
        }, computed: {
            articleList() {
                let vm = this;
                if (vm.currentModel === 'db') {
                    return vm.dbList;
                } else {
                    let tempList = vm.apiList.filter(x => x.worthy >= vm.apiSearch.worthy && x.comment >= vm.apiSearch.comments);
                    if (vm.apiSearch.sortByDate) {
                        tempList.sort((a, b) => {
                            return b.date - a.date;
                        });
                    } else {
                        tempList.sort((a, b) => {
                            return b.worthy - a.worthy;
                        });
                    }
                    return tempList;
                }
            }
        }
    }
</script>
<style>
    .content-inner {
        width: 1200px;
        margin: 0 auto;
    }

    .z-clearfix, .z-hor-feed {
        zoom: 1;
    }

    .z-clearfix:after, .z-clearfix:before, .z-hor-feed:after, .z-hor-feed:before {
        content: "";
        display: table;
    }

    .z-clearfix:after, .z-hor-feed:after {
        clear: both;
    }

    * {
        outline: 0;
    }

    body {
        margin: 0;
        line-height: 1.2;
        background-color: #eee;
        font-family: Helvetica, Hiragino Sans GB, Microsoft Yahei, sans-serif;
        font-size: 14px;
        color: #333;
        -webkit-text-size-adjust: none
    }

    blockquote, button, dl, form, input, ol, p, textarea, ul {
        margin: 0;
        padding: 0
    }

    a {
        color: #5188a6;
        cursor: pointer;
    }

    a, a:hover {
        text-decoration: none;
    }

    li {
        list-style: none;
    }

    @font-face {
        font-family: zdm-icons;
        src: url(https://res.smzdm.com/pc/v1.0/dll/fonts/zdm-icons.2897247.woff2) format("woff2");
        font-weight: 400;
        font-style: normal
    }

    .z-icons, i[class*=" z-icon-"], i[class^=z-icon-] {
        font-family: zdm-icons !important;
        speak: none;
        font-style: normal;
        font-weight: 400;
        font-variant: normal;
        text-transform: none;
        line-height: 1;
        letter-spacing: 0;
        -ms-font-feature-settings: "liga" 1;
        font-feature-settings: "liga";
        font-variant-ligatures: discretionary-ligatures;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    .search-tabs .el-tabs__content {
        overflow: visible;
    }
</style>