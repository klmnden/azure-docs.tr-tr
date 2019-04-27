---
title: Azure DevTest Labs'de bir görüntü fabrikası oluşturma | Microsoft Docs
description: Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma hakkında bilgi edinin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: cf1bb31614c04d6073bc40c510fc43b2f8e4e189
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622655"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma
Bu makalede bulunan örnek betikler kullanarak bir özel görüntü Fabrika ayarlanacağı gösterilmektedir [Git deposu](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory).

## <a name="whats-an-image-factory"></a>Bir görüntü fabrikası nedir?
Bir görüntü factory oluşturan ve dağıtan tüm istenen yapılandırmaları ile düzenli aralıklarla otomatik olarak görüntülerinde yapılandırma olarak kodu bir çözümdür. Görüntü Fabrika görüntüleri her zaman güncel olduğundan ve bu işlem otomatik olarak devam eden bakım neredeyse sıfır olarak ayarlanır. Ve tüm gerekli yapılandırmaları görüntüde zaten olduğu için temel işletim sistemi ile bir VM oluşturulduktan sonra el ile sistem yapılandırmasını zaman kazandırır.

DevTest Labs hazır durumda bir geliştirici Masaüstü almak için önemli Hızlandırıcı, özel görüntüleri kullanıyor. Özel görüntüleri dezavantajı, laboratuvarda korumak için ek bir sorun olmadığını ' dir. Örneğin, zamanla ürünleri deneme sürümleri sona (veya) yeni kullanıma sunulan güvenlik güncelleştirmeleri, düzenli aralıklarla özel görüntüyü yenilemek için bize zorla uygulanmaz. Görüntü factory ile otomatik bir işlem tanımına dayalı olarak özel görüntüleri oluşturmak için kaynak kodu denetimi ve iade görüntü tanımı sahip.

Çözüm ek devam eden bakım maliyetlerini sorununu ortadan kaldırırken özel görüntülerden sanal makineler oluşturma hızı sağlar. Bu çözüm ile otomatik olarak özel görüntüleri oluşturabilir, bunları dağıtmak için diğer DevTest Labs ve eski görüntülerin devre dışı bırakma. Aşağıdaki videoda, görüntü Fabrika ve DevTest Labs'i kullanmaya nasıl uygulandığı hakkında bilgi edinin.  Ücretsiz olarak kullanılabilir ve bulunan tüm Azure Powershell betikleri aşağıda verilmiştir: [ https://aka.ms/dtlimagefactory ](https://aka.ms/dtlimagefactory).

<br/>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Custom-Image-Factory-with-Azure-DevTest-Labs/player]


## <a name="high-level-view-of-the-solution"></a>Çözüme üst düzey görünümü
Çözüm ek devam eden bakım maliyetlerini sorununu ortadan kaldırırken özel görüntülerden sanal makineler oluşturma hızı sağlar. Bu çözüm ile otomatik olarak özel görüntüleri oluşturma ve diğer DevTest Labs kullanarak dağıtın. Azure DevOps (eski adıyla Visual Studio Team Services), tüm işlemler DevTest labs'deki otomatikleştirmek için düzenleme altyapısı olarak kullanın.

![Çözüme üst düzey görünümü](./media/create-image-factory/high-level-view-of-solution.png)

Var olan bir [VSTS uzantısı için DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) tek tek adımları yürütmesine olanak sağlar:

- Özel görüntü oluşturma
- VM oluşturma
- VM silme
- Ortam oluşturma
- Ortamı Sil
- Ortam Doldur

DevTest Labs uzantısını kullanarak, özel görüntüleri DevTest Labs'de otomatik olarak oluşturmaya başlamak için kolay bir yoludur.

PowerShell betiğini kullanarak daha karmaşık bir senaryo için alternatif bir uygulama yok. PowerShell kullanarak DevTest Labs, sürekli tümleştirme ve sürekli teslim (CI/CD) araç zinciri kullanılan temel bir görüntü fabrikası tamamen otomatikleştirebilirsiniz. Bu alternatif bir çözüm izleyen ilkeler şunlardır:

- Yaygın güncelleştirmelerin görüntü Fabrika herhangi bir değişiklik istemeniz gerekir. (yeni bir otomatik olarak eski görüntüler, devre dışı bırakma, özel bir görüntü türü ekleme örnek için yeni bir 'uç noktası' DevTest Labs, özel görüntüleri alabilir ve benzeri ekleniyor.)
- Ortak değişiklikleri kaynak kodu denetimi (kod olarak altyapı) tarafından desteklenir
- DevTest Labs özel görüntüleri alma (laboratuvarlar abonelikleri span) aynı Azure aboneliğinde olmayabilir
- Biz gerektiği gibi ek oluşturucuları çalıştırabileceğinizi için PowerShell betikleri yeniden kullanılabilir olmalıdır

## <a name="next-steps"></a>Sonraki adımlar
Bu bölümün sonraki makaleye geçin: [Azure DevOps bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md)
