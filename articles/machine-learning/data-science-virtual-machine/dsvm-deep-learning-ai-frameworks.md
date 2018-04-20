---
title: Derin öğrenme ve AI çerçeveleri - Azure | Microsoft Docs
description: Derin Öğrenme ve AI çerçeveleri
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: d73869d1371247a269b6601c35b1a938d89176c0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Derin Öğrenme ve AI çerçeveleri
[Veri bilimi sanal makine](http://aka.ms/dsvm) (DSVM) ve [derin öğrenme VM](http://aka.ms/dsvm/deeplearning) Tahmine dayalı analiz yapay Intelligence (AI) uygulamalarla oluşturmanıza yardımcı olmak üzere derin öğrenme çerçeveleri destekler ve bilişsel özellikleri görüntü ve dil anlama gibi. 

Çerçeveler DSVM üzerinde kullanılabilir öğrenme tüm derin ayrıntıları aşağıdadır.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Microsoft Bilişsel Araç Seti (CNTK) Python 3.5 yüklü [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python çalıştırın. <br/>
 * İçinde Jupyter: bağlanmak [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnekleri CNTK dizinini açın. |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | TensorFlow yüklü Python 3. 5 ' [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).  |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python çalıştırın. <br/>
 * İçinde Jupyter: bağlanmak [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnekleri TensorFlow dizinini açın.  |

## <a name="horovod"></a>Horovod

|    |           |
| ------------- | ------------- |
| Nedir?   | TensorFlow için Distribued derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Horovod yüklü Python 3. 5 ' [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Örnekleri bağlantılar      | [https://github.com/uber/horovod/tree/master/examples](https://github.com/uber/horovod/tree/master/examples)     |
| DSVM ilgili araçları      | TensorFlow      |
| Kullanın / çalıştırmak için nasıl?    | Bir terminal adresindeki: doğru ortamı etkinleştirmek ve Python çalıştırın. |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | TensorFlow yüklü Python 3. 5 ' [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). |
| Örnekleri bağlantılar      | https://github.com/fchollet/keras/tree/master/examples      |
| DSVM ilgili araçları      | Microsoft Bilişsel araç seti, TensorFlow, Theano      |
| Kullanın / çalıştırmak için nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python çalıştırın. <br/>
 * İçinde Jupyter: örnekler Github konumdan indirmek için bağlanmak [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek dizinini açın. |

## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Caffe yüklü `/opt/caffe`.    |
| Örnekleri bağlantılar      | Örnekleri dahil edilmiştir `/opt/caffe/examples`.      |
| DSVM ilgili araçları      | Caffe2      |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

VM'nize, oturum açma sonra yeni bir terminal başlatmak ve girmek için X2Go kullanın

```
cd /opt/caffe/examples
jupyter notebook
```

Örnek dizüstü bilgisayarlarla yeni bir tarayıcı penceresi açar.

İkili dosyaları /opt/caffe/build/install/bin yüklenir. 

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Caffe2 yüklü [Python 2.7 (kök) conda ortam](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Kaynak olarak `/opt/caffe2`. |
| Örnekleri bağlantılar      | Örnek not defterlerini JupyterHub dahil edilir. |
| DSVM ilgili araçları      | Caffe      |
| Kullanın / çalıştırmak için nasıl?    | * Adresindeki terminal: etkinleştirme [kök Python ortamı](dsvm-languages.md#python-linux-and-windows-server-2012-edition)Python başlatmak ve caffe2 alın. <br/> * İçinde JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnek dizüstü bilgisayarları bulmak için Caffe2 dizinine gidin. Bazı dizüstü bilgisayarlar Python kodda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin. |
| Notlar derleme | Caffe2 kaynağından Linux'ta oluşturulur ve CUDA, cuDNN ve Intel MKL içerir. Geçerli yürütme kararlılık tüm GPU ve test örnekleri için seçildi 0d9c0d48c6f20143d6404b99cc568efd29d5a4be ' dir. |

## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Bağlayıcı yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL ve ChainerCV de yüklenir.   |
| Örnekleri bağlantılar      | Örnek not defterlerini JupyterHub dahil edilir. |
| DSVM ilgili araçları      | Caffe      |
| Kullanın / çalıştırmak için nasıl?  | * Adresindeki terminal: etkinleştirme [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ortamı, _python_, bağlayıcı içeri aktarın. <br/>
* İçinde JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnek dizüstü bilgisayarları bulmak için bağlayıcı dizinine gidin.


## <a name="deep-water"></a>Derin su

|    |           |
| ------------- | ------------- |
| Nedir?   | H2O için derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Derin su yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve da `/dsvm/tools/deep_water`.   |
| Örnekleri bağlantılar      | Örnek not defterlerini JupyterHub dahil edilir.      |
| DSVM ilgili araçları      | H2O, Sparkling su      |

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

Derin su CUDA 8 cuDNN 5.1 ile gerektirir. Diğer derin öğrenme çerçeveler CUDA 9 ve cuDNN 7 kullanırken bu varsayılan olarak, kitaplık yolu değil. CUDA 8 + cuDNN 5.1 için derin su kullanmak için:

```
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
export CUDA_ROOT=/usr/local/cuda-8.0
```

Derin su kullanmak için:
* Terminal adresindeki: etkinleştirme [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) sonra ortamı, _python_. <br/>
* İçinde JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnek dizüstü bilgisayarları bulmak için deep_water dizinine gidin.

## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | MXNet yüklü `C:\dsvm\tools\mxnet` Windows'da ve `/dsvm/tools/mxnet` Linux üzerinde. Python bağlamaları yüklenen Python 3. 5 ' [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R bağlamaları Ubuntu üzerinde de yüklenir.   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.    |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python çalıştırın. <br/>
 * İçinde Jupyter: bağlanmak [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnekleri mxnet dizinini açın.  |
 | Notlar derleme | MXNet kaynağından Linux üzerinde oluşturulmuştur. Bu yapı CUDA, cuDNN, NCCL ve MKL içerir. |

## <a name="nvidia-digits"></a>NVIDIA BASAMAK

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin NVIDIA sistemden derin öğrenme modelleri hızla eğitim için öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | BASAMAK yüklü `/dsvm/tools/DIGITS` ve kullanılabilir bir hizmet olarak adlandırılan _basamak_.   |
### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

VM X2Go ile oturum açın. Bir terminal hizmetini başlatın:

    sudo systemctl start digits

Hizmeti başlatmak için yaklaşık bir dakika sürer. Bir web tarayıcı başlatmak ve gidin `http://localhost:5000`.



## <a name="nvdia-smi"></a>nvdia SMI

|    |           |
| ------------- | ------------- |
| Nedir?   | GPU etkinliği sorgulamak için NVIDIA aracı      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | _NVIDIA SMI_ sistem yolunda kullanılabilir.   |
| Kullanın / çalıştırmak için nasıl? | Bir komut istemi (Windows) veya bir terminalde (Linux) başlatın, ardından Çalıştır _NVIDIA SMI_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Python 2.7 Theano yüklü (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.   |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | * Bir terminal adresindeki (kök veya py35) istediğiniz python çalıştırın, sonra theano alma Python sürümü etkinleştirin. <br/> 
* Jupyter, Python 2.7 ya da 3.5 çekirdek seçin, ardından theano içeri aktarın.  
<br/>
Son MKL hata olarak çözmek için katman iş parçacığı oluşturma MKL ayarlamanız gerekir:<br/><br/>
_MKL_THREADING_LAYER verme GNU =_
|



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Torch yüklü `/dsvm/tools/torch`. Python 2.7 PyTorch yüklü (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.   |
| Örnekleri bağlantılar      | Torch örnekleri adresindedir `/dsvm/samples/torch`. PyTorch örnekleri adresindedir `/dsvm/samples/pytorch`.      |


## <a name="pytorch"></a>PyTorch

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | PyTorch yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir ve örnekleri de /dsvm/samples/pytorch içinde bulunabilir.      |
| DSVM ilgili araçları      | Torch      |
| Nasıl kullanmak / çalıştırın | 
* Bir terminal adresindeki: doğru ortamı etkinleştirmek ve Python çalıştırın. <br/>
 * İçinde Jupyter: bağlanmak [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), örnekleri PyTorch dizinini açın.  |

## <a name="mxnet-model-server"></a>MXNet Model sunucu

|    |           |
| ------------- | ------------- |
| Nedir?   | MXNet ve ONNX modelleri için HTTP uç noktaları oluşturmak için bir sunucu      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | _mxnet model sunucu_ terminal bulunur.   |
| Örnekleri bağlantılar      | Güncel örnekler için Ara [MXNet modeli sunucu sayfası](https://github.com/awslabs/mxnet-model-server).    |
| DSVM ilgili araçları      | MXNet      |

## <a name="tensorflow-serving"></a>TensorFlow sunma

|    |           |
| ------------- | ------------- |
| Nedir?   | İnferencing TensorFlow model üzerinde çalıştırmak için bir sunucu      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | _tensorflow_model_server_ terminal kullanılabilir.   |
| Örnekleri bağlantılar      | Örnekleri kullanılabilir [çevrimiçi](https://www.tensorflow.org/serving/).      |
| DSVM ilgili araçları      | TensorFlow      |

## <a name="tensorrt"></a>TensorRT

|    |           |
| ------------- | ------------- |
| Nedir?   | NVIDIA çıkarım sunucudan öğrenme derin. |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | TensorRT olarak yüklü bir _apt_ paket.   |
| Örnekleri bağlantılar      | Örnekleri kullanılabilir [çevrimiçi](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).      |
| DSVM ilgili araçları      | Hizmet veren, MXNet Model sunucu TensorFlow  |



