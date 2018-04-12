# Self-Organising Maps or Kohonen's self organizing feature maps

- Type of neural net. Also referred as Kohonen Maps
- Difference with ANN:
   - ANN learns by error-correcting, is supervised, serie of layers
   - SOM learns by competitive learning, deal with unsupervised 2D grid of neurons
- Competitive: When an imput is "presented" to the net, only one of the neuro will be activated -> neurons "compete" for each input.
- Data compression known as vector quantization



## Advantages
**Finding structure**
SOM is meant to be a 2D repr of a multi-dim dataset. Similar inputs in the multi-dim will map the same 2D node. 

In addition to clustering the data into distinct region, regions of similar properties are found (see visualisation of the 2D map)

**Dimensionality reduction**
SOM finds a lower-dim repr while preserving the similarity between records

**Visualisation**

This is the primary purpose of generating a map. 

**Incremental**

The learning of a SOM is incremental thus we can update a map with new data. This is particulary interesting if the data points retain their old label.

## SOM Parameters

* Learning rate $\eta$
* Neighbourhood radius $\sigma$

## Network architecture

The network is created from a 2D lattice of 'nodes', each of which is fully connected to the input layer. Each node has a specific topological position and contains a vector of weights of the same dimension as the input vectors. *However, there are no lateral connections betweens nodes within the lattice.*

## Learning

1. Select randomly an input vector
2. Find the Best Matching Unit
3. Update the learning rate and neighboorhour radius
4. Update the weights

##### Initialisation

The weights are initialized so that 0 < $w$ < 1.  An number of iterations is also chosen to cut off the learning process.

> In practice, we must normalize the data between 0 and 1.

##### Best Matching Unit

We iterate through all the node to calculate the Euclidean distance between each node's weight vector and the current input vector. The node with the smallest distance is the BMU.

> Trick : compute the squared distance to avoid the expensive square root calculation

##### Update of the SOM parameters

* $\sigma_t = \sigma_0 \exp(-\frac{1}{\lambda})$ where $\lambda$ is a time constant and $t$ is the current iteration step.
* $\eta_t = \eta_0 \exp(-\frac{1}{\lambda})$

##### Update the weights of BSU's local neighbourhood

For the BSU, the weight vector is updated according this equation : $W_{t+1} = W_t+\eta_t(I_t-W_t)$.

Basically, we want the weight vector to be slightly closer to the input vector $I$ 

However for the node found to be within the radius of the BSU, we want the weight being updated according to its distance to the BSU (the center). So its weight vector is adjusted as follows $W_{t+1} = W_t+\Theta_t\eta_t(I_t-W_t)$. 

In this equation, $\Theta_t$ is a Gaussian decay so the amount of learning will fade over distance.

$\Theta_t=\exp(-\frac{dist^2}{2\sigma^2_t})$ 

#### Notes

If we were to make a SMO again, even if we use the same stttings, where each groups ends up could be different due to randomization in the learning process.

## Somoclu

It is a library to compute SOM efficiently by parallezing the algorithm. After regrouping the data by the SOM algorithm, it will show a color map for each attribute.

The edge map shows the border between groups in the data. When there is a large change between two nodes in the SOM, the edge is brighter.

The density map shows how many data points each node has.

#### Notes

We can use a clustering algorithm like kmeans or a classification model on the top of the data representation made by SOM.



## Interpretation

Since the SOM tries above all to preserve local structures, the interpretation of the map should predominantly be done locally, based on the local relations of the data items on the map.
The global structure is often useful as well, however.

Yet another method that aids in the interpretation of the maps is to add some external information on the organized map like the class label if available. The distribution of the samples of each class, plotted on the map as a density histogram, may also help in the interpretation process.

If the distributions of the known classes are overlapping, such displays can even be used to explore the degree of overlap in different types of samples. It may be possible to gain insight into whether the classes actually co-exist or whether new kinds of features should be added to the data items to make the classes more easily separable.

#### Evaluation

We can use the classification accuracy to indicate how well the classes are separated on the map, and the
classification accuracy of new samples to measure the generalizability of the result.

Generalizability could be measured as the sensitivity of the map to small variations in the input data, caused either by adding artificial noise or by cross-validation.

## Sources

[yhat_part1](http://blog.yhat.com/posts/self-organizing-maps-1.html)
[ai-junkie](http://www.ai-junkie.com/ann/som/som1.html)

[thesis of Kaski, S., *Data exploration using self-organizing maps.*](http://users.ics.aalto.fi/sami/thesis/thesis_tohtml.html)

[IFC Soft](http://homes.cs.washington.edu/~kthayer/ifcsoft/som.html)

## Tag

dimensionality reduction, vector quantization