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


## 将简单物体应用环境规则
* 选中需要应用的物体
* 在Property界面选择Add，Physics -> Rigid Body with Colliders Preset
* 点击播放，物体就会按照应用的规则开始模拟


