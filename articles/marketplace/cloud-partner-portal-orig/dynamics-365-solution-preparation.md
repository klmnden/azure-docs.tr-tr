---
title: Dynamics 365 çözüm hazırlama | Microsoft Docs
description: Framework paketleme, yükleme ve bileşenleri kaldırılıyor
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricarod.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 2ea62a826b9f99e264cb1b2242f8d8108453d8e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397332"
---
# <a name="dynamics-365-solution-preparation"></a>Dynamics 365 çözüm hazırlama

Dynamics 365 solutioning sistem, paketleme, yükleme ve belirli iş işlevselliği sunan bileşenleri kaldırma için kullanılan bir çerçevedir. Çözümler, ISV'ler ve diğer Microsoft Dynamics 365 iş ortakları tarafından oluşturdukları uzantıları dağıtmak için kullanılır.

Var olan bir Dynamics 365 (xRM) ISV iseniz, büyük olasılıkla zaten yönetilen bir çözüm oluşturdunuz ve bir solution.zip dosyasına sahip. Çözümünüzdeki "Görünen ad" emin olun ve "Description" alanları, müşterilerin görmek için istediğinize yansıtır. Bunlar CRM Online Yönetim Merkezi'nde gösterilir.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Not:** Aşağıdaki paket örnekte çözüm adı "SampleSolution.zip" olduğunu varsayacağız_

Yeni bir ISV iseniz, burada bir çözüm oluşturma hakkında daha fazla ayrıntıya ulaşabilirsiniz: [https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Destek verileri, çözümünüzün gerektiriyorsa:

* Test ortamınızda örnek veriyi oluşturun
* Karşılaştırma kurallarını verileriniz için bir şema oluşturmak için yapılandırma geçiş aracını kullanın.
* Yapılandırma şeması ile proje dosyalarını kaydedin. Yapılandırma verilerinizi güncelleştirirseniz bunu daha sonra gerekecektir.
* Yapılandırma verilerinizi dışarı aktarın. Dışarı aktarma dosyasına dışa aktarma için anlamlı bir ad verin emin olun.
