# 最终实现内容如下
 * 支持多种三维模型文件格式✔️
 * *  OBJ✔️
 * *  FBX✔️
 * 支持多个光源的光照效果，使用着色器渲染✔️
 * * 支持漫反射、高光、法线、高度贴图✔️
 * * 四个光源：红色、蓝色、绿色、白色✔️
 * * 光源位置在不同平面圆周旋转✔️
 * 支持多种视点浏览方式✔️
 * * 以模型为中心的平移缩放旋转✔️
 * * 以视点为中心的场景漫游✔️
 
 
点击查看发布在bilibili上的[程序视频](https://member.bilibili.com/platform/upload/video/frame?type=edit&version=new&bvid=BV1cG4y117Ug)❤️

# 按键指南 
* **以视点为中心的场景漫游：** ‘W’,‘S’，‘A’，‘D’前后左右移动
* **平移：** 
* * 长按‘J’，向x轴正方向进行平移
* * 长按‘K’，向y轴正方向进行平移
* * 长按‘L’，向z轴正方向进行平移
* **缩放：**
* * 长按‘O’, 放大
* * 长按‘P’, 缩小
* **旋转：** 长按‘Z’，以Z轴为轴对模型进行旋转
---
# 解决方案

 ## 光照，太阳为光源
 
由于实现了作业的可选部分**自己使用顶点着色器和片段着色器来实现光照效果**，因此这里以太阳为光源的实现逻辑如下
* 将光源的位置设置成太阳的位置。
* 向着色器传递当前渲染的物体是否是太阳的信息
* 如果是太阳就只进行纹理贴图（这样看起来就是发光的效果），如果是月球和地球就进行正常的phong光照模型渲染。



 ## 纹理，使用图片进行纹理映射
step1:根据球体的公式生成球体的顶点，获得球体的顶点坐标、法线、纹理，并三个顶点为一组（三角形）地返回，见Sphere类

step2:使用soil库读入图像，见Utils类

step3:生成纹理

step4:应用纹理

 ## 使用顶点着色器和片段着色器，自己实现光照效果
 
 光照效果的实现使用的是Phong的反射模型，在Phong的反射模型中包含三个反射部分：环境反射、漫反射、镜面反射。
 
  ![image](https://user-images.githubusercontent.com/44937001/209655350-f651d690-7ea4-4701-ba63-4bcaaccd902c.png)
  
  因为本次作业内容为天体的光照，所以不考虑镜面反射，只实现环境光与漫反射。
  
 天体着色器代码见vertShader.vert与fragShader.frag
 
 ## 鼠标选择，点击不同球体显示不同名称
 step1:获取鼠标点击的坐标
 
 step2:从鼠标点击的坐标中获取前剪裁面坐标位置
 
 step3:发出一条相机出发，穿过前剪裁面坐标的射线
 
 step4:检测射线是否与各个天体的球体相交，返回相交距离最近的物体
 
 step5:贴图显示相交天体的名称

