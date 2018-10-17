---
title: Azure Batch AI hizmeti - Yapay zeka eğitimi | Microsoft Docs
description: GPU ve CPU kümelerinde yapay zeka (AI) ve diğer makine öğrenmesi modellerini eğitmek için yönetilen Azure Batch AI hizmetini kullanma hakkında bilgi edinin.
services: batch-ai
documentationcenter: ''
author: alexsuttonms
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/01/2018
ms.author: danlep
ms.openlocfilehash: 98497812e75d07fc153e0e351331c05484164fdd
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052708"
---
# <a name="what-is-azure-batch-ai"></a>Azure Batch AI nedir?

Azure Batch AI, veri bilimcilerinin ve yapay zeka araştırmacılarının karmaşık bir altyapıyı yönetmek zorunda kalmadan Azure’da makine öğrenimi ve yapay zeka modellerini uygun ölçekte eğitmelerine ve test etmelerine yardımcı olmak için tasarlanmış bir yönetilen hizmettir. İşlem kaynaklarını, çalıştırmak istediğiniz işleri ve model girişleri ile çıkışların depolanmasını istediğiniz konumu tanımladığınızda, Batch AI geri kalan her şeyi sizin için yapar.

Batch AI’ı tek başına veya daha büyük bir geliştirme iş akışının parçası olarak model eğitimi gerçekleştirmek üzere kullanabilirsiniz:

* [GPU](../virtual-machines/linux/sizes-gpu.md) veya CPU kümelerinde makine öğrenimi ve yapay zeka modellerini eğitmek, test etmek ve toplu olarak puanlamak için Batch AI’ı tek başına kullanın. 

* [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) veya diğer [Azure AI Platform araçları](https://azure.microsoft.com/overview/ai-platform/) ile bir iş akışındaki bir Batch AI kümesini hedefleyin. Azure ML veri hazırlama, deneme ve iş geçmişi için zengin bir deneyim sağlar. Azure ML ayrıca eğitilmiş bir modeli bir kapsayıcı içinde paketleyebilir ve bir modeli çıkarım için veya IoT cihazlarına dağıtabilir.  

## <a name="train-machine-learning-and-ai-models"></a>Makine öğrenimi ve yapay zeka modellerini eğitme

Makine öğrenimi modellerinin yanı sıra derin sinir ağları (derin öğrenme) ve diğer yapay zeka yaklaşımlarını eğitmek için Batch AI kullanın. Batch AI [TensorFlow](https://github.com/tensorflow/tensorflow), [PyTorch](https://github.com/pytorch/pytorch), [Chainer](https://github.com/chainer/chainer) ve [Microsoft Cognitive Toolkit (CNTK)](https://github.com/Microsoft/CNTK) dahil popüler açık kaynak yapay zeka kitaplıkları ve çerçeveleri için yerleşik destek sunar.

Sorunun görüldüğü alanı belirleyip verilerinizi hazırladıktan sonra, model fikirlerini test etmek için Batch AI ile etkileşimli olarak çalışın. Ölçekte deneme yapmak için hazır olduğunuzda, MPI veya diğer iletişim kitaplıkları ile birden çok GPU üzerinde işleri başlatın ve paralel olarak daha fazla deneme çalıştırın.

Batch AI çeşitli yollarla uygun ölçekte modeller eğitmenize yardımcı olur. Örnek: 

|  |  |
|---------|---------|
| **Eğitimi dağıtma**<br/>![Dağıtılmış eğitim](./media/overview/distributed-training.png)  | Daha yüksek miktarlarda veri içeren daha geniş ağları eğitmek için birden çok ağa bağlı GPU arasında tek bir iş için eğitimin ölçeğini büyütün.|
| **Deneme**<br/>![Deneme](./media/overview/experimentation.png) | Birden fazla iş ile eğitimin ölçeğini artırın. Yeni fikirleri test etmek veya doğruluk ve performansı iyileştirmek için hiperparametreleri ayarlamak üzere parametre süpürmeleri çalıştırın. |
| **Paralel olarak yürüt**![Paralel yürütme](./media/overview/parallel-execution.png) | İşlerinizi daha hızlı yapmak için bir sunucu grubunda paralel olarak çalıştırarak aynı anda birçok modeli eğitin veya puanlayın.|

Bir modeli eğittiğinizde, eğitim betiğinizin bir parçası olarak yapmadıysanız modeli test etmek veya toplu puanlama gerçekleştirmek için Batch AI kullanın.

## <a name="how-it-works"></a>Nasıl çalışır?

AI eğitimi ve testi için işlem kaynaklarını yönetmek ve işleri zamanlamak için Batch AI SDK’ları, komut satırı betikleri veya Azure portalı kullanın: 

* **VM kümelerini sağlama ve ölçeklendirme** - Düğüm (VM) sayısını seçin ve bir GPU özellikli VM’yi veya eğitim ihtiyaçlarınızı karşılayan başka bir VM’yi seçin. Kaynakları yalnızca onlara ihtiyacınız olduğunda kullanmak için, düğüm sayısını otomatik olarak veya el ile artırın veya azaltın. 

* **Bağımlılıkları ve kapsayıcıları yönetme** - Varsayılan olarak, Batch AI kümeleri GPU veya CPU’lar üzerinde kapsayıcı tabanlı eğitim çerçevelerini çalıştırmak için önceden yüklenmiş bağımlılıklara sahip olan run Linux VM görüntülerini çalıştırır. Ek yapılandırma için, özel görüntüler kullanabilir veya başlangıç betikleri çalıştırabilirsiniz.

* **Verileri dağıtma** - Giriş verileri ve betikleri ile iş çıkışını yönetmek için şu depolama seçeneklerinden birini belirleyin: Azure Files, Azure Blob depolama veya yönetilen bir NFS sunucusu. Batch AI yüksek performanslı paralel dosya sistemleri dahil özel depolama çözümlerini de destekler. Basit yapılandırma dosyalarını kullanarak depolama dosya sistemlerini küme düğümleri ve iş kapsayıcılarına bağlayın.

* **İşleri zamanlama** - Küme kaynaklarını paylaşmak ve düşük öncelikli sanal makinelerden ve ayrılmış örneklerden yararlanmak için işleri öncelik tabanlı bir iş kuyruğuna gönderin.

* **Hataları işleme** - İş durumunu izleyin ve potansiyel olarak uzun süre çalışan işlerde VM hataları oluşması durumunda işleri yeniden başlatın.

* **Sonuçları toplama** - Çıkış günlükleri, Stdout, Stderr ve eğitilen modellere kolayca erişin. Batch AI işlerinizi doğrudan bağlı depolama alanına çıkış göndermek için yapılandırın.

Bir Azure hizmeti olarak, Batch AI güvenlik için rol tabanlı erişim denetimi (RBAC) ve Azure sanal ağları gibi genel araçları destekler.  

## <a name="next-steps"></a>Sonraki adımlar

* Bir makine öğrenimi veya yapay zeka modelini eğitmek için [Batch AI kaynakları](resource-concepts.md) hakkında daha fazla bilgi edinin.

* Batch AI ile [örnek bir derin öğrenme modeli eğitmeye](quickstart-tensorflow-training-cli.md) başlayın.

* Popüler AI çerçeveleri için örnek [eğitim tariflerine](https://github.com/Azure/BatchAI/blob/master/recipes) göz atın.
