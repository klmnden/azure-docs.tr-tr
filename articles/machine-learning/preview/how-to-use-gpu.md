---
title: "Azure Machine Learning için GPU kullanma | Microsoft Docs"
description: "Bu makalede, Azure Machine Learning çalışma ekranındaki derin sinir ağları eğitmek için grafik işlem birimi (GPU) kullanmayı açıklar."
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: ce1557aed09384b0d7a0b65aabd473fe72ab740c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>GPU Azure Machine Learning ile kullanma
Grafik işlem birimi (GPU) genellikle belirli derin sinir ağı modelleri eğitim ortaya çıkar pkı'ya yoğun görevler işlemek için yaygın olarak kullanılır. GPU kullanarak modellerin eğitim süresini önemli ölçüde azaltabilir. Bu belgede, Azure ML çalışma ekranı kullanacak şekilde yapılandırma konusunda bilgi edinin [DSVM (veri bilimi sanal makine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) yürütme hedef olarak GPU ile donatılmış. 

## <a name="prerequisites"></a>Ön koşullar
- Nasıl yapılır bu kılavuzu için öncelikle gereklidir [Azure ML çalışma ekranı yüklemek](quickstart-installation.md).
- NVIDIA Gpu'lara bilgisayarlarda erişiminizin olması gerekir.
    - GPU ile doğrudan yerel makinede (Windows veya macOS), komut dosyalarınızı çalıştırabilirsiniz.
    - GPU olan bir makinede bir Docker kapsayıcısı komut dosyalarını da çalıştırabilirsiniz.

## <a name="execute-in-local-environment-with-gpus"></a>Yürütün _yerel_ GPU ortamı
Azure ML çalışma ekranı GPU ile donatılmış bir bilgisayara yükleyin ve karşı yürütme _yerel_ ortamı. Bu olabilir:
- Bir fiziksel Windows veya macOS makine.
- Windows Azure NC-serisi VM'ler şablon kullanılarak hazırlanmış bir DSVM gibi Windows VM (sanal makine).

Bu durumda, Azure ML çalışma ekranı içinde gerekli özel yapılandırma vardır. Yalnızca tüm gerekli sürücüleri, araç takımları ve GPU özellikli makine öğrenimi kitaplıkları yerel olarak yüklenmiş olduğundan emin olun. Yalnızca karşı yürütme _yerel_ nerede Python çalışma zamanı doğrudan erişim yerel GPU donanım ortamı.

1. AML çalışma ekranı açın. Git **dosya** ve **komut istemini açın**. 
2. Komut satırından GPU etkin derin öğrenme çerçevesi Microsoft Bilişsel araç seti, TensorFlow ve vb. gibi yükleyin. Örneğin:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.1 (1-bit SGD) with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-win_amd64.whl
```

3. Kitaplıkları öğrenme derin yararlanır Python kodu yazın.
4. Seçin _yerel_ ortamı işlem ve Python kodunu yürütün.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>Yürütün _docker_ GPU Linux VM ortamda
Azure ML çalışma ekranı ayrıca desteği yürütme Azure Linux VM'de Docker. Burada, güçlü sanal donanım parçasına pkı'ya yoğun işleri çalıştırmak ve bittiğinde kapatma tarafından yalnızca kullanım için ödeme için harika bir seçeneğiniz vardır. Ubuntu tabanlı DSVM İlkesi GPU herhangi bir Linux sanal makinede kullanmak mümkün olmakla birlikte, gerekli CUDA sürücüleri ve önceden yüklenmiş kitaplıkları'nın Kurulum çok daha kolay hale gelir. İzleyin aşağıdaki adımları:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Ubuntu tabanlı Linux veri bilimi sanal makine oluşturma
1. Web tarayıcınızı açın ve gidin [Azure portalı](https://portal.azure.com)

2. Seçin **+ yeni** portalın sol.

3. "Veri bilimi sanal makine için Linux (Ubuntu)" Market'te arayın.

4. Tıklatın **oluşturma** bir Ubuntu DSVM oluşturmak için.

5. Doldurmak **Temelleri** form gerekli bilgileri.
VM için konum seçerken, GPU VM'ler yalnızca belirli Azure bölgelerde, örneğin, kullanılabilir olduğuna dikkat edin **Orta Güney ABD**. Bkz: [bölgeye göre ürünleri işlem](https://azure.microsoft.com/regions/services/).
Kaydetmek için Tamam'ı **Temelleri** bilgi.

6. Sanal makine boyutunu seçin. NVIDIA GPU yongaları ile donatılmış NC önekli VM'ler ile boyutlarından birini seçin.  Tıklatın **tümünü görüntüle** gerektiği gibi tam listesini görmek için. Daha fazla bilgi edinmek [GPU donatılmış Azure Vm'leri](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

7. Kalan ayarlarını tamamlayın ve satın alma bilgileri gözden geçirin. VM oluşturmak için satın alma'ı tıklatın. Sanal makine için ayrılmış IP adresini not alın. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Azure ML çalışma ekranı içinde yeni bir proje oluşturun 
Kullanabileceğiniz _TensorFlow kullanarak MNIST sınıflandırma_ örnek, veya _CNTK sınıflandırma MNIST kümesiyle_ örnek.

### <a name="create-a-new-compute-target"></a>Yeni bir işlem hedef oluşturma
Azure ML çalışma ekranı komut satırından başlatın. Aşağıdaki komutu girin. Aşağıdaki örnekte yer tutucu metni kendi değerlerinizi adı, IP adresi, kullanıcı adı ve parola ile değiştirin. 

```batch
C:\MyProj> az ml computetarget attach remotedocker --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" 
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Azure ML çalışma ekranına erişim GPU yapılandırın
Proje geri gidin ve açık **dosya görünümü**ve isabet **yenileme** düğmesi. Şimdi iki yeni yapılandırma dosyalarını görmek `my_dsvm.compute` ve `my_dsvm.runconfig`.
 
Açık `my_dsvm.compute`. Değişiklik `baseDockerImage` için `microsoft/mmlspark:plus-gpu-0.7.9` ve yeni bir satır ekleyin `nvidiaDocker: true`. Böylece dosyayı bu iki satır olmalıdır:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9
nvidiaDocker: true
```
 
Şimdi açmak `my_dsvm.runconfig`, değiştirme `Framework` değeri `PySpark` için `Python`. Bu satır görmüyorsanız, varsayılan değer olacaktır beri eklemek `PySpark`.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Başvuru derin öğrenme çerçevesi 
Açık `conda_dependencies.yml` dosya ve framework Python paketlerini öğrenme derin GPU sürümünü kullandığınızdan emin olun. Bu değişikliği yapmak gereken şekilde örnek projelerine CPU sürümleri listelenir.

TensorFlow örneğin: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Örneğin Microsoft Bilişsel Araç Seti için:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.1 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk-2.1-cp35-cp35m-linux_x86_64.whl
```

Microsoft Bilişsel çoklu GPU Vm'lerinde performans iyileştirmeleri sağlayan araç seti 1 bit SGD sürümünü de kullanabilirsiniz. Not [1 bit SGD lisans gereksinimini](https://docs.microsoft.com/cognitive-toolkit/cntk-1bit-sgd-license).

```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip:    
    # use the Linux build of the Microsoft Cognitive Toolkit 2.1 with 1-bit SGD and GPU support
    - https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-linux_x86_64.whl
```

### <a name="execute"></a>Yürütme
Şimdi, Python komut dosyalarını çalıştırmak hazırsınız. Azure ML çalışma ekranı kullanarak içinde çalıştırabilirsiniz `my_dsvm` bağlamı veya başlatın, komut satırından:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
GPU kullandığını doğrulamak için aşağıdakine benzer görmek için çalışma çıktıyı inceleyin:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Tebrikler! Komut, yalnızca GPU güç Docker kapsayıcısı aracılığıyla harnessed!

## <a name="next-steps"></a>Sonraki adımlar
Azure ML galeri derin sinir ağı eğitim hızlandırmak üzere GPU kullanma örneği bakın.
