---
title: Hedef işlem
titleSuffix: Azure Machine Learning service
description: Bir işlem hedefine eğitim betiği veya ana bilgisayar hizmeti dağıtımınız çalıştırdığınız işlem kaynağı belirtmenize olanak sağlar. Bu konum, yerel makinenizde veya bir bulut tabanlı işlem kaynağı olabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755358"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde bir işlem hedefine nedir? 

Bir işlem hedefine eğitim betiği veya ana bilgisayar hizmeti dağıtımınız çalıştırdığınız işlem kaynağı belirtmenize olanak sağlar. Bu konum, yerel makinenizde veya bir bulut tabanlı işlem kaynağı olabilir.

İşlem hedefleri, bilgi işlem ortamınızı kodunuzu değiştirmeden değiştirmek kolaylaştırır.  Tipik bir model geliştirme yaşam döngüsü:

* Geliştirme/deneme az miktarda veriniz üzerinde çalışmaya başlayın. Bu aşamada, yerel bir ortamı kullanmanızı öneririz. Örneğin, yerel bilgisayarınıza veya bulut tabanlı bir VM.
* Büyük veri kümeleri üzerinde eğitim ölçeğini veya dağıtılmış birini kullanarak eğitim [eğitim hedefleri](#train).  
* Çeşitli web barındırma ortamları veya IOT cihazları kullanarak dağıtmak [dağıtım hedefleri](#deploy).

İşlem hedeflerinizi için kullandığınız işlem kaynakları eklenmiş bir [çalışma](concept-workspace.md). İşlem kaynakları yerel makine dışındaki çalışma alanının kullanıcılar tarafından paylaşılır.

## <a name="train"></a> Eğitim hedefleri

Azure Machine Learning hizmeti farklı işlem kaynakları genelinde değişen desteğe sahiptir.  Çeşitli senaryolarda olarak değişiklik gösterebilir destek aşağıda ayrıntılarıyla olsa da, kendi işlem kaynağı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Yönetilen işlem

Bir yönetilen işlem kaynağı oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Bu işlem, machine learning iş yükleri için optimize edilmiştir. Azure Machine Learning işlemi yalnızca yönetilen bir işlem 30 Mayıs 2019 tarihinde olur. Yönetilen ek işlem kaynakları gelecekte eklenebilir.

### <a name="amlcompute"></a> Azure Machine Learning işlem

Azure Machine Learning işlem batch çıkarım (Önizleme) ve eğitim için kullanabilirsiniz.  Bu işlem kaynağı ile aşağıdakiler:

* Tek veya çok node küme
* İhtiyaçlara göre otomatik bir çalıştırma gönderdiğiniz her zaman 
* Otomatik küme yönetimi ve iş zamanlama 
* Hem CPU hem de GPU kaynakları için destek

Azure Machine Learning işlem örnekleri aşağıdakilerden birini oluşturabilirsiniz:

* Azure portal
* Azure Machine Learning SDK'sı
* The Azure CLI

Diğer tüm bilgi işlem kaynakları çalışma alanı dışında oluşturulan ve kendisine bağlı.

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen işlem kaynağı *değil* Azure Machine Learning hizmeti tarafından yönetilir. Bu tür bir Azure Machine Learning dışında bir işlem oluşturun, ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynaklarını korumak için veya makine öğrenimi iş yükleri için performansı artırmak için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [İşlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md)
* [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md)