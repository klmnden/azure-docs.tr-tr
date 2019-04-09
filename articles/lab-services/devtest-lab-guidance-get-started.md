---
title: Azure DevTest Labs'i kullanarak popüler senaryolar
description: Bu makale, kuruluşunuzda Hizmeti'ni kullanmaya başlamak için Azure DevTest Labs ve iki genel yolu kullanmak için birincil senaryolar sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 8736ba4c24ac4c8f8d84345028d1cadfdef38697
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59272400"
---
# <a name="popular-scenarios-for-using-azure-devtest-labs"></a>Azure DevTest Labs'i kullanarak popüler senaryolar
DevTest Labs kuruluş gereksinimlerine bağlı olarak, farklı gereksinimlerini karşılayacak şekilde yapılandırılabilir.  Bu makalede, yaygın senaryolar açıklanmaktadır. Her senaryo, bu senaryoları uygulamak için kullanılacak DevTest Labs ve kaynakları kullanarak duruma avantajları kapsar.  

- Geliştirici Masaüstleri
- Test ortamları
- Eğitim oturumları, uygulamalı laboratuvarlar ve sonları gerçekleştirilen HACK
- Korumalı araştırmalar
- Sınıf laboratuvarları

## <a name="developer-desktops"></a>Geliştirici Masaüstleri
Geliştiriciler genellikle farklı projeler için geliştirme makineler için farklı gereksinimlere sahiptir. DevTest Labs ile geliştiriciler, en yaygın senaryolara uygun şekilde yapılandırılmış bir isteğe bağlı sanal makinelere erişebilir. DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, takımlar arasında tutarlılık sağlamaya genel geliştirme makineler sağlayabilir.
- Geliştiricilerin hızlı bir şekilde geliştirme makinelerinde isteğe bağlı sağlayın veya [var olan önceden yapılandırılmış bir makine talep](devtest-lab-add-claimable-vm.md).
- Geliştiriciler, abonelik düzeyinde izinleri gerek kalmadan kaynakları Self Servis bir şekilde sağlayabilirsiniz.
- BT veya yöneticileri [ağ topolojisi önceden tanımlı](devtest-lab-configure-vnet.md) ve geliştiricilerin doğrudan kullanabilir, basit ve sezgisel bir şekilde özel erişime gerek kalmadan.
- Geliştiriciler için kolayca [özelleştirme](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) geliştirme makineleri gerektiğinde.
- Yöneticiler, sağlayarak maliyetleri denetleyebilirsiniz:
    - Geliştiriciler [daha fazla VM alınamıyor](devtest-lab-set-lab-policy.md#set-virtual-machines-per-user) geliştirme için ihtiyaç duydukları daha
    - [Vm'leri kapatmak](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanılmadığı zaman
    - Yalnızca [sanal makine örneği boyutlarını kümesini sağlayan](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) belirli Laboratuvarları için
    - [Maliyet hedefleri ve bildirimleri yönetme](devtest-lab-configure-cost-management.md) her Laboratuvar için.

Daha fazla bilgi için bkz: [kullanımı Azure DevTest Labs geliştiriciler için](devtest-lab-developer-lab.md). 

## <a name="test-environments"></a>Test ortamları
Oluşturma ve test ortamları yönetme kuruluş çapında önemli miktarda çaba gerektirebilir. DevTest Labs ile test ortamlarını kolayca oluşturulan, güncelleştirilmiş, yinelenen veya. Bu, ekiplerin gerektiğinde tam olarak yapılandırılmış bir ortam erişim sağlar. Bu senaryoda, DevTest Labs, aşağıdaki avantajları sağlar:

- Kuruluşlar, takımlar arasında tutarlılık sağlamaya genel test ortamları sağlayabilirsiniz.
- Test ediciler, hızlı bir şekilde yeniden kullanılabilir şablonları kullanarak Windows ve Linux ortamları sağlayarak kendi uygulama en son sürümünü test edebilirsiniz.
- Yöneticiler, DevOps senaryolarını etkinleştirmek için Azure DevOps lab bağlanabilir
- Laboratuvar sahibi, sağlayarak maliyetleri denetleyebilirsiniz:
    - [Ortamlarda Vm'leri kapatmak](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanılmadığı zaman
    - Yalnızca [bir alt kümesi için sanal makine örneği boyutlarını izin vererek](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) belirli Laboratuvarları
    - [Maliyet hedefleri ve bildirimleri yönetme](devtest-lab-configure-cost-management.md) her Laboratuvar için.

Daha fazla bilgi için [kullanımı Azure DevTest Labs sanal makine ve PaaS için test ortamlarını](devtest-lab-test-env.md).

## <a name="sandboxed-investigations"></a>Korumalı araştırmalar
Geliştiriciler genellikle farklı teknolojiler veya altyapı tasarımı araştırın. DevTest Labs ile oluşturulan tüm ortamları, varsayılan olarak, kendi kaynak grubunda oluşturulur. DevTest Labs kullanıcısı bu kaynaklara yalnızca okuma erişimi alır. Ancak, daha fazla denetime ihtiyaç duyan geliştiriciler için laboratuvar genelinde bir ayar vermek için güncelleştirilebilir [katkıda bulunan haklarına](https://azure.microsoft.com/updates/azure-devtest-labs-view-and-set-access-rights-to-an-environment-rg/) oluşturdukları her ortam için kaynak DevTest Labs kullanıcı.  DevTest Labs ile geliştiriciler, otomatik olarak oluşturdukları laboratuvar ortamları katkıda bulunan izni verilebilir.  Bu senaryo, geliştiricilerin ekleyin ve/veya kendi geliştirme ve test ortamları için ihtiyaç duydukları gibi Azure kaynaklarını değiştirme olanak tanır. [Kaynağı ile maliyet](devtest-lab-configure-cost-management.md#view-cost-by-resource) sayfası maliyet verileri araştırmak için kullanılan her bir ortamın izlemek Laboratuvar sahibini sağlar.

Daha fazla bilgi için [ortam bir kaynak grubuna erişim hakları ayarlama](https://aka.ms/dtl-sandbox).

## <a name="trainings-hands-on-labs-and-hackathons"></a>Eğitimleri, uygulamalı laboratuvarlar ve sonları gerçekleştirilen HACK 
Azure DevTest labs'deki bir laboratuvara atölyeler, eğitimleri, uygulamalı laboratuvarlar, ya da sonları gerçekleştirilen HACK geçici etkinlikler için harika bir kapsayıcı görevi görür.  Hizmet, burada her Yardımcısı Eğitim özdeş ve yalıtılmış ortamlar oluşturmak için kullanabileceğiniz özel şablonları sağlayabilir Laboratuvar oluşturmanıza olanak sağlar. Bu senaryoda, DevTest Labs, aşağıdaki avantajları sağlar:

- [İlkeleri](devtest-lab-set-lab-policy.md) yardımıyla, yalnızca ihtiyaç duydukları sanal makineler gibi kaynakların sayısını get emin olun.
- Önceden yapılandırılmış ve oluşturulan makinelerdir [talep](devtest-lab-add-claimable-vm.md) tek eylem Yardımcısı ile.
- Labs erişerek yardımıyla ile paylaşılır [Laboratuvar için URL](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab).
- [Sona erme tarihleri](devtest-lab-add-vm.md#steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs) artık gerekmeyen sonra makineleri silinir sanal makinelerde emin olun.
- Kolay [Laboratuvar Sil](devtest-lab-delete-lab-vm.md#delete-a-lab) ve tüm [ilgili kaynakları](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) eğitim olduğunda üzerinden.

Daha fazla bilgi için [kullanımı Azure DevTest Labs eğitim](devtest-lab-training-lab.md).  

## <a name="proof-of-concept-vs-scaled-deployment"></a>Kavram kanıtı ölçeklendirilmiş dağıtım karşılaştırması
DevTest Labs'i keşfedin almaya karar verdiğinizde, İleri iki genel yolu vardır: Kavram kanıtı kavramı vs ölçeklendirilmiş dağıtım.  

A **dağıtım ölçeği** gözden geçirme ve yüzlerce veya binlerce geliştiricinin sahip tüm kuruluş için DevTest Labs dağıtma amacına sahip planlama hafta/ay oluşur.

A **kavram kanıtı** dağıtım kuruluş değeri oluşturmak için bir ucuna çaba yerine tek bir takımın odaklanır. Ölçeklendirilmiş dağıtımını düşünme daha cazip olabilir, ancak bir yaklaşım seçeneği kavram kanıtı çok sık başarısız eğilimindedir. Bu nedenle, küçük başlayın, ilk ekibinden öğrenin, iki ila üç ek takımlar aynı yaklaşımı yineleyin ve elde edilen bilgilere dayanan bir ölçeklendirilmiş dağıtım planlama, öneririz. Başarılı bir kavram kanıtı için bir veya iki takımlar seçin ve (geliştirme ortamı vs test ortamları) kendi senaryoları tanımlama, kendi geçerli kullanım örnekleri belge ve DevTest Labs dağıtmak, öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [DevTest Labs kavramları](devtest-lab-concepts.md)
- [DevTest Labs SSS](devtest-lab-faq.md)

