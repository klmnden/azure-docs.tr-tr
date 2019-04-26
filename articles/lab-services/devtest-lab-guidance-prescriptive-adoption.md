---
title: Kuruluşunuz için Azure DevTest Labs benimseyin
description: Bu makalede, kuruluşunuzda Azure DevTest Labs'nu benimseme normatif bir Rehber sağlanır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: e1d119f3c7c5d6dbdb570d362c53b80dad7886bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60198046"
---
# <a name="set-up-azure-devtest-labs-infrastructure-in-your-enterprise"></a>Azure DevTest Labs altyapı kuruluşunuzdaki ayarlama
Kuruluşların hızla nedeniyle bulut benimseme kendi [avantajları](/azure/architecture/cloud-adoption/business-strategy/cloud-migration-business-case) çeviklik, esneklik ve ekonomik içerir. Müşterilerin bulut kullanmak ortak bir ilk adım, geliştirme ve test iş yükleri ile başlatmaktır.  DevTest Labs sağlar [özellikleri](devtest-lab-concepts.md) Kurumsal ve Destek avantajı [anahtar Kurumsal geliştirme ve test senaryoları](devtest-lab-guidance-get-started.md).

Bu iş yüklerinin buluta geçiş yaparken kaygıları ortak bir dizi şöyledir:

- [Geliştirme/test kaynaklarını güvenli hale getirme](devtest-lab-guidance-governance-policy-compliance.md)
- [Maliyet anlama ve yönetme](devtest-lab-guidance-governance-cost-ownership.md)
- Kurumsal güvenlik ve uyumluluk ödün vermeden, geliştiriciler için Self Servis etkinleştirme
- Otomatikleştirme ve DevTest Labs, ek senaryoları kaplayacak şekilde genişletme
- [DevTest Labs tabanlı bir çözüm kaynakları binlerce ölçeklendirme](devtest-lab-guidance-scale.md)
- [DevTest Labs, büyük ölçekli dağıtımlar](devtest-lab-guidance-orchestrate-implementation.md)
- [Kavram kanıtı ile çalışmaya başlama](devtest-lab-guidance-orchestrate-implementation.md)

## <a name="intended-audience"></a>Hedef kitle
Kurumsal odaklı belgeleri BT planlayıcıları ve mimarları oluşturma ve genel dağıtımları gözden geçirme ve işlemleri yöntemler gözlemledikten sorumlu yöneticileri için tasarlanmıştır. Sonuç olarak, bu belge genel işlem vurgular ve sonuçta bir kuruluştaki Azure DevTest Labs benimseme sürücüleri bir güvenli, kararlı geliştirme/test ortamını, yükseltmek için tasarım ilkeleri önerilir.

## <a name="enterprise-customers"></a>Kurumsal müşteriler

Birçok geçerli DevTest Labs Kurumsal müşteriler, geliştirme ve test iş yükleri, kuruluşları için DevTest Labs başarıyla kullanın. [Daha fazla bilgi edinin](https://azure.microsoft.com/en-us/case-studies/?term=DevTest+labs).

## <a name="next-steps"></a>Sonraki adımlar
- [Bir kuruluş için başvuru mimarisi](devtest-lab-reference-architecture.md)
