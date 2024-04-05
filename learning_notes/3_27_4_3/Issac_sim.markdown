# Week report -- 3/27 Issac Sim

## Get started
1. 创建对象
    - Create -> Shapes -> Cubes  
      
2. 调整对象：
    - E：转换为旋转模式，点击轴线按住旋转。多次点击可在全局状态和物体本地状态下切换旋转。本地状态下，图标显示为橙色
    - R：转换为缩放模式，按住轴线进行缩放
    - W：转换为移动模式，按住物体进行移动。再按一次，在物体的轴线上进行移动。多次点击可在全局状态和物体本地状态下切换平移
    - ESC：取消选中的物体

3. 属性窗口：
    - Translate：可调整X，Y，Z轴来平移物体。双击值可平移到指定位置。点击值后蓝色方块可还原为(0,0,0)
    - Orient：调整相应值来调整物体旋转角度

4. 视角
    - F：调整主视角为选中物体
    - 左键按住 + ALT：调整视角角度
    - 滚轮：放大缩小

5. Xform
    - 创建：Create -> Xform，将会被创建在World之下
    - 关系：当物体位于Xform中时，其中物体将继承Xform的属性。无论如何改变Xform组合位置，单个物体位置不会改变

6. 模拟
    - 工具栏中的暂停/播放按钮

7. 时间线
    - windows -> Extensions，输入omni.anim.window.timeline

![Regular instructions](/regular_inputs.png)

## Work flow 工作流程
1. GUI

2. Extensions

3. Standalone Application

4. ROS Application

## GUI 
- Setting up Stage Properties: Edit -> Preferences

- Creating the Physics Scene: 
    * Create -> Physics -> Physics Scene
    * 设置重力，数值为9.8，方向为-Z
    * 在不模拟数百个刚性机器人的情况下，CPU>GPU，使用MBP

- Adding a Ground Plane 添加地平面
    * Create -> Physics -> Ground Plane
    * 防止物体掉到地平面以下

- Lighting 灯光
    * Create -> Light -> Sphere Light
    * 高度与强度可显著改变对比


### 将简单物体应用环境规则
* 选中需要应用的物体
* 在Property界面选择Add，Physics -> Rigid Body with Colliders Preset
* 点击播放，物体就会按照应用的规则开始模拟


# 4/2
### 使用Rigid Body API 和 Collision API
* 选中需要修改的物体
* Property界面中可看到Rigid Body和Collodor
* 根据需求将其取消应用

### Collosion meshes查看
* 在视图左上角有一个眼睛图标
* show by types -> physics -> colliders -> All

### 修改接触与摩擦性能
* Menu > Create > Physics > Physics Material
* 在弹出窗口中选择Rigid Body Material
* 在结构树中会出现PhysicsMaterial
* 通过选中物体，在Property界面选择Materials on Selected Model，并且在下拉栏中选择材质

### 调整物体颜色
* 与调整物体材质一样，Create -> Materials -> OmniPBR
* 选择目标物体，在Property界面选择Materials on Selected Model，并且在下拉栏中选择对应颜色

## 组装简单机器人
### 添加关节
* 先在右侧Stage树中选中父级物体，再按住[^ctrl+shift]选中子级物体
* 右键Create > Physics > Joints > Revolute Joint
* 在子级物体下会出现RevoluteJoint，建议将其重命名，wheel_joint_方位
* 选中wheel，将其轴换为Y轴
* 由于父级物体与子级物体轴式错位的，需要将父级物体Local Rotation改为0，子级物体Local Rotation改为-90
* 重复此操作，将右侧wheel也与父级物体连接，按住[^shift]可整体拖动整个物体

### 添加关节驱动接口
* 同时选中先前添加的两个左右连接关节，选择[^+Add]按钮
* Physics > Angular Drive
* 位置控制：高stiffness，低damping
* 速度控制：0 stiffness，高damping

### 添加Articulor root
* 在stage树上选择身体部件，选择[^+Add]
* Physics > Articulation Root

## 添加摄像头与传感器
### 添加摄像头
* Create > Camera
* 可调整摄像头的位置（轴的中心），并且旋转角度以调整视场（FOV）

### Camera Inspector extension
* Isaac Utils > Workflows > Camera Inspector
* 在添加新的摄像头之后，刷新才能使Camera Inspector extension检测到新的摄像头
* 在顶部的文本框中，可直接复制其中代表摄像头信息的代码

### 创建viewport视窗
* 在camera下拉栏，点击[^Create_Viewpoint]
* 可在其中修改分辨率

### 将摄像头附着于机器人上
* 先将已创建的摄像头重命名
* 将stage树中的摄像头拖拽至想要放置摄像头的机器人部分
* 在Menu Bar中选择 [^Window]， 选择最后的 [^NewViewPort], 并且将新生成的视窗放置在合适的位置
* 在新视窗的左上角选择[^Persepctive]，选择Camera中的对应摄像头
* 选中stage树中的camera可对其的位置、视角做调整


# 4/18
## 使用Python脚本来代替GUI使用
