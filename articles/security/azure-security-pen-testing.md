---
title: "Kalem sınama | Microsoft Docs"
description: "Makale (pentest) işlem sınama sızma genel bir bakış sağlar ve uygulamalarınızı Azure altyapısında çalışan karşı pentest nasıl gerçekleştirin."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: 3ad22e78693c54c62f9230f7f52460e01e5e0022
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="pen-testing"></a>Kalem test etme
Azure uygulama test ve dağıtım için kullanmanın avantajları oluşturulan ortamları hızlı bir şekilde alabilir biridir.  Requisitioning, alma, "racking ve yığınlama hakkında" endişelenmenize gerek yoktur, kendi şirket içi donanım.

Bu harika –, ancak yine de normal güvenlik gerçekleştirdiğiniz emin olun gerekiyorsa durum tespitlerini. Yapmanız gereken şeyleri biri sızma Azure'da dağıttığınız uygulamaları sınayın.

Microsoft yaptığı zaten biliyor olabilirsiniz [bizim Azure ortamının sızma testi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Bu yardımcı Azure geliştirmeleri sürücü. 

Kalem yok, uygulamanızı test etmek, ancak biz, istediğiniz ve kendi uygulamalarınızı test etme kalem gerçekleştirmek gerekiyor, anlama. Uygulamalarınızın güvenliğini, tüm Azure ekosistemi daha güvenli olmasına yardımcı olmak için iyi bir şey olmasıdır.

Uygulamalarınızı, kalem test ettiğinizde, saldırının gibi bize görünebilir. Biz [sürekli izleme](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) için saldırı desenleri ve biz gerekiyorsa, bir olay yanıtlama işlemi başlatır. Bu işe yaramazsa ve biz tespitlerini kalem son kendi testleriniz nedeniyle bir olay yanıtlama harekete geçirirseniz bize yardımcı değil.

Ne yapmanız gerekir?

Kalem için hazır olduğunuzda Azure barındırılan uygulamalarınızı test, bir seçeneğine sahip [bize bildirin](https://portal.msrc.microsoft.com/en-us/engage/pentest). Bildirim sonra Microsoft Hayır yanlışlıkla, (test ettiğiniz IP adresi engelleme gibi) kapanır. Testlerinizi koşulları sınama Azure kalem uymalıdır ve koşullar açıklanan [Microsoft bulut birleşik sızma sınama kuralları of katılım](https://technet.microsoft.com/en-us/mt784683).

Standart testler gerçekleştirebileceğiniz içerir:

* Ortaya çıkarmaya noktalarınızı testleri [açık Web uygulaması güvenlik proje (OWASP) ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Rastgele test](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) , uç noktaları
* [Bağlantı noktası tarama](https://en.wikipedia.org/wiki/Port_scanner) , uç noktaları

Bir işlemi gerçekleştiremezsiniz test türünde herhangi bir tür [hizmet reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırı. Bu bir DoS saldırısı başlatmasını veya belirlemek, göstermek veya DoS saldırısı herhangi bir türde benzetimini ilgili testleri gerçekleştirme içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Kalem ile çalışmaya başlamak hazır, Microsoft Azure üzerinde barındırılan uygulamalarınızı test ediyorsunuz? Bunu yeniden baş üzerinde üzerinden için [sızma testi genel bakış](https://technet.microsoft.com/library/mt784683.aspx) sayfa (ve test etme isteği düğmesine sayfanın sonundaki Oluştur'u tıklatın. 