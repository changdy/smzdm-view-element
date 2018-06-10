<template>
    <div class="dropdown-con" @click.stop v-show="showState">
        <ul class="drop-cate-list">
            <li v-for="(item,index) in items" :data-id="item.id" @mouseenter="hover(index)" :class="{ active: index === showIndex }" :key="item.id">
                <a href="javascript:">
                    <span class="z-icons" :class="'z-icon-' + item.urlNicktitle"></span> {{item.title}}
                </a>
            </li>
        </ul>
        <div class="dropdown-r">
            <div v-for="(tempItem,index) in items" class="drop-cate-wrap" v-show="index === showIndex">
                <table class="drop-menus">
                    <tbody>
                    <tr v-for="children in tempItem.child" :key="children.id">
                        <td class="drop-menu-title"
                            @click="selectSecond(children.id)"
                            :class="{ 'category-selected': selectAll.includes(children.id)}">
                            <a href="javascript:" :data-id="children.id">{{children.title}}</a><span class="split"></span>
                        </td>
                        <td class="drop-menu-items">
                            <a href="javascript:"
                               :key="nodes.id"
                               :data-id="nodes.id"
                               v-for="nodes in children.child"
                               @click="selectThird(children.id,nodes.id)"
                               :class="{ 'category-selected': selectAll.includes(nodes.id)}">{{nodes.title}}</a>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['items', 'multiple', 'showState', 'itemMap'],
        data() {
            return {
                showIndex: 0,
                secondList: [],
                thirdList: []
            }
        },
        methods: {
            hover(index) {
                this.showIndex = index;
            },
            selectSecond(id) {
                let secondArr = this.secondList;
                let thirdArr = this.thirdList;
                if (this.multiple) {
                    if (secondArr.includes(id)) {
                        secondArr.splice(secondArr.findIndex(item => item === id), 1);
                    } else {
                        secondArr.push(id);
                        this.itemMap[id].forEach(x => {
                            thirdArr.splice(thirdArr.findIndex(item => item === x), 1);
                        })
                    }
                } else {
                    secondArr.splice(0, 1, secondArr[0] === id ? 0 : id)
                }
            },
            selectThird(parentId, id) {
                let secondArr = this.secondList;
                let thirdArr = this.thirdList;
                if (this.multiple) {
                    thirdArr.push(id);
                    if (secondArr.includes(parentId)) {
                        // 如果之前就已经全选了 那就删除掉
                        secondArr.splice(secondArr.findIndex(item => item === parentId), 1);
                    } else if (this.itemMap[parentId].every(x => thirdArr.includes(x))) {
                        secondArr.push(parentId);
                        this.itemMap[parentId].forEach(x => {
                            thirdArr.splice(thirdArr.findIndex(item => item === x), 1);
                        })
                    }
                } else {
                    thirdArr.splice(0, 1, thirdArr[0] === id ? 0 : id);
                }
            }
        },
        computed: {
            selectAll() {
                console.log(this.secondList);
                console.log(this.thirdList);
                let tempArr = this.secondList.concat(this.thirdList);
                this.secondList.forEach(x => {
                    tempArr = tempArr.concat(this.itemMap[x]);
                });
                return tempArr;
            }
        }
    }
</script>

<style scoped>
    .category-selected a {
        color: #f04848 !important;
    }

    .dropdown-con {
        position: absolute;
        width: 830px;
        background-color: #fff;
        -webkit-box-shadow: 1px 2px 10px rgba(0, 0, 0, .1);
        box-shadow: 1px 2px 10px rgba(0, 0, 0, .1);
        border: 1px solid #e6e6e6;
        overflow: hidden;
        z-index: 99;
        margin-left: 20px;
    }

    .dropdown-con tr, .dropdown-con td, .dropdown-con li {
        line-height: normal;
    }

    .dropdown-con .dropdown-r {
        overflow: hidden
    }

    .dropdown-con .dropdown-r .drop-cate-wrap {
        padding: 10px 20px;
        font-size: 12px
    }

    .dropdown-con .dropdown-r a {
        font-size: 12px
    }

    .drop-cate-list {
        float: left;
        width: 140px;
        padding: 10px 0;
        background-color: #fafafa;
        border-right: 1px solid #e6e6e6
    }

    .drop-cate-list li a {
        position: relative;
        display: inline-block;
        height: 30px;
        width: 100%;
        margin-right: -1px;
        line-height: 30px;
        background-color: #fafafa;
        border-style: solid;
        border-width: 1px 1px 1px 0;
        border-color: #fafafa #e6e6e6;
        text-align: center;
        z-index: 1;
        color: #666
    }

    .drop-cate-list li a span {
        display: inline-block;
        height: 16px;
        width: 16px;
        margin-right: 20px;
        font-size: 16px;
        vertical-align: -2px
    }

    .drop-cate-list li.active a {
        background-color: #fff;
        border-color: #e6e6e6 #fff;
        color: #333
    }

    .drop-cate-list li.active a span {
        color: #676767
    }

    .drop-menus {
        border-collapse: collapse;
        border-spacing: 0;
        border: none
    }

    .drop-menus td {
        vertical-align: top;
        padding: 0
    }

    .drop-menus td.drop-menu-items {
        padding-bottom: 8px;
    }

    .drop-menus td.drop-menu-items a {
        margin-right: 18px;
        margin-bottom: 2px;
        color: #666
    }

    .drop-menus td a {
        display: inline-block;
        height: 12px;
        margin: 10px 0;
        line-height: 12px;
        color: #333
    }

    .drop-menus td a:hover {
        text-decoration: underline
    }

    .drop-menus .drop-menu-title {
        text-align: right;
        white-space: nowrap;
        vertical-align: top;
        font-weight: 700
    }

    .drop-menus .drop-menu-title a {
        width: 72px;
        vertical-align: middle
    }

    .drop-menus .drop-menu-title .split {
        display: inline-block;
        height: 12px;
        width: 0;
        margin: 0 12px;
        border-right: 1px solid #e6e6e6;
        vertical-align: -2px
    }

    .drop-menus td.drop-menu-items .category-selected {
        color: #f04848;
    }
</style>
