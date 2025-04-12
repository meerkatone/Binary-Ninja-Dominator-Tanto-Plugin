## Examples

When viewing a complex function, these visualizations make it easier to understand control flow:

1. **Full Dominator Tree** shows a simplified tree structure that clarifies which blocks must be executed before others.

2. **Iterated Dominance Frontier** is particularly valuable when working with SSA form. It identifies exactly where phi nodes need to be placed.

3. **Immediate Dominator/Post-Dominator** views provide a quick way to identify the most important control flow dependencies.

To generate a complete Mermaid diagram of a function's dominator tree:

```python
from dominator_plugin import generate_dominator_mermaid

# Get current binary view and function
func = here.function
diagram = generate_dominator_mermaid(bv, func.start)
print(diagram)
```

# Dominator Analysis Plugin for Binary Ninja

This plugin extends the Tanto graph visualization plugin for Binary Ninja by adding advanced dominator and post-dominator tree views. These views help analyze control flow and identify key blocks in a function, particularly useful for advanced program analysis tasks.

## Features

The plugin adds six unique dominator-related views to the Tanto plugin, extending Tanto's built-in capabilities:

### Dominator Views
1. **Iterated Dominance Frontier** - Shows the iterated dominance frontier (useful for phi node placement in SSA)
2. **Immediate Dominator** - Shows only the immediate dominator of the current block
3. **Strict Dominators** - Shows all dominators except the block itself
4. **Full Dominator Tree** - Displays the complete dominator tree for the current function

### Post-Dominator Views
5. **Immediate Post Dominator** - Shows only the immediate post-dominator of the current block
6. **Full Post Dominator Tree** - Displays the complete post-dominator tree for the current function

**Note:** This plugin complements Tanto's built-in dominator views. The built-in views include "Dominators", "Dominance Frontier", "Dominator Tree Children", "Post Dominators", etc.

## Installation

1. Ensure you have the [Tanto plugin](https://github.com/Vector35/tanto) installed
2. Place the `dominator_plugin.py` file in your Binary Ninja plugins directory:
   - Windows: `%APPDATA%\Binary Ninja\plugins`
   - Linux: `~/.binaryninja/plugins`
   - MacOS: `~/Library/Application Support/Binary Ninja/plugins`
3. Restart Binary Ninja or reload plugins

**Important Note:** If you encounter any initialization errors, try the following:
1. Make sure Binary Ninja is fully loaded before using the plugin
2. Open a Tanto view first and then switch to one of the custom views
3. If errors persist, try restarting Binary Ninja

## Mermaid Diagram Generation

The plugin includes several utility functions to generate Mermaid diagrams for different dominator relationships. You can use these from the Binary Ninja Python console:

```python
from dominator_plugin import generate_dominator_mermaid, generate_post_dominator_mermaid

# Generate dominator tree diagram
dominator_diagram = generate_dominator_mermaid(bv, 0x8048000)  # Replace with your function address
print(dominator_diagram)

# Generate post-dominator tree diagram
post_dom_diagram = generate_post_dominator_mermaid(bv, 0x8048000)
print(post_dom_diagram)
```

### Available Mermaid Generator Functions:

1. `generate_dominator_mermaid(bv, function_address)` - Dominator tree
2. `generate_post_dominator_mermaid(bv, function_address)` - Post-dominator tree
3. `generate_dominance_frontier_mermaid(bv, function_address)` - Dominance frontier
4. `generate_post_dominance_frontier_mermaid(bv, function_address)` - Post-dominance frontier
5. `generate_immediate_dominator_mermaid(bv, function_address)` - Immediate dominator relationships
6. `generate_immediate_post_dominator_mermaid(bv, function_address)` - Immediate post-dominator relationships
7. `generate_iterated_dominance_frontier_mermaid(bv, function_address, variable_blocks=None)` - Iterated dominance frontier

These diagrams can be useful for understanding control flow, planning SSA transformations, and visualizing the structure of complex functions.

## Usage

1. Open a binary in Binary Ninja
2. Navigate to a function
3. Open the Tanto view (View > Tanto)
4. Select one of the added views from the dropdown menu

## What are Dominators and Post-Dominators?

- A block A **dominates** block B if all paths from the function entry to B must go through A
- A block A **post-dominates** block B if all paths from B to the function exit must go through A
- The **dominance frontier** of a block A is the set of blocks that are not dominated by A but have predecessors that are dominated by A
- The **post-dominance frontier** of a block A is the set of blocks that are not post-dominated by A but have successors that are post-dominated by A
- The **iterated dominance frontier** is used for determining where to place phi nodes in SSA form

These relationships are useful for understanding control flow, identifying loops and conditional structures, and performing program analysis tasks like variable liveness analysis.
