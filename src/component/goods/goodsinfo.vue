<template>
    <div class="goodsinfo-container">
        <!-- 商品轮播图区域 -->
        <div class="mui-content">
			<div class="mui-card">
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<swiper :lunbotuList="lunboList" :isfull="false"></swiper>
					</div>
				</div>
		</div>
        </div>
        <!-- 商品购买区域 -->
        <div class="mui-card">
				<div class="mui-card-header">{{goodsInfo.title}}</div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<p class="price">
                            市场价:<del>￥{{goodsInfo.market_price}}</del>&nbsp;销售价:{{goodsInfo.sell_price}}<span class="now_price"></span>
                        </p>
                        <p>购买数量:<numbox></numbox></p>
                        <mt-button type="primary" size="small">立即购买</mt-button>
                        <mt-button type="danger" size="small">加入购物车</mt-button>
					</div>
				</div>
				
		</div>

        <!-- 商品参数区域 -->
        <div class="mui-card">
				<div class="mui-card-header">商品参数</div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<p>商品货号:{{goodsInfo.goods_no}}</p>
						<p>库存情况:{{goodsInfo.stock_quantity}}件</p>
						<p>上架时间:{{goodsInfo.add_time|dataFormat}}</p>
					</div>
				</div>
				<div class="mui-card-footer">
                    <mt-button type="primary" size="large" plain @click="getDesc(id)">图文介绍</mt-button>
                    
                    <mt-button type="danger" size="large" plain @click="getComment(id)">商品评论</mt-button>
                </div>
		</div>

    </div>
</template>

<script>
//导入轮播图组件
import swiper from "../subcomponent/swiper.vue"
import numbox from "./goodsinfo_numbox.vue"
export default {
    data(){
        return{
            id:this.$route.params.id,//将路有参数id挂载到id上
            lunboList:[],
            goodsInfo:[]
        }
    },
    created(){
        this.getLunbotu(),
        this.getInfo()

    },
    methods:{
        getLunbotu(){
            
        //获取缩略图
        this.$http.get("api/getthumimages/"+this.id).then(
            function(result){
            result.body.message.forEach(item=>{
                    item.img=item.src
                })
                this.lunboList=result.body.message
            }
        )
    
        },
        getInfo(){
            this.$http.get("api/goods/getinfo/"+this.id).then(
            function(result){
                //先循环轮播图数组每一项 为item增加img属性 因为轮播图组件zhi认识item。img
               
                this.goodsInfo=result.body.message[0]
            }
        )
        },
        getDesc(id){
            //编程式导航 到图文介绍页面
            this.$router.push({name:"goodsdesc",params:{id}})
        },
        getComment(id){
            //编程式导航 到图文介绍页面
            this.$router.push({name:"goodscomment",params:{id}})
        },
    },
    components:{
        swiper,
        numbox
    }
}
</script>

<style lang="scss" scoped>
    .goodsinfo-container{
        background-color: #eee;
        overflow: hidden;
    }
    .now_price{
        color: red;
        font-size: 14px;
        font-weight: bold;
    }
    .mui-card-footer{
        display: block;
        button{
            margin: 15px 0;
        }
    }
</style>

