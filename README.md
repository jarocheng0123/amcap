# 通用摄像头配置工具

**基于Python的通用摄像头参数自动配置工具**
 - 最新版本
```bash
2025/3/31
Windows-ViTai3.py
```


## 项目简介
本工具用于自动配置摄像头的参数（如对比度、饱和度、白平衡等），目前只支持Windows（Ubuntu系统在开发中）具备以下功能：

- 自动检测系统中存在相同 VID/PID 组合的摄像头
- 自动检测并关闭冲突进程
- 多摄像头参数同步配置（支持多设备同时设置）
- 实时视频流显示（带设备信息叠加）
- 硬件信息报告生成（含 VID/PID 检测）
- 支持查找指定 VID/PID 的相机
- 重复 VID/PID 设备检测与警告
## 安装指南

### 系统要求
- Windows 10/11
- Python 3.9+



### 脚本测试环境

- 用户名 
```bash
Windows 10
```
- 电脑信息
```bash
版本	        Windows 11 专业工作站版
版本号	        24H2
安装日期	        ‎2025/‎1/‎6
操作系统版本	26100.3624
序列号	        PF509JD8
体验	        Windows 功能体验包 1000.26100.66.0
```

- 文件名 
```bash
ViTai.py
```

- 图标名
```bash
ViTailogo.ico
```

- Python包
```bash
python-3.9.13-amd64
```

- Python安装路径 
```bash
C:\Program Files\Python39
```

- Python脚本路径 
```bash
C:\Users\Windows 10\Desktop\ViTai.py
```

- 查找指定相机
```python
SPECIFIED_PIDS_VIDS = [("F225", "0001")]
```

### 依赖安装
#### Python安装路径
```bash
 C:\Program Files\Python39\python.exe
```

#### Windows PowerShell (管理员)(A) 使用 pip 安装依赖
```bash
& "C:\Program Files\Python39\python.exe" -m pip install --upgrade pip --verbose pywin32 wmi opencv-python psutil PyCameraList pyinstaller -i https://mirrors.aliyun.com/pypi/simple
```


## 使用说明

### 配置参数
修改代码中的配置区域：
```python
# 摄像头名称
TARGET_CAMERA_NAME = "ViTai"
# 指定PID，VID 组合
SPECIFIED_PIDS_VIDS = [("F225", "0001")]
# 需要关闭的进程
AMCAP_EXE_NAME = "amcap+v3.0.9.exe"
# 摄像头初始化时间
CAMERA_INIT_DELAY = 1
# 显示摄像头画面
SHOW_CAMERA_WINDOW = True
# 显示摄像头属性窗口
SHOW_PROPERTY_WINDOW = False
# 程序退出延时
MAIN_DELAY = 10
# 需要关闭的属性窗口关键字
KEYWORDS = ["ViTai 属性"]
# 读取参数等待时间
PARAM_READ_DELAY = 1
#窗口自动关闭控制
AUTO_CLOSE_WINDOW = False
```
## 打包为exe（Windows）

**Windows PowerShell 使用 PyInstaller 打包**：
```bash
cd "C:\Users\Windows 10\Desktop"
```
```bash
& "C:\Program Files\Python39\python.exe" -m PyInstaller --onefile --distpath "C:\Users\Windows 10\Desktop" --hidden-import=colorama --icon=ViTailogo.ico ViTai3.py
```

## 功能特性
### 1. 硬件信息报告
```
========================================== 设备初始化报告 ==========================================
序号   | 设备名称                        | VID      | PID    | 序列号         | 制造商
----------------------------------------------------------------------------------------------------
0      | Integrated IR Camera           | 174F     | 1820   | N/A            | N/A
0      | Integrated Camera              | 174F     | 1820   | N/A            | N/A
----------------------------------------------------------------------------------------------------
1      | ViTai                          | F225     | 0001   | N/A            | N/A  
2      | ViTai                          | AC20     | 0001   | N/A            | N/A  

==================================== 检测到重复VID/PID设备组合 =====================================
 ❗ VID: 174F, PID: 1820

===================================== 找到指定 PID/VID 的设备 ======================================
 ⚠️ 设备名称: ViTai, VID: F225, PID: 0001

```

### 2. 参数配置示例
- 参数配置需要根据实际情况选择
```python
class CameraConfig:
    PARAM_INFO = {
        "Contrast": {"chinese_name": "对比度", "value": 39, "range": (0, 100)},
        "Hue": {"chinese_name": "色调调节", "value": 0, "range": (-180, 180)},
        # ...更多参数
    }
```
- 部分硬件不支持声明（需要与厂商确认）
```python 
    print("降噪强度:   可能摄像头硬件没有内置降噪功能或驱动不支持调节降噪强度")
    print("数字变焦:   可能需要光学变焦镜头并通过物理控制，或摄像头本身不支持数字变焦")
    print("帧率:      可能摄像头硬件不支持指定的帧率范围或驱动未提供设置接口")
    print("分辨率宽度: 可能摄像头硬件不支持指定的分辨率宽度或驱动未提供设置接口")
    print("分辨率高度: 可能摄像头硬件不支持指定的分辨率高度或驱动未提供设置接口")
    print("色彩模式:   可能摄像头硬件不支持多种色彩模式或驱动未提供设置接口")
```    

## 常见问题解答

**Q: 为什么摄像头无法打开？**
- 检查是否有其他程序占用
- 确认驱动程序正确安装
- 以管理员身份运行程序

**Q: 参数设置无效怎么办？**
- 尝试重启摄像头设备
- 检查硬件兼容性
- 查看设备属性面板确认支持的参数范围

**Q: 视频流显示卡顿？**
- 降低分辨率设置
- 减少参数调整频率
- 检查系统资源占用情况

### 安全声明
1. 本程序需要访问摄像头设备，请确保已获得相应权限
2. 自动关闭进程功能可能会影响其他应用程序，请谨慎使用
3. 参数调整可能导致图像异常，建议先备份原始配置

## 版本更新日志
### v 2025.03.31
- 新增：摄像头属性窗口显示控制开关
- 优化：资源释放验证与异常处理机制

### v 2025.03.21
- 新增重复ID检测功能
- 优化硬件信息报告格式
- 增强参数校验逻辑
- 修复多摄像头同步配置问题

### v 2025.03.08
- 第一版相机调控脚本
