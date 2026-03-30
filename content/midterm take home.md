1a.
$P(0 \text{ arrivals in time } \Delta t )= e^{-\lambda \Delta t}$
$G$ is the average number of transmission attempts per slot => arrivate rate per mini slot is $G/4$. We know that collision happens when 2 nodes start within the same 4 mini-slots.
Expected arrivals during A's transmission: $$\lambda \Delta t = \frac{G}{4}\times 4 = G$$
So probability of no collision is $P (\text{no collision}) = e^{-G}$

1b. Throughput $S = G \times P (\text{success})=Ge^{-G}$
1c. Maximum value of S over all values of G: 
To find max of function $S(G) = Ge^{-G}$, first take the derivative $S'(G) = e^{-G} -Ge^{-G}=e^{-G}(1-G)$. Set $S'(G)=0\rightarrow 1-G=0 \rightarrow G=1$. Maximum value of $S$ is $S_{max}=1\cdot e^{-1}=e^{-1}$.

