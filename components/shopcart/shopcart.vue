<template>
    <div>
        <go-back-header title="购物车"></go-back-header>
        <div class="pay-detail">
            <ul>
                <li class="p-list" v-for="(item,index) in viewProds" :key="item.id">
                    <mt-switch v-model="item.isSelected"></mt-switch>
                    <img :src="item.thumb_path">
                    <div class="pay-calc">
                        <p>{{item.title}}</p>
                        <div class="calc">
                            <span>￥{{item.sell_price}}</span>
                            <span @click="substract(item)">-</span>
                            <span>{{item.num}}</span>
                            <span @click="add(item)">+</span>
                            <a href="javascript:;" @click="del(index)">删除</a>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
        <div class="show-price">
            <div class="show-1">
                <p>总计(不含运费):</p>
                <span>已经选择商品{{computePrice.pickNum}}件，总价￥{{computePrice.totalPrice}}元</span>
            </div>
            <div class="show-2">
                <mt-button type="danger" size="large">去结算</mt-button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {}
    }, created() {
        let prods = this.$store.getters.getProds;
        //判断是否有商品
        if (prods.length === 0) return;
        let url = this.getUrl(prods);
        this.$http.get(url)
            .then(res => {
                let tmp = res.body.message;
                //函数内部完成挂载属性
                tmp = this.convertProds(tmp, prods);
                //将获的最牛的数组数据赋值给store.state
                this.$store.dispatch('saveNewProds', tmp);
            });
    }, methods: {
        substract(p) {
            if (p.num <= 1) return;
            p.num--; //这个属性没有完成setter双向数据绑定
        },
        add(p) {
            p.num++;
        },
        del(index) { //删除数据
            //window对象可以直接用
            if (window.confirm('亲，您确定要删除吗？')) {
                this.$store.dispatch('delProd', index);
            }
        },
        getUrl(prods) {
            let url = this.config.apiPath + '/api/goods/getshopcarlist/';
            prods.forEach(function(p) {
                url += p.id + ','
            });
            return url.substring(0, url.length - 1);
        },
        convertProds(tmp, prods) {
            for (var i = tmp.length - 1; i >= 0; i--) {
                for (var j = prods.length - 1; j >= 0; j--) {
                    if (tmp[i].id == prods[j].id) {
                        //是同一个商品,完成双向数据绑定
                        tmp[i].num = prods[j].num;
                        tmp[i].isSelected = false;
                    }
                }
            }
            return tmp;
        }
    }, computed: {
        computePrice() {
            return this.$store.getters.getInfo;
        },
        viewProds() {
            return this.$store.getters.getProds;
        }
    }
}
</script>
<style scoped>
.pay-detail ul li {
    list-style: none;
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    margin-bottom: 3px;
}

.pay-detail ul {
    padding-left: 5px;
    margin-top: 5px;
}

.pay-detail ul li img {
    width: 80px;
    height: 80px;
    display: inline-block;
    vertical-align: top;
    margin-top: 10px;
}

.pay-detail ul li >:nth-child(1),
.pay-detail ul li >:nth-child(3) {
    display: inline-block;
}

.pay-calc p {
    display: inline-block;
    width: 250px;
    overflow: hidden;
    color: blue;
    font-size: 15px;
    margin-bottom: 10px;
}

.pay-detail ul li >:nth-child(1) {
    line-height: 80px;
}

.calc:nth-child(1) {
    color: red;
    font-size: 20px;
}

.calc span:not(:nth-child(1)) {
    border: 1px solid rgba(0, 0, 0, 0.3);
    display: inline-block;
    width: 20px;
    text-align: center;
}

.calc a {
    margin-left: 20px;
}

.show-1,
.show-2 {
    display: inline-block;
}

.show-1,
.show-2 {
    margin-left: 30px;
}

.show-price {
    background-color: rgba(0, 0, 0, 0.2);
}
</style>
