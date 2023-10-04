Decides which pod goes on which node, but does not place it, that’s the kubelet job. It looks each pod and tries to find the best node for it, following this order:

-   Filter nodes: Capacity
-   Rank the nodes using a priority function

#### It also looks at
-   Resource Requirements and limits
-   Taints and tolerations
-   Node selectors/Affinity