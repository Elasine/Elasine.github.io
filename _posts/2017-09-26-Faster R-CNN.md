2017-09-26-Faster R-CNN

为什么要有ROI？

大量的regin proposals;处理速度慢；无法端对端

提出ROI pooling用于目标检测，允许对CNN中的特征图进行reuse，可以实现训练和测试的显著加速，可提高检测的精度，并且该层有两个输入：

1. 从具有多个卷积核池化的深度网络中获得固定大小的特征图
2. 一个表示所有ROI的N*5矩阵，其中N表示ROI的数目，第一列表示图像的索引，其余四列表示其余的左上角和右下角坐标。

其具体操作如下：

1. 根据输入image,将ROI映射到特征图对应位置

2. 将映射后的区域划分成相同大小的sections

3. 对每个sections进行max pooling 操作


非极大值抑制NMS的具体实现

```Python
import numpy as np

def nms(dets,theshold):
    x1=dets[:,0]
    y1=dets[:,1]
    x2=dets[:,2]
    y2=dets[:,3]
    scores=dets[:,4]
    
    areas=(x2-x1+1)*(y2-y1+1)
    order=scores.argsort()[::-1]
    keep=[]
    while order.size>0:
        i=order[0]
        keep.append(i)
        xx1=np.maximum(x1[i],x1[order[1:]])
        yy1=np.maximum(y1[i],y1[order[1:]])
        xx2=np.minimum(x2[i],x2[order[1:]])
        yy2=np.minimum(y2[i],y2[order[1:]])
        
        w=np.maximum(0.0,xx2-xx1+1)
        h=np.maximum(0.0,yy2-yy1+1)
        inter=w*h
        ovr=inter/(areas[i]+areas[order[1:]-inter])
        
        inds=np.where(ovr<=thresh)[0]
        order=order[inds+1]
   	return keep
```

IOU的具体实现

```python
def iou(bbox1,bbox2):
    x0,y0,x1,y1=bbox1
    x1,y1,x2,y2=bbox2
    s1=(x1-x0)*(y1-y0)
    s2=(x3-x2)*(y3-y2)
    w=max(0,min(x1,x3)-max(x0,x2))
    h=max(0,min(y1,y3)-max(y0,y2))
    inter=w*h
    iou=inter/(s1+s2-inter)
    return iou
```

AP计算

```python
def voc_ap(rec,prec,use_07_metric=False):
	if use_07_metric：
    		#11 point metric
        ap=0
        for t in np.arange(0.,1..1,0.1):
            if np.sum(rec>=t)==0:
                p=0
            else:
                p=np.max(prec[rec>=t])
            ap=ap+p/11
     else:
         mrec = np.concatenate(([0.], rec, [1.]))
         mpre = np.concatenate(([0.], prec, [0.]))
         for i in range(mpre.size - 1, 0, -1):
             mpre[i - 1] = np.maximum(mpre[i - 1], mpre[i])
         i = np.where(mrec[1:] != mrec[:-1])[0]
         ap = np.sum((mrec[i + 1] - mrec[i]) * mpre[i + 1])
     return ap
```

