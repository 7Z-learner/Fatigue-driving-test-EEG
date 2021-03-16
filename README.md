# Fatigue-driving-test-EEG
使用BCI设备采集EEG信号，预处理去除噪声和伪迹，采取EMD、ICA提取特征，特征值选定为模糊熵和样本熵，分类器选择3分类器集成框架或ABC-PSO算法，疲劳分类结果无线传输至Android端APP，用户可自行选择抗疲劳措施，实现行车安全。
### 系统搭建学习记录
#### 1.数据预处理
基本流程：导入数据->检查补充数据（channel locations）->基本处理（tools中重参考、采样率、滤波等）->去除眼电伪迹（run ICA）->分段（extract epochs）->

打开MATLAB，导入EEGLAB，载入数据包，窗口显示EEG数据的详细信息。信息主要含义如下：
- channel per frame：导入的数据通道个数
- frames per epoch：一段数据的总长度，采样点个数
- epochs：指当前数据的段数，原始数据还没有进行分段，因此显示只有一段
- events：检测到当前数据一共有202个events
- sampling rate（HZ）：数据的采样率为1000HZ
- epoch start和epoch end：这个的分段是从0秒开始，到439.879秒结束。还没有进行分段所以看这个数值没有意义
- reference: 指数据的参考点，重参考后会显示重参考的电极点，或者average，目前还没有进行重参考所以是unknown
- channel locations：是否有对通道进行定位，目前显示没有，定位后会显示为yes
- ICA weights：是否对数据进行了ICA独立主成分分析，分析后会显示yes
- dataset size：数据的大小

点击“channel locations”，共33个电极，其中HEO和VEO没有具体数据，可以根据给定的样本数据文本文件，自行添加数据，输入极坐标，点击转换键，可以转换出直角坐标系和球坐标系。

点击“tools”，可对数据做许多处理，如改变采样率、重参考、滤波等，重参考可以选择单电极作为参考，也可删除不必要的电极，一般来说，重参考在前，滤波在后。

“run ICA”可检测EEG数据中的眼电部分，从而针对性删除。数据时间较长，可对波形先分段，再“run ICA”。先滤波，再run ICA。进程结束后，窗口首页ICA Weights显示YES，导入执行完run ICA的文件。run ICA结束后，将处理后的数据保存为.set形式，以便剔除失误，可重新加载原始数据。选择“reject  component by maps”打开每个ICA成分的缩略图，reject包含伪迹的成分，倘若要完全移除伪迹成分，点击“remove component”。而后，“dataset”中会有remove后的.set文件。但还未保存在文档中。

点击“dataset”，有历史修改处理后数据的存档，选择其中一个，点击“plot”，可显示数据波形图，鼠标选择波形段落，可自行删减。



