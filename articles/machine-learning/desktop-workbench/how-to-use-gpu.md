---
title: Azure Machine Learning için GPU kullanmayı | Microsoft Docs
description: Bu makalede, Azure Machine Learning workbench'te derin sinir ağı eğitmek için grafik işlem birimi (GPU) kullanmayı açıklar.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 09d8e3da543cdf4433d986b321697abcad88eb22
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157999"
---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>Azure Machine Learning'de GPU kullanma
Grafik işlem birimi (GPU), genellikle bazı derin sinir ağı modelleri eğitimindeki oluşabilir işlem bakımından yoğun görevlerini işlemek için yaygın olarak kullanılır. GPU'ları kullanarak modellerin eğitim süresini önemli ölçüde azaltabilir. Bu belgede, Azure ML Workbench uygulamasını kullanmak için yapılandırma hakkında bilgi edinin [DSVM (veri bilimi sanal makinesi)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) yürütme hedefi Gpu'lar ile donatılmış. 

## <a name="prerequisites"></a>Önkoşullar
- Bu nasıl yapılır kılavuzunda adımlamak için öncelikle gereken [Azure ML Workbench'i yükleme](../service/quickstart-installation.md).
- NVIDIA GPU'ları ile bilgisayarlarda erişiminiz olması gerekir.
    - Komut dosyalarınızı Gpu'lar ile doğrudan yerel makine üzerinde (Windows veya macOS) çalıştırabilirsiniz.
    - Ayrıca GPU ile Linux makinesinde bir Docker kapsayıcısında komut dosyalarını çalıştırabilirsiniz...

## <a name="execute-in-local-environment-with-gpus"></a>İçinde yürütülen _yerel_ Gpu'lar ile ortam
Azure ML Workbench Gpu'lar ile donatılmış bir bilgisayara yükleyin ve karşı yürütmek _yerel_ ortam. Bu olabilir:
- Fiziksel Windows veya macOS makine.
- Bir Windows sanal makinesi (VM) gibi Azure NC serisi VM'ler şablon kullanılarak hazırlanmış bir Windows DSVM'sini.

Bu durumda, Azure ML Workbench'te gereken özel bir yapılandırma var. Yalnızca tüm gerekli sürücüleri, araç setleri ve GPU etkin machine learning kitaplıkları yerel olarak yüklü olduğundan emin olun. Yalnızca karşı yürütmek _yerel_ burada Python çalışma zamanını doğrudan erişim sağlayabilir yerel GPU donanım ortam.

1. AML Workbench'i açın. Git **dosya** ve **komut istemini Aç**. 
2. Microsoft Bilişsel araç seti, TensorFlow ve vb. gibi ayrıntılı öğrenme GPU özellikli framework komut satırından yükleyin. Örneğin:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.5 with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

3. Derin öğrenme kitaplıkları yararlanan Python kod yazın.
4. Seçin _yerel_ işlem ortamı ve Python kodunu yürütün.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>İçinde yürütülen _docker_ GPU ile Linux sanal ortam
Azure ML Workbench de yürütme, destek Azure Linux VM'de Docker. Burada güçlü sanal donanım parçası üzerinde işlem bakımından yoğun işlerini çalıştırın ve işiniz bittiğinde kapatıp yalnızca kullanım için ödeme harika bir seçenekleri var. Ubuntu tabanlı DSVM İlkesi, GPU'ları herhangi bir Linux sanal makinede kullanmak mümkün olsa da, gerekli CUDA sürücüleri ve önceden yüklenmiş, kitaplıkları'nın Kurulum çok daha kolay hale gelir. İzleyin aşağıdaki adımları:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Azure'da bir Ubuntu tabanlı Linux veri bilimi sanal makinesi oluşturma
1. Web tarayıcınızı açın ve gidin [Azure portalı](https://portal.azure.com)

2. Seçin **+ yeni** portalının sol taraftaki.

3. "Veri bilimi sanal makinesi için Linux (Ubuntu)" Market'te arayın.

4. Tıklayın **Oluştur** bir Ubuntu DSVM oluşturma.

5. Doldurun **Temelleri** formunu gerekli bilgiler ile.
VM'niz için konum seçildiğinde, GPU Vm'lerine yalnızca belirli Azure bölgelerinde Örneğin, kullanılabilir olduğunu unutmayın **Orta Güney ABD**. Bkz: [bölgelere göre kullanılabilir ürünler işlem](https://azure.microsoft.com/regions/services/).
Kaydetmek için Tamam'a tıklayın **Temelleri** bilgileri.

6. Sanal makine boyutu seçin. NVIDIA GPU yongaları ile donatılmış NC önekli VM boyutlarından birini seçin.  Tıklayın **görünümü tüm** gerektiğinde tam listesini görmek için. Daha fazla bilgi edinin [GPU donatılmış Azure Vm'leri](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

7. Geri kalan ayarlar tamamlayın ve satın alma bilgileri gözden geçirin. Satın alma, VM oluşturmak için tıklayın. Sanal makineye ayrılan IP adresini not alın. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Azure ML Workbench'te yeni proje oluşturma 
Kullanabileceğiniz _TensorFlow kullanarak MNIST sınıflandırma_ örnek, veya _CNTK sınıflandırma MNIST kümesiyle_ örnek.

### <a name="create-a-new-compute-target"></a>Yeni bir işlem hedefi oluşturma
Azure ML Workbench komut satırından başlatın. Aşağıdaki komutu girin. Aşağıdaki örnekte yer tutucu metni adı, IP adresi, kullanıcı adı ve parolasını kendi değerlerinizle değiştirin. 

```batch
C:\MyProj> az ml computetarget attach remotedocker --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" 
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Azure ML Workbench GPU erişimi yapılandırma
Proje geri gidin ve açık **dosya görünümü**ve isabet **Yenile** düğmesi. Şimdi iki yeni yapılandırma dosyalarını gördüğünüz `my_dsvm.compute` ve `my_dsvm.runconfig`.
 
Açık `my_dsvm.compute`. Değişiklik `baseDockerImage` için `microsoft/mmlspark:plus-gpu-0.9.9` ve yeni bir satır eklemek `nvidiaDocker: true`. Bu nedenle dosyada şu iki satırı olmalıdır:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9
nvidiaDocker: true
```
 
Artık `my_dsvm.runconfig`, değiştirme `Framework` değerini `PySpark` için `Python`. Bu satırı görmüyorsanız, varsayılan değer olduğundan, ekleme `PySpark`.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Derin öğrenme Framework başvurusu 
Açık `conda_dependencies.yml` dosya ve derin framework Python paketlerini öğrenme GPU sürümünü kullandığınızdan emin olun. Bu değişiklik yapmanız için örnek projelerine CPU sürümleri listelenir.

TensorFlow örneğin: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Microsoft Bilişsel Araç Seti için örnek:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.5 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

### <a name="execute"></a>Yürütme
Artık, Python betikleri çalıştırmaya hazırsınız. Azure ML Workbench kullanarak içinde çalıştırabilirsiniz `my_dsvm` bağlamı veya başlatabilir, komut satırından:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
GPU kullandığını doğrulamak için aşağıdaki gibi görmek için çalıştırma çıktıyı inceleyin:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Tebrikler! Betiğinizi yalnızca GPU gücünü bir Docker kapsayıcısı üzerinden harnessed!

## <a name="next-steps"></a>Sonraki adımlar
Azure ML galeri derin sinir ağı eğitim hızlandırmak için GPU kullanımı örneği bakın.
