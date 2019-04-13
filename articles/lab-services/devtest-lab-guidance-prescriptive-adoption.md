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
ms.openlocfilehash: 67bb81675dabd8aef28693a958a0ac39b1b441e7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549261"
---
# <a name="set-up-azure-devtest-labs-infrastructure-in-your-enterprise"></a>Azure DevTest Labs altyapı kuruluşunuzdaki ayarlama
Kuruluşların hızla nedeniyle bulut benimseme kendi [avantajları](/architecture/cloud-adoption/business-strategy/cloud-migration-business-case) çeviklik, esneklik ve ekonomik içerir. Müşterilerin bulut kullanmak ortak bir ilk adım, geliştirme ve test iş yükleri ile başlatmaktır.  DevTest Labs sağlar [özellikleri](devtest-lab-concepts.md) Kurumsal ve Destek avantajı [anahtar Kurumsal geliştirme ve test senaryoları](devtest-lab-guidance-get-started.md).

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

## <a name="next-steps"></a>Sonraki adımlar
- [Bir kuruluş için başvuru mimarisi](devtest-lab-reference-architecture.md)