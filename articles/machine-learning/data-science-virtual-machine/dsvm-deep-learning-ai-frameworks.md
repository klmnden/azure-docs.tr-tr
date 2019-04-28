---
title: Derin öğrenme ve AI çerçeveleri - Azure | Microsoft Docs
description: Derin öğrenme çerçeveleri ve veri bilimi sanal makinesi üzerinde desteklenen araçlar hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 59f88d54d3542738f1a500c8c476995eb1535ecf
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130288"
---
# <a name="deep-learning-and-ai-frameworks"></a>Derin Öğrenme ve AI çerçeveleri
[Veri bilimi sanal makinesi](https://aka.ms/dsvm) (DSVM) ve [derin öğrenme VM](https://aka.ms/dsvm/deeplearning) Tahmine dayalı analiz ile yapay zeka (AI) uygulamaları oluşturmanıza yardımcı olmak için ayrıntılı öğrenme çerçevelerini destekler ve bilişsel yetenekleri, görüntü ve dil anlama gibi.

Tüm derin öğrenme çerçeveleri DSVM'nin kullanılabilir ilişkin ayrıntılar aşağıdadır.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Python 3.5 yüklenir Microsoft Cognitive Toolkit (CNTK) [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).   |
| Örneklere bağlantılar      | Örnek Jupyter not defterleri bulunur.     |
| DSVM ilgili araçları      | Keras      |
| Kullanma / çalıştırın nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. <br/> * Jupyter'de: Bağlanma [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnekleri CNTK dizinini açın. |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | TensorFlow, üzerinde Python 3.5 yüklenir [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).  |
| Örneklere bağlantılar      | Örnek Jupyter not defterleri bulunur.     |
| DSVM ilgili araçları      | Keras      |
| Kullanma / çalıştırın nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. <br/> * Jupyter'de: Bağlanma [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnekleri TensorFlow dizinini açın.  |

## <a name="horovod"></a>Horovod

|    |           |
| ------------- | ------------- |
| Nedir?   | TensorFlow Distribued derin öğrenme framework      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Horovod üzerinde Python 3.5 yüklenir [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Örneklere bağlantılar      | [https://github.com/uber/horovod/tree/master/examples](https://github.com/uber/horovod/tree/master/examples)     |
| DSVM ilgili araçları      | TensorFlow      |
| Kullanma / çalıştırın nasıl?    | Bir terminal adresindeki: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| Nedir?   | Üst düzey derin öğrenme API      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | TensorFlow, üzerinde Python 3.5 yüklenir [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). |
| Örneklere bağlantılar      | https://github.com/fchollet/keras/tree/master/examples      |
| DSVM ilgili araçları      | Microsoft Bilişsel araç seti, TensorFlow, Theano      |
| Kullanma / çalıştırın nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. <br/> * Jupyter'de: GitHub konumdan örnekleri indirin, bağlanma [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek dizin açın. |

## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Caffe yüklü `/opt/caffe`.    |
| Python 2.7 için geçiş yapma | `source activate root` öğesini çalıştırın |
| Örneklere bağlantılar      | Örnekleri dahil edilecek `/opt/caffe/examples`.      |
| DSVM ilgili araçları      | Caffe2      |

### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?

Oturum açmak için sanal makinenizin, sonra yeni bir terminal başlatıp girin X2Go kullanın

```
cd /opt/caffe/examples
source activate root
jupyter notebook
```

Örnek Not Defterleri ile yeni bir tarayıcı penceresi açılır.

İkili dosyaları içinde /opt/caffe/build/install/bin yüklenir.

Caffe'nın yüklü sürümü, Python 2.7 gerektirir ve varsayılan olarak etkinleştirilen Python 3.5 ile çalışmaz. Çalıştırma `source activate root` Anaconda ortam geçin.

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Caffe2 yüklü [Python 2.7 (kök) conda ortam](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Kaynağı `/opt/caffe2`. |
| Örneklere bağlantılar      | Örnek Not Defterleri JupyterHub dahil edilir. |
| DSVM ilgili araçları      | Caffe      |
| Kullanma / çalıştırın nasıl?    | *, Terminal: etkinleştirme [kök Python ortamı](dsvm-languages.md#python-linux-and-windows-server-2012-edition), Python'ı başlatın ve caffe2 içeri aktarın. <br/> * Giriş JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek not defterleri bulmak için Caffe2 dizine gidin. Bazı dizüstü bilgisayarlar Python kodunda ayarlanacak Caffe2 kök gerektirir; /OPT/caffe2 girin. |
| Notları oluşturun | Caffe2 kaynağından Linux üzerinde kurulmuştur ve CUDA, cuDNN ve Intel MKL içerir. Kararlılık tüm GPU'ları ve test edilen örnekler için seçilen 0d9c0d48c6f20143d6404b99cc568efd29d5a4be, geçerli işlemesidir. |

## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Bağlayıcı yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL ve ChainerCV da yüklenir.   |
| Örneklere bağlantılar      | Örnek Not Defterleri JupyterHub dahil edilir. |
| DSVM ilgili araçları      | Caffe      |
| Kullanma / çalıştırın nasıl?  | *, Terminal: etkinleştirme [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) çalışma ortamı _python_, ardından bağlayıcı içeri aktarın. <br/> * Giriş JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek not defterleri bulmak için bağlayıcı dizine gidin.


## <a name="deep-water"></a>Ayrıntılı su

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme framework H2O      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Ayrıntılı su yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve da `/dsvm/tools/deep_water`.   |
| Örneklere bağlantılar      | Örnek Not Defterleri JupyterHub dahil edilir.      |
| DSVM ilgili araçları      | H2O, Sparkling Water      |

### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?

Ayrıntılı su CUDA 8 cuDNN 5.1 ile gerektirir. Diğer derin öğrenme çerçeveleri CUDA 9 ve cuDNN 7 olarak bu varsayılan olarak, kitaplık yolu değil. CUDA 8 + cuDNN 5.1 için ayrıntılı su kullanmak için:

```
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
export CUDA_ROOT=/usr/local/cuda-8.0
```

Ayrıntılı su kullanmak için:
* Terminalde,: etkinleştirme [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) çalıştırın, ortam _python_. <br/>
* İçinde JupyterHub: [bağlanmak için JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnek not defterleri bulmak için deep_water dizine gidin.

## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | MXNet yüklü `C:\dsvm\tools\mxnet` Windows üzerinde ve `/dsvm/tools/mxnet` Linux üzerinde. Python bağlamaları yüklenen Python 3.5 [Linux ve Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ve Python 3.6 üzerinde [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). R bağlamaları, Ubuntu'da da yüklenir.   |
| Örneklere bağlantılar      | Örnek Jupyter not defterleri bulunur.    |
| DSVM ilgili araçları      | Keras      |
| Kullanma / çalıştırın nasıl?    | *, Bir terminal: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. <br/> * Jupyter'de: Bağlanma [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) veya [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnekleri mxnet dizinini açın.  |
 | Notları oluşturun | MXNet kaynağından Linux üzerinde yerleşik olarak bulunur. Bu derleme CUDA, cuDNN, NCCL ve MKL içerir. |

## <a name="nvidia-digits"></a>NVIDIA BASAMAK

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin ayrıntılı öğrenme modelleri hızla eğitim için NVIDIA sistemden öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | BASAMAK yüklü `/dsvm/tools/DIGITS` ve kullanılabilir bir hizmet olarak adlandırılan _basamak_.   |

### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?

VM X2Go ile oturum açın. Bir terminal hizmeti başlatın:

    sudo systemctl start digits

Hizmeti başlatmak için yaklaşık bir dakika sürer. Bir web tarayıcısını Başlat ve gidin `http://localhost:5000`. BASAMAK güvenli bir oturum açma sağlamaz ve VM dışında açıkta kalmamalıdır unutmayın.



## <a name="nvidia-smi"></a>NVIDIA SMI

|    |           |
| ------------- | ------------- |
| Nedir?   | NVIDIA GPU etkinliği sorgulama aracı      |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | _NVIDIA SMI_ sistemi yolu üzerinde kullanılabilir.   |
| Kullanma / çalıştırın nasıl? | Bir komut istemi (Windows) veya bir terminalde (Linux) başlatın ve ardından çalıştırın _NVIDIA SMI_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Theano Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.   |
| DSVM ilgili araçları      | Keras      |
| Kullanma / çalıştırın nasıl?    | * Bir terminal, (kök veya py35) python çalıştırın, sonra theano alma Python sürümü etkinleştirin. <br/> * Jupyter, Python 2.7 veya 3.5 çekirdek seçin ve ardından theano içeri aktarın.  <br/>Son MKL hatanın geçici olarak çözmek için katman iş parçacığı MKL ayarlamanız gerekir:<br/><br/>_export MKL_THREADING_LAYER=GNU_|



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Torch yüklü `/dsvm/tools/torch`. PyTorch Python 2.7 yüklenir (_kök_), Python 3.5 yanı sıra (_py35_) ortamı.   |
| Örneklere bağlantılar      | Torch örnekleri yer `/dsvm/samples/torch`. PyTorch örnekleri yer `/dsvm/samples/pytorch`.      |


## <a name="pytorch"></a>PyTorch

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | PyTorch yüklü [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Örneklere bağlantılar      | Örnek Jupyter not defterleri dahil edin ve örnekler de /dsvm/samples/pytorch içinde bulunabilir.      |
| DSVM ilgili araçları      | Torch      |
| Nasıl kullanma / çalıştırın |*, Bir terminal: doğru ortamı etkinleştirmek ve Python'ı çalıştırın. <br/> * Jupyter'de: Bağlanma [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), ardından örnekleri PyTorch dizinini açın.  |

## <a name="mxnet-model-server"></a>MXNet modeli sunucusu

|    |           |
| ------------- | ------------- |
| Nedir?   | MXNet ve ONNX modelleri için HTTP uç noktaları oluşturmak için bir sunucu      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | _mxnet modeli sunucu_ terminal bulunur.   |
| Örneklere bağlantılar      | Güncel örnekleri arayabilirsiniz [MXNet modeli sunucu sayfası](https://github.com/awslabs/mxnet-model-server).    |
| DSVM ilgili araçları      | MXNet      |

## <a name="tensorflow-serving"></a>TensorFlow sunma

|    |           |
| ------------- | ------------- |
| Nedir?   | Çıkarım TensorFlow model üzerinde çalıştırmak için bir sunucu      |
| Desteklenen DSVM sürümleri      | Linux     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | _tensorflow_model_server_ terminal kullanılabilir.   |
| Örneklere bağlantılar      | Örnekler mevcuttur [çevrimiçi](https://www.tensorflow.org/serving/).      |
| DSVM ilgili araçları      | TensorFlow      |

## <a name="tensorrt"></a>TensorRT

|    |           |
| ------------- | ------------- |
| Nedir?   | Derin öğrenme NVIDIA çıkarımı sunucudan vm'si. |
| Desteklenen DSVM sürümleri      | Ubuntu     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | TensorRT yüklü olarak bir _apt_ paket.   |
| Örneklere bağlantılar      | Örnekler mevcuttur [çevrimiçi](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).      |
| DSVM ilgili araçları      | TensorFlow hizmet verebilmesi için MXNet modeli sunucusu  |



