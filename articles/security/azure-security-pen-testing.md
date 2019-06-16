---
title: Sızma testi | Microsoft Docs
description: Makale (pentest) işlem sızma genel bir bakış sağlar ve Azure altyapısında çalışan uygulamalarınızı karşı pentest nasıl gerçekleştirin.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: 8835c4534b6dab1e8dbfb3546257ae4bc3b9d7af
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610922"
---
# <a name="penetration-testing"></a>Sızma Testi
Uygulamayı test etme ve dağıtım için Azure kullanmanın avantajları oluşturulmuş ortamları hızlıca elde edebilirsiniz biridir. Requisitioning, alma ve "raflama ve yığınlama" endişelenmek zorunda olmadığınız kendi şirket içi donanım.

Bu harika –, ancak normal güvenlik gerçekleştirdiğiniz emin olmak yine aksaklıkla. Büyük olasılıkla yapmak istediğiniz şey biri sızma Azure'da dağıttığınız uygulamaları test etme.

Microsoft gerçekleştireceğini zaten biliyor olabilirsiniz [sızma testi bizim Azure ortamının](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Bu yardımcı, Azure geliştirmeleri yönlendirin.

Biz sizin için uygulamanızı sızma testi yok, ancak ve istediğiniz kendi uygulamalarınızı test etme yapmanız gerektiğini biliyoruz. Bunun nedeni, uygulamalarınızın güvenliğini, tüm Azure ekosistemi daha güvenli olmasına yardımcı çok iyi bir şey olmasıdır.

15 Haziran 2017'den itibaren Microsoft artık Azure kaynaklarına karşı sızma testi yürütmek için Ön onay gerektirir. Microsoft Azure karşı angajmanları resmi belge yaklaşan sızma isteyen müşteriler doldurun izlemeleri [Azure hizmet sızma testi bildirimi form](https://portal.msrc.microsoft.com/en-us/engage/pentest). Bu işlem yalnızca Microsoft Azure ve Microsoft bulut hizmeti için diğer uygulanamaz ilişkilidir.

>[!IMPORTANT]
>Sızma testi etkinlikleri Microsoft'tan bildiren artık gerekli değildir ancak müşteriler hala uymak zorundadır [Microsoft bulut birleşik sızma testi kuralları of Engagement](https://technet.microsoft.com/mt784683).

Standart testler gerçekleştirmek şunları içerir:

* Testleri ortaya çıkarmak için uç noktalarınız üzerinde [açık Web uygulaması güvenlik Project (OWASP) ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Rastgele test](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) , uç noktalar
* [Bağlantı noktası tarama](https://en.wikipedia.org/wiki/Port_scanner) , uç noktalar

Bir yerine getiremez test türünde herhangi bir [hizmet reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırı. Bu, bir DoS saldırısı başlatmasını veya belirleme, göstermek veya DoS saldırısı herhangi bir türde benzetimini ilgili testleri gerçekleştirme içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Resmi Microsoft Azure'da barındırılan uygulamalarınız üzerinde bir gelecek sızma belgelemek istiyorsanız gidin üzerinde [sızma testi kuralları of Engagement](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) ve sınama bildirim formu doldurun.
