# aic-flickr8k-rev
Image captioning has become a fundamental operation that allows the automatic generation of text descriptions of images. However, most existing work focused on performing the image captioning task in English, and only a few proposals exist that address the image captioning task in Arabic. This paper focuses on understanding the factors that affect the performance of machine learning models performing Arabic image captioning (AIC). In particular, we focus on transformer-based models for AIC and study the impact of various text-preprocessing methods: CAMeL Tools, ArabertPreprocessor, and Stanza. Our study shows that using CAMeL Tools to preprocess text labels improves the AIC performance by up to 34-92% in the BLEU-4 score. In addition, we study the impact of image recognition models. Our results show that ResNet152 is better than EfficientNet-B0 and can improve BLEU scores performance by 9-11%. Furthermore, we investigate the impact of different datasets on the overall AIC performance and build an extended version of the Arabic Flickr8k dataset. Using the extended version improves the BLEU-4 score of the AIC model by up to 148%. Finally, utilizing our results, we build a model that significantly outperforms the state-of-the-art proposals in AIC by up to 196-379% in the BLUE-4 score.
\
Paper: [Here](https://www.sciencedirect.com/science/article/pii/S131915782300304X)


## Experimental Evaluation



### Comparison with the previous studies.

Our best-performing model (using ResNet152 and CAMeL preprocessing) is compared with previous studies ElJundi et al. [28], Hejazi et al. [29], and Emami et al. [30]. In all previous works, the Arabic Flickr8k dataset is used for training their models. Table 12 summarizes all the results for all metrics in our study. Figure 14 show the comparison results of B1, B2, B3, and B4 scores. Using the Flickr8k dataset, our model scored 0.4894, 0.317, 0.213, and 0.145 for B1, B2, B3, and B4, respectively. We improve the result using the same dataset by 25-47%, 29-64%, 42-103%, and 58-154%, for B1, B2, B3, and B4, respectively. Using the Flickr8k-Rev dataset, our model significantly improved over previous studies and scores 0.616, 0.47, 0.359, and 0.273 for B1, B2, B3, and B4, respectively. This is an performance improvement of 58-86%, 91-144%, 139-242%, and 197-379%, for B1, B2, B3, and B4, respectively. The above results show that our proposed model can achieve better results up to an order of magnitude.
\
![Comparison table](https://raw.githubusercontent.com/AshwagAlsayed/aic-flickr8k-rev/main/tab-results.png "Comparison with previouse resuts")

### Qualitative Results.
Figure 15 shows three sample images from the test set. The captions labeled with 1 are generated using the Flickr8k-Rev dataset, while the captions labeled with 2 are generated using the Flickr8k dataset. In both cases, we used CAMeL Tools as the preprocessor. In all three examples, the captions with 1 labels are more accurate and have fewer errors. In Figure 15a, the second sentence suffers from additional "ة" characters in the last word. The second caption in Figure 15b has a grammatical error. Finally, in Figure 15c, the first caption also describes the color of the dog as "بني", which means brown. Because Flickr8k has some spelling and grammatical errors, these errors are carried over to the results.
\
![Qualitative Results](https://raw.githubusercontent.com/AshwagAlsayed/aic-flickr8k-rev/main/qual-results.png "Qualitative Results")

## Citation

Please cite this [paper](https://www.sciencedirect.com/science/article/pii/S131915782300304X):
<pre><code>
@article{alsayed2023performance,
  title={A performance analysis of transformer-based deep learning models for Arabic image captioning},
  author={Alsayed, Ashwaq and Qadah, Thamir M and Arif, Muhammad},
  journal={Journal of King Saud University-Computer and Information Sciences},
  volume={35},
  number={9},
  pages={101750},
  year={2023},
  publisher={Elsevier}
}
</code></pre>

## References
[28] O. ElJundi, M. Dhaybi, K. Mokadam, H. M. Hajj, D. C. Asmar, Resources and end-to-end neural network models for arabic image captioning., in: VISIGRAPP (5: VISAPP), 2020, pp. 233–241.
\
[29] H. Hejazi, K. Shaalan, Deep learning for arabic image captioning: A comparative study of main factors and preprocessing recommendations, International Journal of Advanced Computer Science and Applications 12 (11) (2021).
\
[30] J. Emami, P. Nugues, A. Elnagar, I. Afyouni, Arabic image captioning using pre-training of deep bidirectional transformers, in: Proceedings of the 15th International Conference on Natural Language Generation, 2022, pp.40–51.
