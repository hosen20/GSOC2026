The choice of graphs reflects two complementary strategies for modeling jet substructure: the GCN graph treats each particle as a node and aggregates neighbor information uniformly through k‑nearest neighbor edges in angular space, which captures the overall topology of the jet in a simple, efficient way; meanwhile, the GAT graph uses the same node and edge construction but applies attention weights to neighbors, allowing the model to emphasize particles that are more discriminative (such as soft wide‑angle radiation typical of gluons or high‑
𝑝
𝑇
 constituents in quark jets). In short, GCN provides a strong baseline by averaging local features, while GAT adds adaptivity and interpretability by learning which connections matter most for quark/gluon classification.
