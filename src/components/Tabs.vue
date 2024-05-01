<template>
  <div class="custom-tab " :class="{ active: isActive }" @click="selectTab">
    <slot></slot>
  </div>
</template>
  
  <script setup>
import { ref, watch, defineProps, defineEmits } from "vue";

const props = defineProps({
  value: String, // Unique identifier for the tab
  activeTab: String, // The currently active tab
});

const emit = defineEmits(["tabSelected"]);

const isActive = ref(false);

// Watch for changes in the activeTab prop
watch(
  () => props.activeTab,
  (newValue) => {
    isActive.value = newValue === props.value;
  }
);

// Method to emit the tabSelected event
const selectTab = () => {
  emit("tabSelected", props.value);
};
</script>
  
  <style scoped>
.custom-tab {
  display: inline;
  cursor: pointer;
  padding: 10px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.custom-tab.active {
  background-color: #007bff;
  color: #fff;
}

.tab-content {
  margin-top: 20px;
}

.custom-tab {
  cursor: pointer;
  padding: 10px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.custom-tab.active {
  background-color: #007bff;
  color: #fff;
}
</style>
  