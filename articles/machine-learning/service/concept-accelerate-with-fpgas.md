---
title: Bir FPGA ve Project Brainwave nedir? -Azure Machine Learning
description: Azure üzerinde modelleri ve derin sinir ağları ile FPGA hızlandırma konusunda bilgi edinin. Bu makalede bir alanda programlanabilir kapı dizileri (FPGA) ve Azure Machine Learning için bir Azure FPGA modelinizi dağıtırken gerçek zamanlı yapay zeka (AI) nasıl sağladığını tanıtır.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 9/24/2018
ms.openlocfilehash: adcd812bc63f98e4cbde949946bf32a037c6d704
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158700"
---
# <a name="what-is-fpga-and-project-brainwave"></a>FPGA ve Project Brainwave nedir?

Bu makalede bir alanda programlanabilir kapı dizileri (FPGA) ve Azure Machine Learning için bir Azure FPGA modelinizi dağıtırken gerçek zamanlı yapay zeka (AI) nasıl sağladığını tanıtır.

FPGA dizi programlanabilir mantıksal bloğu içerir ve sunmanın hiyerarşisini eşitliyor. Bağlantılar çeşitli yolları sonrası üretim yapılandırılması bu blokları izin verin. FPGA programlama ve diğer yongalardan karşılaştırıldığında performansı sağlar.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA vs. CPU, GPU ve ASIC

![Azure Machine Learning FPGA karşılaştırması](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|İşlemci||Açıklama|
|---|:-------:|------|
|Özel uygulama tümleşik devreler|ASICS|Özel bağlantı hatları, Google TensorFlow işlemci birimi (TPU) gibi yüksek verimlilik sağlar. Gereksinimleriniz değiştikçe bunlar yapılandırılamaz.|
|Alanda programlanabilir kapı dizileri|FPGA|FPGA, azure'da kullanılabilen gibi ASICS yakın bir performans sağlar, ancak esnek ve sunmanın yeni mantığını uygulamak için zaman içinde.|
|Grafik işleme birimleri|GPU'ları|Daha hızlı CPU'lar daha görüntü işleme sırasında getirerek paralel işleme özellikleri sunan yapay ZEKA hesaplamalar için popüler bir seçimdir.|
|Merkezi işlem birimleri|CPU’lar|Genel amaçlı işlemci performansı grafik ve video işleme için uygun değil.|

## <a name="project-brainwave-on-azure"></a>Azure'da Project Brainwave

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) tabanlı Intel FPGA cihazlarda, veri uzmanlarının ve geliştiricilerin gerçek zamanlı AI hesaplamalar hızlandırmak için kullanın, Microsoft'un ekonomik donanım mimari.  Bu FPGA özellikli mimarisi sunar **performans**, **esneklik**, ve **ölçek** ve Azure'da kullanıma sunuldu.

**FPGA gerçek zamanlı çıkarım istekleri için düşük gecikme süresine ulaşmanız mümkün kılar.** Toplu işleme, büyük miktarda veri toplama ve donanım kullanımı artırmak için bir işlemci besleme anlamına gelir. Daha fazla veri işlenmesi gerektiğinden, toplu işlem gecikme süresi neden olabilir, ancak üretilen işi artırabilir. Project Brainwave uygulamaları sinir işleme birimi, toplu işleme gerektirmez; Bu nedenle gecikmesi birçok kez daha düşük CPU ve GPU olabilir.

### <a name="reconfigurable-power"></a>Sunmanın güç
**FPGA, makine öğrenimi modelleri farklı türleri için yapılandırılabilen.** Bu esneklik en iyi sayısal duyarlık ve kullanılan bellek modeli göre uygulamaları hızlandırmak kolaylaştırır.

Yeni makine öğrenimi tekniklerinden düzenli bir şekilde Geliştiriliyor ve Project Brainwave'nın donanım tasarımı da hızlı bir şekilde gelişmektedir. FPGA sunmanın olduğundan, hızla değişen yapay ZEKA algoritmalarının gereksinimleri ile güncel kalın mümkündür.

### <a name="whats-supported-on-azure"></a>Azure'da desteklenen özellikler
**Microsoft Azure, dünyanın en büyük bulut yatırımı FPGA.** Azure'nın ölçek altyapısında Project Brainwave çalıştırabilirsiniz.

Bugün, Project Brainwave destekler:
+ Görüntü sınıflandırma ve tanıma senaryoları
+ TensorFlow dağıtım
+ Üst: ResNet 50, ResNet 152, VGG-16, SSD VGG ve DenseNet 121
+ Intel FPGA donanım 

Bu FPGA özellikli donanım mimarisi kullanarak, eğitilen sinir ağları, hızlı ve daha düşük gecikme süresi ile çalıştırın. Project Brainwave arasında FPGA, hizmetin ölçeğini genişletin için önceden eğitilmiş derin sinir ağı (DNN) paralel hale getirebilirsiniz. Dnn'leri, öğrenme veya güncelleştirilmiş ağırlıklara eşleşmiş aktarımı için derin bir özelliği Oluşturucu olarak önceden eğitilmiş.

### <a name="scenarios-and-applications"></a>Senaryolar ve uygulamalar

Project Brainwave Azure Machine Learning ile tümleşiktir. DNN değerlendirme, Bing Arama derecelendirmesi ve yazılım için Microsoft'un kullandığı FPGA diğer görevler için CPU boşaltılırken gecikme süresini azaltmak için ağ (SDN) hızlandırma tanımlanır.

Aşağıdaki senaryolarda FPGA Project Brainwave mimarisine kullanın:
+ [Optik denetleme sistemi otomatik](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Land kapak eşleme](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>FPGA azure'da dağıtın

Azure FPGA dağıtımı için bir özelliği Oluşturucu desteklenen Dnn'leri kullanarak azure'da bir görüntü tanıma hizmeti oluşturmak için iş akışı şu şekildedir:

1. Bir dosya olan bir hizmet tanımı oluşturmak için Python için Azure Machine Learning SDK'sı kullanan temel TensorFlow üzerinde bir işlem hattı grafiklerin (girdi, özelliği oluşturucu ve sınıflandırıcı) açıklayan. Dağıtım komut otomatik olarak grafikler ve tanımı bir ZIP dosyası olarak sıkıştırıp ve ZIP Azure Blob depolama alanına yükleyin.  DNN FPGA üzerinde çalıştırmak için Project Brainwave üzerinde zaten dağıtılmış.

1. Azure Blob Depolama alanında ZIP dosyasıyla SDK'sını kullanarak modeli kaydedin.

1. Hizmet SDK'sını kullanarak kayıtlı modeli ile dağıtın.

Dağıtımına başlamak eğitilen DNN modellere FPGA Azure ile bu makalede, cloud **"[model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)"**.


## <a name="next-steps"></a>Sonraki adımlar

Bu videoları ve bloglar out denetleyin:

+ [Hiperölçekli donanım: ML uygun ölçekte Azure + FPGA üzerine: Build 2018'e (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [Microsoft FPGA tabanlı yapılandırılabilir bulut (video) içinde](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave için gerçek zamanlı AI: Proje giriş sayfası](https://www.microsoft.com/research/project/project-brainwave/)
