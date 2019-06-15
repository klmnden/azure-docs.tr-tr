---
title: Çalışma alanı nedir
titleSuffix: Azure Machine Learning service
description: Çalışma alanı, Azure Machine Learning hizmeti için en üst düzey kaynaktır. Bu günlükler, ölçümler, çıkış ve komut dosyalarınızın anlık görüntüsünü de dahil olmak üzere tüm eğitim çalıştırmalarının geçmişini tutar. Hangi eğitim çalıştırmanın en iyi modeli belirlemek için bu bilgileri kullanın
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 2f3d9eeca1404fcae121ae5fead222cbde4037b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059252"
---
# <a name="what-is-an-azure-machine-learning-service-workspace"></a>Bir Azure Machine Learning hizmeti çalışma alanı nedir?

Çalışma alanı, Azure Machine Learning hizmeti, Azure Machine Learning hizmeti kullanırken oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerde sağlamak için en üst düzey kaynaktır.  Çalışma alanı, günlükler, ölçümler, çıkış ve komut dosyalarınızın anlık görüntüsünü de dahil olmak üzere tüm eğitim çalıştırmalarının geçmişini tutar. Hangi eğitim çalıştırmanın en iyi modeli belirlemek için bu bilgileri kullanın.  

İstediğiniz bir modeli oluşturduktan sonra çalışma alanı ile kaydedin. Ardından Azure Container Instances, Azure Kubernetes hizmeti veya bir REST tabanlı bir HTTP uç noktası olarak bir alanda programlanabilir kapı dizileri (FPGA) dağıtmak için kayıtlı model ve puanlama betik'ni kullanın. Ayrıca, Azure IOT Edge cihazına bir modül olarak model dağıtabilirsiniz.

## <a name="taxonomy"></a>Sınıflandırma 

Çalışma alanının bir taksonomi, aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Aşağıdaki bileşenler bir çalışma alanının diyagramda gösterilmektedir:

+ Bir çalışma alanı içerebilir [not defteri Vm'leri](quickstart-run-cloud-notebook.md), bulut kaynaklarına Azure Machine Learning çalıştırmak için gereken Python ortamını ile yapılandırılmış.
+ [Kullanıcı rolleri](how-to-assign-roles.md) diğer kullanıcıları, ekipleri veya projeleri çalışma alanınızda paylaşın olanak sağlar.
+ [Hedef işlem](concept-azure-machine-learning-architecture.md#compute-targets) denemelerinizi çalıştırmak için kullanılır.
+ Çalışma alanı oluşturduğunuzda [ilişkili kaynakları](#resources) ayrıca sizin için oluşturulur.
+ [Denemeleri](concept-azure-machine-learning-architecture.md#experiments) eğitim çalıştığı modellerinizi oluşturmak için kullanın.  Oluşturma ve çalıştırma denemeleri
    + [Azure Machine için Python SDK'sı Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
    + [Makine öğrenimi denemeleri (Önizleme) otomatik](how-to-create-portal-experiments.md) bölümünde Azure portalında.
    + [Görsel arabirim (Önizleme)](ui-concept-visual-interface.md).
+ [İşlem hatları](concept-azure-machine-learning-architecture.md#ml-pipelines) modelinizi yeniden eğitme ve eğitim için yeniden kullanılabilir iş akışlarıdır.
+ [Veri kümeleri](concept-azure-machine-learning-architecture.md#datasets-and-datastores) modeli eğitimi ve işlem hattı oluşturmak için kullandığınız veri Yönetimi Yardımı.
+ Dağıtmak istediğiniz bir model aldıktan sonra kayıtlı bir modeli oluşturun.
+ Kayıtlı model ve puanlama betiğine oluşturulacağı bir [dağıtım](concept-azure-machine-learning-architecture.md#deployment).

## <a name="tools-for-workspace-interaction"></a>Çalışma alanı etkileşimi için Araçlar

Aşağıdaki yollarla çalışma alanınız ile etkileşim kurabilirsiniz:

+ Web üzerinde:
    + [Azure portalı](https://portal.azure.com)
    + [Görsel arabirim (Önizleme)](ui-concept-visual-interface.md)
+ Azure Machine Learning Python kullanarak [SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
+ Azure Machine Learning kullanarak komut satırında [CLI uzantısı](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)

## <a name="machine-learning-with-a-workspace"></a>Machine learning çalışma alanı ile

Makine öğrenimi görevlerini okuyun ve/veya yapıtları çalışma alanınıza yazma. 

+ Bir model - eğitmek için bir deneme çalıştırma yazma, çalıştırma sonuçları çalışma alanına denemeler yapın.
+ Kullanım otomatik ML model - eğitmek için çalışma alanına eğitim sonuçları yazar.
+ Bir modeli, çalışma alanına kaydedin.
+ Model dağıtma - bir dağıtım oluşturmak için kayıtlı modeli kullanır.
+ Oluşturun ve yeniden kullanılabilir iş akışlarını çalıştırın.
+ Görünüm makine öğrenme denemeleri, işlem hatları, modelleri, dağıtımları gibi yapılarını.
+ İzleme ve izleme modeller.

## <a name="workspace-management"></a>Çalışma alanı yönetimi

Ayrıca, aşağıdaki çalışma alanı yönetim görevlerini gerçekleştirebilirsiniz:

| Çalışma alanı yönetim görevi   | Portal              | SDK        | CLI        |
|---------------------------|------------------|------------|------------|
| Çalışma alanı oluşturma        | **&check;**     | **&check;** | **&check;** |
| İşlem kaynaklarını oluşturmak ve yönetmek    | **&check;**   | **&check;** |  **&check;**   |
| Çalışma alanı erişimi yönetme    | **&check;**   | |  **&check;**    |
| Not Defteri VM oluşturma | **&check;**   | |     |

Hizmet tarafından başlama [çalışma alanı oluşturma](setup-create-workspace.md).

## <a name="resources"></a> İlişkili kaynakları

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından kullanılan bazı Azure kaynakları otomatik olarak oluşturur:

+ [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/): Eğitim sırasında ve bir modeli dağıtırken kullandığınız docker kapsayıcıları kaydeder. ACR maliyetleri en aza indirmektir **yavaş yüklenen** dağıtım yansımaları oluşturulana kadar.
+ [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): Çalışma alanı için varsayılan veri deposu olarak kullanılır.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): İzleme, modelleri hakkında bilgi depolar.
+ [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/): Hedefleri ve gerekli olan diğer hassas bilgiler depolar gizli dizileri tarafından kullanılan işlem tarafından çalışma alanı.

> [!NOTE]
> Yeni sürümler oluşturmaya ek olarak, var olan Azure hizmetleri de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti ile çalışmaya başlamak için bkz:

+ [Azure Machine Learning hizmetine genel bakış](overview-what-is-azure-ml.md)
+ [Çalışma Alanı oluşturma](setup-create-workspace.md)
+ [Çalışma Alanını Yönetme](how-to-manage-workspace.md)
+ [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
