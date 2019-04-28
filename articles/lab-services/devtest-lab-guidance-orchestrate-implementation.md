---
title: Azure DevTest Labs uygulaması düzenleyin
description: Bu makalede işlemlerini uygulaması için Azure DevTest Labs, kuruluşunuzdaki yönergeleri sağlanır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: e0ac09a68bda539fe7abd05fce1739d1a58a3c99
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127353"
---
# <a name="orchestrate-the-implementation-of-azure-devtest-labs"></a>Azure DevTest Labs uygulamasını düzenleyin
Bu makalede, hızlı dağıtım ve Azure DevTest Labs uygulaması için önerilen bir yaklaşım sağlar. Aşağıdaki görüntüde, genel işlem sırasında çeşitli sektör gereklilikleri ve senaryoları desteklemek için esneklik gözleme normatif bir Rehber olarak vurgular.

![Azure DevTest Labs uygulamaya yönelik adımlar](./media/devtest-lab-guidance-orchestrate-implementation/implementation-steps.png)

## <a name="assumptions"></a>Varsayımlar
Bu makalede, DevTest Labs pilot uygulamadan önce aşağıdakilerin yerinde olduğunu varsayar:

- **Azure aboneliği**: Pilot ekip bir Azure aboneliğine kaynak dağıtmaya erişimine sahiptir. Yalnızca geliştirme ve test iş yükleri olduğundan, ek kullanılabilir görüntüler ve Windows sanal makinelerinde daha düşük fiyatlar için Kurumsal geliştirme ve test teklifini seçebilmek için önerilir.
- **Şirket içi erişim**: Gerekirse, şirket içi erişim zaten yapılandırıldı. Express Route veya siteden siteye VPN bağlantısı aracılığıyla şirket içi erişim gerçekleştirilebilir. Express Route üzerinden bağlantıyı genellikle kurmak için birçok hafta sürebilir, Express Route Proje başlamadan önce oluşturmuş olmanız önerilir.
- **Pilot takımlar**: DevTest Labs kullanan ilk geliştirme proje takımları, geçerli geliştirme ve test etkinlikleri birlikte belirlenmiştir ve hedefleri/gereksinimleri/hedefleri bu takımlar için oluşturun.

## <a name="milestone-1-establish-initial-network-topology-and-design"></a>1. Aşama: İlk ağ topolojisi ve tasarım
Azure DevTest Labs çözümünü dağıtırken odak ilk alan sanal makineler için planlı bağlantı oluşturmaktır. Aşağıdaki adımlar, gerekli yordamları özetlemektedir:

1. Tanımlama **başlangıç IP adresi aralıklarını** Azure DevTest Labs aboneliği atanmış. Bu adım, sanal makinelerin sayısı beklenen kullanım, büyük yeterli blok gelecekteki genişleme için sağlayabilmesi tahmin gerektirir.
2. Tanımlamak **istenen erişim yöntemlerine** içine DevTest Labs (örneğin, dış / iç erişim). Bir anahtar Bu adımda sanal makinelerin genel IP adreslerine sahip olup olmadığını belirlemek için noktasıdır (diğer bir deyişle, doğrudan internet'ten erişilebilir).
3. Tanımlamak ve kurmak **bağlantı yöntemlerinin** Azure geri kalanı ile bulut ortamı ve şirket içi. Express Route ile zorlamalı yönlendirme etkinleştirilirse sanal makinelerin Kurumsal Güvenlik Duvarı'nı geçirmek için uygun proxy yapılandırmaları gerekir olasıdır.
4. VM olması durumunda **etki alanına katılmış**, bir bulut tabanlı etki alanına (örneğin AAD Dizin Hizmetleri) veya bir şirket içi etki alanı katılmaları belirleyin. Şirket içi, sanal makinelerin katıldığı active Directory'de hangi kuruluş birimi (OU) belirler. Ayrıca, kullanıcıların katılın (veya etki alanında makine kayıtları oluşturma yeteneği olan bir hizmet hesabı oluşturmak için) erişimi olmasını onaylayın

## <a name="milestone-2-deploy-the-pilot-lab"></a>2. Aşama: Pilot Laboratuvar dağıtma
Ağ topolojisini yerleştirildikten sonra ilk/pilot Laboratuvar aşağıdaki yararlanarak oluşturulabilir adımları:

1. İlk DevTest Labs ortam oluşturma (adım adım yönergeler bulunabilir [burada](https://github.com/Azure/fta-devops/blob/master/devtest-labs/articles/devtest-labs-walkthrough-it.md))
2. İzin verilen VM görüntüleri ve boyutları lab ile kullanmak için bu seçeneği belirleyin. Özel görüntüleri Azure DevTest Labs ile kullanmak için karşıya yüklenebilir olup olmadığını belirleyin.
3. Laboratuvar erişim ilk rol temel erişim denetimleri (RBAC) (Laboratuvar sahibini ve Laboratuvar kullanıcıları) Laboratuvar için oluşturarak güvenli hale getirin. DevTest Labs ile kimliği için Azure Active Directory ile eşitlenen active directory hesaplarını kullanmanızı öneririz.
4. DevTest Labs, zamanlamalar, yönetim, talep edilebilir VM'ler, özel görüntüleri ve formülleri maliyeti gibi ilkelerini kullanmak için yapılandırın.
5. Çevrimiçi bir depo gibi Azure depoları/Git kurun.
6. Genel veya özel depolarda veya ikisinin birleşimi kullanımını karar verin. JSON şablonları, dağıtımlar ve uzun vadeli sustainment için düzenleyin.
7. Gerekirse, özel yapıtlar oluşturma. Bu adım isteğe bağlıdır. 

## <a name="milestone-3-documentation-support-learn-and-improve"></a>3. Aşama: Belgeler, destek, öğrenin ve geliştirin
İlk pilot takımlar, başlamak için kapsamlı destek gerektirebilir. Belgeleri emin olmak için deneyimlerini kullanın ve Azure DevTest Labs'i sürekli dağıtımı için yerinde desteğidir.

1. Pilot takımlar kendi yeni DevTest Labs kaynaklarına (tanıtımlar, belgeler) sunar.
2. Pilot takımların deneyimleri temel alarak, planlama ve belge gerektiği şekilde sunun
3. (Oluşturma ve sağlama erişim, vb. laboratuvarlar, yapılandırma) yeni takımların onboarding işlemi resmileştirin
4. İlk katılım üzerinde bağlı olarak, özgün tahmin IP adres alanı yine de makul ve doğru olduğundan emin olun
5. Uygun uyumluluk ve güvenlik incelemelerini tamamlandığından emin olunmasını

## <a name="next-steps"></a>Sonraki adımlar
Bu serideki sonraki makaleye bakın: [Azure DevTest Labs altyapı İdaresi](devtest-lab-guidance-governance-resources.md)
