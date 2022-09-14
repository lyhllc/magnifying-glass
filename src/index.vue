<script setup name="GoodsImage">
  import { ref, computed } from 'vue';
  import { useMouseInElement } from '@vueuse/core';

  const target = ref(null);
  const props = defineProps({
    mianImages: String,
  });
  // isOutside是否进入指定区域 进入为false 否则为true
  // elementX 鼠标X位置
  // elementY 鼠标Y位置
  const { isOutside, elementX, elementY } = useMouseInElement(target); // useMouseInElement(指定的区域)鼠标进入的位置
  
  const images = computed(() => props.mianImages);
  const position = computed(()=>{
    let x = elementX.value - 100;
    let y = elementY.value - 100;
    x = x < 0 ? 0 : x;
    y = y < 0 ? 0 : y;
    x = x > 200 ? 200 : x;
    y = y > 200 ? 200 : y;
    return {
      x,
      y,
    };
  });
  </script>
  
  <template>
    <!-- {{isOutside}}
       X: {{elementX}}
       Y: {{elementY}} -->
    <div class="goods-image">
  
      <!-- 显示在右侧的放大之后的区域 -->
      <div class="large"
           v-show="!isOutside"
           :style="[{backgroundImage:'url('+images+')', backgroundPosition: `-${position.x*2}px -${position.y*2}px`}]"></div>
  
      <div class="middle" ref="target">
        <img :src="images" alt="" />
        <!-- 移动遮罩 -->
        <div class="layer" ref="target" v-show="!isOutside " :style="{ left:position.x+'px', top: position.y+'px' }"></div>
      </div>
  
    </div>
  </template>
  
    <style scoped lang="scss">
    .goods-image {
      width: 100%;
      height: 100%;
      position: relative;
      display: flex;
      z-index: 10;
      .large {
        position: absolute;
        top: 0;
        left: 412px;
        width: 100%;
        height: 100%;
        box-shadow: 0 0 10px rgba(0,0,0,0.1);
        background-repeat: no-repeat;
        // 放大一倍
        background-size: 200% 200%;
        background-color: #f8f8f8;
      }
      .middle {
        width: 100%;
        height: 100%;
        background: #f5f5f5;
        position: relative;
        cursor: move;
        img{
          width: 100%;
          height: 100%;
          object-fit: cover;
        }
        .layer {
          width: 50%;
          height: 50%;
          background: rgba(0,0,0,.2);
          left: 0;
          top: 0;
          // 可以移动
          position: absolute;
        }
      }
      // .small {
      //   width: 80px;
      //   li {
      //     width: 68px;
      //     height: 68px;
      //     margin-left: 12px;
      //     margin-bottom: 15px;
      //     cursor: pointer;
      //     &:hover,
      //     &.active {
      //       border: 2px solid red;
      //     }
      //   }
      // }
    }
    </style>
  