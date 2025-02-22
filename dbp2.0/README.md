# Knowing the No-match: Entity Alignment with Dangling Cases

> This paper studies a new problem setting of entity alignment for knowledge graphs (KGs). Since KGs possess different sets of entities, there could be entities that cannot find alignment across them, leading to the problem of dangling entities. As the first attempt to this problem, we construct a new dataset and design a multi-task learning framework for both entity alignment and dangling entity detection. The framework can opt to abstain from predicting alignment for the detected dangling entities. We propose three techniques for dangling entity detection that are based on the distribution of nearest-neighbor distances, i.e., nearest neighbor classification, marginal ranking and background ranking. After detecting and removing dangling entities, an incorporated entity alignment model in our framework can provide more robust alignment for remaining entities. Comprehensive experiments and analyses demonstrate the effectiveness of our framework. We further discover that the dangling entity detection module can, in turn, improve alignment learning and the final performance. The contributed resource is publicly available to foster further research.

## Dataset

The DBP2.0 dataset can be downloaded from the [figshare repository](https://doi.org/10.6084/m9.figshare.14872080). It has three entity alignment settings, i.e., ZH-EN, JA-EN and FR-EN. Each setting has the following files:

* ent_links: reference entity alignment;
* rel_triples_1: relation triples in the ZH or JA or FR KG, list of triples like (h \t r \t t);
* rel_triples_2: relation triples in the EN KG;
* splits/train_links: training data for entity alignment, list of pairs like (e1 \t e2);
* splits/valid_links: validation data for entity alignment;
* splits/test_links: test data for entity alignment;
* splits/train_unlinked_ent1: training data for dangling entity detection, list of dangling entities in the ZH or JA or FR KG;
* splits/train_unlinked_ent2: training data for dangling entity detection, list of dangling entities in the EN KG;
* splits/valid_unlinked_ent1: validation data for dangling entity detection, list of dangling entities in the ZH or JA or FR KG;
* splits/valid_unlinked_ent2: validation data for dangling entity detection, list of dangling entities in the EN KG;
* splits/test_unlinked_ent1: test data for dangling entity detection, list of dangling entities in the ZH or JA or FR KG;
* splits/test_unlinked_ent2: test data for dangling entity detection, list of dangling entities in the EN KG;

The statistics of DBP2.0 are listed below.

<table style='text-align:right; border:1px'>
    <tr>
        <th>Setting</th>
        <th>KG</th><th># Entities</th><th># Relations</th><th># Triples</th><th># Alignment</th>
    </tr>
    <tr>
        <td rowspan="2">ZH-EN</td>
        <td>ZH</td><td>84,996</td><td>3,706</td><td>286,067</td><td rowspan="2">33,183</td>
    </tr>
     <tr>
        <td>EN</td><td>118,996</td><td>3,402</td><td>586,868</td>
    </tr>
    <tr>
        <td rowspan="2">JA-EN</td>
        <td>JA</td><td>100,860</td><td>3,243</td><td>347,204</td><td rowspan="2">39,770</td>
    </tr>
     <tr>
        <td>EN</td><td>139,304</td><td>3,396</td><td>668,314</td>
    </tr>
    <tr>
        <td rowspan="2">FR-EN</td>
        <td>FR</td><td>221,327</td><td>2,841</td><td>802,678</td><td rowspan="2">123,952</td>
    </tr>
     <tr>
        <td>EN</td><td>278,411</td><td>4,598</td><td>1,287,231</td>
    </tr>
</table>

For dangling entity detection, we split 30% of dangling entities for training, 20% for validation and others for testing. The splits of reference entity alignment follow the same partition ratio.

## Source Code

> Coming soon! (I need some time to clean the code and integrate it into OpenEA)

### Dependencies
* [OpenEA](https://github.com/nju-websoft/OpenEA)

### Running 

> Due to the instability of embedding-based methods, it is acceptable that the results fluctuate a little bit (1-2%) when running code repeatedly.

> If you have any difficulty or question in running code and reproducing expriment results, please email to zqsun.nju@gmail.com.

## Citation
If you use the DBP2.0 dataset or our source code, please kindly cite it as follows:   
```
@inproceedings{DBP2,
  author    = {Zequn Sun and
               Muhao Chen and
               Wei Hu},
  title     = {Knowing the No-match: Entity Alignment with Dangling Cases},
  booktitle = {ACL},
  year      = {2021}
}
```
