---
title: Azure Stack günlük ve veri işleme | Microsoft Docs
description: Azure Stack bilgileri nasıl topladığı hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2019
ms.author: PatAltimore
ms.reviewer: chengwei
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 7beb9dc31e8987d69c2cf9cf64be36caf0e0e308
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56318930"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack günlük ve müşteri verilerini işleme 
*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*  

Ölçüde Microsoft işlemci veya subprocessor kişisel verilerin Azure Stack bağlantılı olarak, Microsoft'un sunduğu tüm müşteriler için etkili 25 Mayıs 2018'den itibaren taahhüt içinde (a) "kişisel verilerin işlenmesini; GDPR"Hazırlama"Veri koruma koşulları"bölümünün [çevrimiçi hizmet koşulları](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551078818&sdata=cpWsfZTBHpqEFr50DWQOryq342U8shgeFgMXVPQz5ug%3D&reserved=0) ve (b), Avrupa Birliği genel veri koruma yönetmeliği'koşullarını ek 4 [çevrimiçi hizmet koşulları](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551088813&sdata=bv1CBiaCnYmjiv6S0dFCbWEd4fNCkPBjBwgylNa%2FNt0%3D&reserved=0). 

Azure Stack müşteri veri merkezlerinde bulunan gibi Microsoft yalnızca Microsoft ile paylaşılan verilerinin veri denetleyicisi, [tanılama](azure-stack-diagnostics.md), [Telemetri](azure-stack-telemetry.md), ve [Faturalama](azure-stack-usage-reporting.md).  

## <a name="data-access-controls"></a>Veri erişim denetimleri 
Belirli bir destek talebi araştırmak için atanan Microsoft çalışanlarının, şifrelenmiş verilere salt okunur erişim verilir. Microsoft çalışanları ayrıca gerekirse verileri silmek için kullanılan araçlara erişebilir. Tüm müşteri verilerine erişim denetlenir ve günlüğe kaydedilir.  

Veri erişimi denetimlerini:
1.  Veriler yalnızca en fazla 90 gün sonra çalışması için korunur.
2.  Müşteri, her zaman bu 90 günlük süre içinde herhangi bir zamanda kaldırılan verilerin seçeneği vardır.
3.  Microsoft çalışanları olay temelinde verilere erişim verilir ve yalnızca gerektiğinde Yardım için destek sorunu çözün. 
4.  Olay Microsoft OEM iş ortaklarıyla Müşteri verilerinin nerede paylaşmalıdır müşteri onayı zorunludur.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>Veri sahibi istekleri (DSR) denetimleri yapın müşterilerin var mı?
Daha önce bahsedildiği gibi Microsoft Müşteri istek başına isteğe bağlı veri silme destekler. Müşteriler, bizim destek mühendisi dilediğiniz zaman verileri kalıcı olarak silinmeden önce müşteri'nin, seçme, belirli bir çalışması için tüm günlükleri silmek isteyebilirsiniz.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>Microsoft, veri silindiğinde müşterilere bildirmek mu?
Otomatik veri silme eylemi için (çalışması kapattıktan sonra 90 gün) biz değil proaktif bir şekilde müşterilere ulaşın ve silme hakkında bildirim yollayın. 

İsteğe bağlı veri silme eylemi için Microsoft destek mühendisine nerede bunlar isteğe bağlı verileri silme işlemini başlatabilir ve bittiğinde onaylama telefonda müşteriyle sağlayabilirler aracına erişebilir.

## <a name="diagnostic-data"></a>Tanılama verileri
Destek işleminin bir parçası olarak, Azure Stack operatörleri için [tanılama günlükleri paylaş](azure-stack-diagnostics.md) sorun giderme sürecini kolaylaştırmak için Azure Stack desteği ve mühendislik ekipleri ile.

Bir aracı Microsoft sağlar ve müşterilerin toplamak ve karşıya yüklemek betik tanılama günlük dosyaları istendi. Toplandığında, günlük dosyalarını üzerinden aktarılır Microsoft'a şifreli bağlantı HTTPS korumalı. HTTPS kablo üzerinden şifreleme sağladığından, aktarım sırasında şifreleme için gereken parolası yoktur. Alındıktan sonra günlükleri şifrelenir ve otomatik olarak destek servis talebi kapatıldığında ettikten 90 gün sonra silinmelerinden depolanır.

## <a name="telemetry-data"></a>Telemetri verileri
[Azure Stack telemetrisini](azure-stack-telemetry.md) bağlı kullanıcı deneyimi aracılığıyla Microsoft Sistem verilerini otomatik olarak yükler. Azure Stack operatörleri, herhangi bir zamanda telemetri özellikleri ve gizlilik ayarlarını özelleştirmek için denetimleri içerir.

Microsoft, kredi kartı numaraları, kullanıcı adları ve parolalar, e-posta adresleri veya benzer hassas bilgiler gibi hassas verileri toplamak istediğiniz değil. Hassas bilgilerin yanlışlıkla alındı karar verirseniz, sileceğiz. 

## <a name="billing-data"></a>Faturalama verileri
[Azure Stack faturalama](azure-stack-usage-reporting.md) genel Azure'nın faturalama ve kullanım işlem hattı ve bu nedenle Microsoft Uyumluluk kuralları ile uyumlu yararlanır.

Azure Stack operatörlerinin Azure Stack, kullanım bilgilerini Azure'a faturalandırması iletecek şekilde yapılandırabilirsiniz. Bu,-,-kullandıkça faturalandırma modeli seçen çok düğümlü Azure Stack müşterileri için gereklidir. Kullanım raporlama alınan telemetri bağımsız olarak denetlenir ve kapasite modeli istemeyen çok düğümlü müşterilere veya Azure Stack geliştirme Seti'ni kullanıcılar gerekli değildir. Bu senaryolar için kullanım raporlamayı kullanarak kapatılabilir [kayıt betiği](azure-stack-usage-reporting.md).


## <a name="next-steps"></a>Sonraki adımlar 
[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md) 
