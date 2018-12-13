---
title: Yönetme, kaydetme, dağıtma ve ML modelleri izleyin
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetini dağıtmak, yönetmek ve sürekli olarak geliştirmek için Modellerinizi izleme için kullanmayı öğrenin. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 25f149ad4df43a7e5b443d6abd72be91072cb47f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250221"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, Azure Machine Learning hizmeti dağıtma, yönetme ve sürekli olarak geliştirmek için Modellerinizi izlemek için nasıl kullanılacağını öğrenebilirsiniz. Azure Machine Learning ile yerel makinenizde veya diğer kaynaklardan eğitilmiş modeller dağıtabilirsiniz. 

Tam dağıtım iş akışı aşağıdaki diyagramda gösterilmektedir: [ ![Azure Machine Learning için dağıtım iş akışı](media/concept-model-management-and-deployment/deployment-pipeline.png) ](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Dağıtım iş akışı, aşağıdaki adımları içerir:
1. **Modeli kaydetmeyi** , Azure Machine Learning hizmeti çalışma alanında barındırılan bir kayıt defterinde
1. **Bir görüntüyü kaydedin** , bir model Puanlama betiğine ve taşınabilir bir kapsayıcıda bağımlılıkları ile eşleşmesini 
1. **Dağıtma** bulutta veya uç cihazlarında bir web hizmeti olarak görüntüsü
1. **İzleme ve veri toplama**

Her adım, bağımsız olarak veya tek dağıtım komutun bir parçası olarak gerçekleştirilebilir. Ayrıca, dağıtımı ile tümleştirebilirsiniz bir **CI/CD iş akışı** Bu grafikte gösterildiği gibi.

[ !['Azure Machine Learning sürekli tümleştirme/sürekli dağıtım (CI/CD) döngüsü'](media/concept-model-management-and-deployment/model-ci-cd.png) ](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)


## <a name="step-1-register-model"></a>1. Adım: Modeli kaydetme

Model kayıt defteri, Azure Machine Learning hizmeti çalışma alanınızdaki tüm modelleri izler.
Modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz.

Görüntü tarafından kullanılmakta olan modeller nelze odstranit.

## <a name="step-2-register-image"></a>2. Adım: Görüntüyü kaydedin

Model kullanmak için gereken tüm bileşenler birlikte güvenilir model dağıtımı için görüntüler sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Model
* Puanlama altyapısı
* Puanlama dosyası veya uygulama
* Modeli Puanlama için gereken tüm bağımlılıkları

Görüntü ayrıca günlüğe kaydetme ve izleme için SDK bileşenleri içerir. SDK'sı günlükleri verilerini ince ayar yapma veya giriş ve çıkış modeli de dahil olmak üzere modelinizi yeniden eğitme için kullanılabilir.

Azure Machine Learning en popüler çerçeveleri destekler, ancak genel pıp'in yüklü olan herhangi bir çerçeveyi çalışabilir.

Bu nedenle çalışma alanınızı oluştururken diğer birçok diğer Azure kaynakları bu çalışma alanı tarafından kullanıldı.
Görüntüyü oluşturmak için kullanılan tüm nesneler, çalışma alanınızda Azure depolama hesabında depolanır. Görüntü oluşturulur ve Azure Container Registry'de depolanır. Ayrıca görüntü kayıt depolanır ve görüntünüzü bulmak için sorgulanabilir görüntü oluşturulurken, ek meta veri etiketleri sağlayabilirsiniz.

## <a name="step-3-deploy-image"></a>3. adım: Görüntüsü dağıtma

Buluta veya uç cihazlarında kayıtlı görüntülerini dağıtabilirsiniz. Tüm izlemek için gerekli kaynaklar, Yük Dengeleme ve otomatik ölçeklendirme, dağıtım işlemi modeliniz oluşturur. Dağıtılan hizmetlere erişmek için sertifika tabanlı kimlik doğrulaması ile dağıtım sırasında güvenlik varlıklar sağlayarak güvenliği sağlanabilir. Ayrıca, daha yeni bir görüntüyü kullanmak için mevcut bir dağıtımı yükseltebilirsiniz.

Web hizmeti dağıtımları da aranabilir. Örneğin, belirli bir model veya görüntü tüm dağıtımları için arama yapabilirsiniz.

[ ![Çıkarım hedefleri](media/concept-model-management-and-deployment/inferencing-targets.png) ](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

Aşağıdakiler için kendi görüntülerinizi dağıtabilirsiniz [dağıtım hedefleri](how-to-deploy-and-where.md) bulutta:

* Azure Container Örneği
* Azure Kubernetes Service
* Azure FPGA makineler
* Azure IOT Edge cihazları

Hizmetinizi dağıtılan gibi çıkarım isteği otomatik olarak yük dengeli ve isteğe bağlı herhangi bir ani artış karşılamak için küme Ölçeklendirildi. [Hizmetinizi hakkında telemetri tutulabilir](how-to-enable-app-insights.md) çalışma alanınızla ilişkili Azure Application Insights hizmetine.

## <a name="step-4-monitor-models-and-collect-data"></a>4. adım: İzleyici modeller ve veri toplama

Giriş, çıkış ve diğer ilgili veri modelinizden izleyebilmek model günlük ve veri yakalama için bir SDK'sı kullanılabilir. Veriler, çalışma alanınız için Azure depolama hesabındaki bir blob olarak depolanır.

Modelinizi SDK'yı kullanmak için SDK, Puanlama betik veya uygulamanızı almanız gerekir. Ardından SDK parametre sonuçları ya da giriş ayrıntıları gibi verileri günlüğe kaydetmek için de kullanabilirsiniz.

Kendiniz karar verirseniz [model verisi toplamayı etkinleştir](how-to-enable-data-collection.md) kimlik bilgileri, kişisel blob mağazası gibi verilerini yakalamak için gerekli ayrıntıları dağıttığınız görüntünün her zaman otomatik olarak sağlanır.

> [!Important]
> Microsoft modelinizden topladığı verileri görmez. Çalışma alanınız için Azure depolama hesabına doğrudan bir veri gönderilmedi.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile.
