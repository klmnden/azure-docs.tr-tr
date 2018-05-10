---
title: Kalem sınama | Microsoft Docs
description: Makale (pentest) işlem sınama sızma genel bir bakış sağlar ve uygulamalarınızı Azure altyapısında çalışan karşı pentest nasıl gerçekleştirin.
services: security
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2018
ms.author: barclayn
ms.openlocfilehash: a64316eda25bd02f89b5afdd7b98c0193381d023
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="pen-testing"></a>Kalem test etme
Azure uygulama test ve dağıtım için kullanmanın avantajları oluşturulan ortamları hızlı bir şekilde alabilir biridir.  Requisitioning, alma, "racking ve yığınlama hakkında" endişelenmenize gerek yoktur, kendi şirket içi donanım.

Bu harika –, ancak yine de normal güvenlik gerçekleştirdiğiniz emin olun gerekiyorsa durum tespitlerini. Yapmanız gereken şeyleri biri sızma Azure'da dağıttığınız uygulamaları sınayın.

Microsoft yaptığı zaten biliyor olabilirsiniz [bizim Azure ortamının sızma testi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Bu yardımcı Azure geliştirmeleri sürücü. 

Kalem yok, uygulamanızı test etmek, ancak biz, istediğiniz ve kendi uygulamalarınızı test etme kalem gerçekleştirmek gerekiyor, anlama. Uygulamalarınızın güvenliğini, tüm Azure ekosistemi daha güvenli olmasına yardımcı olmak için iyi bir şey olmasıdır.

Ne yapmanız gerekir?

15 Haziran 2017'ten itibaren Microsoft artık bir sızma yürütmek için Ön onay gerektiren Azure kaynaklarına karşı testleri. Microsoft Azure karşı katılımlar sınama resmi belge yaklaşan sızma isteyen müşteriler doldurmak izlemeleri [Azure hizmeti sızma test bildirimi form](https://portal.msrc.microsoft.com/en-us/engage/pentest). Bu işlem yalnızca Microsoft Azure ve uygulanamaz diğer Microsoft bulut hizmeti ilgilidir. 

>[!IMPORTANT] 
>Microsoft etkinlikler sınama kalemin bildiren artık gerekli değildir ancak müşteriler hala uymalıdır [Microsoft bulut birleşik sızma sınama kuralları of katılım](https://technet.microsoft.com/mt784683). 

Standart testler gerçekleştirebileceğiniz içerir:

* Ortaya çıkarmaya noktalarınızı testleri [açık Web uygulaması güvenlik proje (OWASP) ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Rastgele test](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) , uç noktaları
* [Bağlantı noktası tarama](https://en.wikipedia.org/wiki/Port_scanner) , uç noktaları

Bir işlemi gerçekleştiremezsiniz test türünde herhangi bir tür [hizmet reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırı. Bu bir DoS saldırısı başlatmasını veya belirlemek, göstermek veya DoS saldırısı herhangi bir türde benzetimini ilgili testleri gerçekleştirme içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Kalem ile çalışmaya başlamak hazır, Microsoft Azure üzerinde barındırılan uygulamalarınızı test ediyorsunuz? Bunu yeniden baş üzerinde üzerinden için [sızma testi genel bakış](https://technet.microsoft.com/library/mt784683.aspx) sayfa (ve test etme isteği düğmesine sayfanın sonundaki Oluştur'u tıklatın. 
