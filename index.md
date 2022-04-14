## Reproduction Blog: Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction

## Authors:
Lang Feng - 5446511 - L.Feng-2@student.tudelft.nl,  
Haoran Wang - 5468175 - H.WANG-46@student.tudelft.nl,  
Zhongbo Yao - 5473543 - Z.Yao-2@student.tudelft.nl,  
Sasha Koryakin  

## Introduction
### Background
Automatic identification of salient aspects are widely used in areas like advertisements recomandation, summary generation and recomandation justification. However, current models ignore the fact that the latent hierarchies exist among words and a single word may have different semantics in different contexts, which affects the accuracy negatively. The authors of the paper Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction [1] proposes the model HDAE to improve current models. The HDAE is improved based on the the models using Euclidean distance and seedwords.

### Problem Formulation
The problem can be seen as extract an aspect $a_i$($a_i \in A_C$) from a given review sentence $x^s = \{x_1, x_2, ..., x_i\}$, where $x_i$ represents each single word in the review sentence and $i$ refers to the index. In the training process, every aspect $a_i$ is represented by seed words $[s_{i, 1}, s_{i, 2}, ... , s_{i, j}]$. The output is the probability distribution $p^a_s = <p_s^{a1}, p_s^{a2}, ..., p_s^{ai}>$, indicating the probability that a review sentence can be classified to a specific aspect.

### Big Idea
The HDAE can be divided into three composing parts, namely a hyperbolic aspect classifier, an aspect disentanglement module and an aspect-aware regularization.

#### Hyperbolic Aspect Classifier
Unlike conventional classifiers which use Euclidean distance to measure the similarity or the relationship among the input vectors, the hyperbolic aspect classifier transfers the class distances to the hyperbolic space. It is proved that the hyperbolic representation is more suitable in representing hierarchical data and exploring their relationships [2]. It is used here to fully capture the hierarchies among input vectors.  
![](https://i.imgur.com/aSqY2yl.png)  
The projection of hyperbolic space can be seen from the picture above. The hyperbolic distances can be derived from a mapping function from tangent space. The formulation is shown as following:

$d^{exp}_P(x, y) = d_p(exp_0(x), exp_0(y))$

The item $d^{exp}_P$ represents the hyperbolic distance and $d_P$ means the distance in the tangent space.



<img src="https://i.imgur.com/NNEfZwP.png" width="350">

The tangent space can be unstood through the above ficture. It is derived from a unit open ball, which can be represented as $P^n = (\beta, g^{\beta}_x)$, where $\beta$ shows the direction  of a point and $g^{\beta}_x$ reflects the distance of a vector from the origin. To calculate the distance in the tangent space, the following formula is used:
$d_P(x, y) = arcosh(1 + 2\frac{||x - y||^2}{(1 - ||x||^2)(1 - ||y||^2)})$

Therefore, the hyperbolic distance $d^{exp}_P$ can be derived and is used to measure the hierarchies among words.

#### Aspect Disentanglement Module
The aspect dientanglement module is applied to identify the different semantic meanings of a single word in different contexts. The conventional networks fail to do this because they assign uniform values to the seedwords, which are used to represent aspects. The authors introduce the Gaussian distribution into the seedwords. Therefore, the different semantic in different contexts can be learnt by training the model.

#### Aspect-aware Regularization
The regularization is introduced to encpurage the independence of the different latent semantic meanings. By doing this, the model is expected to learn distinguishable semantic meanings to represent a review sentence more comprehensively. The value of the regularization is determined by three iterms, including seed word dependence, latent semantic independence and refined seed word representation.
 
 ### Euclidean Aspect Extractor
The hyperbolic aspect extractor is derived from the Euclidean aspect extractor, which applies the Euclidean distance to measure the relationship among input vectors. 

The model firstly transfer the input sentences into the segment representation vector $v_s$, which is generated from the following formula:
$v_s = \sum_{i = 1}^{n}c_i v_{x_i}$
$c_i$ is the related weight calculated by self-attention method.

Then the model produces the output probability distribution via the function:
$p^a_s = softmax(Wv_s + b)$

The following function is used to extract aspects from the seed words:
$a_i = \sum_{j}^{N} z_{i, j}s_{i, j}$
All the aspects compose the aspect matrix $A$.

Then the segment reconstructed vector $r_s$ is calculated by the function:
$r_s = A^Tp^{asp}_s$

In the end, the distance between two input vectors are caluclated as loss function:
$J_r(\theta) = \sum_{x^s \in C}\sum_{i=1}{n}max(0, 1 - r_s v_s + r_s v_{ni})$

### Hyperbolic Aspect Classifier
The principle of the hyperbolic aspect classifier is simliar to the Euclidean aspect classifer but the reconstructed vector is transferred to the hyperbolic space.

### Aspect Disentanglement Module
In oder to assign different weights to the semantics in different contexts, the seedword semantic vectors are mapped to Gaussian distribution.


### Aspect-Aware Regularization
The regularization aims at encouraging the independence of semantics and has three objectives: minimize the seed word dependence, minimize the latent semantic independence and minimize the aspect scope confinement.

### Objective 
The final objective of the model is to learn the parameters to minimize the total loss function:
$J(\theta) = J_r(\theta) + \lambda J_d(\theta) + J_{d1}(\theta) + J_{d2}(\theta) + J_{d3}(\theta)$
where $J_r$ measures the hyperbolic distance, $J_{d1}, J_{d2}, J_{d3}$ measures the value of regularization and $J_d$ is introduced from the paper [3].


### Datasets
The dataset used in our reproduced project is the restaurant reviews derived from the SemEval-2016 Aspect-based Sentiment Analysis task (Pontiki et al. 2016). The original dataset covers six languages: English (En), Spanish (Sp), French (Fr), Russian (Ru), Dutch (Du), and Turkish (Tur). However, data in Turkish and Dutch is not tested in our project as a great deviation from the reported results in the original paper(Chang et al. 2021) is observed. We consider the reason to be that we are using a different computing resource(Macbook with M1 chip, without CUDA) compared with the authors. All the results in our project are derived from this modified dataset.



## Reproducibility Approach
There was no existing code originally. We were provided with a GitHub repository, https://github.com/stangelid/oposum, which is meant to preprocess the raw text files into easy-to-manipulate .hdf5 binaries. We managed to make contact with the author of the paper and got their code, which can be found here https://github.com/johnnyjana730/HDAE.   
In this reproduction, we managed to reproduce HDAE in Table 1 and Table 2 and also hyperparameters check, including lr (learning rate), hyper-beta, and gb_temp.

### Reproduced Table 1

|Categories |Bags |KBs  |Boots|B/T  |TVs  |VCs  |
|-----------|-----|-----|-----|-----|-----|-----|
|HDAE(paper)|68.8 |72.2 |64.0 |72.0 |71.2 |66.9 |
|Reproduced |66.9 |69.2 |63.3 |67.3 |65.5 |60.9 |

<img src="https://i.imgur.com/lWMcugm.png" width="500">

From the image above, we can see that we get similar results with the paper. However, due to the limitation of computability, our reproduced results are not as good as the paper. But we can also see that the differences are not large, which means we can get results closer to the paper given enough computability.

### Reproduced Table 2

|EN     |SP     |FR     |RU   |DU   |TUR  |
|-------|-------|-------|-----|-----|-----|
|0.543  |0.657  |0.529  |0.584|0.601|0.234|   
|0.542  |0.603  |0.509  |0.581|0.581|0.5  |    
|0.557  |0.641  |0.526  |0.627|0.592|0.339|   
|0.56   |0.663  |0.423  |0.591|0.604|0.258|   
|0.572  |0.609  |0.527  |0.626|0.574|0.266|    
|0.557  |0.638  |0.493  |0.586|0.624|0.202|    
|0.569  |0.622  |0.515  |0.585|0.61 |0.315|   
|0.545  |0.623  |0.515  |0.628|0.574|0.492|  
|0.562  |0.656  |0.479  |0.612|0.55 |0.298|  
|0.589  |0.653  |0.564  |0.598|0.595|0.258| 

<img src="https://i.imgur.com/PeHNqZI.png" width="500">

We see that the majority of results from the paper fall inside the boxplot result from the reproduction, the main outlier is Turkish whose median lies far below the paper result.  

A small statistical analysis is performed to check this,
from the data we get a standart gaussian distribution with:  
mean = 0.3162  
variance = 0.0973  
x = 0.508  
Using a cdf calculation ( P(X <= x) ) we get cdf = 0.9692, which for our case become the p-value = 1-0.9692 = 0.0308, which typically is regarded as significant.


### Hyperparameters Check
We checked the sensitivity of three hyperparameters, including lr (learning rate), hyper-beta and gb_temp, and whether the author has chosen the best hyperparameters.
#### lr (Learning Rate)

We chose 6 different values for learning rate, 0.5, 0.05, 0.005, 0.0005, 0.00005, and 0.000005. After getting the results, we compare them with the result in the paper, which is the baseline of our analysis.

|lr              |EN    |SP    |FR    |RU    |
|----------------|------|------|------|------|
|lr=0.005 (paper)|57.9  |65.7  |48.6  |62.9  |
|lr=0.5          |56.7  |59.5  |51.9  |62.6  |
|lr=0.05         |58.6  |65.9  |45.9  |58.3  |
|lr=0.005        |58.4  |64.4  |51.2  |61.0  |
|lr=0.0005       |56.9  |66.9  |44.8  |58.4  |
|lr=0.00005      |58.1  |64.8  |59.8  |60.4  |
|lr=0.000005     |55.7  |60.0  |53.8  |58.1  |


|                          |                          |
:-------------------------:|:-------------------------:
|![](https://i.imgur.com/341xXRH.png)|![](https://i.imgur.com/2S2AoTZ.png)|
|![](https://i.imgur.com/Pzckzxg.png)|![](https://i.imgur.com/rAS4Msa.png)|


From the image above, we can see that the results for different learning rates are similar to the result in the paper. However, researchers of this paper did not choose the best learning rate, especially for the FR and RU datasets. FR dataset got the best performance when the learning rate equals 0.00005 with the F1 score of 59.8. For EN and SP datasets, the best results we got are similar to the results in the paper.  
Moreover, the reproduced curves do not have big fluctuations except for the FR curve, which means the performance of the model is not sensitive to the learning rate.


#### hyper-beta

|hyper-beta   |EN    |SP    |FR    |RU    |
|-------------|------|------|------|------|
|0.02 (paper) |57.9  |65.7  |48.6  |62.9  |
|0.2          |55.9  |64.3  |43.8  |59.5  |
|0.02         |58.4  |64.4  |51.2  |61.0  |
|0.002        |56.0  |62.9  |53.8  |55.2  |
|0.0002       |58.3  |65.0  |51.7  |58.3  |

|                          |                          |
:-------------------------:|:-------------------------:
|![](https://i.imgur.com/hre4WlR.png)|![](https://i.imgur.com/KDpUeAQ.png)|
|![](https://i.imgur.com/OwZ0Ce7.png)|![](https://i.imgur.com/bHMdFgN.png)|

The parameter hyper-beta is introduced by the Poincaré ball model, in which hyper-beta controls parameters related to the open unit ball. In order to explore whether the model is sensitive to this parameter, experiments are conducted by changing nothing but the value of hyper beta.

The results and the corresponding values of hyper beta are listed in the table above. It can be seen that the values of f1 fluctuate with the hyper-beta varying, which is different from the results in the original paper(Chang et al. 2021) that exactly remains the same.

According to the graph, we conclude that the hyper-beta does not affect the model directly because the value of f1 does not change monotonically as hyper beta changes monotonically. But we do not exclude the possibilities the hyper beta affects the model indirectly by affecting other parameters.


#### gb_temp

|gb_temp      |EN    |SP    |FR    |RU    |
|-------------|------|------|------|------|
|paper        |57.9  |65.7  |48.6  |62.9  |
|1e-1         |53.8  |54.7  |47.3  |53.3  |
|1e-2         |47.2  |54.4  |51.0  |56.4  |
|1e-3         |58.1  |60.9  |53.1  |58.5  |
|1e-4         |49.9  |54.9  |45.0  |55.4  |
|1e-5         |51.8  |62.2  |48.9  |59.1  |
|1e-6         |56.6  |59.1  |51.9  |63.2  |

|                          |                          |
:-------------------------:|:-------------------------:
|![](https://i.imgur.com/QDy2ZBZ.png)|![](https://i.imgur.com/CioB50a.png)|
|![](https://i.imgur.com/b3TSeDH.png)|![](https://i.imgur.com/ZJSVBlN.png)|

The hyperparameter gb_temp is used here to control to what enxtent the output is encoded as one-hot. From the images above, we can see that the hyperparameter gb_temp has not too much influence on the results because the reproduced lines do not have too much fluctuation. Therefore, the performance of the model is not very sensitive to the hyperparameter gb_temp. Moreover, it may seem that researchers have chosen the best gb_temp since the paper results are better than our reproduced results. However, for SP and RU datasets, our overall results are not as good as paper results. Actually, we cannot compare the results since the reproduced results for the same default hyperparameter also have large differences.

## Conclusion
In this project, we reproduced the results in the paper *Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction*. The reproduction has roughly similar results compared to the original paper, convincing the effectiveness of the disentangled representation model when handling the aspect extraction. Even though due to our devices and the limited time we could not fulfill all of the results experimented in the paper, we experimented with most of the representative experiments and do the hyperparameters check to see the influence of hyperparameters including learning rate, hyper-beta, and gb-temp. We also do the result analysis and plot the figure for a better comprehension of the whole project and model. 


## Contributions
- Lang Feng
    - Training, Reproduced table 1, Hyperparameters check (learning_rate), Result analysis, Blog
- Haoran Wang
    - Training, Reproduced table 1, Hyperparameters check (hyper_beta), Result analysis, Blog 
- Zhongbo Yao
    - Training, Reproduced table 1, Hyperparameters check (gb_temp), Result analysis, Blog
- Sasha Koryakin
    - Training, Reproduced table 2, Result analysis, Blog



## References
[1] Tai, Chang-You, Ming-Yao Li, and Lun-Wei Ku. "Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction." arXiv preprint arXiv:2112.09215 (2021). 

[2] Chami, Ines, et al. "Low-dimensional hyperbolic knowledge graph embeddings." arXiv preprint arXiv:2005.00545 (2020).

[3] Karamanolakis, Giannis, Daniel Hsu, and Luis Gravano. "Leveraging just a few keywords for fine-grained aspect detection through weakly supervised co-training." arXiv preprint arXiv:1909.00415 (2019).
