ѡ��μ� Quora �ⳡ������Ҫ��Ϊ��ѧϰһ�� NLP����Ȼ���Դ����������ڹ�� kagglers ��˽�ķ����ջ�ͦ��

�ȼ��ܽ�һ���ҵķ�������Ҫʹ�������� model��
 1. �ݶ�������LightGBM��
 2. ���ѧϰ��LSTM with concatenation �� Decomposable attention��

ֵ��һ��������ѧϰ����������У�Ч�����Ǻܾ��޵ģ��������������̣��˵��˵Ķ������ơ������� Quora ������������ pretrained embedding �ʻ������Ƿ�Χ�����ݼ���Ҳ���ڲ���ƴд���󣬶˵��˵����ѧϰ �ܿ�͵�����ƿ��������������� deep model �� lgb model �� stacking���� deep model �� lgb model ��������ȡ�������� deep model concate һЩ ��������ȡ���˲���Ľ����

# ���
����򵥽���һ�±�������α��������췽 Quora ������һ��������֪ʶ�ʴ���վ��Quora һ����Ҫ�Ĳ�Ʒԭ���ǣ�����ÿ��ҳ�涼�ǲ�ͬ�����⣬��������Ļش�����Ҫ�ش��ظ������⣻Ҳ���������𰸵��û���һ��ҳ����ܻ�ȡ��Ҫ����Ϣ����һ���򵥵����ӣ���What is the most populous state in the USA?�� �� ��Which state in the United States has the most people?�� ������������Ȼ������ͬ��������ͬ�����壬Ӧ����һ������ҳ����֡�

# Data Exploration
## ���ݼ�
Quora ѵ�����ݼ���40������������Ƶ�����ԡ����£�  
![](https://leanote.com/api/file/getImage?fileId=5950739bab64416f6a000c65)  
������id��qid1��qid2�������⣨question1��question2�������Ƿ��ظ���is_isduplicate����������ɡ�

## ����ָ�� evaluation
������Ϊ�Ƿ����ظ�����ĸ��ʣ�ʹ�� log loss ��Ϊ���۱�׼����ѧ���ʽ���£�  
![](https://leanote.com/api/file/getImage?fileId=5950739bab64416f6a000c66)  
�������������������������ָ�껹�У�

 - Precision��P=TP/(TP+FP)
 - Recall��R=TP/(TP+FN)
 - F1-score��2/(1/P+1/R)
 - ROC/AUC��TPR=TP/(TP+FN), FPR=FP/(FP+TN)

## ��ƽ��� train��test ���ݼ�
��������һ�� Quora �����еĵ�һ��С���ӡ���train �� test ����������ռ�ı����ǲ�ͬ�ġ�

���ȣ����ǿ��Լ���� train ���ݼ�������ƽ��ֵΪ p = 0.3692���Գ��� 0.3692 ΪԤ������һ���ύ���õ��� test �ϵ� Public LB ����Ϊ 0.55������ train �ϵ� log loss�����Ϊ 0.6585�����Կ��� train��test ���ݼ��ֲ��ǲ�����ġ�

�������Ǽ���һ�� test ���ݼ������ľ�ֵ�����ݣ�
 - ������ log loss �Ķ���
 - p = 0.3692 ʱ��logloss Ϊ 0.554

 �ɵã�  
![](https://leanote.com/api/file/getImage?fileId=5952131eab6441560e0013a3)  
r Ϊ����������r = 0.174���Գ��� 0.174 �ٴ��ύ��Public LP Ϊ 0.463��
���Կ�����test ���ݼ����ظ�����ı����� train ��С�˲��١���ô��ν�������ƽ��������أ�Quora �����д����Ҫʹ�������ַ�����
 - �� train �з��ظ�����ԣ����������й�������ʹ train ������������Ϊ 0.174
 - ͨ��һ��ת���������� train ��ѵ��ģ�͵�Ԥ����ת���� test ���ݼ��ķֲ���
 - ͨ�� Keras �� class_weight ����Ȩ��
 
����1��������������ѵ�������ָ�ѵ��������֤����ʱ����Ҫע��Էָ������ݼ��ֱ�����������������������й¶�����ڱ��α�������Ҫʹ���˷���2�����£�
 - a = 0.174 / 0.3692, b = (1 - 0.174) / (1 - 0.3692)
 - f(x) = a * x / (a * x + b * (1 - x))
ͨ��ת����f(0) = 0; f(1) = 1; f(0.3692) = 0.174  
![](https://leanote.com/api/file/getImage?fileId=59522cbdab644153e30015fe)  
�����Ƶ��͸���ϸ�ķ������Բο���
[How many 1's are in the Public LB?](https://www.kaggle.com/davidthaler/how-many-1-s-are-in-the-public-lb)
[Statistically valid way to convert training predictions to test predictions](https://www.kaggle.com/c/quora-question-pairs/discussion/31179)

# ��������
���������ǻ���ѧϰ�Ļ�����ͬʱҲ�ǳ����ص�һ�������Ҫ��һ����ҵ��֪ʶ�Ͷ����ݵĶ����������������ж���Ҫ�أ�����һ�仰�����ݺ����������˻���ѧϰ�����ޣ���ģ�ͺ��㷨ֻ�Ǳƽ�������ޡ���������һ�仰��Garbage in, garbage out�����ڴ󲿷� Kaggle �������㷨�ǱȽϹ̶��ģ������������Ǿ����ɼ���ؼ������ء�

ʲô���������̣����ǿ�һ����������ͼ���ܽᣬ�ο�[����](https://www.zhihu.com/question/29316149)��  
![](https://leanote.com/api/file/getImage?fileId=59547652ab644175ca0009f3)  
Quora ��������ʹ�õ�������200�����ң���Ҫ���Է�Ϊ�����֣�

 - nlp �����������ı��ھ����������磺������ַ����Ȳ�������ͬ���ʸ�������/��TfidfȨ�أ�������� Embedding �ĸ��� distance���ȵ�
 - ������������ train��test �������ɵ� graph ��ȡ�ĸ�������������Ϊ node�������Ϊ edge�������磺�������Ƶ�ʣ���������������⹲ͬ�ھӵĸ���������� PageRank���ȵȡ����������� Kaggle ��̳�����˲������飬�󲿷ֶ���Ϊ���� Quora ��ĳ�ֹ����챾�α������ݼ����µ� leakage��

## nlp ����
�ı�����������
 - �ַ����ȡ����Ȳ����
 - ���ʳ��ȡ����Ȳ����
 - ƽ�����ʳ��ȡ���ֵ
 - ���ַ���д���ʸ�������ֵ

��ͬ����������
 - ��ͬ���ʸ���������
 - ���� Tfidf Ȩ����ͬ���ʸ���������
 - ȥ��ͣ�ô���ͬ���ʸ���������
 - ��ͬ���ʸ���������
 - ȥ��ͣ�ô���ͬ unigrams��bigrams��trigrams ���ʸ���������
 - �ʸɻ���ͬ���ʸ���������

Embedding �������ֱ�ʹ���� glove��google news��fasttext����
 - cosine/euclidean/jaccard/minkowski/... distance
 - word mover's distance������ wmd �򵥵Ľ��ܿ��Բο�[����](https://www.zhihu.com/question/29978268?sort=created)

Fuzzy ���������� [fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy)����
 - QRatio��WRatio��partial ratio...

SimHash ���������� [simhash](https://github.com/leonsim/simhash)��:
 - word/word bigrams/word trigrams distance
 - character bigrams/character trigrams distance

Tfidf Vector/Counter Vector ������
 - tfidf mean/sum/len
 - ���� Counter Vector��char���� unigram/bigram/trigram jaccard ����
 - ���� Tfidf��char���� unigram/bigram/trigram euclidean ����
 - ���� Tfidf��char���� oof��out of fold������
 - ���� Tfidf��char���� SVD��[Singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition)����SVD oof��������
 - ���� Tfidf��word���� euclidean ����
 - ���� Tfidf��word���� oof ����

POS��part of speech����NER��named entity recognizer������������ Stanford CoreNLP:
 - Noun��n����Adjective��a����Verb��v����Personal Pronoun��prp����WH-Pronoun��wp����Numbers��cd�� �� POS �������ο�[��ǩ](https://stackoverflow.com/questions/1833252/java-stanford-nlp-part-of-speech-labels)��
 - NER ƥ������

���治������������������ kagglers �ķ����μ� Kaggle ����һ��Ҫ������ע��̳������������ top teams �����һЩ���ǵ�ȡʤ����������ֱ���� github ��Դ��

## ����������magic feature��
����������������� Quora �����������Ĳ����ˡ���������Ч��֮�ã������ڴ�Ҷ����òμ���һ���� NLP ������

�ǵõ�ʱ����������һ����ʱ���ҵĳɼ���0.20+�������ᵽ�������� Public LB������ top teams �ĳɼ����ǳ���0.13+�����뷽�跨���� nlp �������Ľ� model��ȴֻ�ܵõ������޵����������������� kagglers ���������Ƿ��ֵ� magic feature��
 - ������������ݼ��е�Ƶ��Խ�ߣ����ظ�����ĸ���Խ��
 - ������е��������⣬��ͬ�ھ�Խ�ࣨ��֪����� a - b���� a �� b ��Ϊ�ھӣ������ظ�����ĸ���Խ��

�����������ھ�һ�������������������������塣��������һ����α��������ݹ��ɣ�����ԣ�train��test ���ݼ�ÿһ�ж���һ������ԡ�
����֪��������Ȼ״̬�����ⲻ�ǳɶԳ��ֵģ���Ȼ��ͨ��ĳ�ַ�ʽ���������������Щ���������ι��ɵ��أ����ǲ��ö�֪��Quora �����Ҳû�л�Ӧ������⡣������ Quora �ٷ���[����](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs)�У����Է���һЩ��˿����

> Therefore, we supplemented the dataset with negative examples. One
> source of negative examples were pairs of ��**related questions**�� which,
> although pertaining to similar topics, are not truly semantically
> equivalent.

���Կ�������ԵĹ��ɺ͡�������⡱�йأ�������ͬһ����ǩ��e.g.���ѧ���µļ������������Ƚ�����������ԡ���������������Ӧ�������� Quora ��������Ա���ĳЩ���ɡ�

����ھ����������أ����԰����ݼ��г��ֵ�ÿ�����⿴��һ���ڵ㣨node����ÿ������Կ����ڵ�ıߣ�edge������ˣ��͹�������״�ṹ������չʾ��������һ����ͼ��[�ο�](https://www.kaggle.com/davidthaler/duplicates-of-duplicates)����  
![](https://leanote.com/api/file/getImage?fileId=59562829ab64414a81001826)  
��Ȼ��ʵ�� NLP ��������Щ������������ûʲô�ã����Ǵ� Quora ������������У����ǿ��Է��ֺܶ�����˼�Ķ��������磬�������ÿ������� PageRank�������ҳ�ÿ����ͼ�еĽڵ㡢�����ȵȡ��������ڵ����ڵ��ţ�[clipue](https://en.wikipedia.org/wiki/Clique_%28graph_theory%29)����С���֤��Ϊ����Ч�� magic feature��

# Models
�ڱ�������Ҫʹ�������� model���ݶ����������ѧϰ���ݶ������õ� LightGBM���� xgboost �ٶȿ��˲��٣����ѧϰ�õ� Keras��backend �� TensorFlow��
ʹ����ȫ�ֵ� 5 Fold �� validation������ Stacking��
## LightGBM
�ݶ�������һ�ֻ��ھ��������㷨���� Kaggle �����п������������㷨�������ʷǳ��ߣ����Ҿ�������Ϊ best single model�����������׼ȷ�ʸߣ��ٶȿ죬���Ҷ������еĸ��ֿ�ֵ��infֵ�����ޣ�ʹ�������ǳ����㡣[LightGBM](https://github.com/Microsoft/LightGBM) ��΢��Դ��һ�������ܵ��ݶ�����ʵ�֡�Ҳ������ Quora �����е��������ֱ�����ѵ�� L1 base model���� L2 stacking��

### ��������
 - num_leaves��������Ҷ���������������ĸ��Ӷȡ�
 - min_data_in_leaf����Ҷ�ڵ����������ݸ�������ߴ�ֵ���Խ��� over-fitting ����
 - learning_rate��ѧϰ����
 - feature_fraction��ÿ�ε������ѡ��һ���� feature ����ѵ�����������ѵ���ٶȣ����� over-fitting ����
 - bagging_freq��ÿ���� k �Σ���һ�� bagging���� bagging_fraction һ����Ч
 - bagging_fraction�� ÿ�ε��������ѡ�񲿷� data������ѵ�������ѵ���ٶȣ����� over-fitting

����ɼ� [Parameters Tuning](https://github.com/Microsoft/LightGBM/blob/master/docs/Parameters-tuning.md)��[Parameters](https://github.com/Microsoft/LightGBM/blob/master/docs/Parameters.md)
## Deep Models
Deep Learning �⼸���� NLP ������ٷ�չ���������롢��ͼ˵�����Զ�ժҪ���ּ����������в��� kaggler ������֪�� Deep NLP �ж��������ƶȵı����������ģ�Deep NLP �� Quora ������Ҳû���ô��ʧ�����˵��˵�ѧϰ��ʽ�������˷�ʱ�������������̡��������һ�����ڱ�����ʹ�õ����� deep model�����⣬���������󿴵��� kaggler ʹ�� 1D CNN ����˷ǳ�����ĳɼ�������Ҳһ������¡�

### LSTM with concatenation
������һ�� model �Ľṹ������ͼ������ Quora [����](https://engineering.quora.com/Semantic-Question-Matching-with-Deep-Learning)��  
![](https://leanote.com/api/file/getImage?fileId=5957047dab64412309000c2c)  
�ֱ�ʹ���� [GloVe](https://nlp.stanford.edu/projects/glove/)��[Google News](https://code.google.com/archive/p/word2vec/)��[fasttext](https://github.com/facebookresearch/fastText/blob/master/pretrained-vectors.md) ѵ���õĴ�������Ҳ�������� train��test ���ݼ��е�������Ϊ������ѵ����������Ч��һ�㣩������Щ����������������Ե� embedding��������� embedding ���� LSTM �㣬�õ���������� representation����ƴ�Ӻ����� Dense layer���õ���������
�򵥵� 5 fold��reweighted�����Եõ� 0.26 ���ҵĳɼ�����΢��һ�½ṹ��ƴ��һЩǰ���ᵽ�� NLP �������������������Դﵽ 0.14 ���ҡ���Ҫע������ֵ�ı�׼����ȱʡֵ����

### Decomposable attention
��� model ���� attention ���ƣ����� Google Research ����[����](https://arxiv.org/abs/1606.01933)��������� attention �� token alignment��decomposable attention ���������ǣ������٣������� model ���ѵ��Ч��û�н��ͣ�ѵ���ٶ������һ������ͨ��������е�ÿһ�Ե�����ѵ��һ�� attention model��soft alignment����Ȼ��Ƚ϶����Ķ�������ܱȽϽ�����ж��Ƿ�������ظ�������ͼ������[����](https://arxiv.org/abs/1606.01933)����  
![](https://leanote.com/api/file/getImage?fileId=5958b5f2ab644174f30012ee)  
ͬ������ͨ�� concate ������߳ɼ���

### 1D CNN
���� LSTM��CNN Ҳ���������� Deep NLP model �С�LSTM ������Ҫ���м�������� embedding�����ܱ� CNN ��һЩ���� CNN ͨ��ʹ�ò�ͬ�� kernel size ����ģ��ͬ����Ĺ�ϵ��kernel size Ϊ 3�����Կ��� trigrams words�������Գ�����������Ƚ��Ѵ���Quora ���ݼ��������е��ʵĸ�����ֵԼΪ 11��std ԼΪ 6������ CNN �� Quora ���ݼ��ϵı��ֻ��Ǻܺõġ�

Quora ��������ʵ�ֿ��Բο�[����](https://www.kaggle.com/rethfro/1d-cnn-single-model-score-0-14-0-16-or-0-23)������� embedding �󣬷ֱ����벻ͬ kernel size��1 ~ 6�� �ľ���㣬ÿ�������������� GlobalAveragePooling1D �ػ���ѹ����Ȼ�������������� absolute difference �� multiplying��û��ֱ�� concatenate�������ͨ�� Dense �㣬�õ������ƥ������

# Ensemble
[Ensembel learing](https://en.wikipedia.org/wiki/Ensemble_learning) ��ͨ��ѵ�����ֲ�ͬ�Ļ���ѧϰ�㷨���ֱ�����Ԥ�⣬Ȼ��ͨ��ĳ�ַ�ʽ�������Ȩƽ����stacking��bagging �ȵȣ�����ЩԤ�������������Ϊ���ս�������� model ��Ԥ����Խ׼ȷ��ͬʱ��ͬ model ��������Խ�ͣ�ensemble Ч��Խ�á�

Ensemble �� Kaggle �����зǳ���Ҫ��һ�����ڡ�top team emsemble ��ʮ�ϰ� model �ǳ����������� Quora ������ѵ���� 10 ������ model��ʹ���� stacking �ͼ�Ȩƽ������ emsemble �������Ƚ���һ�� stacking���򵥵�˵����ͨ��ѵ��һ������ѧϰģ�����ܽἸ������ѧϰģ�͵�Ԥ������Ȼ����������Ԥ�⡣��ͼ������[����](https://dnc1994.com/2016/04/rank-10-percent-in-first-kaggle-competition/)��չʾ��һ�� 5 fold stacking ���ӣ�  
![](https://leanote.com/api/file/getImage?fileId=5959c835ab644133a3000baf)  
�� 4 �� fold����ɫ���Σ���ѵ����һ�� model��Ȼ���� 1 �� fold����ɫ���Σ���Ԥ���������� 5 ��ѵ���󣬽��õ��� 5 ��Ԥ������5 ����ɫ���Σ������������Ϊ�ϲ� model ��һ�� feature������ emsemble ��ϸ���ܿɼ�[����](https://mlwave.com/kaggle-ensembling-guide/)��

�������ҵĵ�һ�� model ʹ���� 1 �� lgb model��2 �� lstm concate features model��2 �� decomposable attention concate features model���ڶ���ʹ����һ�� lgb model �� stacking���õ���� A��0.127+��Public LB�������⣬��ѵ���� 1 �� lgb model�������� lstm �� decomposable attention oof prediction ��Ϊ�������õ���� B��0.128+����A �� B pearson �����ԼΪ 0.98��ȡƽ��ֵ�õ����ս����0.125+����Ҳ���˶� A �� B ��һ�ε����� stacking������������롣�� deep model oof prediction ��Ϊ�������ǽ���ǰ����ų��Եģ�û�뵽�ܵõ��� stacking ���Ľ����

# Postprocess
���ύ���ǰ����Ԥ��������ǰ���ᵽ��ת�� f(x) = a * x / (a * x + b * (1 - x))����ƽ�� train��test �ֲ��Ĳ��졣

���˵һ�±�����������̳������һ������˼�� postprocessing��[����](https://www.kaggle.com/divrikwicky/semi-magic-postprocess-0-005-lb-gain)�������߻��ڣ�������� A �� B �ظ������� B �� C �ظ��������� A �� C ҲӦ���ظ��Ĵ����ԣ���Ԥ������������У׼��������õ��ύ��������һ�£������ύ�����0.04+��0.121+ Public LB��0.125+ Private LB��������һЩ top team �����е���ֱ�Ӷ���������Խ���������ȡ��Ȼ�� stacking��Ҳ��Ч����Ⱥ���ʼ��ʱ�����ù�����Ĵ�����������ѵ������a = b��b = c �� a = c��a ��= b��b = c���� a ��= c����û����߾ͷ����ˣ����Ȼ�����������

# Lessons Learned
����һЩ top team ����ķ������о��Լ���������ߵĵط���
 - ѵ����������� model������ random forest��knn �ȣ����Ӷ�����
 - deep model ��������ر��ȶ������Գ�����ÿ�� fold ���ò�ͬ�� seed �� bagging ����
 - ƴд��飨������ʹ���� PyEnchant��û�ио�����������ߣ������ˣ����ı�����
 - ���Ի����ַ��� deep model������ƴд����Ӧ���и��ߵĵֿ��������� model ������

Quora �������������ܣ�����Ȥ��ͬѧ���Կ���[����](https://www.kaggle.com/c/quora-question-pairs/discussion/34325)
# �ο�

 - [LSTM with word2vec embeddings](https://www.kaggle.com/lystdo/lstm-with-word2vec-embeddings)
 - [How many 1's are in the Public LB?](https://www.kaggle.com/davidthaler/how-many-1-s-are-in-the-public-lb)
 - [Magic Features (0.03 gain)](https://www.kaggle.com/jturkewitz/magic-features-0-03-gain)
 - [Duplicates of Duplicates](https://www.kaggle.com/davidthaler/duplicates-of-duplicates)
 - [PageRank on Quora - A basic implementation](https://www.kaggle.com/shubh24/pagerank-on-quora-a-basic-implementation)
 - [1D CNN (single model score: 0.14, 0.16 or 0.23)](https://www.kaggle.com/rethfro/1d-cnn-single-model-score-0-14-0-16-or-0-23)
 - [Semi-Magic PostProcess (0.005 LB gain)](https://www.kaggle.com/divrikwicky/semi-magic-postprocess-0-005-lb-gain)
 - [Another data leak](https://www.kaggle.com/c/quora-question-pairs/discussion/33287)
 - [0.29936 solution](https://www.kaggle.com/c/quora-question-pairs/discussion/32313)
 - [Is That a Duplicate Quora Question?](https://www.linkedin.com/pulse/duplicate-quora-question-abhishek-thakur)
 - [Keras Decomposable Attention](http://git.iguokr.com/mobile_group/Onigiri_IOS/merge_requests/291)