# 一个图片放大镜功能

/*
  @@@ imageZoomUrl 图片的url
*/
<template>
    <div class="image-content" ref='imageContent' @mousemove='moveIn'>  <!-- :style="contentStyle" -->
        <img :src="imageZoomUrl" alt=""  style='width:100%;height:100%'>
        <div v-if='moveEaraShow'   class="image-zoom-eara" :style="[moveEaraStyle,bgZoom]"></div>
    </div>
</template>
 
<script>
export default {
    name:'magnifier',
    data() {
        return {
            moveEaraShow:false, // 显示/隐藏图片放大区域
            moveEaraStyle:{
                top:'0px',
                left:'0px',
                backgroundPosition:"0px 0px"
            },
            scaleX:0,
            scaleY:0,
        };
    },
    props:{
        imageZoomUrl:{
            type:String
        },
    },
    computed:{
        bgZoom(){
            return {backgroundImage:"url("+this.imageZoomUrl+")"}
        }
    },
    methods:{
        moveIn(e){
            var x_page=e.pageX
            var y_page=e.pageY
            //获取当前div的位置信息
            var rect=this.$refs.imageContent.getBoundingClientRect()
            //鼠标位置限制在容器区域内
            if((x_page>rect.left&&x_page<rect.right)&&(y_page>rect.top&&y_page<rect.bottom)){
                this.$set(this.moveEaraStyle,'left',x_page-rect.left-70+'px')  //0
                this.$set(this.moveEaraStyle,'top',y_page-rect.top-70+'px')  //20
                this.$set(this.moveEaraStyle,'backgroundPosition',(70-(x_page-rect.left)*this.scaleX)+"px "+(70-(y_page-rect.top)*this.scaleY)+"px")  //20
                this.moveEaraShow=true
            }else{
                this.moveEaraShow=false
            }
        },
        imageContent(val){
            var img=new Image()
            var self=this
            img.src=val
            img.onload=function(){
                // console.log(img.width,img.height)
                self.scaleX=img.width/350
                self.scaleY=img.height/260
            }
        }
    },
    watch:{
        imageZoomUrl(val){
            this.imageContent(val)
        }
    },
    mounted(){
        this.imageContent(this.imageZoomUrl)
    },
    components:{}
    
}
</script>
<style scoped lang='scss'>
    .image-content{
        position: relative;
        box-sizing: border-box;
        width:350px;
        height:260px;
        & .image-zoom-eara{
            width:140px;
            height:140px;
            position: absolute;
            /*cursor: crosshair;*/
            background-repeat: no-repeat;
            z-index: 100000;
            border-radius: 50%;
            box-shadow: 1px 3px 6px black;
        }
    }
</style>
