<template>
  <div>
     <div class="mask" v-if="isApp"></div>
       <div class="payment-wrap" v-if="isApp">
         <h2>选择支付方式 <span class="close-pay" @click="closeIcon"></span></h2>
          <p>请在30分钟内完成支付 金额
              <b v-if="type==1">¥{{telPrice}}</b>
              <b v-if="type==2 && flowType==1">¥{{countryPrice}}</b>
              <b v-if="type==2 && flowType==2">¥{{provincePrice}}</b>
          </p>
          <ul class="payment-order-info mb-10 bg-fff">
              <li class="pay-alipay">
                  支付宝
                  <i class="right pay-select" :class="{'on':payType==2}" @click="payType = 2"></i>
              </li>
              <li class="pay-unionPay" >
                 银联支付
              <i class="right pay-select" :class="{'on':payType==1}" @click="payType = 1"></i>
              </li>
          </ul>
          <div class="payment-btn">
            <payBar :isSuccess='0' :orderId="orderId" :payChannel="payType" fromPage="tel" :payPrice="payPrice"></payBar>
          </div>
      </div>
  </div>
</template>

<script>
import payBar from '@/components/payBar'
export default {
  name:"payBox",
  data () {
     return {
      payType:2,
      isApp:this.GetQueryString('isApp')
     }
  },
  props:['telPrice','type','flowType','countryPrice','provincePrice','orderId','payPrice'],
  components:{
    payBar
  },
  methods:{
    // url里面拿需要的参数
  GetQueryString (name) {
    var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i')
    var r = window.location.search.substr(1).match(reg)
    if (r != null) return r[2]
    return null
  },
   closeIcon (){
     this.$emit("close");
   }
  }
}
</script>

<style scoped lang="less">
  .payment-wrap {

  position: fixed;
  bottom: 0;
  left: 0;
  z-index: 20;
  width: 100%;
  background: #fff;
  overflow: hidden;
  h2 {
    height: 37px;
    line-height: 37px;
    font-size: 16px;
    background: #f0f0f0;
    text-align: center;
    position: relative;
    .close-pay {
      width: 15px;
      height: 15px;
      position: absolute;
      top: 50%;
      right: 10px;
      margin-top: -6px;
      background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC0AAAAtBAMAAADINP+pAAAAHlBMVEUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAC3KG9qAAAACXRSTlMAAZsCoEpGodmnQymXAAAAt0lEQVQ4y7XTsQrCMBhF4UpfIBR0d3RzKDiLk5sODm66+A6Ojl2LS95WuF64hDM3S0j4TuGHplt+ra/tub8V7fe5NPdjffzv67bhZ98PVUG4j6udgnCdHJAnCEcQniAcQXiCcAThTWCOwByBOQJzBJM4A3GsfqqfnLJGeo+q75PPAwONqqHJi4Ym1wxHcs3w3ZMrOJE7AEdwEEfQv8URmCcgTxCeoPD/V3DBe1Hgd7F5akvw6pZfP4OcUtafZMLsAAAAAElFTkSuQmCC) no-repeat;
      background-size: 100%;
    }
  }
  p {
    font-size: 13px;
    text-align: center;
    padding: 11px 0;
    b {
      color: #f00;
    }
  }
  .payment-btn {
    width: 92%;
    margin: 15px auto;
    height: 40px;
    line-height: 40px;
    text-align: center;
    color: #fff;
    font-size: 16px;
    background: #F1002D;
    border-radius: 4px;
  }
}



.payment-order-info {padding: 0 10px;
  li {
    padding: 12px 0;
    & + li {
      border-top: 1px solid #f0f0f0;
    }
  }
}
.pay-unionPay:before, .pay-alipay:before, .pay-wx:before {
  content: '';
  display: inline-block;
  width: 26px;
  height: 26px;
  margin: 0 5px;
  background: no-repeat center;
  background-size: contain;
  vertical-align: middle;
}
.pay-unionPay {
  &:before {
    background-image: url(data:image/png;base64,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);
  }
}
.pay-alipay {
  &:before {
    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAA0CAMAAADypuvZAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAB3VBMVEUAAAAAoOkAoOkAoOkAoOkAoOkAoOkAoOkAoOkAoOkAoOkQpuogrOweq+wSp+v2/P7////w+f7Q7fuA0PQaqus8tu7y+v4or+xAuO9YwfH0+/4MpOqg3Pf5/f4KpOqM1PXg8/zq9/285vl/z/R0y/PT7/sfrOwvsu3C6PoEoekdq+wwsu0bqusIo+qS1vZlxvJ6zfTK6/r6/f/k9f2+5/mP1fVZwfEhrOwOperu+f4LpOoTp+uf2/f8/v9WwPB4zfOI0/U3tO7l9f265flsyPIrsO3s+P33/P4Zqesjrezr+P295vlOvfA5te5Ju+9jxfKQ1vXJ6/r9/v8BoOnG6vqN1fVfw/G55fn+//8krexPvfCt4PhQvvAGouq35PkNpeqX2PY6tu71+/4Jo+o+t+7d8vyB0PQssO3B6Pro9v3Z8fw2tO4tse3A6Pocquvc8vy25PnX8Pyc2vZUv/AUp+vx+v4Ho+q45fkFoukCoelNvfDE6fra8fxvyfM9t+6K0/WH0vUmruyU1/bz+/74/P5yyvNixfHi9Pyh3Pdzy/Pn9v2O1fURpuoVqOsqsO2Z2fZox/IDoelTv/B7zvQxsu1RvvDI6/pLvO9wyvPP7fs4te5Iu+9qx/J3zPMYqeuUN66zAAAACnRSTlMAHITO9M8DffelJJyWDgAAAAFiS0dEEJWyDSwAAAAHdElNRQfgBxIJLC54lq/IAAACNUlEQVRIx2NgAAJGJmYWLiIACysTIwMEsLFzEKMDAjjY2cB6OInXAgKcIF3spOnh4mIH+ocEt0FdyMjARKoeLi4mBmbsEtw8QMCLXY6ZAXtY8/ELAIEgdk0sDNjFeQTwaOIa5pqEBPACYeppEuFBAFGwOjEkER7uAQ89cQkYkARrkoLzJaRxapLBGQqycjg1yStgAkWwJiVi/IQAymBNKqRpUgVrUiNJkzpYjwae0NPU0lbX0UWR1wNr0selSc3A0AiswtjE1MwcKmghBhKxtMKuydrGFiWMFWTsLIDC9mCOAxdWTY5OmHFj5Ozi6gZiuHtg1eTpJSBg623gI+Hr5x8QGISmW54Lm6bgEIGgUB24GHdYeASSHttIrJqiBIx8UIXVHASR0lB0TCympjgBA4wIildB9qZlQmIwqiZN96RkLPGaguqz1LR0ZE1qAhlY9PBmYoRnlkN2MExTjkAupp68fLC6gkJUfcZFxXxgTSUCpWXoeswhesorKquq0S2sAQeEgkAtmp46SDYKBGdY3+gIVF1gTfUC/KLIWnQbIPFrAsuvjU1SGJp0mgVaWitgWsra2iFyHUiO5u5UQNPE1QZkdXVXNfb09pn1W0JkWiagunjiJA1UTVyTMYJXCSPxcHFNsXFH1sQ1FTVWpk3nwgqs7RWQNHFJz5gJ09FuOosLJ5g41VsAObt7zJ5jP1e/ah4XAeDKQFSjCBWw4GoS4APM5DU+yGrmkNWgIq/pRlYjkYzmKABF4bBB5ML7ugAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAxNi0wNy0xOFQwOTo0NDo0NiswODowMKqKEDcAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMTYtMDctMThUMDk6NDQ6NDYrMDg6MDDb16iLAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAABJRU5ErkJggg==);
  }
}
.pay-wx {
  &:before {
    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE4AAABNCAYAAAAIPlKzAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTdDMUMyNTBBMzZBMTFFNDk0Q0VFNjUzRTA0OTYwMDgiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTdDMUMyNTFBMzZBMTFFNDk0Q0VFNjUzRTA0OTYwMDgiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo1N0MxQzI0RUEzNkExMUU0OTRDRUU2NTNFMDQ5NjAwOCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDo1N0MxQzI0RkEzNkExMUU0OTRDRUU2NTNFMDQ5NjAwOCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PsDv8+cAABCjSURBVHja7FwJeFTl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);
  }
}
.pay-select {
  width: 20px;
  height: 20px;
  margin-top: 5px;
  border: 1px solid #ddd;
  border-radius: 50%;
  box-sizing: border-box;
  float: right;
  &.on {
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAqCAMAAADyHTlpAAAAt1BMVEUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD////+/v5iYmIvLy/q6uojIyPNzc0PDw/6+vrf398pKSn29vbw8PDn5+fj4+Pc3NzX19dpaWlTU1NHR0c2NjYeHh4ZGRkVFRUKCgry8vLs7OzR0dHGxsbDw8NeXl4+Pj52I0mRAAAAHHRSTlMA7TH7+dXPr5uOgDYGLyQI6rY/9PHuPrkJtCYlemfuaAAAAVNJREFUOMul1dl2gjAQBuCJhFXEDdS2E4vazdrNtnZ//+fqaQqEAEng+F+Fw3dBmGQG5ISTwLUJpcR2g0kI6vQ8gqUkXk8BB2OsZTxogEPfwoZY/rAqIwcVcSJZ9keoDOlLcoqaTEs2GqE2o6jYkYOGOPnefDTGz/6nZabWgNMxtsgJr6aWHPLFX409nTy/WmcrDyCMdXLJdpmNQ5jpJWOb7GEGgUHeXmdPAbgtJbpga+WukGgDEe/SmuT7z0KA5suH5SrVSKRAC8kYt0LePWOZFh/wfcm4VUkkYluPwnJ5/4JSbHCxYhUS3XIJnm645fLjFSsJisIWdsurWZM4kY/Lzxvj2VzUZBxWDuF+q5Do1Y72/p2xT1mKow1zLCddfTXJ+X//k6/h+qC4ht0uNyzMLWPRvhF1am/CElPTFDlTt+LTzg3ePDaOHEZixCWUJg0j7hcVU5huZt6y/wAAAABJRU5ErkJggg==) no-repeat;
    background-size: cover;
    border: none;
  }
}
.gray-bg {
  background: #d9d9d9;
}
.mask {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.2);
}
</style>
