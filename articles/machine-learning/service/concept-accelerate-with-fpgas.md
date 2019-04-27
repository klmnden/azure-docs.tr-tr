---
title: FPGA ve Project Brainwave nedir?
titleSuffix: Azure Machine Learning service
description: Azure üzerinde modelleri ve derin sinir ağları ile FPGA hızlandırma konusunda bilgi edinin. Bu makalede bir alanda programlanabilir kapı dizileri (FPGA) ve Azure Machine Learning hizmeti için bir Azure FPGA modelinizi dağıtırken gerçek zamanlı yapay zeka (AI) nasıl sağladığını tanıtır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 1/10/2019
ms.custom: seodec18
ms.openlocfilehash: 716de6d491be753c5c32c0a2774404140010f72c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821229"
---
# <a name="what-are-fpgas-and-project-brainwave"></a>FPGA ve Project Brainwave nedir?

Bu makalede bir alanda programlanabilir kapı dizileri (FPGA) ve Azure Machine Learning hizmeti için bir Azure FPGA modelinizi dağıtırken gerçek zamanlı yapay zeka (AI) nasıl sağladığını tanıtır.

Sunmanın hiyerarşisini eşitliyor ve FPGA programlanabilir mantıksal blokları dizisi içerir. Bağlantılar çeşitli şekillerde sonra üretim yapılandırılması bu blokları izin verin. Diğer yongalardan karşılaştırıldığında FPGA programlama ve performans sağlar.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA vs. CPU, GPU ve ASIC

Aşağıdaki tablo ve diyagram nasıl FPGA diğer işlemcilere karşılaştırma gösterir.

![Azure Machine Learning hizmeti FPGA karşılaştırma diyagramı](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|İşlemci||Açıklama|
|---|:-------:|------|
|Özel uygulama tümleşik devreler|ASICS|Özel bağlantı hatları, Google TensorFlow işlemci birimi (TPU) gibi yüksek verimlilik sağlar. Gereksinimleriniz değiştikçe bunlar yapılandırılamaz.|
|Alanda programlanabilir kapı dizileri|FPGA'lar|FPGA, azure'da kullanılabilen gibi ASICS yakın bir performans sağlar. Bunlar ayrıca yeni mantığını uygulamak için zaman içinde esnek ve sunmanın.|
|Grafik işleme birimleri|GPU'ları|Yapay ZEKA hesaplamalar için popüler bir seçimdir. GPU'ları, daha hızlı CPU'lar daha görüntü işleme sırasında kolaylaştırır, paralel işleme özellikleri sunar.|
|Merkezi işlem birimleri|CPU’lar|Genel amaçlı işlemcilerde, performansını, grafik ve video işleme için ideal değildir.|

## <a name="project-brainwave-on-azure"></a>Azure'da Project Brainwave

[Project Brainwave](https://www.microsoft.com/en-us/research/project/project-brainwave/) Microsoft gelen donanım mimaridir. Bu, gerçek zamanlı AI hesaplamalar hızlandırmak için hangi veri bilimcilerine ve geliştiricilere kullanmak Intel FPGA cihazlarda temel alır. FPGA özellikli bu mimari, performans, esneklik ve ölçek sağlar ve Azure'da kullanıma sunuldu.

FPGA gerçek zamanlı çıkarım istekleri için düşük gecikme süresine ulaşmanız mümkün kılar. Zaman uyumsuz istekler (toplu) gerekli değildir. Toplu işlem gecikme süresi, daha fazla veri işlenmesi gerektiğinden neden olabilir. Project Brainwave uygulamaları sinir işleme birimi, toplu işleme gerektirmez; Bu nedenle gecikmesi CPU ve GPU işlemcilerle karşılaştırıldığında birçok kez daha düşük olabilir.

### <a name="reconfigurable-power"></a>Sunmanın güç
Makine öğrenimi modellerini farklı türleri için FPGA yeniden yapılandırabilirsiniz. Bu esneklik en iyi sayısal duyarlık ve kullanılan bellek modeli göre uygulamaları hızlandırmak kolaylaştırır. FPGA sunmanın olduğundan, yapay ZEKA algoritmalarının hızla değişen gereksinimleri ile güncel kalın.

### <a name="whats-supported-on-azure"></a>Azure'da desteklenen özellikler
Microsoft Azure, dünyanın en büyük bulut yatırımı FPGA. Azure'nın ölçek altyapısında Project Brainwave çalıştırabilirsiniz.

Bugün, Project Brainwave destekler:
+ Görüntü sınıflandırma ve tanıma senaryoları
+ TensorFlow dağıtım
+ Üst: ResNet 50, ResNet 152 VGG-16, SSD VGG ve DenseNet 121
+ Intel FPGA donanım 

Bu FPGA özellikli donanım mimarisi kullanarak, eğitilen sinir ağları, hızlı ve daha düşük gecikme süresi ile çalıştırın. Project Brainwave arasında FPGA, hizmetin ölçeğini genişletin için önceden eğitilmiş derin sinir ağı (DNN) paralel hale getirebilirsiniz. Dnn'leri, öğrenme veya güncelleştirilmiş ağırlıklara eşleşmiş aktarımı için derin bir özelliği Oluşturucu olarak önceden eğitilmiş.

### <a name="scenarios-and-applications"></a>Senaryolar ve uygulamalar

Project Brainwave Azure Machine Learning ile tümleşiktir. DNN değerlendirme, Bing Arama derecelendirmesi ve yazılım için Microsoft'un kullandığı FPGA gecikme süresi, diğer görevler için CPU boşaltılırken azaltmak için ağ (SDN) hızlandırma tanımlanır.

Aşağıdaki senaryolarda FPGA Project Brainwave mimarisine kullanın:
+ [Optik denetleme sistemi otomatik](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Land kapak eşleme](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>FPGA azure'da dağıtın

Azure'da bir görüntü tanıma hizmeti oluşturmak için Azure FPGA dağıtımı için bir özelliği Oluşturucu desteklenen Dnn'leri kullanabilirsiniz:

1. Kullanım [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) bir hizmet tanımı oluşturmak için. Bir hizmet tanımı, bir işlem hattı üzerinde TensorFlow tabanlı grafikler (girdi, özelliği oluşturucu ve sınıflandırıcı) açıklayan bir dosyadır. Dağıtım komut otomatik olarak grafikler ve tanımı bir ZIP dosyası olarak sıkıştırır ve ZIP Azure Blob depolama alanına yükler. DNN FPGA üzerinde çalıştırmak için Project Brainwave üzerinde zaten dağıtılmış.

1. SDK ZIP dosyasını Azure Blob Depolama kullanarak modeli kaydedin.

1. Kayıtlı modeli ile hizmet SDK'sını kullanarak dağıtın.

DNN eğitilen modelleri için Azure bulutunda FPGA dağıtımına başlamak için bkz: [model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu videoları ve bloglar out denetleyin:

+ [Hiperölçekli donanım: ML uygun ölçekte Azure + FPGA üzerinde: Build 2018'e (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [Microsoft FPGA tabanlı yapılandırılabilir bulut (video) içinde](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave için gerçek zamanlı AI: Proje giriş sayfası](https://www.microsoft.com/research/project/project-brainwave/)
