<script setup>
import { MarkerType, addEdge, useNodeId, useVueFlow } from "@vue-flow/core";

const props = defineProps(["label", "data"]);

const { removeNodes, addEdges, getIncomers, getOutgoers } = useVueFlow();

const nodeId = useNodeId();

//function to know whether particular node has sibling or not
function hasSiblingNode(id) {
  const parentId = getIncomers(id)[0].id; //no nodes contain more than one parent node except handle node
  return getOutgoers(parentId).length > 1;
}

//function to get all siblings of particular id
function getAllSiblings(id) {
  const parentId = getIncomers(id)[0].id; //no nodes contain more than one parent node except handle node
  return getOutgoers(parentId);
}

function handleDeleteNode() {
  const targetOfSelected = getOutgoers(nodeId).map((node) => node.id);
  const sourceOfSelected = getIncomers(nodeId).map((node) => node.id);

  console.log(
    "clicked node",
    nodeId,
    "; outgoers",
    targetOfSelected,
    "; incomers",
    sourceOfSelected,
    "; hasSibling node",
    hasSiblingNode(nodeId)
  );

  //case-I: there are no sibling nodes of the clicked node.[ie. No multiple node case]
  if (!hasSiblingNode(nodeId)) {
    removeNodes([nodeId]);

    //connect source and target after removal
    targetOfSelected.map((targetId) => {
      const edgeId = (Math.random() * 100).toFixed(4);

      addEdges([
        {
          id: `edge-${edgeId}`,
          label: `edge-${edgeId}`,
          source: sourceOfSelected[0], //coz source always gonna be single except for handle
          target: targetId,
          type: "smoothstep",
          animated: true,
          markerEnd: MarkerType.ArrowClosed,
        },
      ]);
    });
  }

  //   case-II: There are sibling nodes of the clicked node. [ie. Multple node case]
  if (hasSiblingNode) {
    const removeBothConfirm = prompt(
      "this will remove both nodes.Are you okay?"
    );

    if (removeBothConfirm === "y") {
      console.log(getAllSiblings(nodeId));
      getAllSiblings(nodeId).map((node) => removeNodes(node.id));
    }
  }
}
</script>

<template>
  <p class="custom-node">
    {{ props.label
    }}<button @click="handleDeleteNode" v-if="props.data.canBeDeleted">
      D
    </button>
  </p>
</template>

<style scoped>
.custom-node {
  background-color: blue;
  padding: 10px;
  color: white;
}
</style>
