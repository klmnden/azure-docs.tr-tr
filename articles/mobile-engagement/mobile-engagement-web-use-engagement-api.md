---
title: Azure Mobile Engagement Web SDK API'leri | Microsoft Docs
description: En son güncelleştirmeleri ve Azure Mobile Engagement için Web SDK'sı için yordamlar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: 6d2ae75b384b60d0383c1682a00a4fc0d19d0f43
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Bir web uygulamasına Azure Mobile Engagement API kullanın
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu belge nasıl söyler belgeye bir ektir için [Mobile Engagement bir web uygulaması tümleştirme](mobile-engagement-web-integrate-engagement.md). Azure Mobile Engagement API uygulama istatistikleri rapor için nasıl kullanılacağı hakkında kapsamlı bilgi sağlar.

Mobile Engagement API'si tarafından sağlanan `engagement.agent` nesnesi. Azure Mobile Engagement Web SDK diğer adı olan varsayılan `engagement`. Bu diğer adı SDK yapılandırmasından tanımlayabilirsiniz.

## <a name="mobile-engagement-concepts"></a>Mobile Engagement kavramları
Aşağıdaki bölümleri ortak İyileştir [Mobile Engagement kavramları](mobile-engagement-concepts.md) web platformu için.

### <a name="session-and-activity"></a>`Session` ve `Activity`
Kullanıcı iki etkinlik arasında birden fazla birkaç saniye boyunca boşta kalırsa, kullanıcının etkinlikler dizisini iki ayrı oturumlara ayrılır. Bu birkaç saniye oturum zaman aşımı denir.

Web uygulamanız kullanıcı etkinlikleri sonuna tek başına bildirme değil ise (çağırarak `engagement.agent.endActivity` işlevi), Mobile Engagement sunucunun otomatik olarak kullanıcı oturumunun üç uygulama sayfası kapatıldıktan sonra dakika içinde süresi dolar. Bu sunucu oturum zaman aşımı çağrılır.

### `Crash`
Yakalanmayan JavaScript özel durumlarının otomatik raporları varsayılan olarak oluşturulmaz. Kilitlenme ancak rapor kullanarak el ile `sendCrash` (kilitlenme bildirimi bölümüne bakın) işlev.

## <a name="reporting-activities"></a>Raporlama etkinlikleri
Kullanıcı etkinliğini raporlama, bir kullanıcı yeni bir etkinlik başlatıldığında ve kullanıcı geçerli etkinliği sona erdiğinde içerir.

### <a name="user-starts-a-new-activity"></a>Kullanıcı yeni bir etkinlik başlatır
    engagement.agent.startActivity("MyUserActivity");

Çağırmanız gerekir `startActivity()` her zaman kullanıcı etkinliği değiştirir. Bu işlev ilk çağrıda yeni bir kullanıcı oturumu başlatır.

### <a name="user-ends-the-current-activity"></a>Kullanıcı geçerli etkinliği sona erer
    engagement.agent.endActivity();

Çağırmanız gerekir `endActivity()` en az bir kez kullanıcı tamamlandığında son etkinliklerini. Bu Mobile Engagement Web SDK'sı kullanıcının şu anda boş olduğunu ve kullanıcı oturumunu oturum zaman aşımı süresi dolduktan sonra kapalı olması gerektiğini bildirir. Çağırırsanız `startActivity()` oturum, yalnızca oturum zaman aşımı süresi dolmadan önce devam ettirilir.

Güvenilir arama Gezgini penceresi kapatıldığında yok olduğundan, genellikle zor veya bir web ortamı içindeki kullanıcı etkinlikleri sonuna catch mümkün değildir. İşte bu nedenle Mobile Engagement sunucunun otomatik olarak kullanıcı oturumunun üç uygulama sayfası kapatıldıktan sonra dakika içinde süresi dolar.

## <a name="reporting-events"></a>Raporlama olayları
Raporlama olayları üzerinde oturum olayları ve tek başına olayları ele alınmaktadır.

### <a name="session-events"></a>Oturum olayları
Oturum olaylar, genellikle kullanıcının oturumu sırasında bir kullanıcı tarafından gerçekleştirilen eylemleri bildirmek için kullanılır.

**Ek veriler olmadan örneği:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Örnek ek veriler ile:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Tek başına olayları
Oturum olayları, bir oturum bağlamı dışında tek başına olaylar gerçekleşebilir.

Bunun için kullanmak ``engagement.agent.sendEvent`` yerine ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Hata Raporlama
Hata raporlama, oturum hataları ve tek başına hataları ele alınmaktadır.

### <a name="session-errors"></a>Oturum hataları
Oturum hatalar genellikle bir etkisi kullanıcının oturumu sırasında kullanıcıya hatalarını bildirmek için kullanılır.

**Ek veriler olmadan örneği:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Örnek ek veriler ile:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Tek başına hataları
Oturum hatalarının aksine, bir oturum bağlamı dışında tek başına hatalar oluşabilir.

Bunun için kullanmak `engagement.agent.sendError` yerine `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Raporlama işleri
Raporlama, hataları ve bir işi sırasında meydana gelen olayları raporlama ve kilitlenme raporlama işleri kapsar.

**Örnek:**

AJAX isteği izlemek istiyorsanız, aşağıdaki kullanırsınız:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Bir işi sırasında hata raporlama
Hataları geçerli kullanıcı oturum çalışan bir işi yerine ile ilgili olabilir.

**Örnek:**

Bir AJAX isteği başarısız olursa bir hata raporu istiyorsanız:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Raporlama işi sırasında olayları
Olaylar ilgili olabileceğini yerine çalıştırılan bir iş geçerli kullanıcı oturum teşekkürler `engagement.agent.sendJobEvent` işlevi.

Bu işlev tıpkı `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Kilitlenme raporlama
Kullanım `sendCrash` rapor işleve çöküyor el ile.

`crashid` Kilitlenme türünü tanımlayan bir dize bağımsız değişkeni aşağıdaki gibidir.
`crash` Genellikle olmayan bağımsız değişken bir dize olarak kilitlenme yığın izlemesi.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Ek parametreler
Bir olay, hata, etkinlik veya iş için rasgele verileri ekleyebilirsiniz.

Veriler, herhangi bir JSON nesnesi (ancak bir dizi veya ilkel tür) olabilir.

**Örnek:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Sınırlar
Ek parametreler geçerli anahtarları, değer türleri ve boyutu için normal ifadeler alanlarda kısıtlamalardır.

#### <a name="keys"></a>Anahtarlar
Nesne tablosundaki her anahtarın şu normal ifadeyle aynı olması gerekir.

    ^[a-zA-Z][a-zA-Z_0-9]*

Anahtarları en az bir harf ile başlamalıdır Bunun anlamı arkasından harf, rakam veya alt çizgi ile (\_).

#### <a name="values"></a>Değerler
Dize, sayı ve Boolean türleri için sınırlı değerlerdir.

#### <a name="size"></a>Boyut
(Mobile Engagement Web SDK'sı, JSON'da kodlar) sonra ek özellikler çağrı başına 1024 karakterle sınırlıdır.

## <a name="reporting-application-information"></a>Uygulama bilgilerini raporlama
El ile (veya başka bir uygulamaya özgü bilgileri) izleme kullanarak raporlayabilirsiniz `sendAppInfo()` işlevi.

Bu bilgi artımlı olarak gönderilebilir unutmayın. Belirli bir aygıt için belirli bir anahtarın yalnızca en son değeri korunur.

Olay ek özellikler gibi uygulama bilgilerini soyut için herhangi bir JSON nesnesi kullanabilirsiniz. Diziler veya alt nesneler (JSON serileştirmesi kullanan) düz dize olarak davranılır unutmayın.

**Örnek:**

Kullanıcının cinsiyeti ve doğum tarihi göndermek için bir kod örneği şöyledir:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Sınırlar
Uygulama bilgilerini uygulanan anahtarları ve boyutu için normal ifadeler alanlarda kısıtlamalardır.

#### <a name="keys"></a>Anahtarlar
Nesne tablosundaki her anahtarın şu normal ifadeyle aynı olması gerekir.

    ^[a-zA-Z][a-zA-Z_0-9]*

Anahtarları en az bir harf ile başlamalıdır Bunun anlamı arkasından harf, rakam veya alt çizgi ile (\_).

#### <a name="size"></a>Boyut
(Mobile Engagement Web SDK'sı, JSON'da kodlar sonra) uygulama bilgilerini çağrı başına 1024 karakterle sınırlıdır.

Önceki örnekte, sunucuya gönderilen JSON 44 karakter olacak:

    {"birthdate":"1983-12-07","gender":"female"}
