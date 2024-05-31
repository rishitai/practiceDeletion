<script setup>
import { ref } from "vue";
import { MarkerType, VueFlow } from "@vue-flow/core";

const nodes = ref([
  {
    id: "start",
    type: "input",
    deletable: false,
    position: { x: 300, y: 0 },
    label: "Start",
  },
  {
    id: "initiator",
    label: "initiator",
    type: "default",
    position: { x: 300, y: 100 },
    draggable: false,
    deletable: false,
  },
  {
    id: "end",
    type: "output",
    label: "Stop",
    position: { x: 300, y: 400 },
    deletable: false,
  },
]);

const edges = ref([
  {
    id: "first-edge",
    label: "first-edge",
    source: "start",
    target: "initiator",
    type: "straight",
    style: { strokeWidth: 4 },
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
  },
  {
    id: `end-edge`,
    label: "end-edge",
    type: "straight",
    source: `initiator`,
    target: "end",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
  },
]);
</script>

<template>
  <div class="vueflow-container">
    <VueFlow :nodes="nodes" :edges="edges" />
  </div>
</template>
