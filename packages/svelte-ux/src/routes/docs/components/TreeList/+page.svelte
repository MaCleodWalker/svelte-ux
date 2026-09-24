<script lang="ts">
  import { TreeList, Icon } from 'svelte-ux';
  import { mdiFolder, mdiFileDocumentOutline } from '@mdi/js';
  import type { TreeNode } from '@layerstack/utils/array';
  import Preview from '$lib/components/Preview.svelte';

  const basicNodes: TreeNode[] = [
    {
      id: 'fruits',
      name: 'Fruits',
      level: 1,
      children: [
        { id: 'apples', name: 'Apples', level: 2, children: [] },
        { id: 'bananas', name: 'Bananas', level: 2, children: [] },
        { id: 'oranges', name: 'Oranges', level: 2, children: [] },
      ],
    },
    {
      id: 'vegetables',
      name: 'Vegetables',
      level: 1,
      children: [
        { id: 'carrots', name: 'Carrots', level: 2, children: [] },
        { id: 'broccoli', name: 'Broccoli', level: 2, children: [] },
      ],
    },
  ];

  const fileTreeNodes: TreeNode[] = [
    {
      id: 'src',
      name: 'src',
      level: 1,
      children: [
        {
          id: 'components',
          name: 'components',
          level: 2,
          children: [
            { id: 'TreeList', name: 'TreeList.svelte', level: 3, children: [] },
            { id: 'TableOfContents', name: 'TableOfContents.svelte', level: 3, children: [] },
          ],
        },
        {
          id: 'routes',
          name: 'routes',
          level: 2,
          children: [
            { id: 'page-ts', name: '+page.ts', level: 3, children: [] },
            { id: 'page-svelte', name: '+page.svelte', level: 3, children: [] },
          ],
        },
        { id: 'index-ts', name: 'index.ts', level: 2, children: [] },
      ],
    },
    {
      id: 'package-json',
      name: 'package.json',
      level: 1,
      children: [],
    },
    {
      id: 'readme-md',
      name: 'README.md',
      level: 1,
      children: [],
    },
  ];

  let selectedNode: TreeNode | null = null;
</script>

<h1>Examples</h1>

<h2>Basic</h2>

<Preview>
  <div class="p-2 border rounded bg-surface-100 max-w-xs">
    <TreeList nodes={basicNodes} let:node>
      <span class="text-sm">{node.name}</span>
    </TreeList>
  </div>
</Preview>

<h2>Custom Styling / Classes</h2>
<p class="text-sm text-surface-content/60 mb-2">
  You can customize the tree styling by passing class names or functions for <code>ul</code> and
  <code>li</code> containers via the <code>classes</code> prop.
</p>

<Preview>
  <div class="p-2 border rounded bg-surface-100 max-w-xs">
    <TreeList
      nodes={basicNodes}
      classes={{
        ul: 'pl-4 border-l border-surface-content/10 ml-1',
        li: 'my-1 list-disc list-inside text-sm text-primary',
      }}
      let:node
    >
      <span>{node.name}</span>
    </TreeList>
  </div>
</Preview>

<h2>Interactive Selection</h2>
<p class="text-sm text-surface-content/60 mb-2">
  Click on a node to select it. The active state can be styled dynamically based on the current
  selection.
</p>

<Preview>
  <div class="grid grid-cols-2 gap-4">
    <div class="p-2 border rounded bg-surface-100">
      <TreeList nodes={basicNodes} classes={{ ul: 'pl-4 ml-1' }} let:node>
        <button
          type="button"
          class="flex items-center w-full text-left text-sm px-2 py-1 rounded hover:bg-surface-content/5 transition-colors {selectedNode?.id ===
          node.id
            ? 'bg-primary/10 text-primary font-semibold'
            : ''}"
          on:click={() => (selectedNode = node)}
        >
          {node.name}
        </button>
      </TreeList>
    </div>

    <div class="p-4 border rounded bg-surface-100 flex flex-col justify-center">
      <h3 class="text-xs uppercase text-surface-content/50 font-bold mb-1">Selected Node</h3>
      {#if selectedNode}
        <div class="text-sm font-semibold text-primary">{selectedNode.name}</div>
        <div class="text-xs text-surface-content/60 mt-1">ID: {selectedNode.id}</div>
      {:else}
        <div class="text-sm text-surface-content/40 italic">None selected</div>
      {/if}
    </div>
  </div>
</Preview>

<h2>File Explorer Example</h2>
<p class="text-sm text-surface-content/60 mb-2">
  Using custom slots, you can easily build highly visual hierarchical views like a file explorer.
</p>

<Preview>
  <div class="p-2 border rounded bg-surface-100 max-w-xs">
    <TreeList nodes={fileTreeNodes} classes={{ ul: 'pl-4 ml-1' }} let:node>
      <div class="flex items-center gap-1.5 py-1 text-sm text-surface-content/85">
        {#if node.children.length > 0}
          <Icon data={mdiFolder} class="text-warning" size="18px" />
        {:else}
          <Icon data={mdiFileDocumentOutline} class="text-surface-content/40" size="18px" />
        {/if}
        <span>{node.name}</span>
      </div>
    </TreeList>
  </div>
</Preview>
