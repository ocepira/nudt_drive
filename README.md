# 自动驾驶场景

本项目基于VAD库实现自动驾驶场景。支持推理、攻击、防御。

## 环境变量

| 变量名 | 是否必填 | 描述 |
|--------|---------|------|
| input_path | 必填 | 指定输入路径，在此路径下有权重文件和数据集文件 |
| output_path | 必填 | 指定输出路径，在此路径下保存生成的对抗样本和防御训练的权重 |
| process | 必填 | 指定进程名称，支持枚举值（第一个为默认值）: `test`, `attack`, `defense` |
| image-path | 必填 | 输入图像路径，当process为`attack`或`defense`时必填 |
| attack-method | 选填 | 指定攻击方法，若process为`attack`则必填，支持枚举值（第一个为默认值）: `fgsm`, `pgd`, `bim`,`badnet`, `squareattack`, `nes` |
| defense-method | 选填 | 指定防御方法，若process为`defense`则必填，支持枚举值（第一个为默认值）: `fgsm`, `pgd` |
| save-path | 选填 | 对抗样本保存路径 |
| save-original-size | 选填 | 是否保存原始尺寸的对抗样本 |
| config | 选填 | test config file path，当process为`test`时必填 |
| checkpoint | 选填 | checkpoint file，当process为`test`时必填 |
| steps | 选填，默认为10 | 攻击迭代次数(PGD/BIM) |
| alpha | 选填，默认为2/255 | 攻击步长(PGD/BIM) |
| epsilon | 选填，默认为8/255 | 扰动强度 |
| device | 选填，默认为0 | 使用哪个gpu |
| workers | 选填，默认为0 | 加载数据集时workers的数量 |

## 快速开始
python main.py 

python main.py --process test ./projects/configs/VAD/VAD_tiny_stage_1.py  ./ckpts/VAD_tiny.pth --launcher none --eval bbox  


python main.py --process attack  --image-path ./data/nuscenes/samples/CAM_BACK/n008-2018-08-01-15-16-36-0400__CAM_BACK__1533151603537558.jpg  --save-path ./OUTPUT/defense_fgsm_custom.png --attack-method pgd

python main.py --process defense --image-path ./data/nuscenes/samples/CAM_BACK/n008-2018-08-01-15-16-36-0400__CAM_BACK__1533151603537558.jpg  --save-path ./OUTPUT/defense_fgsm_custom.png  --defense-method pgd

## 构建 Docker 镜像
docker build -t vad:latest .