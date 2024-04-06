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
### 打开编辑器
* Window > Script Editor
* 可拖动编辑器窗口调整位置
* 在编辑器中，选择 Tab > Add Tab 可添加编辑窗口
* 点击[^Run]可开始运行当前窗口中的脚本

### REPL 扩展
* 确保当前正在运行一个Issac Sim的实例
* 在Window > Extension中搜索 Isaac Sim REPL。若没有启用，则选择启用。若希望在每次启动Issac Sim的时候都启用，则打开Autoload
* 打开一个终端，在终端中输入" telnet localhost 8223 "，并且会打开一个python shell
* 按[^ctrl+D]退出

### Python API
* 如下脚本设置了一个平面，一个默认灯光和一个具有物理和刚性碰撞预设的长方体。将其复制到编辑器窗口中，点击[^Run]开始运行
> **注意**
> 
> 下面的脚本应当在一个空舞台上运行，并且仅仅运行一次


``` Python
from pxr import UsdPhysics, PhysxSchema, Gf, PhysicsSchemaTools, UsdGeom
import omni

stage = omni.usd.get_context().get_stage()

# Setting up Physics Scene
gravity = 9.8
scene = UsdPhysics.Scene.Define(stage, "/World/physics")
scene.CreateGravityDirectionAttr().Set(Gf.Vec3f(0.0, 0.0, -1.0))
scene.CreateGravityMagnitudeAttr().Set(gravity)
PhysxSchema.PhysxSceneAPI.Apply(stage.GetPrimAtPath("/World/physics"))
physxSceneAPI = PhysxSchema.PhysxSceneAPI.Get(stage, "/World/physics")
physxSceneAPI.CreateEnableCCDAttr(True)
physxSceneAPI.CreateEnableStabilizationAttr(True)
physxSceneAPI.CreateEnableGPUDynamicsAttr(False)
physxSceneAPI.CreateBroadphaseTypeAttr("MBP")
physxSceneAPI.CreateSolverTypeAttr("TGS")

# Setting up Ground Plane
PhysicsSchemaTools.addGroundPlane(stage, "/World/groundPlane", "Z", 15, Gf.Vec3f(0,0,0), Gf.Vec3f(0.7))

# Adding a Cube
path = "/World/Cube"
cubeGeom = UsdGeom.Cube.Define(stage, path)
cubePrim = stage.GetPrimAtPath(path)
size = 0.5
offset = Gf.Vec3f(0.5,0.2,1.0)
cubeGeom.CreateSizeAttr(size)
cubeGeom.AddTranslateOp().Set(offset)

# Attach Rigid Body and Collision Preset
rigid_api = UsdPhysics.RigidBodyAPI.Apply(cubePrim)
rigid_api.CreateRigidBodyEnabledAttr(True)
UsdPhysics.CollisionAPI.Apply(cubePrim)
```

### Issac Sim的核心Python API
* 接下来的脚本与上文脚本做了同样的设置，但更简洁

```
import numpy as np
from omni.isaac.core.objects import DynamicCuboid
from omni.isaac.core.objects.ground_plane import GroundPlane
from omni.isaac.core.physics_context import PhysicsContext

PhysicsContext()
GroundPlane(prim_path="/World/groundPlane", size=10, color=np.array([0.5, 0.5, 0.5]))
DynamicCuboid(prim_path="/World/cube",
    position=np.array([-.5, -.2, 1.0]),
    scale=np.array([.5, .5, .5]),
    color=np.array([.2,.3,0.]))
```

## Omnigraph
### 设置舞台
* 创建地板
* 使用下方的content窗口来导航至Isaac/Robots/JetBot，并将jetbot.usd拖拽到舞台上

