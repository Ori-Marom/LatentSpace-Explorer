# A guided exploration

This walkthrough presents the existing application. All example words must exist in the
loaded vocabulary; use the word list to choose alternatives when needed.

## 1. Find a neighborhood

Open **Navigation**, search for a familiar lowercase word, and center the view on it.
Inspect nearby labels, then switch to 3D. Try another combination of PCA axes.

**Look for:** whether an apparent cluster persists across views. A single projection can
hide meaningful differences in the original space.

## 2. Compare two metrics

In **Distance**, enter two known words. Calculate **Cosine**, then **Euclidean**.
Repeat with a different pair.

**Look for:** the distinction between orientation and separation. Higher cosine similarity
indicates greater alignment; lower Euclidean distance indicates less separation.

## 3. Trace an analogy

In **Arithmetic**, enter `king - man + woman` and select **Calculate**. Spaces around the
operators are optional. Examine the top-five matches and the path drawn on the 2D graph.

**Look for:** which words the loaded embeddings associate with the resulting vector.
The familiar analogy is an illustrative query; no specific winning result is guaranteed.
Arithmetic visualization switches to 2D if necessary.

## 4. Investigate a group

In **Subspace**, choose several related words from the list, or select a word in the graph
and use **Add from Graph**. Choose the number of neighbors and select **Calculate Centroid**.
Then add a less related word and calculate again.

**Look for:** how changing group membership shifts its average vector and nearest neighbors.

## 5. Create a semantic direction

In **Projection**, use two known words to define an axis. Inspect the resulting 1D arrangement
with the available pan and zoom controls.

**Look for:** relative position along your chosen direction, rather than treating that axis
as an objective label for the words.

## 6. Revisit a perspective

Return to **Navigation**, change the PCA axes, then use **Undo** and **Redo**.
This demonstrates reversible axis navigation through stored commands.

## Suggested presentation narrative

> LatentSpace Explorer connects numerical word embeddings to an interactive desktop interface.
> It keeps full vectors for calculations and PCA vectors for visualization, then exposes
> similarity, arithmetic, group centroids, and custom projections through focused tools.
> The software structure separates data management, math strategies, and reversible navigation.

The banner in the README is illustrative artwork, not an application screenshot or measured plot.
