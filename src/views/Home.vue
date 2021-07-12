<template>
  <div>
    <van-row>
      <van-col span="24">
        <van-tabs @click="onClick" sticky title-active-color="#E32DAB" color="#E32DAB" :line-width="100" :line-height="2">

          <van-tab v-for="index in categories.length" :title="categories[index-1].name" class="tab">

            <van-card v-for="(item,index) in phones"
                      :price="item.price"
                      :desc="item.desc"
                      :title="item.title"
                      :thumb="item.thumb"
            >
              <template #tags>
                <van-tag v-for="tag in item.tag" color="#f2826a" style="margin-left: 5px;">{{tag.name}}</van-tag>
              </template>
              <template #footer>
                <van-button round type="info" size="mini" @click="buy(index)">购买</van-button>
              </template>
            </van-card>

          </van-tab>
        </van-tabs>
      </van-col>
    </van-row>

    <van-sku
        v-model="show"
        :sku="sku"
        :goods="goods"
        :hide-stock="sku.hide_stock"
        @buy-clicked="onBuyClicked"
    >
      <template #sku-actions="props">
        <div class="van-sku-actions">

          <!-- 直接触发 sku 内部事件，通过内部事件执行 onBuyClicked 回调 -->
          <van-button
              square
              size="large"
              type="danger"
              @click="props.skuEventBus.$emit('sku:buy')"
          >
            买买买
          </van-button>
        </div>
      </template>
    </van-sku>
  </div>
</template>

<script>
import {
  Toast,
  PullRefresh,
  Swipe,
  SwipeItem
} from 'vant';
export default {
  comments:{
    [PullRefresh.name]: PullRefresh,
    [Swipe.name]: Swipe,
    [SwipeItem.name]: SwipeItem
  },
  data() {
    return {
      categories: [
        {name: '魅焰红',
         type: 1},
        {name: '极光蓝',
          type: 2},
        {name: '铂光金',
          type: 3},
        {name: '幻夜黑',
          type: 4},
      ],
      "phones": [
        {
          "tag": [
            {
              "name": "720P珍珠屏"
            },
            {
              "name": "Micro USB接口"
            }
          ],
          "id": 1,
          "title": "Honor 8A",
          "price": "2800.00.00",
          "desc": "魅焰红",
          "thumb": "../static/e84a2e03-7f19-41d2-98a5-a5c16b7e252d.jpg"
        },
        {
          "tag": [
            {
              "name": "内存3GB"
            },
            {
              "name": "Micro USB接口"
            }
          ],
          "id": 5,
          "title": "HUAWEI nova 5 Pro",
          "price": "5450.00.00",
          "desc": "魅焰红",
          "thumb": "../static/8a0f5be0-3c78-4f23-b58b-dc2a92f1f95a.jpg"
        },
        {
          "tag": [
            {
              "name": "720P珍珠屏"
            },
            {
              "name": "存储32GB"
            }
          ],
          "id": 9,
          "title": "SAMSUNG G S10",
          "price": "7254.00.00",
          "desc": "魅焰红",
          "thumb": "../static/a4f0cef8-59da-4f7c-abfa-d373f6648035.jpg"
        },
        {
          "tag": [
            {
              "name": "F/1.8光圈"
            },
            {
              "name": "Micro USB接口"
            }
          ],
          "id": 13,
          "title": "VIVO X27",
          "price": "2888.00.00",
          "desc": "魅焰红",
          "thumb": "../static/cdf065ec-e409-4204-93e6-600e172e461a.jpg"
        },
        {
          "tag": [
            {
              "name": "720P珍珠屏"
            },
            {
              "name": "Micro USB接口"
            }
          ],
          "id": 17,
          "title": "Meizu 16s",
          "price": "1220.00.00",
          "desc": "魅焰红",
          "thumb": "../static/1a2b8e30-6e98-405f-9a18-9cd31ff96c35.jpg"
        }
      ],
      show: true,
      sku: '',
      goods: ''
    }
  },
  created(){
    const _this = this
    axios.get('http://localhost:8181/phone/index').then(function (resp) {
      // _this.phones = resp.data.data.phones
      // _this.categories = resp.data.data.categories
    })
  },
  methods: {
    // onClick(index) {
    //   const _this = this
    //   axios.get('http://localhost:8181/phone/findByCategoryType/'+this.categories[index].type).then(function (resp) {
    //     _this.phones = resp.data.data
    //   })
    // },
    buy(index){
      this.show = true
      const _this = this
      axios.get('http://localhost:8181/phone/findSpecsByPhoneId/'+this.phones[index].id).then(function (resp) {
        _this.goods = resp.data.data.goods
        _this.sku = resp.data.data.sku
      })
    },
    onBuyClicked(item){
      console.log(item)
      this.$store.state.specsId = item.selectedSkuComb.s1
      this.$store.state.quantity = item.selectedNum
      this.$router.push('/addressList')
    }
  }
}
</script>
