<script setup>
import { ref } from "vue";
import { MarkerType, VueFlow } from "@vue-flow/core";
import CustomNode from "./custom_nodes/CustomNode.vue";

const nodes = ref([
  {
    id: "start",
    type: "input",
    position: { x: 300, y: -100 },
    label: "Start",
  },
  {
    id: "node-0.5",
    label: "node-0.5",
    type: "custom",
    position: { x: 300, y: 0 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "node-1",
    label: "node-1",
    type: "custom",
    position: { x: 300, y: 100 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "1.1",
    type: "custom",
    label: "1.1",
    position: { x: 220, y: 200 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "1.2",
    type: "custom",
    label: "1.2",
    position: { x: 420, y: 200 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "h-1",
    type: "custom",
    label: "h-1",
    position: { x: 300, y: 300 },
  },
  {
    id: "2.1",
    type: "custom",
    label: "2.1",
    position: { x: 220, y: 400 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "2.2",
    type: "custom",
    label: "2.2",
    position: { x: 420, y: 400 },
    data: {
      canBeDeleted: true,
    },
  },
  {
    id: "h-2",
    type: "custom",
    label: "h-2",
    position: { x: 300, y: 500 },
  },
  {
    id: "end",
    type: "output",
    label: "Stop",
    position: { x: 300, y: 600 },
  },
]);

const edges = ref([
  {
    id: "first-edge",
    label: "first-edge",
    source: "start",
    target: "node-0.5",
    type: "straight",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: "0.5-1",
    label: "0.5-1",
    source: "node-0.5",
    target: "node-1",
    type: "straight",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `i-1.1`,
    label: "i-1.1",
    type: "straight",
    source: `node-1`,
    target: "1.1",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `i-1.2`,
    label: "i-1.2",
    type: "straight",
    source: `node-1`,
    target: "1.2",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `1.2-h-1`,
    label: "1.2-h-1",
    type: "straight",
    source: `1.2`,
    target: "h-1",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `1.1-h-1`,
    label: "1.1-h-1",
    type: "straight",
    source: `1.1`,
    target: "h-1",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `h1-2.1`,
    label: "h1-2.1",
    type: "straight",
    source: `h-1`,
    target: "2.1",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `h1-2.2`,
    label: "h1-2.2",
    type: "straight",
    source: `h-1`,
    target: "2.2",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `2.1-h-2`,
    label: "2.1-h-2",
    type: "straight",
    source: `2.1`,
    target: "h-2",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `2.2-h-2`,
    label: "2.2-h-2",
    type: "straight",
    source: `2.2`,
    target: "h-2",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
  {
    id: `end-edge`,
    label: "end-edge",
    type: "straight",
    source: `h-2`,
    target: "end",
    markerEnd: MarkerType.ArrowClosed,
    selectable: false,
    animated: true,
    type: "smoothstep",
  },
]);
</script>

<template>
  <div class="vueflow-container">
    <VueFlow :nodes="nodes" :edges="edges" :delete-key-code="null">
      <template #node-custom="props">
        <CustomNode
          :label="props.label"
          :position="props.position"
          :data="props.data"
        ></CustomNode>
      </template>
    </VueFlow>
  </div>
</template>
