<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuwrapper">
      <ul>
        <li v-for="(item, index) in goods" class="menu-item" :class="{current: currentIndex === index}" @click="selectMenu(index, $event)">
          <span class="text border-1px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{ item.name }}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodswrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{ item.name }}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="selectFood(food, $event)">
              <div class="icon">
                <img  width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{ food.name }}</h2>
                <p class="desc">{{ food.description }}</p>
                <div class="extra">
                  <span>月售{{ food.sellCount }}份</span>
                  <span>好评率{{ food.rating }}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{ food.price }}</span>
                  <span class="old" v-show="food.oldPrice">￥{{ food.oldPrice }}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cart-control :food="food"></cart-control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopping-cart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopping-cart>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  import shoppingCart from '@/components/shoppingcart/shoppingCart'
  import cartControl from '@/components/cartcontrol/cartControl'
  import Food from '@/components/food/Food'

  // const ERR_OK = 0

  export default {
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      shoppingCart,
      cartControl,
      Food
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuwrapper, {
          click: true
        })

        this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
          click: true,
          probeType: 3
        })

        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },

      _calculateHeight () {
        let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook')
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      },
      selectMenu (index, event) {
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook')
        let element = foodList[index]
        this.foodsScroll.scrollToElement(element, 300)
        // console.log(index)
      },
      selectFood (food, event) {
        if (!event._constructed) {
          return
        }
        this.selectedFood = food
        this.$refs.food.show()
      }
    },
    created () {
      // this.$axios.get('/api/goods').then((response) => {
      //   if (response.data.errno === ERR_OK) {
      //     this.goods = response.data.data
      //     this.$nextTick(() => {
      //       this._initScroll()
      //       this._calculateHeight()
      //       // console.log(this.listHeight)
      //       // console.log(this.scrollY)
      //     })
      //     // console.log(this.goods)
      //   }
      // })
      this.$axios.get('https://www.easy-mock.com/mock/59bc021ee0dc663341ac13a5/eleme/goods').then((response) => {
        this.goods = response.data.goods
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
          // console.log(this.listHeight)
          // console.log(this.scrollY)
        })
        // console.log(this.goods)
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  }
</script>

<style lang="scss" scoped>
@import "../../common/scss/mixin";

.goods {
  display: flex;
  position: absolute;
  top: 174px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
  .menu-wrapper {
    flex: 0 0 80px;
    width: 80px; //兼容
    background: #f3f5f7;
    .menu-item {
      display: table;
      width: 56px;
      height: 54px;
      padding: 0 12px;
      line-height: 14px;
      &.current {
        position: relative;
        z-index: 10;
        margin-top: -1px;
        background: #fff;
        font-weight: 700;
        .text {
          @include border-none();
        }
      }
      .icon {
        display: inline-block;
        vertical-align: top;
        width: 12px;
        height: 12px;
        margin-right: 2px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        &.decrease {
          @include bg-image('decrease_3');
        }
        &.discount {
          @include bg-image('discount_3');
        }
        &.guarantee {
          @include bg-image ('guarantee_3');
        }
         &.invoice {
          @include bg-image('invoice_3');
        }
        &.special {
          @include bg-image('special_3');
        }
      }
      .text {
        display: table-cell;
        width: 56px;
        vertical-align: middle;
        @include border-1px(rgba(7, 17, 27, 0.1));
        font-size: 12px;
      }
    }
  }
  .foods-wrapper {
    flex: 1;
    .title {
      padding-left: 14px;
      height: 26px;
      line-height: 26px;
      border-left: 2px solid #d9dde1;
      font-size: 12px;
      color: rgb(147, 153, 159);
      background: #f3f5f7; 
    }
    .food-item {
      display: flex;
      margin: 18px;
      padding-bottom: 18px;
      @include border-1px(rgba(1, 17, 27, 0.1));
      &:last-child {
        @include border-none();
        margin-bottom: 0;
      }
      .icon {
        flex: 0 0 57px;
        margin-right: 10px;
      }
      .content {
        flex: 1;
        .name {
          margin: 2px 0 8px 0;
          height: 14px;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7, 17, 27);
        }
      }
      .desc {
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
      .extra {
        line-height: 10px;
        font-size: 10px;
        color: rgb(147, 153, 159);
        .count {
          margin-right: 12px;
        }
      }
      .price {
        font-weight: 700;
        line-height: 24px;
        .now {
          margin-right: 8px;
          font-size: 14px;
          color: rgb(240, 20, 20);
        }
        .old {
          text-decoration: line-through;
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
      }
      .cartcontrol-wrapper {
        position: absolute;
        right: 0;
        bottom: 12px;
      }
    }
  }
}
</style>