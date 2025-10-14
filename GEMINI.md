<!--
  gemini.md — Gemini CLI Prompt Template
  Purpose: Generate a static web page that visualizes a graph on GitHub Pages
  Author: [Your Name]
  Notes:
    - Input 1: graph type (undirectional or directional)
    - Input 2: graph structure
      * Option 1: edges array, e.g. [[1,2],[1,3],[1,4],[2,4]]
      * Option 2: adjacency array, e.g. [[], [2,3,4],[1,4],[1],[1,2]]
    - Requirements:
      * Generate graph with minimal edge crossing
      * Drag nodes interactively
      * Reset layout button
      * TODO: support edge weights
-->

## System
You are an AI assistant that generates static websites with interactive graphs.  
The website should be ready to push to GitHub and serve via https://pages.github.com.  
Use modern HTML/CSS/JS, and a graph visualization library (like D3.js, vis.js, cytoscape.js, or similar).  
Your goal is to produce a complete `index.html` (and optional CSS/JS) that meets the requirements.

---

## User
I want a static website for visualizing a graph.  
- Input 1: graph type: "undirectional" or "directional"  
- Input 2: graph structure:  
  - Option 1: edges array, e.g. [[1,2],[1,3],[1,4],[2,4]]  
  - Option 2: adjacency array, e.g. [[], [2,3,4],[1,4],[1],[1,2]]  

The graph should:  
- Minimize edge crossing  
- Allow nodes to be dragged  
- Include a button to reset the layout  
- TODO: later support edge weights  

Generate the website code and provide instructions to push it to GitHub Pages.

---

## Model
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Interactive Graph</title>
  <style>
    body { font-family: Arial, sans-serif; margin:0; padding:0; }
    #graph { width: 100vw; height: 90vh; border: 1px solid #ccc; }
    #controls { padding: 10px; }
    button { padding: 5px 10px; margin-right: 10px; }
  </style>
  <script src="https://unpkg.com/vis-network/standalone/umd/vis-network.min.js"></script>
</head>
<body>
  <div id="controls">
    <button id="resetBtn">Reset Layout</button>
  </div>
  <div id="graph"></div>

  <script>
    // TODO: replace this example with dynamic input parsing
    const graphType = "undirectional"; // input 1
    const edgesArray = [[1,2],[1,3],[1,4],[2,4]]; // input 2 (edges array)
    // const adjacencyArray = [[], [2,3,4],[1,4],[1],[1,2]]; // alternative

    // Convert edges / adjacency to vis.js nodes and edges
    const nodes = [];
    const nodeIds = new Set();
    edgesArray.forEach(edge => { nodeIds.add(edge[0]); nodeIds.add(edge[1]); });
    nodeIds.forEach(id => nodes.push({id, label: id.toString()}));

    const edges = edgesArray.map(edge => {
      return {from: edge[0], to: edge[1], arrows: graphType==="directional"?"to":""};
    });

    const container = document.getElementById('graph');
    const data = { nodes: new vis.DataSet(nodes), edges: new vis.DataSet(edges) };
    const options = {
      physics: { stabilization: false },
      layout: { improvedLayout: true, hierarchical: false },
      interaction: { dragNodes: true, dragView: true, zoomView: true }
    };
    const network = new vis.Network(container, data, options);

    // Reset layout button
    document.getElementById('resetBtn').onclick = () => network.stabilize();
  </script>
</body>
</html>

