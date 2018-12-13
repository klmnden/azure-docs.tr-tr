---
title: Azure DevTest Labs'i kullanmaya başlama
description: Bu makale, kuruluşunuzda Hizmeti'ni kullanmaya başlamak için Azure DevTest Labs ve iki genel yolu kullanmak için birincil scnearios sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: 87baef8ddb5b5d8fc979ba5afb9f9b13cb4fc2ef
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876545"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Azure DevTest Labs'i kullanmaya başlama
DevTest Labs'i keşfedin almaya karar verdiğinizde ilet – iki genel yol vardır kavram kanıtı vs ölçeklendirilmiş dağıtım. 

A **dağıtım ölçeği** gözden geçirme ve yüzlerce veya binlerce geliştiricinin sahip tüm kuruluş için DevTest Labs dağıtma amacına sahip planlama hafta/ay oluşur. 

A **kavram kanıtı dağıtımı** kuruluş değeri oluşturmak için bir ucuna çaba yerine tek bir takımın odaklanır. Ölçeklendirilmiş dağıtımını düşünme daha cazip olabilir, ancak bir yaklaşım seçeneği kavram kanıtı çok sık başarısız eğilimindedir. Bu nedenle, küçük başlayın, ilk ekibinden öğrenin, iki ila üç ek takımlar aynı yaklaşımı yineleyin ve elde edilen bilgilere dayanan bir ölçeklendirilmiş dağıtım planlama, öneririz. Başarılı bir kavram kanıtı için bir veya iki takımlar seçin ve (geliştirme ortamı vs test ortamları) kendi senaryoları tanımlama, kendi geçerli kullanım örnekleri belge ve DevTest Labs dağıtmak, öneririz. 

## <a name="scenarios"></a>Senaryolar
DevTest Labs uygulaması için üç ana senaryo vardır: 

- Geliştirici Masaüstleri
- Test ortamları
- Labs/eğitim/HACK maratonunda

## <a name="developer-desktops"></a>Geliştirici Masaüstleri
Geliştiriciler genellikle farklı projeler için geliştirme makineler için farklı gereksinimlere sahiptir. DevTest Labs ile geliştiriciler, en sık karşılaşılan senaryolardan uyacak şekilde önceden yapılandırılmış olan isteğe bağlı vm'lere erişebilir. DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, ortak bir geliştirme ve test ortamlarını takımlar arasında tutarlılık sağlamaya sağlayabilir.
- Geliştiriciler, geliştirme makinelerinde isteğe bağlı hızlı bir şekilde sağlayabilirsiniz
- Geliştiriciler kaynakları Self Servis bir yolla abonelik düzeyi izinleri gerek kalmadan sağlayabilirsiniz
- BT yöneticileri, ağ topolojisi önceden tanımlayabilir ve geliştiriciler doğrudan yararlanabilir, basit ve sezgisel bir şekilde özel erişime gerek kalmadan
- Gerektiği şekilde geliştiricilerin kolay geliştirme makinelerinde özelleştirebilirsiniz
- Yöneticiler, sağlayarak maliyetleri denetleyebilirsiniz:
    - Geliştiriciler, geliştirme için ihtiyaç duydukları çok daha fazla VM alınamıyor
    - Kullanımda olduğunda VM'ler kapatılır
    - Yalnızca belirli iş yükleri için sanal makine örneği boyutlarını kümesini izin verme

## <a name="test-environments"></a>Test ortamları
Oluşturma ve test ortamları yönetme kuruluş çapında önemli miktarda çaba gerektirebilir. DevTest Labs ile test ortamları oluşturulabilir kolayca, güncelleştirilmiş veya yinelenen, ekiplerin tam olarak yapılandırılmış bir ortamı, gerektiğinde erişim sağlar. Bu senaryoda, DevTest Labs, aşağıdaki avantajları sağlar:

- Test ediciler, hızlı bir şekilde yeniden kullanılabilir şablonlar ve yapıtlar aracılığıyla Windows ve Linux ortamları sağlayarak kendi uygulama en son sürümünü test edebilirsiniz.
- Test ediciler, yük testi birden çok test aracısına sağlayarak ölçeklendirebilirsiniz
- Yöneticiler, DevOps senaryolarını etkinleştirmek için Azure DevOps Laboratuvar bağlanabilir
- Yöneticiler, sağlayarak maliyetleri denetleyebilirsiniz:
    - Test edenler istedikleri çok daha fazla VM alınamıyor
    - Kullanımda olduğunda VM'ler kapatılır
    - Yalnızca belirli iş yükleri için sanal makine örneği boyutlarını kümesini izin verme

## <a name="labs-workshops-trainings-and-hackathons"></a>Laboratuvarlar, atölyeler, eğitimleri ve sonları gerçekleştirilen HACK  
Azure DevTest labs'deki bir laboratuvara atölyeler, uygulamalı laboratuvarlar, eğitim veya sonları gerçekleştirilen HACK gibi geçici etkinlikler için harika bir kapsayıcı görevi görür. Hizmet, burada her Yardımcısı Eğitim özdeş ve yalıtılmış ortamlar oluşturmak için kullanabileceğiniz özel şablonları sağlayabilir Laboratuvar oluşturmanıza olanak sağlar. Yalnızca bunlar ihtiyacınız ve eğitim için gereken sanal makineler gibi-yeterli kaynak - içeren eğitim ortamları her Yardımcısı için kullanılabilir olmasını sağlamak için ilkeler uygulayabilirsiniz. Son olarak, Laboratuvar, tek bir tıklamayla erişebilirsiniz yardımıyla ile kolayca paylaşabilirsiniz. Sınıf sonlandırdığını sonra Laboratuvar ve tüm ilgili kaynakları silmek kolay bir işlemdir.


## <a name="next-steps"></a>Sonraki adımlar
Bu serideki sonraki makaleye bakın: [DevTest Labs dağıtımınızın ölçeğini artırmak](devtest-lab-guidance-scale.md)
