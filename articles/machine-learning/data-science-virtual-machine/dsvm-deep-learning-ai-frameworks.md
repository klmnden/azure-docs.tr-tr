---
title: "Derin öğrenme ve AI çerçeveleri - Azure | Microsoft Docs"
description: "Derin Öğrenme ve AI çerçeveleri"
keywords: "Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 89a8cde0dbb7fe7ccfbb6d068411aaf5488c532f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Derin Öğrenme ve AI çerçeveleri
[Veri bilimi sanal makine](http://aka.ms/dsvm) (DSVM) ve [derin öğrenme VM](http://aka.ms/dsvm/deeplearning) Tahmine dayalı analiz yapay Intelligence (AI) uygulamalarla oluşturmanıza yardımcı olmak üzere derin öğrenme çerçeveleri destekler ve bilişsel özellikleri görüntü ve dil anlama gibi. 

Çerçeveler DSVM üzerinde kullanılabilir öğrenme tüm derin ayrıntıları aşağıdadır.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Python 2.7 içinde Microsoft Bilişsel Araç Seti (CNTK) yüklü olduğu _kök_ Python 3.5 yanı sıra, ortam, _py35_ ortamı.   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | Jupyter açın ve ardından CNTK klasörü arayın  |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Linux üzerinde TensorFlow Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı. Windows üzerinde Tensorflow Python 3.5 yüklü olduğu (_py35_) ortamı.  |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.     |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | Jupyter açın ve sonra TensorFlow klasörü arayın.  |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Python 2.7 Keras yüklü (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.   |
| Örnekleri bağlantılar      | https://github.com/fchollet/keras/tree/master/examples      |
| DSVM ilgili araçları      | Microsoft Bilişsel araç seti, TensorFlow, Theano      |
| Kullanın / çalıştırmak için nasıl?    | Github konumdan örnekleri indirin, ~/notebooks altındaki bir dizine kopyalayın ve Jupyter'de açın   |




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

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Caffe2 yüklü `/opt/caffe2`. Ayrıca Python 2.7 için kullanılabilir (_kök_) conda ortamı.     |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir     |
| DSVM ilgili araçları      | Caffe      |
| Kullanın / çalıştırmak için nasıl?    | Jupyter açın ve örnek dizüstü bilgisayarları bulmak için Caffe2 dizinine gidin. Bazı dizüstü bilgisayarlar Python kodda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin.   |


## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Bağlayıcı, Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı. ChainerRL ve ChainerCV de yüklenir.   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.      |
| DSVM ilgili araçları      | Caffe      |

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

Bir terminal istediğiniz Python sürümünü etkinleştir (_kök_ veya _py35_), çalışma _python_, bağlayıcı içeri aktarın. Jupyter, Python 2.7 ya da 3.5 çekirdek seçin, ardından bağlayıcı içeri aktarın.


## <a name="deep-water"></a>Derin su

|    |           |
| ------------- | ------------- |
| Nedir?   | H2O için derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | Derin su yüklü `/dsvm/tools/deep_water`.   |
| Örnekleri bağlantılar      | Örnekleri derin su sunucu üzerinden kullanılabilir.      |
| DSVM ilgili araçları      | H2O, Sparkling su      |

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

X2Go kullanarak VM'ye bağlanın. Bir terminal derin su sunucunun başlatın:

    java -jar /dsvm/tools/deep_water/h2o.jar

Ardından bir tarayıcı açın ve bağlanmak `http://localhost:54321`.



## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme çerçevesi      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | MXNet yüklü `C:\dsvm\tools\mxnet` Windows'da ve `/dsvm/tools/mxnet` Linux üzerinde. Python bağlamaları Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı. R bağlamaları de yüklenir.   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir.    |
| DSVM ilgili araçları      | Keras      |
| Kullanın / çalıştırmak için nasıl?    | Jupyter açın ve ardından mxnet klasörü arayın  |

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



## <a name="nvdia-smi"></a>nvdia-smi

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
| Kullanın / çalıştırmak için nasıl?    | Bir terminal (kök veya py35) istediğiniz python çalıştırın, sonra theano alma Python sürümü etkinleştirin. Jupyter, Python 2.7 ya da 3.5 çekirdek seçin, ardından theano içeri aktarın.  |



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
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | PyTorch Python 3.5 yüklü (_py35_) ortamı.   |
| Örnekleri bağlantılar      | Örnek Jupyter not defterleri dahil edilir ve örnekleri de /dsvm/samples/pytorch içinde bulunabilir.      |
| DSVM ilgili araçları      | Torch      |

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

Bir terminal çalıştırmak _python_, torch içeri aktarın. Jupyter, Python 3.5 çekirdek seçin, ardından torch içeri aktarın.


## <a name="mxnet-model-server"></a>MXNet Model Server

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
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?  | _tensorflow_model_server_ is available at the terminal.   |
| Örnekleri bağlantılar      | Örnekleri kullanılabilir [çevrimiçi](https://www.tensorflow.org/serving/).      |
| DSVM ilgili araçları      | TensorFlow      |
