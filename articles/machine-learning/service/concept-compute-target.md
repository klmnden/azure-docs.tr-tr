---
title: 'Hedef işlem: nerede eğitme ve modelleri dağıtma'
titleSuffix: Azure Machine Learning service
description: Eğitmeniz veya dağıtmanız modelinizin Azure Machine Learning hizmeti ile istediğiniz tanımlayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/10/2019
ms.openlocfilehash: a7944b284a9c1c0424af54874554d05d49ad4b20
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806037"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning-service"></a>Azure Machine Learning hizmetinde işlem hedefleri nelerdir? 

A **hedef işlem** belirtilen işlem kaynak/çalıştırdığınız eğitim betiği veya ana bilgisayar hizmeti dağıtımınız ortamıdır. Bu konum, yerel makinenizde veya bir bulut tabanlı işlem kaynağı olabilir. Kullanarak işlem hedefleri, kodunuzu değiştirmek zorunda kalmadan, bilgi işlem ortamınızı daha sonra değiştirmek için kolaylaştırır.  

Tipik bir model geliştirme yaşam döngüsünde şunu yapabilirsiniz:
1. Az miktarda veriniz denemek ve geliştirme ile başlayın. Bu aşamada, yerel öneririz, işlem hedefi olarak ortam (yerel bilgisayar veya bulut tabanlı VM). 
2. Büyük veri için ölçeği büyütün veya bunlardan birini kullanarak eğitim dağıtılmış [eğitim işlem hedeflerini](#train).  
3. Modeliniz hazır olduktan sonra ortamı veya bunlardan birini IOT cihazıyla barındırma Web dağıtma [dağıtım işlem hedeflerini](#deploy).

İşlem hedeflerinizi için kullandığınız işlem kaynakları eklenmiş bir [çalışma](concept-workspace.md). İşlem kaynakları yerel makine dışındaki çalışma alanının kullanıcılar tarafından paylaşılır.

## <a name="train"></a> Eğitim işlem hedefleri

Azure Machine Learning hizmeti farklı işlem kaynakları genelinde değişen desteğe sahiptir.  Kendi işlem kaynağı olsa da ekleyebilirsiniz çeşitli senaryolarda değişebilir desteği.

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]

Daha fazla bilgi edinin [ayarlama ve model yönetimi için bir işlem hedefine kullanma](how-to-set-up-training-targets.md).

## <a name="deploy"></a>Dağıtım hedefleri

Aşağıdaki işlem kaynakları, model dağıtımı barındırmak için kullanılabilir.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

Bilgi [nerede ve nasıl bir işlem hedefine modelinizin dağıtılacağı](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning işlem (yönetilen)

Bir yönetilen işlem kaynağı oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Bu işlem, machine learning iş yükleri için optimize edilmiştir. Azure Machine Learning işlemi yalnızca yönetilen bir işlem 30 Mayıs 2019 tarihinde olur. Yönetilen ek işlem kaynakları gelecekte eklenebilir.

Azure Machine Learning işlem batch çıkarım (Önizleme) ve eğitim için kullanabilirsiniz.  Bu işlem kaynağı ile aşağıdakiler:

* Tek veya çok node küme
* İhtiyaçlara göre otomatik bir çalıştırma gönderdiğiniz her zaman 
* Otomatik küme yönetimi ve iş zamanlama 
* Hem CPU hem de GPU kaynakları için destek

Azure portal, CLI veya SDK'sı ile Azure Machine Learning işlem örnekleri oluşturabilirsiniz. Bunları oluşturduğunda otomatik olarak diğer tür işlem hedef aksine çalışma alanınızı bir parçası olur.

## <a name="unmanaged-compute"></a>Yönetilmeyen işlem

Yönetilmeyen işlem hedefi *değil* Azure Machine Learning hizmeti tarafından yönetilir. Bu tür bir Azure Machine Learning dışında işlem hedefi oluşturmak ve ardından çalışma alanınıza ekleyin. Yönetilmeyen işlem kaynaklarını korumak için veya makine öğrenimi iş yükleri için performansı artırmak için ek adımlar gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Modelinizi eğitmek için bir işlem hedefine ayarlayın](how-to-set-up-training-targets.md)
* [Bir işlem hedefine modelinizi dağıtma](how-to-deploy-and-where.md)