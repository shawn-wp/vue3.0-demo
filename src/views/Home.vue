<template>
  <div class="home">
    <h1 ref="demo">这是一个vue3.0的测试demo</h1>
    <!-- <h1>X: {{ x }}</h1>
    <h1>Y: {{ y }}</h1>-->
    <button @click="addCount">+</button>
    <h1>{{ count }}</h1>
    <button @click="goAbout">goAbout</button>
  </div>
</template>

<script>
import { ref, reactive, onMounted, onUnmounted, computed, watch } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";

// 获取鼠标位置
function useMousePosition() {
  const x = ref(0);
  const y = ref(0);

  function getMousePosi(e) {
    x.value = e.pageX;
    y.value = e.pageY;
  }

  onMounted(() => {
    document.body.addEventListener("mousemove", getMousePosi);
  });

  onUnmounted(() => {
    document.body.removeEventListener("mousemove", getMousePosi);
  });
  return { x, y };
}

// reactive 数据测试
function useDataTest() {
  const data = reactive({
    test: "测试用数据"
  });

  return { data };
}

// 实现一个计数器
function useCountAdd() {
  const store = useStore();

  const count = computed({
    get() {
      console.log("执行了computed的get");
      return store.state.count;
    },
    set(val) {
      console.log("执行了computed的set");
      store.commit("inc", val);
    }
  });

  function addCount() {
    count.value++;
  }

  watch(count, value => {
    console.log("count is change", value);
  });

  return { count, addCount };
}

// 获取dom节点测试
function useDomTest() {
  const demo = ref(null);
  // 必须把dom操作放在onMounted()中
  onMounted(() => {
    demo.value.style.color = "skyblue";
  });

  return { demo };
}

export default {
  name: "Home",
  setup() {
    // 路由跳转测试
    const router = useRouter();
    function goAbout() {
      router.push("/about");
    }

    // 获取鼠标位置
    // const { x, y } = useMousePosition();

    // reactive 数据测试
    const { data } = useDataTest();

    // 实现一个计数器
    const { count, addCount } = useCountAdd();

    // dom节点测试
    let { demo } = useDomTest();

    return {
      // x,
      // y,
      data,
      count,
      addCount,
      demo,
      goAbout
    };
  }
};
</script>
