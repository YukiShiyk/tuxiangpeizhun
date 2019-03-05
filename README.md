
# 数字图像处理第二次实验报告
## 图像配准
## 自动化66史玉康
## 2161800034

### （一）实验要求
要求根据已给的两幅图像，在各幅图像中随机找出7个点，计算出两幅图像之间的转换矩阵H，并且输出转换之后的图像。
注：已给图像分别为Image A和Image B。
### （二）实现过程
使用MATLAB实现点映射的图像配准包括以下一个步骤：

* 将图像读入到MATLAB的工作区
* 指定图像中的成对控制点
* 保存控制点对
* 指定要使用的变换类型
* 对待配准的图像进行空间几何变换，使之对准

### （三）实验结果



配对结果
![图像配准](https://raw.githubusercontent.com/YukiShiyk/tuxiangpeizhun/master/图像配准.PNG)

### （三）实验代码
```matlab
  clear all;
clc;
I1=imread('ImageA.jpg');
I2=imread('ImageB.jpg');
unregistered =I2;%%未配准图像
rect=I1;%%参考图像
cpselect(unregistered,rect);%%%选择点对，选完后记得保存
uiwait(msgbox('Click OK after closing the CPSELECT window.','Waiting...'));%创建一个按钮，等待用户反映
fixedPoints=round(fixedPoints);
movingPoints=round(movingPoints);
figure; imshow(rect)
hold on
plot(fixedPoints(:,1),fixedPoints(:,2),'xw') 
title('rect')
figure; imshow(unregistered)
hold on
plot(fixedPoints(:,1),fixedPoints(:,2),'xw') 
title('unregistered')
movingPointsAdjusted = cpcorr(movingPoints,fixedPoints,...
                              unregistered(:,:,1),rect(:,:,1));%调整控制点位置
%tform = cp2tform(movingPointsAdjusted,movingPoints,'linear conformal');%%控制点的空间变换
tform=fitgeotrans(fixedPoints,movingPoints,'similarity'); 
%registered = imtransform(unregistered,tform,'XData',[1 300], 'YData',[1 300]);%%对图像进行重采样
Iout=imwarp(I2,tform);   
subplot(2,2,1)
imagesc(rect)
title('Original image 1')
subplot(2,2,3)
imagesc(unregistered)
title('Unmatched image 2')
subplot(2,2,2)
imagesc(rect)
title('Original image 1')
subplot(2,2,4)
imagesc(Iout)
title('Matched image 2')
colormap (gray)

```
