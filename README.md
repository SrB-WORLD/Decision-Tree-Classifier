# Decision Tree Classifier

This project implements a custom decision tree classifier in C++. It builds a classification model using entropy and information gain, storing data in a tree-like structure. The classifier can process categorical data, calculate entropy and information gain for feature selection, and provide predictions on unseen data.

## Table of Contents
- [Features](#features)
- [Implementation Details](#implementation-details)
- [Prerequisites](#prerequisites)
- [Limitations](#limitations)
- [License](#license)

## Features
- **Entropy and Information Gain Calculations**: Automatically calculates entropy and information gain for optimal split selection.
- **Dynamic Tree Building**: Dynamically builds a decision tree based on the data provided, including pruning to remove redundant nodes.
- **Efficient Node and Edge Management**: Manages tree nodes and edges using pointers to optimize memory usage.
- **Tree Querying**: Allows querying the tree to classify new data points.

## Implementation Details

This classifier builds a decision tree with the following key components:
- **TreeNode and EdgeNode Structures**: `TreeNode` and `EdgeNode` structs are used to represent nodes in the decision tree, with `TreeNode` for decision points and `EdgeNode` for edges between nodes.
- **Entropy Calculation**: The entropy of each column is computed using a logarithmic formula to measure impurity.
- **Information Gain Calculation**: Information gain is computed for each feature to decide the best split at each level.
- **Tree Pruning**: Nodes with redundant information are removed to simplify the model without losing accuracy.

## Prerequisites
- **C++ Compiler**: You need a C++11-compatible compiler (like g++ or clang++).
- **Standard Libraries**: The project uses `<vector>`, `<string>`, `<unordered_map>`, and `<cmath>` for data processing.


## Limitations
**Categorical Data Only**: Currently only supports categorical data and requires all features to be pre-defined.
**Static Column Order**: Assumes a static order of attributes, based on initial input data.
**Limited to Entropy-Based Splitting**: This implementation focuses on entropy and information gain without other impurity measures (like Gini).

## License

This project is licensed under the MIT License. See LICENSE for details.

