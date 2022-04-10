## Reproduction Blog: Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction

## Authors:
Lang Feng,  
Haoran Wang - 5468175,  
Zhongbo Yao,  
Sasha Koryakin

## Introduction
This blog aims to describe our efforts in reproducing the paper "Hyperbolic Disentangled Representation for Fine-Grained Aspect Extraction", written by Chang-You Tai, Ming-Yao Li, and Lun-Wei Ku, 2021.


#### HDAE



## Datasets




## Reproducibility Approach
### Reproducing Table 1
|           |Bags |KBs  |Boots|B/T  |TVs  |VCs  |
|-----------|-----|-----|-----|-----|-----|-----|
|HDAE(paper)|68.8 |72.2 |64.0 |72.0 |71.2 |66.9 |
|Reproduced |66.9 |69.2 |63.3 |67.3 |65.5 |60.9 |






### Reproducing Table 2

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


![Boxplot of F1 scores per language, black crosses repesent scores found in paper](table1/table1_boxplot.png)

### Hyperparameters Check
##### lr (Learning Rate)
|lr              |EN    |SP    |FR    |RU    |
|----------------|------|------|------|------|
|lr=0.005 (paper)|57.9  |65.7  |48.6  |62.9  |
|lr=0.5          |56.7  |59.5  |51.9  |62.6  |
|lr=0.05         |58.6  |65.9  |45.9  |58.3  |
|lr=0.005        |58.4  |64.4  |51.2  |61.0  |
|lr=0.0005       |56.9  |66.9  |44.8  |58.4  |
|lr=0.00005      |58.1  |64.8  |59.8  |60.4  |
|lr=0.000005     |55.7  |60.0  |53.8  |58.1  |


##### hyper-beta
|hyper-beta   |EN    |SP    |FR    |RU    |
|-------------|------|------|------|------|
|0.02 (paper) |57.9  |65.7  |48.6  |62.9  |
|0.2          |55.9  |64.3  |43.8  |59.5  |
|0.02         |58.4  |64.4  |51.2  |61.0  |
|0.002        |56.0  |62.9  |53.8  |55.2  |
|0.0002       |58.3  |65.0  |51.7  |58.3  |


## Conclusion







## Contributions





## References


