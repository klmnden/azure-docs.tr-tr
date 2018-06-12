---
title: Bir FPGA nedir? – Proje Brainwave – Azure Machine Learning
description: Modelleri ve derin sinir ağları FPGAs ile hızlandırmak öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: service
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5ed94c3b750c927ec48959c12388bd22de3d3df4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261959"
---
# <a name="what-is-fpga-and-project-brainwave"></a>FPGA ve proje Brainwave nedir?

Bu makalede alan programlanabilir kapısı dizilerine (FPGA) ve Azure machine gerçek zamanlı AI sağlamak learning FPGA nasıl tümleştirildiği tanıtılmaktadır.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs vs. CPU ve GPU'ya ASIC

Reconfigurable hiyerarşisini sonra üretim farklı şekillerde yapılandırılacak blokları sağlayan bağlantılar ve FPGAs programlanabilir mantık blokları dizisi içerir.

FPGAs programlamasına ve diğer yongaları performans karşılaştırma bileşimini sağlar:

![Azure Machine Learning FPGA karşılaştırması](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Merkezi işlem birimi (CPU)** genel amaçlı işlemciler. CPU performans grafik ve video işleme için uygun değildir.
- **Grafik işlem birimi (GPU)** paralel işleme sunar ve AI hesaplamalar için popüler bir seçimdir. CPU daha hızlı görüntü işleme GPU sonucu ile işleme paralel.
- **Uygulamaya özgü tümleşik devreler (ASIC)**, Google TensorFlow işlemci gibi özelleştirilmiş devreler birimleridir. Bu yongaları en yüksek verimlilik sunarken ASIC esnek olmayan.
- **FPGAs**olanlar gibi Azure üzerinde kullanılabilir, performans ASIC yakın sağlar ancak daha sonra yeniden yapılandırılması için esneklik sunar.

FPGAs her yeni Azure sunucusu sunulmuştur. Bing arama sıralama, derin sinir ağı (DNN) değerlendirmesi ve yazılım tanımlı ağ (SDN) hızlandırma Microsoft FPGAs zaten kullanıyor. Bu FPGA uygulamalar diğer görevler için CPU boşaltılırken gecikme süresini azaltın.

## <a name="project-brainwave-on-azure"></a>Azure üzerinde proje Brainwave

Proje Brainwave tasarlanmış bir donanım mimarisi Intel'in FPGA cihazlarda dayalı olan ve gerçek zamanlı AI hesaplamalar hızlandırmak için kullanılan ' dir. Bu ekonomik FPGA etkin mimarisi ile eğitilmiş bir sinir ağı hızlı bir şekilde olabildiğince ve düşük gecikme süresi ile çalıştırılabilir. Proje Brainwave önceden eğitilen DNNs bir hizmetin ölçeğini genişlet FPGAs arasında paralel hale.

- Performans

    FPGAs gerçek zamanlı inferencing istekleri için düşük gecikme süresine ulaşmanız mümkün kılar. Toplu işleme istek küçük parçalara kesme ve donanım kullanımını iyileştirmek için bir işlemciye besleme anlamına gelir. Toplu işleme etkin değil ve gecikme neden olabilir. Toplu işleme brainwave gerektirmez; Bu nedenle 10 kez daha düşük CPU ve GPU'ya karşılaştırıldığında gecikme olur.

- Esneklik

    FPGAs makine öğrenimi modellerini farklı türleri için yeniden. Bu esneklik en iyi sayısal duyarlık ve kullanılan bellek modeli dayalı uygulamalar hızlandırmak kolaylaştırır.

    Düzenli olarak yeni machine learning teknikler geliştirilen ve proje Brainwave'nin donanım tasarımı da hızlı bir şekilde gelişen. FPGAs reconfigurable olduğundan, hızla değişen AI algoritmaları gereksinimleriyle güncel mümkündür.

- Ölçek

    Microsoft Azure dünyanın en büyük bulut yatırım FPGAs ' dir. Azure'nın ölçek altyapısında Brainwave çalıştırabilirsiniz.

Proje Azure Machine Learning ile tümleşik Brainwave önizlemesi şu anda kullanılamıyor. Ve sınırlı bir önizleme bu bilgi işlem hızı işletmelerin ve tesis yararlanabilir böylece proje Brainwave kenarına getirmek de kullanılabilir.

Geçerli Önizleme'de, Brainwave TensorFlow dağıtım ve görüntü sınıflandırma ve tanınması için Intel FPGA donanımda ResNet50 tabanlı sinir ağları sınırlıdır. Daha fazla galeri modelleri ve diğer çerçeveler desteklemek için bir plan yok.

Aşağıdaki senaryolarda FPGA proje Brainwave mimarisine kullanın:

- Otomatik optik denetleme sistemi. Bkz: [gerçek zamanlı AI: Microsoft Project Brainwave önizlemesini bildirdi](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Kara kapak eşleme. Bkz: [derin öğrenme çıkarım FPGAs kara kapak havadan görüntüleri terabayt üzerinde eşleme gerçekleştirmek için nasıl kullanılacağını](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Bir web hizmeti için bir FPGA dağıtmayı?

Üst düzey akış featurizer ResNet50 kullanarak azure'da bir görüntü tanıma hizmeti oluşturmak için aşağıdaki gibidir:

1. ResNet50 Brainwave içinde zaten dağıtılmış. Diğer grafiklerle (tarihi giriş, Sınıflandırma ve benzeri) TensorFlow oluşturmak ve bir ardışık düzen tanımlayın (giriş featurize -> -> sınıflandırmak) bir hizmet tanımı json dosyasını kullanma. Tanım ve grafiklerinizi zip dosyasına sıkıştırın ve ZIP dosyasının Azure Blob depolama alanına yükleyin.
2. Blob depolamada ZIP dosyasının Azure ML Model yönetim API'sini kullanarak modeli kaydedin.
3. Hizmeti Azure ML Model yönetim API'sini kullanarak kayıtlı modeliyle dağıtın.

Makalede, bu işlem hakkında daha fazla bilgi [bir FPGA Azure Machine Learning ile bir web hizmeti olarak modeli dağıtmak](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>FPGA kullanmaya başlama

Azure Machine Learning donanım hızlandırılmış modelleri eğitilmiş DNN modeller Azure bulutta FPGAs dağıtmanıza izin verin. Başlamak için bkz:

- [Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning donanım hızlandırılmış proje Brainwave tarafından desteklenen modelleri](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Sonraki adımlar

[Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)

[FPGA SDK'sını yükleyin öğrenin](reference-fpga-package-overview.md)

[Ölçekte donanım: Azure + FPGA üstünde ölçekte ML: derleme 2018 (video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[Microsoft içinde yapılandırılabilir bulut FPGA tabanlı (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft Project Brainwave için gerçek zamanlı AI'nı sunar](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)