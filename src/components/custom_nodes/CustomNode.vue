<script setup>
import { MarkerType, addEdge, useNodeId, useVueFlow } from "@vue-flow/core";

const props = defineProps(["label", "data"]);

const { removeNodes, addEdges, getIncomers, getOutgoers } = useVueFlow();

const nodeId = useNodeId();

//function to know whether particular node has sibling or not
function hasSiblingNode(id) {
  const parentId = getIncomers(id)?.[0]?.id; //no nodes contain more than one parent node except handle node
  return getOutgoers(parentId).length > 1;
}

function hasMoreThanEqual2Sibling(id) {
  const parentId = getIncomers(id)[0].id;
  return getOutgoers(parentId).length > 2;
}

//function to get all siblings of particular id
function getAllSiblings(id) {
  const parentId = getIncomers(id)[0]?.id; //no nodes contain more than one parent node except handle node
  return getOutgoers(parentId);
}

function isHandleDirectChild(id) {
  const childTypes = getOutgoers(id).map((node) => node.type);
  return childTypes.includes("handle");
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
    hasSiblingNode(nodeId),
    "; hasmore than 2 sibling node",
    hasMoreThanEqual2Sibling(nodeId),
    "; is target direct child",
    isHandleDirectChild(nodeId)
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
  if (hasSiblingNode(nodeId)) {
    console.log("clicked node ko sibling", getAllSiblings(nodeId));

    // case II.1:more than 2 sibling nodes: just remove node.
    if (hasMoreThanEqual2Sibling(nodeId)) {
      if (isHandleDirectChild(nodeId))
        // II.1.1: No child in between[handle is direct child]
        removeNodes(nodeId); //no need to track edges
      else {
        //II.1.2: there are child in between

        removeNodes(nodeId);

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
    } else {
      // case-II.2:just 2 sibling nodes: remove both nodes along with handle node. track edges

      // II.2.1: ask for confirmation:
      // const removeBothConfirm = prompt(
      //   `this will remove both nodes upto ${targetOfSelected[0]}.Are you okay?`
      // ); [temp]

      if (isHandleDirectChild(nodeId)) {
        console.log("have handle as direct child");
      } else {
        console.log("handle isnot direct child");
      }

      //II.2.2: get outgoing node of connected handle node
      const handleNodeOutgoerIds = getOutgoers(targetOfSelected[0]).map(
        (node) => node.id
      );
      console.log("handleNode ko outgoer", handleNodeOutgoerIds);

      // if (removeBothConfirm === "y") {
      // //II.2.3: remove both the node
      // getAllSiblings(nodeId).map((node) => removeNodes(node.id));

      // //II.2.4: remove that single handle node
      // removeNodes(targetOfSelected[0]);

      // //II.2.5: connect the gap between parent and child node of removed node
      // handleNodeOutgoerIds.map((targetId) => {
      //   const edgeId = (Math.random() * 100).toFixed(4);
      //   addEdges({
      //     id: `edge-${edgeId}`,
      //     label: `edge-${edgeId}`,
      //     source: sourceOfSelected[0], //coz source always gonna be single, except for handle
      //     target: targetId,
      //     animated: true,
      //     markerEnd: MarkerType.ArrowClosed,
      //   });
      // });
      // } [temp]
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
