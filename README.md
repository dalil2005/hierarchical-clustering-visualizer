# Hierarchical Clustering and Similarity Measures: Explained and Simulated

An English explanation of **similarity and distance measures** and **hierarchical (agglomerative) clustering**, with an interactive web lab where you can drag points, watch clusters merge step by step, and build a dendrogram.

**Live demo:** enable GitHub Pages and open `https://<your-github-username>.github.io/<repo-name>/`

Suggested repo name: `hierarchical-clustering-visualizer`

## Project structure

```
hierarchical-clustering-project/
├── index.html   # Interactive lab with built-in explanation (no dependencies)
├── README.md    # This document
├── LICENSE      # MIT
└── .gitignore
```

## 1. Why similarity measures matter

Every clustering method needs a way to say how alike two items are. A **distance** is small for similar items. A **similarity** is large for similar items. The choice of measure changes the clusters you get.

| Measure | Formula | Best for |
|---------|---------|----------|
| Euclidean | `sqrt(Σ (aᵢ − bᵢ)²)` | Straight-line distance, numeric features |
| Manhattan | `Σ \|aᵢ − bᵢ\|` | Grid-like paths, less affected by outliers |
| Chebyshev | `max \|aᵢ − bᵢ\|` | When the largest single difference matters |
| Minkowski | `(Σ \|aᵢ − bᵢ\|ᵖ)^(1/p)` | General family: p = 1 is Manhattan, p = 2 is Euclidean |
| Cosine similarity | `(A·B) / (‖A‖ ‖B‖)` | Direction, not length (text, embeddings) |
| Jaccard | `\|A ∩ B\| / \|A ∪ B\|` | Sets, tags, binary data |

Conversions: cosine distance = 1 − cosine similarity, and Jaccard distance = 1 − Jaccard similarity.

**Tips:** scale features first, because a feature with a large range dominates Euclidean distance. Use cosine for text, and Jaccard for sets.

### Example

A = (7, 3), B = (3, 8): Euclidean = √(16 + 25) ≈ 6.40, Manhattan = 4 + 5 = 9, Chebyshev = max(4, 5) = 5.

## 2. Hierarchical clustering

Hierarchical clustering builds a **tree of clusters** instead of one flat split. The common bottom-up version is **agglomerative**:

1. Start with every point as its own cluster.
2. Find the two closest clusters.
3. Merge them and record the distance.
4. Repeat until one cluster remains.

```text
clusters = each point alone
while more than one cluster:
    find pair (A, B) with smallest linkage distance
    merge A and B, remember the distance
```

The record of merges is drawn as a **dendrogram**. The height of each join is the distance at which the merge happened. **Cutting** the tree with a horizontal line gives a chosen number of clusters.

### Linkage rules

How do you measure distance between two *clusters*?

| Linkage | Distance between clusters | Behavior |
|---------|---------------------------|----------|
| Single | Closest pair of points | Can chain clusters together, finds elongated shapes |
| Complete | Farthest pair of points | Compact, similar-sized clusters |
| Average | Mean of all pairwise distances | A balance of the two |
| Ward | Smallest increase in within-cluster variance | Compact clusters, needs Euclidean distance |

### Reading a dendrogram

- Points that join low in the tree are very similar.
- A **long vertical gap** before a merge means the clusters being joined are far apart. Cutting just below the biggest gap is a good way to choose k.

### Worked example (1D)

Points: `1, 2, 6, 7, 15`, single linkage.

| Step | Merge | Distance |
|------|-------|----------|
| 1 | {1} and {2} | 1 |
| 2 | {6} and {7} | 1 |
| 3 | {1,2} and {6,7} | 4 (closest pair is 2 and 6) |
| 4 | {1,2,6,7} and {15} | 8 (closest pair is 7 and 15) |

Cutting between heights 4 and 8 gives two clusters: {1,2,6,7} and {15}.

## 3. Hierarchical clustering vs K-Means

| | Hierarchical | K-Means |
|---|---|---|
| Need k in advance | No, choose by cutting the tree | Yes |
| Result | Full tree (dendrogram) | One flat split |
| Speed | Slower, about O(n²) memory or worse | Fast on large data |
| Randomness | Deterministic | Depends on the random start |
| Cluster shape | Depends on linkage | Round clusters |

## 4. The simulation

Open `index.html` in any browser. No install or build step is needed.

**Part 1, similarity measures:** drag points A and B and see Euclidean, Manhattan, Chebyshev, Minkowski (with a p slider), cosine similarity and distance, and the angle. A second box computes Jaccard similarity and Dice for two sets of words.

**Part 2, hierarchical clustering:**

| Control | What it does |
|---------|--------------|
| Points (6 to 30) | Number of data points |
| Linkage | Single, complete, average, or Ward |
| Distance | Euclidean, Manhattan, Chebyshev, or cosine |
| Data shape | Blobs, uniform, or two elongated bands (shows single-linkage chaining) |
| Play / Next merge / Back | Step through the merges |
| Clusters now | Slide to cut the dendrogram at any level |
| New points | Generate a new data set |

Outlined points show the pair about to merge. The dashed line on the dendrogram shows the current cut.

## 5. Publish on GitHub Pages

1. Create a **public** repository on github.com.
2. Upload the files from inside the unzipped folder so `index.html` is at the top level.
3. Go to **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After 1 to 2 minutes your link is `https://<your-github-username>.github.io/<repo-name>/`. Replace both placeholders with your real names.

## 6. Courses and resources

| Resource | Provider | Level |
|----------|----------|-------|
| [Clustering, ML Crash Course](https://developers.google.com/machine-learning/clustering) | Google for Developers | Beginner |
| [scikit-learn: hierarchical clustering](https://scikit-learn.org/stable/modules/clustering.html#hierarchical-clustering) | scikit-learn docs | Intermediate |
| [SciPy: cluster.hierarchy](https://docs.scipy.org/doc/scipy/reference/cluster.hierarchy.html) | SciPy docs | Intermediate |
| [StatQuest: hierarchical clustering (search)](https://www.youtube.com/results?search_query=StatQuest+hierarchical+clustering) | Josh Starmer, YouTube | Beginner |
| [An Introduction to Statistical Learning](https://www.statlearning.com/) | Free book | Intermediate |
| [Kaggle Learn](https://www.kaggle.com/learn) | Kaggle | Beginner |

Course pages change over time, so check that each link is still current.

## License

MIT. See `LICENSE`.
