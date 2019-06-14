# Notes!!
There are certain **difference** between present version and offical openlab-mmdetection , such as `demo.py` file ,  cfg attribute is integrated to model in the latest version ,so that only 2 passing parameters are necessary compared to the former 3.<br>
**I don't think it worth tracing on codebase's updating , present version is sufficient .** Thus I won't keep on attaching notes to newer mmdetection codebase.If you insist that the newer ,the better ,this repo also helps as I have checked the difference between two repos ,only to find that **no big changes are made in recent codebase** .  
# mmdetection-annotated 
**Notice** : Work finished! And more annotation will be added if necessary.Still to be contunued....~~I'm back again , and concentrate on it now.Focused on some experiments recently , I may postpone updates on training parts.To be contunued....(well,I promise to finish the rest work before **Jul**.Blame it on examinations...)~~</br>
## Introduction
Refer to the execllent implemention here:https://github.com/open-mmlab/mmdetection ,and thanks to author [Kai Chen](https://github.com/hellock).</br>
Open-mmlab project , which contains various models and implementions of latest papers , achieves great results in detection/segmentataion tasks , and is kind enough for rookies in CV field.</br>
## Getting started
More information about installation or pre-train model downloads , pls refer to [officia mmdetection](https://github.com/open-mmlab/mmdetection) or [blog here](https://blog.csdn.net/mingqi1996/article/details/88091802)</br>
* **Test on images</br>**
You can test on Mask-RCNN demo by running the script `demo.py`.
I have just rewritten the demo file to detect on single image or a folder as follow:
```
import ipdb
import sys,os,torch,mmcv
from mmcv.runner import load_checkpoint
#下面这句import的时候定位并调用Registry执行了五个模块的注册
from mmdet.models import build_detector	
from mmdet.apis import inference_detector, show_result

if __name__ == '__main__':
	# ipdb.set_trace()
	cfg = mmcv.Config.fromfile('configs/mask_rcnn_r101_fpn_1x.py')
	# cfg = mmcv.Config.fromfile('configs/faster_rcnn_r50_fpn_1x.py')
	cfg.model.pretrained = None		#inference不设置预训练模型
	#inference只传入cfg的model和test配置，其他的都是训练参数
	model = build_detector(cfg.model, test_cfg=cfg.test_cfg)
	_ = load_checkpoint(model, 'weights/mask_rcnn_r101_fpn_1x_20181129-34ad1961.pth')
	# print(model)  # 展开模型

	# test a single image
	img= mmcv.imread('/py/pic/2.jpg')
	result = inference_detector(model, img, cfg)
	show_result(img, result)

	# # test a list of folder
	# path='/py/mmdetection/images/'
	# imgs= os.listdir(path)
	# for i in range(len(imgs)):
	# 	imgs[i]=os.path.join(path,imgs[i])
	# # imgs = ['/py/pic/4.jpg', '/py/pic/5.jpg']
	# for i, result in enumerate(inference_detector(model, imgs, cfg, device='cuda:0')):
	#     print(i, imgs[i])
	#     show_result(imgs[i], result)

```
* **Debug**</br>
You can debug by setting breakpoint with method of adding `ipdb.set_trace()`
* **Hook**</br>
If you want to inspect on intermediate variables , `hook.py` can be a provision served as a reference for your work.
## Annotations
Annotations are attached everywhere in the code(surely only the part I have read , and the not finished part will be completed as soon as possible). Beside , `annotation` folder contains some interpreting documents as well.</br>
* **Model visualization**</br>
Take Mask-RCNN for example , the model can be visualized as follow:(more details refere to [model-structure-png](https://github.com/ming71/mmdetection-annotated/blob/master/annotation/model_vis/maskrcnn-model-inference.png))
<div align=center><img src="https://github.com/ming71/mmdetection-annotated/blob/master/annotation/model_vis/inference.png"/></div>

* **Configuration**</br>
Explicit describtion on config file , take Mask RCNN for example , refer to [mask_rcnn_r101_fpn_1x.py](https://github.com/ming71/mmdetection-annotated/blob/master/annotation/mask_rcnn_r101_fpn_1x.py)</br>
* **MMCV&MMDET**</br>
Specification of mmcv lib and a partial of mmdet(more details about various models will be updated later ).</br>

## Detection Results</br>
Test on Mask RCNN model:</br>
<div align=center><img src="https://github.com/ming71/mmdetection-annotated/blob/master/outputs/_s1019.png"/></div>
<div align=center><img  src="https://github.com/ming71/mmdetection-annotated/blob/master/outputs/_screenshot_02.04.2019.png"/></div>
<div align=center><img  src="https://github.com/ming71/mmdetection-annotated/blob/master/outputs/_screenshot_071019.png"/></div>


## Training</br>
### **dataset**<br>
- You can just use COCO dataset , refer [here](https://blog.csdn.net/mingqi1996/article/details/88091802).<br>
- If you want to train on your customed dataset labeled by `labelme` , you need first convert json files to COCO style , this [toolbox](https://github.com/ming71/toolbox) may help you ;<br>
- If you want to train on your customed dataset labeled by `labelImg` , you need first convert xml files to COCO style , this [toolbox](https://github.com/ming71/toolbox) may also help you .<br>
- I have tested on these tools recently to make sure them still work well, if questiones still arised , desrcibe on issue please or contact me , thanks.<br>

### learning rate
Remember to set lr in config file according to your <u>**own GPU_NUM**</u> !!!!(eg.1/8 of default lr for 1 GPU)

## Future work</br>
Mmdetection performs better than many classical implementions , it's really a excellent work , can be called as ‘Chinese Detectron’ :p .I will update this project with annotations for more details in the future, letting more people make a good use of this great work.You can continue to foucus on this repo.</br>
BTW , this repo is just used for better comprehension , if you ask for better performance or latest paper implementions ,please keep eyes on [mmdetection](https://github.com/open-mmlab/mmdetection)</br>

