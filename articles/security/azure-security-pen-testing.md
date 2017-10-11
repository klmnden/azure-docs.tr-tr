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
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 070e848f753452953b9e5dfe94799e7c0a314530
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="pen-testing"></a>Kalem test etme
Uygulama test ve dağıtım için Microsoft Azure kullanma hakkında harika şeylerden geliştirmek, test ve uygulamalarınızı dağıtmak için bir şirket içi altyapı araya gerekmez biridir. Tüm altyapı tarafından Microsoft Azure platform hizmetlerini hallolduğuna. Requisitioning, alma, "racking ve yığınlama hakkında" endişelenmenize gerek yoktur, kendi şirket içi donanım.

Bu harika –, ancak yine de normal güvenlik gerçekleştirdiğiniz emin olun gerekiyorsa durum tespitlerini. Yapmanız gereken şeyleri biri sızma Azure'da dağıttığınız uygulamaları sınayın.

Microsoft yaptığı zaten biliyor olabilirsiniz [bizim Azure ortamının sızma testi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Bu bizim platform geliştirmemize yardımcı olur ve güvenlik denetimleri geliştirme, yeni güvenlik denetimleri tanıtımı ve bizim güvenlik işlemleri geliştirme açısından bizim Eylemler size yol gösterir.

Kalem yok, uygulamanızı test etmek, ancak biz, istediğiniz ve kendi uygulamalarınızı test etme kalem gerçekleştirmek gerekiyor, anlama. Uygulamalarınızın güvenliğini, tüm Azure ekosistemi daha güvenli olmasına yardımcı olmak için iyi bir şey olmasıdır.

Uygulamalarınızı, kalem test ettiğinizde, saldırının gibi bize görünebilir. Biz [sürekli izleme](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) için saldırı desenleri ve biz gerekiyorsa, bir olay yanıtlama işlemi başlatır. Bu işe yaramazsa ve biz tespitlerini kalem son kendi testleriniz nedeniyle bir olay yanıtlama harekete geçirirseniz bize yardımcı değil.

Ne yapmanız gerekir?

Kalem için hazır olduğunuzda Azure barındırılan uygulamalarınızı test, bir seçeneğine sahip [bize bildirin](https://portal.msrc.microsoft.com/en-us/engage/pentest). Biliyoruz sonra belirli sınamalar gerçekleştirmek oluşturacağız, biz yanlışlıkla (test ettiğiniz IP adresi engelleme gibi) kapattığınız kalmaz, testlerinizi koşulları sınama Azure kalem uygun ve koşullar açıklanan sürece [Microsoft bulut birleşik sızma sınama kuralları of katılım](https://technet.microsoft.com/en-us/mt784683).
Standart testler gerçekleştirebileceğiniz içerir:

* Ortaya çıkarmaya noktalarınızı testleri [açık Web uygulaması güvenlik proje (OWASP) ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Rastgele test](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) , uç noktaları
* [Bağlantı noktası tarama](https://en.wikipedia.org/wiki/Port_scanner) , uç noktaları

Bir işlemi gerçekleştiremezsiniz test türünde herhangi bir tür [hizmet reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırı. Bu bir DoS saldırısı başlatmasını veya belirlemek, göstermek veya DoS saldırısı herhangi bir türde benzetimini ilgili testleri gerçekleştirme içerir.

Kalem ile çalışmaya başlamak hazır, Microsoft Azure üzerinde barındırılan uygulamalarınızı test ediyorsunuz? Bunu yeniden baş üzerinde üzerinden için [sızma testi genel bakış](https://technet.microsoft.com/library/mt784683.aspx) sayfa (ve test etme isteği düğmesine sayfanın sonundaki Oluştur'u tıklatın. Ayrıca, hüküm ve koşullar ve Azure veya herhangi bir Microsoft hizmeti ile ilgili güvenlik açıkları nasıl rapor üzerinde yardımcı bağlantılar sınama kalem hakkında daha fazla bilgi bulabilirsiniz.
