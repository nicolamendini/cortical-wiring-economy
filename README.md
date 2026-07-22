# When is a Cortical Map Worth the Wiring?

Why does the visual cortex of some species organise into smooth maps of orientation preference, while in others (mostly rodents) it shows an apparently random 'salt-and-pepper' pattern? This demo follows one
self-organising network from visual input to learned cortical structure, then
uses it to motivate a simple answer: evolution may be balancing the cost of
nearby and far-reaching connections.

The model is inspired by **GCAL**, a model in which visual experience, recurrent
activity and Hebbian learning jointly shape receptive fields and cortical maps
([Stevens et al., 2013](https://doi.org/10.1523/JNEUROSCI.1037-13.2013)). The
version shown here adds a second cortical stage, modelling cortical layer 4 and layers 2/3.

## 1. Emergent connectivity

Read the animation from bottom to top. The **LGN** relays small patches of the
visual input to **layer 4 (L4)**. With experience, L4 neurons learn selective
receptive fields: each becomes especially responsive to an edge at a particular
angle. **Layers 2/3 (L2/3)** receive both excitation and inhibition from L4,
then combine short-range interactions with learned, longer-range lateral
connections.

Red marks net excitation, blue marks net inhibition and the black dot is the
model neuron whose incoming connections are being followed. The dashed line on
L2/3 is the neuron's preferred orientation axis. As learning proceeds, the long-range
connections develop a patchy structure and tend to reach other regions that represent
related orientations.

![Development of afferent, feedforward and lateral connectivity](demo_assets/01_laminar_connectivity.gif)

## 2. Let the activity settle

The learned network does not answer a visual input all at once. Activity is
allowed to settle through a short recurrent conversation. L4 turns the LGN
signal into a sparse cortical code that remains strongly tied to the incoming
image. L2/3 then uses its lateral interactions to refine that code into a
sharper response.

![Activity settling through LGN, L4 and L2/3](demo_assets/02_activity_settling.gif)

## 3. Repeated activity becomes a map

Every settled response leaves a small trace in the connections. Hebbian
learning strengthens pathways between neurons that are repeatedly active
together. Across many natural images, those local changes consolidate into
large-scale orientation maps in both L4 and L2/3.

Colour shows preferred edge orientation. The black-and-white panels show the
corresponding Fourier power: the emerging ring is the signature of orientations
repeating at a characteristic spacing across the cortical sheet.

![Development of L4 and L2/3 orientation maps](demo_assets/03_orientation_maps.gif)

## 4. A comparison with real connectivity
Samples of learned L2/3 connection fields show remarkably similar connectivity profiles in animal species. The right panel shows an anatomical
tracing from tree-shrew visual cortex, reproduced from
[Bosking et al. (1997)](https://doi.org/10.1523/JNEUROSCI.17-06-02112.1997).
The model panel shows an importance sample from one neuron's learned patchy
lateral excitatory field. Its grey line marks that neuron's preferred orientation
axis. A small continuous positional scatter makes the model lattice less
conspicuous; its mean displacement is the same two-cell value explored in the
[microdomains demo](https://github.com/nicolamendini/microdomains).

<table>
  <tr>
    <th width="50%">Self-organising model</th>
    <th width="50%">Tree shrew</th>
  </tr>
  <tr>
    <td><img src="demo_assets/case_3649_connectivity_flipped_alpha05.png" alt="Learned patchy L2/3 excitation and preferred orientation axis in the model"></td>
    <td><img src="F8.large.jpg" alt="Anatomically traced horizontal connections in tree-shrew visual cortex"></td>
  </tr>
</table>

## 5. An evolutionary trade-off

In our work, we argue that **local connection pools** (number of possible local connections) build
and support domains of similarly tuned neurons. By contrast, **global connection pools** (number of possible long-range connections)
are the much larger territory from which selective long-range connections can
be drawn. Large local domains require a significant metabolic cost to build, but their pooling can make
the network robust enough to tolerate much sparser global wiring.

Consistent with this wiring-economy premise, primate L2/3 neurons receive two to five times fewer synapses than mouse neurons ([Wildenberg et al., 2021](https://doi.org/10.1016/j.celrep.2021.109709)).

We show an example of realised local and long-range connections from the model. Short-range
excitatory neighbourhood forms the dense central cluster, while a sparse sample
of learned, longer-range excitatory connections forms the lighter patches around it.

<p align="center">
  <img src="demo_assets/local_global_legend_bottom_left_bounded.png" width="62%" alt="Filled local and hollow sparse global excitatory connections within the cortical sheet boundary, with a connection-count legend in the lower-left corner">
</p>

Our central claim is that evolution prefers a connectivity scheme based on large domains, leading to topological maps, when the total cost of dense local connectivity and sparse global connectivity falls below that of a competing scheme made of little local connectivity and dense global connectivity, leading to salt-and-pepper maps. The model behind the salt-and-pepper scheme is based on largely overlapping computations (micro-GCAL, see [microdomains demo](https://github.com/nicolamendini/microdomains) but not shown here).

In our formalisation, total realised wiring is written as

$$
C = \frac{L}{1+s_L} + \frac{G}{1+s_G},
$$

where $L$ and $G$ are the available local and global pools, and $s_L$
and $s_G$ describe how sparse their realised connections can be while the
network remains stable. The simulations identify the stability-preserving
local/global sparsity combinations; the least costly one is selected for each
pool size. The important idea is the trade-off, not the algebra: **topological
maps are favoured when the global savings enabled by domain pooling exceed the
local cost of building those domains.**

## 6. Does the trade-off match real animals?

To make a deliberately simple cross-species test, orientation-domain size is
used as a proxy for the local pool. Self-organising models predict that larger
domains require wider local interactions to establish them. The global pool is
estimated as the number of neurons inside an elliptical cylinder reaching the
maximum measured span of horizontal lateral connectivity. Both imagined
cylinders extend through the full cortical depth; areal neuronal density
therefore converts their footprints into candidate-neuron counts.

These are **candidate connection pools, not synapse counts**. They measure the
space from which a neuron could draw local or global partners—and therefore the
opportunity for sparse wiring.

| Species / area | Organisation | Span r (mm) | Elongation s | Neurons under 1 mm² η (10³) | Global pool P (10³) | Map period Λ (mm) | Local/domain pool D (10³) |
|---|---|---:|---:|---:|---:|---:|---:|
| Hamster V1 | Salt-and-pepper | 1.5 | 0.76† | 100† | 537 | 0.10† | — |
| Rat V1 | Salt-and-pepper | 1.8 | 0.76† | 100 | 774 | 0.10† | — |
| Squirrel V1 | Salt-and-pepper | 1.8 | 0.76 | 100† | 774 | 0.10† | — |
| Rabbit V1 | Salt-and-pepper | 1.5 | 0.76† | 100† | 537 | 0.10† | — |
| Tree shrew V1 | Topological | 2.5 | 0.51 | 100† | 1,000 | 0.63 | 31 |
| Cat V1 | Topological | 3.5 | 0.56† | 100 | 2,160 | 1.00 | 79 |
| Ferret V1 | Topological | 3.5 | 0.56† | 100† | 2,160 | 0.85 | 57 |
| Squirrel monkey V1 | Topological | 1.6 | 0.61 | 250† | 1,230 | 0.51 | 51 |
| Squirrel monkey V2 | Topological | 2.5 | 0.68† | 170 | 2,270 | 1.02 | 139 |
| Owl monkey V1 | Topological | 1.6 | 0.61 | 250† | 1,230 | 0.65 | 59 |
| Macaque V1 | Topological | 3.7 | 0.56 | 250 | 6,000 | 0.76 | 113 |
| Macaque V2 | Topological | 4.0 | 0.63 | 170 | 5,380 | 0.95 | 120 |

— No measurable orientation domain. † Estimated value. Full references and
estimation rules are provided in the
preprint/book chapter
[*What do species differences in cortical map organization reveal about the evolution of cortical circuitry?*](https://github.com/nicolamendini/evolution-of-cortical-modules).
Here the two original tables are merged so the local and global pool estimates
can be read together.

## The result

When these animal estimates are placed against the transition predicted by the
model, species with salt-and-pepper and topological maps separate clearly. The
same simple wiring-economy argument that emerges from the simulations therefore
organises the comparative data across primate and non-primate species.

The left panel plots global connection-pool size against wiring cost: simply the
number of local and global connections that remain after sparsity. The solid
curves are the model predictions, while the points show individual species.
Blue gives the cost if a species used a salt-and-pepper organisation; orange
gives the cost if it used a topological organisation. Lower is cheaper.

The right panel condenses this comparison into the ratio of topological cost to
salt-and-pepper cost. Above 1, salt-and-pepper uses fewer connections; at 1,
the two organisations cost the same; and below 1, the topological map is
cheaper. The model predicts a transition between these regimes at a global
pool size of roughly $9 \times 10^5$ neurons.

[![Predicted wiring-cost transition and animal data](demo_assets/crossing.png)](crossing.pdf)

The broad picture is simple: the cortex can spend connections locally to build
a map, or spend them globally to coordinate a less ordered sheet. Different
brains appear to settle on different sides of the same economical compromise.
