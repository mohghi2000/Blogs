# Histograms of Oriented Gradients for Human Detection




 RGB colour space with no gamma correction.
 [-1,0,1] gradient filter with no smoothing.
 Linear gradient voting into 9 orientation bins in 0-180
 16×16 pixel blocks of four 8×8 pixel cells.
 Gaussian spatial window with variance=8.
 L2-Hys(Lowe-style clipped L2 norm) block normalization.
 Block spacing stride of 8 pixels(hence 4-fold coverage of each cell).
 64×128 detection window
 Linear SVM classifier


 Cell：统计梯度直方图的最小单元，论文中为8*8。
 Blocks：做直方图归一化的单元，论文中为2×2个cell，即block大小为16×16.

 使用gamma变换对图片进行归一化。
 计算每个像素点的梯度方向。
 在Cell中对梯度方向进行统计，得到直方图。
 在Block中对Cell的梯度直方图进行归一化，Block以窗口滑过每个Cell，Block可重叠。
 将每个block的直方图串联起来，形成整幅图片的特征向量。
 这里需要注意，每个Cell可能作为多个Block的子部分被归一化放到整幅图片的特征向量中。
 使用Linear SVM算法对特征向量进行分类，得到最终模型。


 Pixel representation: grayscale, RGB, LAB.
 Optionally with power law (gamma) equalization or log compression
 Best: LAB and RGB with square root gamma compression


 Various 1-D point derivatives
 3×3 sobel masks.
 2×2 diagonal ones (the most compact centred 2-D derivative masks)





 Calculate a weighted vote for each pixel based on the orientation of the gradient element centred on it.
 Accumulated into orientation bins over local spatial regions that called cells


 Gradient strengths vary over a wide range owing to local variations in illumination and fore-background contrast, so local contrast normalization is essential.
 Grouping cells into larger spatial blocks and contrast normalization each block separately.
 Overlapping of the blocks seems redundant but improves the performance significantly.
 论文中使用了两种算子：R-HOG和C-HOG。
 Vertical cell (2×1) and horizontal cell (1×2) are also considered.
 It’s useful to down-weight pixels near the edges of the blocks by applying a Gaussian spatial window to each pixel before accumulating orientation votes into cells.

 L2-norm
 L2-Hys
 L1-norm
 L1-sqrt


 Compare with previous algorithm
 Effect of gradient scale
 Effect of orientation bins’ number
 Effect of normalization method
 Effect of overlap
 Block大小与Cell大小不同带来的效果变化
 检测窗口大小不同带来的效果变化
 SVM参数带来的效果变化

 Miss Rate：错检率，所有判为有行人的sample中，被错判（没有行人被判为有行人）的样本比例。
 FPPW：False Positives Per Window，平均每个窗口的漏检率，漏检率为所有有行人的sample中，被判为没有行人的样本比例。平均到每个检测窗口

 [1]. Dalal N, Triggs B. Histograms of oriented gradients for human detection[C]//Computer Vision and Pattern Recognition, 2005. CVPR 2005. IEEE Computer Society Conference on. IEEE, 2005, 1: 886-893.
 [2].	http://blog.csdn.net/pp5576155/article/details/7023709
 [3].	http://blog.csdn.net/icvpr/article/details/8454527
 [4].	http://blog.csdn.net/carson2005/article/details/7782726