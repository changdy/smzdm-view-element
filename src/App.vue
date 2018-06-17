<template>
    <div>
        <el-row>
            <el-col :span="12" :offset="4">
                <el-tabs type="border-card" @tab-click="handleSwitch" class="search-tabs">
                    <el-tab-pane label="API搜索">
                        <el-form :inline="true">
                            <category
                                    :items="items"
                                    :multiple="true"
                                    @update-value="getSelectInfo"
                            />
                            <el-form-item label="点值">
                                <el-input v-model.number="apiSearch.worthy"/>
                            </el-form-item>
                            <el-form-item label="评论">
                                <el-input v-model.number="apiSearch.comments"/>
                            </el-form-item>
                            <el-form-item label="页码">
                                <el-input v-model.number="apiSearch.pages"/>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" icon="el-icon-search" @click="search('api')">查询</el-button>
                            </el-form-item>
                        </el-form>
                    </el-tab-pane>
                    <el-tab-pane label="数据库搜索">
                        <el-form :inline="true">
                            <el-form-item label="目录">
                                <el-input v-model="dbSearch.category" readonly/>
                            </el-form-item>
                            <el-form-item label="标题">
                                <el-input v-model="dbSearch.category"/>
                            </el-form-item>
                            <el-form-item label="点值">
                                <el-input v-model.number="dbSearch.worthy"/>
                            </el-form-item>
                            <el-form-item label="评论">
                                <el-input v-model.number="dbSearch.comments"/>
                            </el-form-item>
                            <el-form-item label="时间">
                                <el-date-picker
                                        value-format="yyyy-MM-dd"
                                        v-model="dbSearch.dateRange"
                                        type="daterange"
                                        range-separator="至"
                                        start-placeholder="开始日期"
                                        end-placeholder="结束日期">
                                </el-date-picker>
                            </el-form-item>
                            <el-form-item>
                                <el-button type="primary" icon="el-icon-search" @click="search('db')">查询</el-button>
                            </el-form-item>
                        </el-form>
                    </el-tab-pane>
                </el-tabs>
            </el-col>
        </el-row>
        <el-row style="margin-top: 20px">
            <el-col :span="18" :offset="4" class="z-clearfix">
                <article-list :items="articleList"/>
            </el-col>
        </el-row>
        <el-row v-show="currentModel === 'db'">
            <el-col :span="6" :offset="8">
                <el-pagination
                        @size-change="handleSizeChange"
                        @current-change="handleCurrentChange"
                        :current-page.sync="dbSearch.pageIndex"
                        :page-sizes="[50, 100,200]"
                        :page-size="dbSearch.pageSize"
                        layout="total, sizes, prev, pager, next, jumper"
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

    export default {
        components: {
            articleList,
            category
        },
        methods: {
            handleSizeChange(val) {
                this.pageSize = val;
            }, handleCurrentChange(val) {
                // todo 触发下一页
            }, handleSwitch(tab, event) {
            }, search(value) {
                if (value === 'api') {
                    this.currentModel = 'api';
                } else {
                    this.currentModel = 'db';
                }
            }, getSelectInfo(secondList, thirdList) {
                // todo 处理下层事件
            }
        }, data() {
            let item = JSON.parse(window.localStorage.category);
            let itemMap = JSON.parse(window.localStorage.categoryItem);
            return {
                currentModel: 'api',
                apiSearch: {
                    category: "",
                    worthy: 0,
                    comments: 0,
                    pages: 2
                },
                dbSearch: {
                    dateRange: [],
                    category: "",
                    worthy: 0,
                    comments: 0,
                    pageIndex: 1,
                    pageSize: 100
                },
                total: 500,
                articleList: [],
                items: item,
                itemMap: itemMap
            };
        },

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