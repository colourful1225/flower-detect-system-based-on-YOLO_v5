# 花卉识别检测系统（基于 YOLOv5）

基于 YOLOv5 的鲜花识别检测系统 — 提供花卉目标检测的训练与推理代码与说明。该仓库语言以 Python 为主，适合用于教学、实验与工程化快速验证。
只是本人的校内小项目，仍有许多不足之处，欢迎指出与讨论

## 主要功能
- 基于 YOLOv5 的花卉目标检测模型训练与推理
- 支持自定义数据集（YOLO 格式）
- 提供训练、评估与检测（推理）脚本示例
- 可替换预训练权重（如 yolov5s / yolov5m / yolov5l）

## 项目结构（示例）
说明：具体文件/目录名请以仓库实际内容为准
- data/
  - flowers.yaml        # 数据集描述文件（类别、训练/验证路径）
  - images/             # 图片目录（train/val/test）
  - labels/             # 标签目录（对应 YOLO 格式 .txt）
- weights/              # 预置或训练得到的模型权重
- scripts/
  - train.py            # 训练脚本（或使用原始 YOLOv5 的 train.py）
  - detect.py           # 推理/检测脚本
  - val.py              # 评估脚本
- requirements.txt      # 依赖
- README.md

## 环境依赖
建议使用虚拟环境并安装依赖（请根据仓库中的 requirements.txt 调整）：
- Python 3.8+
- PyTorch（与 CUDA 版本匹配）
- OpenCV
- tqdm、numpy、pandas 等

示例命令：
git clone https://github.com/colourful1225/flower-detect-system-based-on-YOLO_v5.git
cd flower-detect-system-based-on-YOLO_v5
python -m venv venv

# windows (PowerShell)
venv\Scripts\Activate.ps1
pip install -r requirements.txt

（若仓库使用官方 YOLOv5 代码，需按 Ultralytics YOLOv5 的说明安装依赖）

## 数据集格式（YOLO 标注）
- 每张图片对应一个同名 .txt 文件放在 labels/ 中
- 每行：<class_index> <x_center> <y_center> <width> <height>
  - 坐标与宽高均为归一化到 [0,1] 的相对值
- 在 data/flowers.yaml 中定义类别数与训练/验证路径，例如：
train: ../data/images/train
val: ../data/images/val
nc: 5
names: ['rose','tulip','daisy','sunflower','lily']

## 快速开始（训练示例）
示例（如果使用典型的 YOLOv5 脚本）：
python train.py --img 640 --batch 16 --epochs 50 --data data/flowers.yaml --weights yolov5s.pt --name flower_exp

参数说明（示例）：
- --img: 输入尺寸
- --batch: 批大小
- --epochs: 训练轮数
- --data: 数据配置文件
- --weights: 初始权重（可用预训练权重或断点）
- --name: 本次实验名（结果保存在 runs/train/ 下）

## 推理 / 检测（示例）
python detect.py --weights runs/train/flower_exp/weights/best.pt --source data/images/test --img 640 --conf 0.25 --save-txt --save-conf

常用参数：
- --source: 图片/文件夹/视频/摄像头
- --conf: 置信度阈值
- --save-txt: 保存检测框到 txt
- --save-conf: 在 txt 中保存置信度

## 模型评估（示例）
python val.py --weights runs/train/flower_exp/weights/best.pt --data data/flowers.yaml --img 640 --batch 16

会输出 mAP、precision、recall 等指标。

## 常见问题与调优建议
- 数据不平衡：尝试数据增强、类别权重或更多样本
- 过拟合：降低学习率、使用早停、增加数据增强或减小模型容量
- 精度较低：尝试更长训练、较大模型（yolov5m/yolov5l）或更高分辨率输入

## 许可与致谢
- 请在使用或发布研究成果时注明本项目与原始 YOLOv5（Ultralytics）相关的参考资料

## 贡献
欢迎提交 Issue、Pull Request 或在仓库中讨论改进建议。贡献内容包括但不限于：
- 修复 bug、完善文档
- 新增数据集转换脚本或增强策略
- 提供模型压缩/部署（ONNX / TensorRT / TorchScript）示例

## 欢迎交流
如果你有疑问、建议或想一起合作：
- 提交 Issue（推荐首选）或 Pull Request
- 在仓库 README 下留言讨论
- 你也可以在代码中添加联系方式（邮箱、微信、微博等），或直接通过 GitHub 个人主页与仓库所有者联系

期待你的反馈与贡献！
