<template>
  <div id="LeftNav" class="pos-left-nav">
    <ul>
      <li class="active"><i class="icon iconfont icon-goumai"></i><div>收银</div></li>
      <li><i class="icon iconfont icon-31dianpu"></i><div>店铺</div></li>
      <li><i class="icon iconfont icon-hanbao"></i><div>商品</div></li>
      <li><i class="icon iconfont icon-31huiyuanqia"></i><div>会员</div></li>
      <li><i class="icon iconfont icon-wxbbaobiao"></i><div>统计</div></li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "LeftNav",
  data: () => ({

  })
}
</script>
<style  scoped>
  .pos-left-nav{
    float: left;
    color: #fff;
    font-size: 14px;
    height: 100%;
    width: 6%;
    background-color: #1D8CE0;
  }

  .iconfont{
    font-size: 24px;
  }

  .pos-left-nav ul{
    margin: 0;
    padding: 0;
  }

  .pos-left-nav ul li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #2E9BEC;
    padding: 10px;
    cursor: pointer;
  }

  .pos-left-nav ul li:hover,.pos-left-nav ul li.active{
    background-color: #106CB1;
  }
</style>
