---
title: "Mobil katılım verme API genel bakış"
description: "Kendi araçlarında yararlanmak için kullanıcının cihazları tarafından oluşturulan ham verilerinizi dışarı aktarma hakkında temel bilgileri öğrenin"
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: 346e0e480ff84ee849f135a7605d27df9e32f966
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Mobil katılım verme API genel bakış
## <a name="introduction"></a>Giriş
Bu belgede, kendi araçlarında yararlanmak için kullanıcının cihazları tarafından oluşturulan ham verilerinizi dışarı aktarma hakkında temel bilgileri öğreneceksiniz.

## <a name="pre-requisites"></a>Ön koşullar
Mobile Engagement'tan ham verileri dışarı aktarma gerektirir:

* API'lerini kullanabilmek için API kimlik doğrulama kurulumu (bkz [kimlik doğrulama el ile kuruluma](mobile-engagement-api-authentication-manual.md)),
* REST API'lerini kullanma veya [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Bir Azure depolama hesabı.

> [!NOTE]
> Biz de mükemmel bildirmek [Microsoft Azure Storage Gezgini](http://storageexplorer.com/), en az geliştirme aşaması sırasında kullanımı kolay bir kullanıcı Arabirimi Azure Storage ile etkileşmek için sağlar.
> 
> 

## <a name="what-can-be-exported"></a>Ne verilebilir mi?
Mobile Engagement türlerde veri toplamak, kullanıcıların sağlar ve bu nedenle bu farklı veri türleri için uygun verme çeşitli türlerde sahiptir.
Dışarı aktarma 2 temel türleri şunlardır:

* Anlık görüntü: kullanılan genellikle verilerini dışarı aktarmak için bir durumu temsil eden ve Mobile Engagement geçmişi yok. Bu etiketler (app-info) belirteçleri veya itme kampanya geribildirimler örneğin içerir. Sonuç olarak dışarı aktarma bu tür bir tarihe ilgili değildir.
* Geçmiş: Bu dışarı aktarma türü olayları veya etkinlikleri gibi zamana örneğin öğelerden veriler için kullanılır.

Aşağıdaki tabloda ayrıntısına tüm olası dışarı aktarmaları açıklanmaktadır:

| Dışarı aktarma türü | Veri türü | Açıklama |
| --- | --- | --- |
| Anlık Görüntü |İstek bildirimi |Anında iletme kampanyalarını geribildirimler başına DeviceID/UserID temelinde verilmesini oluşturur |
| Anlık Görüntü |Etiket |Her bir cihaza ilişkili etiketleri (app-info) verilmesini oluşturur |
| Anlık Görüntü |Cihaz |Verme (modeli, yerel ayar, saat dilimi,...) technicals, etiketler, ilk görülen süresi gibi cihazlara ilişkin verileri çoğunu oluşturur... |
| Anlık Görüntü |Belirteç |Tüm geçerli belirteçleri verme oluşturur |
| Geçmiş |Etkinlik |Belirli bir süre üzerinde her cihazlar için tüm etkinlikleri verme oluşturur |
| Geçmiş |Olay |Belirli bir süre üzerinde her cihazlar için tüm etkinlikleri verme oluşturur |
| Geçmiş |İş |Belirli bir süre üzerinde her cihazlar için tüm işleri verme oluşturur |
| Geçmiş |Hata |Her cihaz için tüm hataların bir dışarı aktarma üzerinde belirli bir süre oluşturur |

## <a name="how-does-it-work"></a>Nasıl çalışır?
Dışarı aktarma uzun çalışan görevler büyük veri dosyalarını oluşturabilir. Bu nedenle bunlar hemen indirmek için bir dosya geri dönmek için çağrılamaz.
Mobile Engagement'tan verileri dışarı aktarmak için oluşturmanız gerekecektir bir **dışarı aktarma işi** genellikle belirlediğiniz API aracılığıyla:

* Dışarı aktarma (anlık görüntü veya geçmiş) türü
* Veri türü
* **Azure depolama kapsayıcısının** (yazma erişimi olan geçerli bir SAS dahil) verme sonucunu yazılacağı.
* Örneğin örnek kapsayıcı URL parametresi https://[StorageAccountName].blob.core.windows.net/[ContainerName olacaktır]? [SASWritePermissionsToken]  

Gerçek dünya örneği aşağıda verilmiştir. https://testazmeexport.BLOB.Core.Windows.NET/test1234azme?sv=2015-12-11&SS=b&SRT=SCO&SP=rwdlac&SE=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr https = & SIG = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3B

Lütfen işinizi başlatılması birkaç dakika sürebilir ve ardından, birkaç saniye küçük uygulamalar için çok fazla kullanıcı ya da etkinliği olan uygulamalar için birkaç saat çalıştırın unutmayın.

İşi oluşturulduktan sonra hala çalışıyor veya tamamlanmış olan görmek için durumunu denetlemek mümkündür.

İş başarılı sonra sonuçta ortaya çıkan veri dosyası sağlanan depolama kapsayıcısı üzerinde kullanılabilir.

