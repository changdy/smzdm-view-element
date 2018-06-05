<template>
    <div class="content-inner">
        <div id="feed-wrap" class="z-clearfix">
            <article-list :items="items"/>
        </div>
    </div>
</template>

<script>
    // import category from "./component/category.vue";
    import articleList from "./component/article-list.vue";

    export default {
        components: {
            // category,
            articleList
        },
        data() {
            return {
                items: [{
                    "date": "2018-06-04 21:21:32",
                    "mall": "天猫精选",
                    "articleId": 9552316,
                    "channel": "国内",
                    "worthy": 632,
                    "collection": 347,
                    "pic": "http://qny.smzdm.com/201806/03/5b13625915ad17183.jpg_d320.jpg",
                    "title": "绝对值 : Apple 苹果 iPhone 8 智能手机 64GB 全网通 红色特别版",
                    "url": "http://www.smzdm.com/p/9552316",
                    "price": "4398元包邮（下单立减，12期免息）",
                    "comment": 592,
                    "unworthy": 191
                }, {
                    "date": "2018-06-04 21:19:49",
                    "mall": "京东",
                    "articleId": 9529885,
                    "channel": "发现",
                    "worthy": 1,
                    "collection": 0,
                    "pic": "http://qny.smzdm.com/201806/01/5b10855b3c8a27374.jpg_d320.jpg",
                    "title": "洛克（ROCK）车载手机支架 车载无线充电器 *5件",
                    "url": "http://www.smzdm.com/p/9529885",
                    "price": "122.5元（需用券，合24.5元/件）",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 21:19:10",
                    "mall": "天猫精选",
                    "articleId": 9559339,
                    "channel": "国内",
                    "worthy": 147,
                    "collection": 58,
                    "pic": "http://y.zdmimg.com/201712/26/5a41dc2c0afcf7673.jpg_d320.jpg",
                    "title": "绝对值 : Apple 苹果 iPhone X 智能手机 全网通 64GB",
                    "url": "http://www.smzdm.com/p/9559339",
                    "price": "7198元包邮（双重优惠）",
                    "comment": 165,
                    "unworthy": 360
                }, {
                    "date": "2018-06-04 21:09:52",
                    "mall": "京东",
                    "articleId": 9522392,
                    "channel": "国内",
                    "worthy": 1321,
                    "collection": 628,
                    "pic": "http://y.zdmimg.com/201805/25/5b0763fc232098640.jpg_d320.jpg",
                    "title": "绝对值 : vivo Z1 全面屏AI双摄 智能手机 4GB+64GB",
                    "url": "http://www.smzdm.com/p/9522392",
                    "price": "1478元包邮（双重优惠）",
                    "comment": 867,
                    "unworthy": 242
                }, {
                    "date": "2018-06-04 20:58:38",
                    "mall": "天猫精选",
                    "articleId": 9572215,
                    "channel": "国内",
                    "worthy": 2,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201805/31/5b0fb6643ba5c1605.jpg_d320.jpg",
                    "title": "6日0点 : POWER SUPPORT AirJacket iPhone X 超薄手机壳  磨砂全黑",
                    "url": "http://www.smzdm.com/p/9572215",
                    "price": "118.5元包邮（前550件）",
                    "comment": 2,
                    "unworthy": 11
                }, {
                    "date": "2018-06-04 20:53:45",
                    "mall": "京东",
                    "articleId": 9572161,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201806/04/5b15362e0b4557995.jpg_d320.jpg",
                    "title": "飞利浦（PHILIPS）BT2000A 音乐手鼓 无线蓝牙音箱 便携迷你音响 手机/电脑小音响 户外运动 蓝色",
                    "url": "http://www.smzdm.com/p/9572161",
                    "price": "199元",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:52:28",
                    "mall": "京东",
                    "articleId": 9572153,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201806/04/5aa9cc7183c811524.jpg_d320.jpg",
                    "title": "先锋（Pioneer）SEC-CL31入耳式立体声HIFI音乐手机耳机 金 *5件",
                    "url": "http://www.smzdm.com/p/9572153",
                    "price": "245元（合49元/件）",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:50:50",
                    "mall": "天猫精选",
                    "articleId": 9571958,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201806/04/5b15319d814666917.jpg_d320.jpg",
                    "title": "芥末绿车载手机架支架导航架吸盘式出风口汽车用通用多功能",
                    "url": "http://www.smzdm.com/p/9571958",
                    "price": "5.8元（需用券）",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:40:58",
                    "mall": "京东",
                    "articleId": 9572027,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 1,
                    "pic": "http://y.zdmimg.com/201805/18/5afe7093ed88d2467.jpg_d320.jpg",
                    "title": "漫步者（EDIFIER） H180 手机耳机 立体声耳塞 白色 *9件",
                    "url": "http://www.smzdm.com/p/9572027",
                    "price": "224元（合24.89元/件）",
                    "comment": 4,
                    "unworthy": 2
                }, {
                    "date": "2018-06-04 20:39:47",
                    "mall": "京东",
                    "articleId": 9572010,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201806/04/5a4c32d027d35295.jpg_d320.jpg",
                    "title": "纽曼 NM-XK07 防缠绕音乐手机耳机 红色 *16件",
                    "url": "http://www.smzdm.com/p/9572010",
                    "price": "218.4元（合13.65元/件）",
                    "comment": 2,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:37:46",
                    "mall": "京东",
                    "articleId": 9571988,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 0,
                    "pic": "http://y.zdmimg.com/201806/04/5b10fe4b36aab9840.jpg_d320.jpg",
                    "title": "LESAILES 飞遁 6001吃鸡王座 手机游戏支架",
                    "url": "http://www.smzdm.com/p/9571988",
                    "price": "139元",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:35:46",
                    "mall": "京东",
                    "articleId": 9571962,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 1,
                    "pic": "http://y.zdmimg.com/201805/18/5afe7099823ec3284.jpg_d320.jpg",
                    "title": "漫步者（EDIFIER） H180 手机耳机 立体声耳塞 黑色 *8件",
                    "url": "http://www.smzdm.com/p/9571962",
                    "price": "212元（合26.5元/件）",
                    "comment": 0,
                    "unworthy": 0
                }, {
                    "date": "2018-06-04 20:24:23",
                    "mall": "京东",
                    "articleId": 9571855,
                    "channel": "发现",
                    "worthy": 0,
                    "collection": 1,
                    "pic": "https://y.zdmimg.com/files/9e0ca1bfa761216c032d4aed016e42b4.jpg_d320.jpg",
                    "title": "Msvii 摩斯维 苹果/iphone7plus/8 plus 手机壳",
                    "url": "http://www.smzdm.com/p/9571855",
                    "price": "10元包邮（需用券）",
                    "comment": 0,
                    "unworthy": 0
                }]
            };
        },

    }
</script>
<style>
    .content-inner {
        width: 1200px;
        margin: 0 auto;
    }

    #feed-wrap {
        padding-bottom: 0;
        margin-bottom: 28px;
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
</style>