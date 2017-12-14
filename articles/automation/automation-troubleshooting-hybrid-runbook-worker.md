---
title: "Azure Otomasyon karma Runbook çalışanı sorunlarını giderme | Microsoft Docs"
description: "Belirtiler, nedenleri ve Azure automation'da en yaygın karma Runbook çalışanı sorunlara yönelik açıklanmaktadır."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 75f4ac1bc940a2b1d8e4ac6aeac8b80c642489da
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a>Karma Runbook çalışanı için sorun giderme ipuçları

Bu makalede Otomasyon karma Runbook çalışanları ile karşılaşabilirsiniz ve bunları gidermek için olası çözümleri önerir hatalarında sorun giderme yardım sağlar.

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a>Bir runbook işi askıya alındı durumu ile sona erer

Runbook'unuzda üç kez yürütmek kısa bir süre içinde çalışırken askıya alınır. Runbook başarıyla tamamlanmasını kesintiye koşullar vardır ve ilgili hata iletisi nedenini gösteren ek bilgiler içermez. Bu makalede, karma Runbook çalışanı runbook yürütme hataları ile ilgili sorunları için sorun giderme adımları sağlar.

Bu makalede Azure sorunu ele alınmamışsa Azure forumları ziyaret [MSDN ve yığın taşması](https://azure.microsoft.com/support/forums/). Bu forumları veya çok sorununuzu nakledebilirsiniz [ @AzureSupport Twitter'da](https://twitter.com/AzureSupport). Ayrıca, size bir Azure destek isteği seçerek dosya **alma desteği** üzerinde [Azure Destek](https://azure.microsoft.com/support/options/) site.

### <a name="symptom"></a>Belirti
Runbook yürütme başarısız olur ve döndürülen hata, "iş işlem beklenmedik şekilde durdurulduğundan 'Etkinleştir' çalıştırılamıyor eylemi. İş eylemi 3 kez denendi."

Hatanın birkaç olası nedenleri şunlardır: 

1. Karma çalışanı bir proxy veya güvenlik duvarı arkasında olduğunu
2. Karma çalışanı üzerinde çalıştığı bilgisayar en düşük daha azı [donanım gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker)  
3. Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>: 1 karma Runbook çalışanı proxy veya güvenlik duvarının arkasındaysa nedeni
Karma Runbook çalışanı üzerinde çalıştığı bilgisayar güvenlik duvarı veya proxy sunucunun arkasında olduğu ve giden ağ erişimi izin verilen veya doğru şekilde yapılandırılmış.

#### <a name="solution"></a>Çözüm
Bilgisayarın, bağlantı noktası 443 üzerinde *.azure automation.net giden erişimi olduğunu doğrulayın. 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Neden 2: Bilgisayar değerinden en düşük donanım gereksinimleri vardır.
Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi halde, kaynak kullanımı diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden Çekişme bağlı olarak bilgisayar üzerinde kullanılan haline gelir ve runbook işi gecikmeler veya zaman aşımları neden. 

#### <a name="solution"></a>Çözüm
İlk karma Runbook çalışanı özelliği yürütmek için atanan bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın.  Aşması durumunda, karma Runbook çalışanı işlemlerinin performansını ile Windows arasında herhangi bir bağıntı belirlemek için CPU ve bellek kullanımını izleyin.  Bellek veya CPU baskısı ise, bu yükseltme veya ek işlemciler ekleme ya da kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösteriyor olabilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimleri destekleyebilen farklı işlem kaynak seçin.         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>3. neden: Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

#### <a name="solution"></a>Çözüm
Denetleme **Microsoft SMA** açıklama ile ilgili bir olayın olay günlüğünü *işlemi Win32 çıkış koduyla [4294967295]*.  Bu hatanın nedenini larınızda kimlik doğrulaması yapılandırılmış veya belirtilen karma çalışanı grubu için farklı çalıştır kimlik bilgileri henüz ' dir.  Lütfen gözden [Runbook izinleri](automation-hrw-run-runbooks.md#runbook-permissions) doğru şekilde yapılandırdığınız kimlik doğrulaması runbook'larınızın onaylamak için.  

## <a name="next-steps"></a>Sonraki adımlar

Otomasyon diğer sorunlarını giderme Yardımı için bkz [Azure Automation ile ilgili genel sorunları giderme](automation-troubleshooting-automation-errors.md) 