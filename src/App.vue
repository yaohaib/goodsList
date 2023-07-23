<template>
  <div class="content">
    <div>
      <el-col style="margin-bottom: 15px;">
        <el-button @click="sortByCategory">
          按分类 <img :src="categoryDes ? '../static/asc.png' : '../static/dec.png'" style="width:15px; height:15px">
        </el-button>
        <el-button @click="sortByDate">
          按日期<img :src="dateDes ? '../static/asc.png' : '../static/dec.png'" style="width:15px; height:15px">
        </el-button>
      </el-col>
      <el-row :gutter="20" v-for="(category) in categories" :key="category.id">

        <el-col :span="24" style="margin-bottom: 15px;">
          <el-text class="headContext" size="large">{{ category.name }}</el-text>
        </el-col>

        <el-col :span="6" v-for="(item, itemIndex) in category.items" :key="itemIndex" :draggable="isDraggable"
          @dragstart="handleDragStart(category, itemIndex, item)" @dragover="handleDragOver"
          @drop="handleDrop(category, itemIndex)" style="text-align:center; margin-bottom: 20px;">

          <el-card shadow="hover">
            <img :src="item.url" class="image" />
            <div class="goodsDes">
              <span>{{ item.name }}</span>
              <div class="bottom">
                <time class="time">{{ item.createdAt }}</time>
              </div>
            </div>
          </el-card>

        </el-col>

        <!-- 添加一个虚拟的占位元素,解决当分类为空后，无法拖入数据问题 -->
        <el-col v-if="category.items.length === 0" :draggable="true" @dragover="handleDragOver(category, 0)"
          @drop="handleDrop(category, 0)" style="text-align:center">
          <el-card shadow="hover">
            <img src="../public/static/nothing.png" class="image" />
            <div style="padding: 14px">
              <span>虚位以待</span>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
// 分类数据
const categories = ref([
  {
    name: "家具",
    id: 1,
    items: [
      {
        id: 1,
        parentId: 1,
        name: "单人沙发小户型懒人沙发椅客厅阳台休闲老虎凳轻奢酒店单椅设计师",
        url: '../static/jiaju-1.jpg',
        createdAt: "2023-07-18 12:00:01",
      },
      {
        id: 2,
        parentId: 1,
        name: "奶油风小户型客厅布艺沙发现代简约网红ins民宿公寓直排双人沙发",
        url: '../static/jiaju-2.jpg',
        createdAt: "2023-07-18 12:00:02",
      },
      {
        id: 3,
        parentId: 1,
        name: "弧形布艺沙发意式极简客厅小户型异形可拆洗奶油风转角羊羔绒沙发",
        url: '../static/jiaju-3.jpg',
        createdAt: "2023-07-18 12:00:03",
      },
      {
        id: 4,
        parentId: 1,
        name: "2023年流行沙发科技布客厅简约现代轻奢小户型茶几组合套装弧形",
        url: '../static/jiaju-4.jpg',
        createdAt: "2023-07-18 12:00:04",
      }
    ],
  },
  {
    name: "食品",
    id: 2,
    items: [{
      id: 1,
      parentId: 2,
      name: "层层拉丝！坚果酥饼干单独小包装网红零食办公室解饿休闲食品小吃",
      url: '../static/food-1.jpg',
      createdAt: "2023-07-19 12:00:01",
    },
    {
      id: 2,
      parentId: 2,
      name: "掌柜推荐 扁桃仁夹心酥 零食软心酥夹心脆牛扎杏仁饼牛轧饼干",
      url: '../static/food-2.jpg',
      createdAt: "2023-07-19 12:00:02",
    },
    {
      id: 3,
      parentId: 2,
      name: "山药坚果沙琪玛低软糯无蔗糖早餐夜宵充饥脂零食散装食品糕点整箱",
      url: '../static/food-3.jpg',
      createdAt: "2023-07-19 12:00:03",
    },
    {
      id: 4,
      parentId: 2,
      name: "零食大礼包整箱送女友生日礼物休闲食品小吃儿童超大巨型包七夕节",
      url: '../static/food-4.jpg',
      createdAt: "2023-07-19 12:00:04",
    }],

  },

  {
    name: "衣服",
    id: 3,
    items: [{
      id: 1,
      parentId: 3,
      name: "女童套装夏装2023夏季新款中大童休闲条纹polo衫短袖牛仔裤两件套",
      url: '../static/clothes-1.jpg',
      createdAt: "2023-07-20 12:00:01",
    },
    {
      id: 2,
      parentId: 3,
      name: "男童短袖套装2023新款夏装网红小男孩休闲痞帅潮流炸街儿童衣服酷",
      url: '../static/clothes-2.jpg',
      createdAt: "2023-07-20 12:00:02",
    },
    {
      id: 3,
      parentId: 3,
      name: "官网耐克顿儿童运动服套装2023夏季新款款韩版女童短袖短裤两件套",
      url: '../static/clothes-3.jpg',
      createdAt: "2023-07-20 12:00:03",
    },
    {
      id: 4,
      parentId: 3,
      name: "韩国TY童装潮牌男童polo衫夏装2023新款儿童条纹短袖t恤大童上衣",
      url: '../static/clothes-4.jpg',
      createdAt: "2023-07-20 12:00:04",
    }],

  },

  {
    name: "图书",
    id: 4,
    items: [{
      id: 1,
      parentId: 4,
      name: "彩图注音大开本脑筋急转弯 有声伴读版一二三年级谜语大全小学生课外阅读书籍成语故事接龙益智读物6-9岁儿童智力大挑战的书幼儿园",
      url: '../static/book-1.jpg',
      createdAt: "2023-07-20 12:00:01",
    },
    {
      id: 2,
      parentId: 4,
      name: "少年探索发现宇宙之谜世界未解之谜科学之谜恐龙世界中国之谜中小学生三四五六年级课外读物青少年儿童科普大百科全书十万个为什么",
      url: '../static/book-2.jpg',
      createdAt: "2023-07-20 12:00:02",
    },
    {
      id: 3,
      parentId: 4,
      name: "精装 福尔摩斯探案全集 正版全套 原著中文版 无删减 硬壳柯南道尔侦探悬疑推理小说世界名著青少年小学生课外阅读书籍正版夏洛克",
      url: '../static/book-3.jpg',
      createdAt: "2023-07-20 12:00:03",
    },
    {
      id: 4,
      parentId: 4,
      name: "正版全套50册图书清仓特价书籍团队管理书籍按斤卖鬼谷子狼道墨菲定律断舍离说话技巧励志书籍推荐书籍批发",
      url: '../static/book-4.jpg',
      createdAt: "2023-07-20 12:00:04",
    }],

  },

  {
    name: "运动",
    id: 5,
    items: [{
      id: 1,
      parentId: 5,
      name: "SEAN LEE腹肌家用健身器材健腹器减瘦肚子腹部训练神器收腹卷腹机",
      url: '../static/sport-1.jpg',
      createdAt: "2023-07-21 12:00:01",
    },
    {
      id: 2,
      parentId: 5,
      name: "肘撑自动健腹轮回弹卷腹轮腹肌轮男女收腹瘦肚子神器运动健身器材",
      url: '../static/sport-2.jpg',
      createdAt: "2023-07-21 12:00:02",
    },
    {
      id: 3,
      parentId: 5,
      name: "SEAN LEE健腹轮自动回弹卷腹轮练腹肌神器瘦肚子肘撑家用健身器材",
      url: '../static/sport-3.jpg',
      createdAt: "2023-07-21 12:00:03",
    },
    {
      id: 4,
      parentId: 5,
      name: "COLORFIT万向轮健腹盘滑盘健身静音腹肌男女家用健身收腹核心力量",
      url: '../static/sport-4.jpg',
      createdAt: "2023-07-21 12:00:04",
    }],

  },
]);
const isDraggable = ref(true)
const categoryDes = ref(true) // 为真升序 为false倒序
const dateDes = ref(true) // 为真升序 为false倒序
let draggedData = null;

// 按分类排序
const sortByCategory = () => {
  isDraggable.value = true
  categoryDes.value = !categoryDes.value
  categoryDes.value ? categories.value.sort((a, b) => a.id - b.id) : categories.value.sort((a, b) => b.id - a.id)
}

// 按日期排序
const sortByDate = () => {
  isDraggable.value = false
  dateDes.value = !dateDes.value
  categories.value.forEach((element) => {
    dateDes.value ? element.items.sort((a, b) => new Date(a.createdAt) - new Date(b.createdAt)) :
      element.items.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
  })
}

// 在拖拽开始时设置数据传输的数据类型和数据
const handleDragStart = (category, itemIndex, item) => {
  draggedData = { category, itemIndex, item };
}

// 阻止默认行为，使元素可拖放
const handleDragOver = () => {
  if (!isDraggable.value) return
  event.preventDefault();
  event.dataTransfer.dropEffect = "move";
}



const handleDrop = (targetCategory, targetItemIndex) => {
  // 获取拖拽的数据
  console.log(draggedData)
  const { category, itemIndex, item } = draggedData;
  
  if (targetCategory.items.length === 0) {
    if (category.items === undefined) {
      category.items = [];
    }
    targetCategory.items.push(item);
    category.items.splice(itemIndex, 1);
  } else {
    // 判断拖拽元素与放置的目标元素是否时同一分类
    if (category === targetCategory) {
      category.items.splice(itemIndex, 1, ...category.items.splice(targetItemIndex, 1, category.items[itemIndex]))
    } else {
      targetCategory.items.splice(targetItemIndex, 0, category.items[itemIndex]);
      category.items.splice(itemIndex, 1);
    }
  }
}
</script>

<style lang="scss">
.content {
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;

  .headContext {
    font-size: 25px !important;
    font-weight: bold;
  }

  .image {
    width: 150px;
    height: 150px;
  }

  .goodsDes {
    padding: 20px;
    white-space: nowrap;
    font-family: PingFangSC-Regular;
    font-size: 14px;
    line-height: 20px;
    padding-top: 10px;
    height: 40px;
    white-space: normal;
    color: #443f3f;

    span {
      overflow: hidden;
      text-overflow: ellipsis;
      /* 超出部分省略号 */
      word-break: break-all;
      /* 设置省略字母数字 */
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      /* 显示的行数 */
    }



    .bottom {
      margin-top: 5px;
      color: #9b9b9b;

    }
  }

}

.el-row {
  margin-bottom: 20px;
}

.el-row:last-child {
  margin-bottom: 0;
}

.el-card {
  width: 21rem !important;
}
</style>
