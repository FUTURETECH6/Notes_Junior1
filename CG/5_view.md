## 2D viewing transformation



**Aspect Ratio** 长宽比



gluOrthoD(left, right, bottom, top)

glViewport(x, y, width, height)





**projection**

* Perspective Projection
    * PRP (Projection Relative Point)
* Prallel Projection



## View specifications

<!--以正上方和相机上方的向量的叉积作为x轴-->

<!--正上方和x轴的叉积作为y轴-->

* VPR: view reference point
* viewDirection
* upVector (照片的上方向，大致即可，不一定要和viewDirection正交)

<img src="assets/image-20201020224447721.png" style="zoom: 67%;" />

新的坐标系：

<img src="assets/image-20201020224959826.png" style="zoom: 67%;" />



## World-to-View Transformation

* **View orientation**
    * <img src="assets/image-20201021120021992.png" style="zoom:67%;" />
* **Transformation for Perspective Projection**
    * <img src="assets/image-20201021120123457.png" style="zoom:67%;" />
* **View window**
    * <img src="assets/image-20201021120226216.png" style="zoom:67%;" />



**Results**

* Perspective
    * <img src="assets/image-20201021120507557.png" style="zoom:67%;" />
* Parallel
    * <img src="assets/image-20201021120516079.png" style="zoom:67%;" />



**View Volume**

* 透视投影：四棱台
* 平行投影：长方体