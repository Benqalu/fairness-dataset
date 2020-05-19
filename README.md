# Fairness Dataset

## 数据集

总共有四个数据集：
 - adult
 - broward
 - compas
 - hospital
 数据需要解压缩出.npz文件

## Sensitive Attribute
每个数据集的都有两个sensitive attribute：race 和 gender，均为binary。

## 数据文件内容
 每个数据集有5个内容，key分别是['X','Y','S']
 1. X是non-sensitive attribute的feature matrix
 2. Y是label，是一个2D-array，其中除了hospital之外皆为binary（shape=(?,1)），hosptial的label有4中，故shape=(?,4)
 3. S是sensitive attribute，固定有两列，第一列固定是race，第二列是gender，S要与X作为一个整体成为target model的input
 4. 所有数据均已做one-hot处理

## 输出格式需求
请求的输出格式：考虑到实验结果图的要求，请输出：
 1. X：target model input的原始数据；
 2. S：target model input 的 sensitive attribute；
 3. y：target model 的 true label；
 4. y_pred：target model在 X上预测的结果，是probability（如果是>=2 classes，那么就是softmax的结果，下同）
 5. y_pred_defense：target model在有defense 的情况下X上预测的结果，是probability；
 6. l：MIA 的 label，表示数据是属于training (1) 还是 testing (0)；
 7. l_pred：MIA在(4)上的预测结果（无defense），是probability；
 8. l_pred_defense：MIA在(5)上的预测结果（有defense）
 9. 以上数据(1)-(7)需要以原始的x为基准一一对应

## Dataset Setting
 - 不使用Shadow Model
 - 首先将数据集均分为两份，前50%为train_target,后50%为test_target，作为target model的train和test
 - 使用train_target的前50%，与test_target作为MIA的training dataset，然后使用

## 其他
如果defense 算法本身有调节参数（例如，defense budget），请条件参数从no defense 到最强defense的至少6个参数结果。如果是这种情况，因为某个参数已经包含了no defense 参数的结果，故需求中的(4)和(7)可以省略，用对应无defense 参数结果的(5)和(8)替代。