---
title: Bir FPGA nedir? – Project Brainwave – Azure Machine Learning
description: Modelleri ve derin sinir ağları ile FPGA hızlandırma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5c8efcbb5f2e9014c9edabfc2aee37c8c29ea0b4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646771"
---
# <a name="what-is-fpga-and-project-brainwave"></a>FPGA ve Project Brainwave nedir?

Bu makalede bir alanda programlanabilir kapı dizileri (FPGA) ve FPGA gerçek zamanlı AI sağlamak için Azure machine learning ile nasıl tümleştirildiği tanıtır.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA vs. CPU, GPU ve ASIC

Sunmanın hiyerarşisini sonra üretim farklı şekilde yapılandırılacak blokları sağlayan bağlantılar ve FPGA programlanabilir mantıksal blokları dizisi içerir.

FPGA programlama ve diğer yongalardan performans karşılaştırma sağlar:

![Azure Machine Learning FPGA karşılaştırması](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Merkezi işlem birimi (CPU)** genel amaçlı işlemci. CPU performansı, grafik ve video işleme için uygun değil.
- **Grafik işlem birimi (GPU)** paralel işleme özelliği sunar ve yapay ZEKA hesaplamalar için popüler bir seçenektir. GPU'ları CPU daha hızlı görüntü işleme sonucunda işlemeyi paralel.
- **Özel uygulama tümleşik devreler (ASIC)**, Google TensorFlow işlemci gibi özelleştirilmiş devreler birimleridir. Bu yongaları en yüksek verimlilik sunarken, ASICS faaliyetini.
- **FPGA**gibi Azure üzerinde mevcut kodlar, ASIC yakın bir performans sağlar ancak daha sonra yeniden yapılandırılması esnekliği sunar.

FPGA her yeni Azure sunucusu sunulmuştur. Bing arama, sıralama, derin sinir ağı (DNN) değerlendirmesi ve yazılım tanımlı ağ (SDN) hızlandırma Microsoft FPGA zaten kullanıyor. Bu FPGA uygulamalar, diğer görevler için CPU boşaltılırken gecikme süresini azaltın.

## <a name="project-brainwave-on-azure"></a>Azure'da Project Brainwave

Project Brainwave tasarlanmış bir donanım mimarisiyle Intel FPGA cihazlara dayanarak ve gerçek zamanlı AI hesaplamalar hızlandırmak için kullanılan ' dir. Bu ekonomik FPGA özellikli mimarisi ile eğitilmiş bir sinir ağı olabildiğince çabuk şekilde ve daha düşük gecikme süresi ile çalıştırılabilir. Project Brainwave arasında FPGA, hizmetin ölçeğini genişletin için önceden eğitilmiş Dnn'leri paralel hale getirebilirsiniz.

- Performans

    FPGA gerçek zamanlı çıkarım istekleri için düşük gecikme süresine ulaşmanız mümkün kılar. Toplu işleme, bozucu bir istek küçük parçalara ayarlama ve bunları donanım kullanımı artırmak için bir işlemci besleme anlamına gelir. Toplu işlem etkili değildir ve gecikmeye neden. Toplu işleme brainwave gerektirmez; Bu nedenle gecikme süresi 10 kez daha düşük CPU ve GPU gereklidir.

- Esneklik

    FPGA, makine öğrenimi modelleri farklı türleri için yapılandırılabilen. Bu esneklik en iyi sayısal duyarlık ve kullanılan bellek modeli göre uygulamaları hızlandırmak kolaylaştırır.

    Yeni makine öğrenimi tekniklerinden düzenli bir şekilde Geliştiriliyor ve Project Brainwave'nın donanım tasarımı da hızlı bir şekilde gelişmektedir. FPGA sunmanın olduğundan, hızla değişen yapay ZEKA algoritmalarının gereksinimleri ile güncel kalın mümkündür.

- Ölçek

    Microsoft Azure, dünyanın en büyük bulut yatırımı FPGA. Azure'nın ölçek altyapısında Brainwave çalıştırabilirsiniz.

Azure Machine Learning ile tümleştirilmiş Project Brainwave önizlemesi şu anda kullanılabilir. Ve sınırlı bir önizleme bu bilgi işlem hızı işletmelerin ve tesis avantajlarından yararlanabilmeniz için edge Project Brainwave getirmek de kullanılabilir.

Geçerli Önizleme'de, TensorFlow dağıtım ve görüntü sınıflandırma ve tanıma için Intel FPGA donanımda ResNet50 tabanlı sinir ağları Brainwave sınırlıdır. Daha fazla galeri model ve diğer çerçeveleri desteklemeye yönelik planlar mevcuttur.

Aşağıdaki senaryolarda FPGA Project Brainwave mimarisine kullanın:

- Otomatik olarak optik denetleme sistemi. Bkz: [gerçek zamanlı AI: Microsoft Project Brainwave önizlemesini duyurdu](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Land kapak eşleme. Bkz: [FPGA derin öğrenme çıkarımı için Land kapak havadan görüntüleri terabaytlarca üzerinde eşleme gerçekleştirmek için nasıl kullanılacağını](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Bir web hizmeti için bir FPGA dağıtmayı?

Bir özelliği Oluşturucu ResNet50 kullanarak azure'da bir görüntü tanıma hizmeti oluşturmaya yönelik üst düzey akışı şu şekildedir:

1. ResNet50 zaten Brainwave içinde dağıtılır. TensorFlow ile diğer grafikleri (tarih girişi, Sınıflandırma ve benzeri) oluşturun ve bir işlem hattı tanımlayın (Giriş -> özellik kazandırın -> sınıflandırmak) bir hizmet tanımı json dosyasını kullanarak. Graflar ve tanımı bir zip dosyası olarak sıkıştırıp ve Azure Blob depolama alanına ZIP dosyasını karşıya yükleyin.
2. Blob depolamada zip dosyası ile Azure ML Model Yönetimi API'si kullanarak modeli kaydedin.
3. Azure ML Model Yönetimi API'sini kullanarak kayıtlı modeliyle hizmeti dağıtın.

Makalede, bu işlem hakkında daha fazla bilgi [model üzerinde bir FPGA Azure Machine Learning ile bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>FPGA kullanmaya başlayın

Azure Machine Learning donanım hızlandırılmış modelleri eğitilen DNN modelleri Azure bulutunda FPGA dağıtmanıza izin verin. Başlamak için bkz:

- [Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning donanım hızlandırılmış Project Brainwave tarafından desteklenen modeller](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Sonraki adımlar

[Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)

[FPGA SDK'sını yükleme hakkında bilgi edinin](reference-fpga-package-overview.md)

[Hiperölçekli donanım: ML uygun ölçekte Azure + FPGA üzerine: Build 2018'e (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[Microsoft FPGA tabanlı yapılandırılabilir bulut (video) içinde](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft Project Brainwave için gerçek zamanlı AI'nı sunar](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)