---
title: Desteklenebilirlik ve kullanımdan kaldırma ilkesi için Azure konuk işletim kılavuzu | Microsoft Docs
description: Hangi Microsoft olarak yönlendirilmesinin Azure konuk işletim sistemine bulut Hizmetleri tarafından kullanılan destekleyecek hakkında bilgi sağlar.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 9/20/2017
ms.author: raiye
ms.openlocfilehash: dfa3bac95b9827789950b4931e3198237de4a1fd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34608571"
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure konuk işletim sistemi desteklenebilirlik ve kullanımdan kaldırma İlkesi
Bu sayfadaki bilgiler Azure konuk işletim sistemi için ilgili ([konuk işletim sistemi](cloud-services-guestos-update-matrix.md)) bulut Hizmetleri worker ve web rolleri (PaaS). Sanal makineler (Iaas) için geçerli değildir.

Microsoft olan bir yayımlanan [konuk işletim sistemi için destek ilkesi](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Okumakta olduğunuz sayfa artık ilkenin nasıl uygulandığı açıklanmaktadır.

İlke

1. Microsoft destekleneceğini **en az konuk işletim sisteminin en son iki ailesi**. Bir aile devre dışı bırakıldığında, müşterilerin daha yeni bir desteklenen konuk işletim sistemi ailesi güncelleştirmek için resmi tarihten itibaren 12 ay sahip.
2. Microsoft destekleneceğini **en az desteklenen konuk işletim sistemi aileleri son iki sürümü**.
3. Microsoft destekleneceğini **en az Azure SDK'ın en son iki sürümleri**. SDK sürümü devre dışı bırakıldığında, müşterilerin daha yeni bir sürüme güncelleştirmek için resmi tarihten itibaren 12 ay gerekmektedir.

Bazen, birden çok iki ailesi ya da sürümlerden desteklenmiyor olabilir. Resmi konuk işletim sistemi destek bilgileri görüntülenecek [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-version-is-retired"></a>Bir konuk işletim sistemi sürümü ne zaman devre dışı bırakıldı
Yeni konuk işletim sistemi **sürümleri** en son MSRC güncelleştirmeleri içerecek şekilde ayda hakkında sunulur. Normal aylık güncelleştirmeleri nedeniyle bir konuk işletim sistemi sürümü normalde yaklaşık 60 gün sonra yayınlandığı devre dışı. Bu etkinlik kullanılabilir her ailesi için en az iki konuk işletim sistemi sürümleri tutar.

### <a name="process-during-a-guest-os-family-retirement"></a>İşlem sırasında bir konuk işletim sistemi ailesi devre dışı bırakma
Devre dışı bırakma duyurdu sonra müşterilerin eski ailesi resmi olarak hizmetinden kaldırılmadan önce bir 12 ay "geçiş" süresine sahip. Bu geçiş süresi Microsoft kümeleri genişletilmiş. Güncelleştirmeleri gönderilecektir [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

Aşamalı devre dışı bırakma işlemi altı (6) ay geçiş dönemi içinde başlar. Bu süre boyunca:

1. Microsoft, müşterilerinin devre dışı bırakma size bildirir.
2. Azure SDK'sının daha yeni sürüm Çekildi konuk işletim sistemi ailesi Destek olmaz.
3. Yeni dağıtımları ve bulut Hizmetleri redeployments Çekildi ailesinde izin verilmiyor

Microsoft, en son MSRC güncelleştirmeleri "sona erme tarihini" bilinen geçiş döneminin son gününü kadar ekleme yeni konuk işletim sistemi sürümü tanıtmak devam edecektir. Sona erme tarihini bulut hala çalışan hizmetler Azure SLA altında desteklenmeyen. Microsoft, yükseltme, silmek veya bu tarihten sonra bu hizmetlerini durdurmak için kendi takdirine bağlı sahiptir.

### <a name="process-during-a-guest-os-version-retirement"></a>İşlem sırasında bir konuk işletim sistemi sürümü devre dışı bırakma
Müşteriler otomatik olarak güncelleştirmek için konuk işletim ayarlarsanız, bunlar hiçbir zaman konuk işletim sistemi sürümleri yapılacağı hakkında endişelenmeye gerek yoktur. Bunlar her zaman en son konuk işletim sistemi sürümünü kullanacak.

Konuk işletim sistemi sürümleri her ayda bir yayınlanır. Normal sürümleri oranını nedeniyle, her bir sürümü sabit bir kullanım ömrü vardır.

Kullanım ömrü içine 60 günde bir sürümüdür "*devre dışı*". "Devre dışı" sürüm portaldan kaldırılır anlamına gelir. Sürüm artık CSCFG yapılandırma dosyasından ayarlayabilirsiniz. Var olan dağıtımlar çalıştıran bırakılır. Ancak yeni dağıtımlar ve var olan dağıtımlar için kod ve yapılandırma güncelleştirmeleri değil izin verilecek.

"Devre dışı olma", sonra biraz zaman konuk işletim sistemi sürümü "*süresi*" ve hala bu sürümünü çalıştıran herhangi bir yüklemesi yükselttiyseniz ve konuk işletim sistemi gelecekte otomatik olarak güncelleştirmek için ayarlanmış zorla. Disablement gelen süre sonu değişebilir şekilde sona erme toplu olarak gerçekleştirilir.

Bu nokta müşteri geçişleri kolaylaştırmak için Microsoft'un istediğiniz kadar uzun yapılabilir. Üzerinde herhangi bir değişiklik duyurulacaktır [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Devre dışı bırakma sırasında bildirimleri
* **Aile devre dışı bırakma** <br>Microsoft, blog gönderileri ve portal bildirim kullanır. Devre dışı bırakılan bir konuk işletim sistemi ailesi hala kullanan müşteriler, atanan hizmet yöneticileri doğrudan iletişim (e-posta, portal iletileri, telefon araması) üzerinden bildirilecek. Tüm değişiklikleri için gönderilen [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).
* **Sürüm devre dışı bırakma** <br>Tüm değişiklikleri ve bunlar ortaya tarihleri nakledilir [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)sürüm, devre dışı bırakıldı ve sona erme dahil olmak üzere. Devre dışı konuk işletim sistemi sürümü veya ailesi üzerinde çalışan dağıtımları varsa Hizmetleri Yöneticiler e-postaları alır. Bu e-postaları zamanlama farklılık gösterebilir. Bu zamanlama resmi bir SLA olmasa da genellikle en az bir ay disablement önce oldukları.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Geçiş etkileri nasıl en aza indirebileceğiniz?**

Bulut Hizmetleri Tasarlama için en son konuk işletim sistemi ailesi kullanmanızı öneririz.

1. Geçişinizi daha yeni bir aile erken planlama başlatın.
2. Yeni ailesi üzerinde çalışan bulut hizmetiniz test etmek için geçici test dağıtımları ayarlayın.
3. Konuk işletim sistemi sürümünüzü ayarlamak **otomatik** (osVersion = * içinde [.cscfg](cloud-services-model-and-package.md#cscfg) dosyası) geçiş yeni konuk işletim sistemi sürümleri için otomatik olarak oluşmaz.

**Ne web Uygulamam işletim sistemi ile daha derin tümleştirme gerektiriyor?**

Web uygulama Mimarinizi işletim sisteminin temel özelliklerine bağlıdır, desteklenen platform özelliklerini gibi kullandığınız [başlangıç görevleri](cloud-services-startup-tasks.md) veya diğer genişletilebilirlik mekanizması. Alternatif olarak, ayrıca kullanabileceğiniz [Azure sanal makineleri](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (Iaas – hizmet olarak altyapı), temel işletim sistemini bakımından sorumlu olduğu.

## <a name="next-steps"></a>Sonraki adımlar
En son gözden [konuk işletim sistemi sürümleri](cloud-services-guestos-update-matrix.md).
