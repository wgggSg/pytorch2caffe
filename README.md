# pytorch2caffe

A package to convert pytorch model to caffe model. 

## Note

- This package supports `F.interpolate` with `mode='bilinear'` argument and `scale_factor` argument.
It depends on caffe with `Upsample` layer as follows:
```
# argument height and width
layer{
    name:"Upsample_nearest"
    type:"Upsample"
    bottom:"Conv2d_87" #Blob Conv2d_87's shape is [1,16,32,32]
    top:"Upsample_nearest" #Blob Upsample_nearest's shape is [1,16,HEIGHT,WIDTH]
    upsample_param{
        mode: NEAREST # or BILINEAR
        height: HEIGHT
        width: WIDTH
    }
}
# argument height_scale and width_scale
layer{
    name:"Upsample_nearest"
    type:"Upsample"
    bottom:"Conv2d_87" #Blob Conv2d_87's shape is [1,16,32,32]
    top:"Upsample_nearest" #Blob Upsample_nearest's shape is [1,16,32*HEIGHT_SCALE,32*WIDTH_SCALE]
    upsample_param{
        mode: NEAREST # or BILINEAR
        height_scale: HEIGHT_SCALE
        width_scale: WIDTH_SCALE
    }
}
```
See [**jnulzl/caffe_plus**](https://github.com/jnulzl/caffe_plus) 

## Installation


    git clone https://github.com/wgggSg/pytorch2caffe
    cd pytorch2caffe
    python3 setup.py install

## Tutorials

Note: import torch first then import pytorch2caffe

    import torch
    from torchvision.models import resnet
    from pytorch2caffe import pytorch2caffe

    name='resnet18'
    resnet18=resnet.resnet18()
    resnet18.eval()
    dummy_input=torch.ones([1,3,224,224])
    pytorch2caffe.trans_net(resnet18, dummy_input, name)
    pytorch2caffe.save_prototxt('{}.prototxt'.format(name))
    pytorch2caffe.save_caffemodel('{}.caffemodel'.format(name))
