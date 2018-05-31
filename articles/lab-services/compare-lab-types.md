---
title: Azure Lab Services’te farklı laboratuvar türlerini karşılaştırma | Microsoft Docs
description: Azure Lab Services (önceki adıyla DevTest Labs) kullanarak oluşturabileceğiniz farklı laboratuvar türlerini açıklar ve karşılaştırır.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 22a1c90dd1a1ca305431d91a801e5293a6d08703
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361191"
---
# <a name="compare-managed-and-devtest-labs-in-azure-lab-services"></a>Azure Lab Services’te yönetilen ve DevTest laboratuvarları karşılaştırma
İki tür laboratuvar oluşturabilirsiniz: Azure Lab Services ile **yönetilen laboratuvarlar** ve Azure DevTest Labs ile **özel laboratuvarlar** . Yalnızca bir laboratuvardaki gereksinimlerinizi eklemek ve laboratuvar için gerekli altyapıyı ayarlayıp yönetmeyi hizmete bırakmak istiyorsanız, **yönetilen laboratuvarlardan** birini seçin. Şu anda **sınıf laboratuvarı**, Azure Lab Services ile oluşturabileceğiniz tek yönetilen laboratuvar türüdür. Kendi altyapınızı yönetmek istiyorsanız, Azure DevTest Labs kullanarak bir **özel laboratuvar** oluşturun.

Aşağıdaki bölümlerde bu laboratuvarlar hakkında daha ayrıntılı bilgi verilmektedir. 

## <a name="managed-labs"></a>Yönetilen laboratuvarlar
Yönetilen laboratuvarlar, özel gereksiniminize uygun olan farklı türde laboratuvarlar sunar. Şu anda Azure Lab Services yönetilen laboratuvar olarak yalnızca **sınıf laboratuvarını** destekler. Yönetilen laboratuvarlar çok az kurulumla hemen çalışmaya başlamanızı sağlar. Hizmet, VM’leri tasarlamaktan hataları işlemeye ve altyapıyı ölçeklendirmeye varan tüm laboratuvar altyapısı yönetimi konularını ele alır. Yönetilen bir laboratuvar oluşturmak için ilk olarak kuruluşunuza ait bir laboratuvar hesabı oluşturmanız gerekir. Laboratuvar hesabı, kuruluştaki tüm laboratuvarların yönetildiği merkezi hesap olarak görev yapar. 

Bu yönetilen laboratuvarlarda Azure kaynakları oluşturup kullandığınızda hizmet, dahili Microsoft aboneliklerinde kaynaklar oluşturup yönetir. Bunlar sizin Azure aboneliğinizde oluşturulmaz. Hizmet bu kaynakların dahili Microsoft aboneliklerindeki kullanımını takip eder. Bu kullanım, laboratuvar hesabını içeren Azure aboneliğinize faturalanır.   

**Yönetilen laboratuvarlara ilişkin kullanım örneklerinden** bazıları aşağıda verilmiştir: 

- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, ev ödevi veya kişisel projeleri için VM’leri kullanabilecekleri sınırlı sayıda saat verin.
- İşlem yoğunluklu veya grafik yoğunluklu araştırmalar gerçekleştirmek üzere yüksek performanslı işlem VM’leri içeren bir havuz oluşturun. Gerektiğinde VM’leri çalıştırın ve işiniz bittikten sonra makineleri temizleyin. 
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.  
- Bir hackathon barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 


## <a name="devtest-labs"></a>DevTest laboratuvarları
Tüm altyapıyı ve yapılandırmayı kendi başınıza, kendi aboneliğiniz içinde yönetmek istediğiniz senaryolar olabilir. Bunu yapmak için, Azure portalında Azure DevTest Labs ile özel bir laboratuvar oluşturabilirsiniz. Bu laboratuvarlar için bir laboratuvar hesabı oluşturmanız gerekmez. Bu laboratuvarlar, laboratuvar hesabında (yönetilen laboratuvarlar için mevcuttur) gösterilmez.  

**DevTest laboratuvarları kullanmaya ilişkin kullanım örneklerinden** bazıları aşağıda verilmiştir: 

- Bir hackathon veya bir konferansta uygulamalı oturum barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 
- Uygulamanızla yapılandırılan bir VM havuzu oluşturun ve takımınızın yoğun hata ayıklama için bir sanal makineyi kolayca kullanmasını sağlayın.  
- Geliştiricilere, ihtiyaç duydukları tüm araçlarla yapılandırılmış sanal makineler sağlayın. Maliyeti en aza indirmek için otomatik başlatma ve kapatma zamanlayın. 
- Dağıtımınızın bir parçası olarak tekrarlanan şekilde bir test makineleri laboratuvarı oluşturun. En son bitleri test edin ve işiniz bittiğinde test makinelerini temizleyin. 
- Farklı şekilde yapılandırılmış çeşitli sanal makineler ve ölçeklendirme ile performans testi için birden fazla test aracısı ayarlayın. 
- Ürününüzün en son sürümü ile yapılandırılmış bir laboratuvar kullanarak müşterilerinize eğitim oturumları sunun. Her müşteriye laboratuvarda kullanmak üzere sınırlı sayıda saat verin. 


## <a name="managed-labs-vs-devtest-labs"></a>Yönetilen laboratuvarlar ve DevTest laboratuvarları
Aşağıdaki tabloda Azure Lab Services tarafından desteklenen iki laboratuvar türü karşılaştırılmaktadır: 

| Özellikler | Yönetilen laboratuvarlar | DevTest laboratuvarları |
| -------- | ----------------  | ---------- |
| Laboratuvarda Azure altyapısı yönetimi. |  Hizmet tarafından otomatik olarak yönetilir | Kendi başınıza yönetirsiniz  |
| Altyapı sorunlarında yerleşik esneklik | Hizmet tarafından otomatik olarak gerçekleştirilir | Kendi başınıza yönetirsiniz  |
| Abonelik yönetimi | Hizmet, hizmeti destekleyen Microsoft abonelikleri içinde kaynak ayırmayı gerçekleştirir. Ölçeklendirme hizmet tarafından otomatik olarak gerçekleştirilir. | Kendi Azure aboneliğinizde kendi başınıza yönetirsiniz. Abonelikler otomatik ölçeklendirilmez. |
| Laboratuvar içinde Azure Resource Manager dağıtımı | Kullanılamaz | Kullanılabilir |

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)
- [Özel bir laboratuvarı ayarlama](tutorial-create-custom-lab.md)
