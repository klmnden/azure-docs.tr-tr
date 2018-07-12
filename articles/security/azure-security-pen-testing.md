---
title: Sızma testi | Microsoft Docs
description: Makale (pentest) işlem sızma genel bir bakış sağlar ve Azure altyapısında çalışan uygulamalarınızı karşı pentest nasıl gerçekleştirin.
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
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970828"
---
# <a name="pen-testing"></a>Sızma testi
Uygulamayı test etme ve dağıtım için Azure kullanmanın avantajları oluşturulmuş ortamları hızlıca elde edebilirsiniz biridir.  Requisitioning, alma ve "raflama ve yığınlama" endişelenmek zorunda olmadığınız kendi şirket içi donanım.

Bu harika –, ancak normal güvenlik gerçekleştirdiğiniz emin olmak yine aksaklıkla. Yapmanız gereken şeylerden biri sızma Azure'da dağıttığınız uygulamaları test etme.

Microsoft gerçekleştireceğini zaten biliyor olabilirsiniz [sızma testi bizim Azure ortamının](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Bu yardımcı, Azure geliştirmeleri yönlendirin. 

Kalem yok, uygulamanızı test edin, ancak ve istediğiniz kendi uygulamalarınızı test etme kalem yapmanız gerektiğini biliyoruz. Bunun nedeni, uygulamalarınızın güvenliğini, tüm Azure ekosistemi daha güvenli olmasına yardımcı çok iyi bir şey olmasıdır.

Ne yapmanız gerekir?

15 Haziran 2017'den itibaren Microsoft artık bir sızma yürütmek için Ön onay gerektirir. Azure kaynaklarına karşı testler. Microsoft Azure karşı angajmanları resmi belge yaklaşan sızma isteyen müşteriler doldurun izlemeleri [Azure hizmet sızma testi bildirimi form](https://portal.msrc.microsoft.com/en-us/engage/pentest). Bu işlem yalnızca Microsoft Azure ve Microsoft bulut hizmeti için diğer uygulanamaz ilişkilidir. 

>[!IMPORTANT] 
>Sızma testi etkinlikleri Microsoft'tan bildiren artık gerekli değildir ancak müşteriler hala uymak zorundadır [Microsoft bulut birleşik sızma testi kuralları of Engagement](https://technet.microsoft.com/mt784683). 

Standart testler gerçekleştirmek şunları içerir:

* Testleri ortaya çıkarmak için uç noktalarınız üzerinde [açık Web uygulaması güvenlik Project (OWASP) ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Rastgele test](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) , uç noktalar
* [Bağlantı noktası tarama](https://en.wikipedia.org/wiki/Port_scanner) , uç noktalar

Bir yerine getiremez test türünde herhangi bir [hizmet reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırı. Bu, bir DoS saldırısı başlatmasını veya belirleme, göstermek veya DoS saldırısı herhangi bir türde benzetimini ilgili testleri gerçekleştirme içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Kalem ile kullanmaya başlamak hazır, Microsoft Azure'da barındırılan uygulamalarınız test ediyorsunuz? Bu nedenle, daha sonra gidin üzerinde [sızma testi genel bakış](https://technet.microsoft.com/library/mt784683.aspx) sayfasında (ve bir testi isteği düğme sayfanın alt kısmındaki Oluştur'a tıklayın. 
