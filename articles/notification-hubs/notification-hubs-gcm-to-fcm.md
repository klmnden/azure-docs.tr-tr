---
title: Azure Notification Hubs ve Google Firebase Cloud Messaging (FCM) geçiş
description: Azure Notification hubs'ı FCM geçiş Google GCM nasıl ele açıklar.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/10/2019
ms.author: jowargo
ms.openlocfilehash: 4cbfc67bc66e84b4743f3326db40872241e5d474
ms.sourcegitcommit: f24b62e352e0512dfa2897362021b42e0cb9549d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59506414"
---
# <a name="azure-notification-hubs-and-the-google-firebase-cloud-messaging-fcm-migration"></a>Azure Notification Hubs ve Google Firebase Cloud Messaging (FCM) geçiş

## <a name="current-state"></a>Geçerli durum

Ne zaman Google, geçiş Google Cloud Messaging (GCM) öğesinden için Firebase Cloud Messaging (FCM), bizim bildirimleri değişiklik uyum sağlamak için Android cihazlarına nasıl gönderdiğimiz ayarlamak zorunda gibi anında iletme hizmetlerini duyurdu.

Size sunduğumuz hizmeti arka ucuna güncelleştirildi ve ardından güncelleştirmeleri bizim API ve SDK'lar gerektiği gibi yayımlanan. Kararlılığımızın ile müşteri etkiyi en aza indirmek için mevcut GCM bildirim şemalar ile uyumluluğu korumak için karar yaptık. Bu şu anda bildirimleri FCM FCM eski modda kullanarak Android cihazları için göndermesi anlamına gelir. Sonuç olarak, FCM, yük biçimi ve yeni özellikler dahil olmak üzere doğru desteği eklemek istiyoruz. Daha uzun vadeli bir değişikliği ve geçerli geçiş mevcut uygulamaları ve SDK uyumluluk koruma üzerinde odaklanmıştır. Size bildirim doğru biçimde gönderildiğinden emin olmak ve uygulamanıza (birlikte, SDK'mız) GCM veya FCM SDK'ları kullanabilirsiniz.

Bazı müşteriler Google uyarı bildirimleri için bir GCM uç noktası kullanan uygulamalar hakkında kısa bir süre önce bir e-posta aldı. Bu yalnızca bir uyarı oluştu ve hiçbir şey bozuk – uygulamanızın Android bildirimleri işleme için hala Google'a gönderilir ve Google yine de bunları işler. GCM uç noktası açıkça kendi hizmet yapılandırmasında belirtilen bazı müşteriler, kullanım dışı uç nokta hala kullanıyordu. Biz bu aralık zaten tanımlanmış ve Google e-posta gönderildiğinde sorunu düzeltme çalışma.

Biz bu kullanım dışı bırakılan uç noktaya değiştirildi ve düzeltme dağıtılır.

## <a name="going-forward"></a>Bundan sonra

Google'nın FCM ile ilgili SSS ifadesini içeren herhangi bir şey yapmanız gerekmez. İçinde [FCM ile ilgili SSS](https://developers.google.com/cloud-messaging/faq), Google söylediğiniz "İstemci SDK'ları ve GCM belirteçleri devam süresiz olarak çalışacak şekilde. Ancak, sizin için FCM geçirmezseniz Google Play Hizmetleri Android uygulamanızda en son sürümünü hedefleyecek şekilde mümkün olmayacaktır."

Uygulamanızı GCM kitaplığı kullanıyorsa, devam edin ve FCM Kitaplığı'nda uygulamanızı yükseltmek için Google'nın yönergeleri izleyin. SDK'mız ya ile uyumlu olduğundan (bizim SDK sürümü ile güncel olduğunuz sürece) bizim tarafında uygulamanızda herhangi bir şeyi güncelleştirmeniz gerekmez.

## <a name="questions-and-answers"></a>Sorular ve yanıtlar

Bazı biz müşterilerden duydunuz sık sorulan soruların yanıtlarını şu şekildedir:

**S:** Ne kesme tarihe göre uyumlu olacak şekilde yapmak ihtiyacım (Google geçerli kesme tarihinden 29 Mayıs olduğu ve değişebilir)?

**C:** Bir şey yok. Biz varolan GCM bildirim şeması ile uyumluluk tutacaktır. GCM anahtarınızı herhangi bir GCM SDK'ları ve kitaplıkları uygulamanız tarafından kullanılan olarak normal şekilde çalışmaya devam eder.

Olursa/olduğunda yeni özelliklerden yararlanmak için FCM SDK'ları ve kitaplıkları için yükseltmeye karar, GCM anahtarınızı çalışmaya devam eder. İstediğiniz, ancak Firebase yeni bir Firebase projesi oluştururken, mevcut GCM projenize eklediğiniz olun FCM anahtarı kullanarak geçiş yapabilirsiniz. Bu, GCM SDK'ları ve kitaplıklarını kullanmaya devam edebilirsiniz uygulamanın eski sürümlerini çalıştıran müşterilere ile geriye dönük uyumluluk garanti eder.

Yeni FCM proje oluşturma ve yeni FCM gizli dizi ile Notification hubs'ı güncelleştirdikten sonra varolan GCM projesine ekleme değil yeni FCM anahtarı eski GCM bağlantı olduğundan, özelliği anında iletme bildirimleri göndermek, geçerli uygulama yüklemelerini kaybedersiniz. Proje.

**S:** Bu e-posta kullanılan eski GCM uç noktaları hakkında neden alıyorum? Yapmak ne gerekir?

**C:** Bir şey yok. Biz yeni uç noktalar için geçiş ve değişiklik gerekli olmayacak biçimde yakında tamamlanmış olacaktır. Bir şey bozuk, bizim eksik bir uç nokta, uyarı iletileri yalnızca Google'nın neden oldu.

**S:** Nasıl miyim yeni FCM SDK'ları ve kitaplıkları mevcut kullanıcıların bozmadan geçiş yapabilir?

Y: Herhangi bir zamanda yükseltin. Google mevcut GCM SDK kitaplıkları ve herhangi bir kullanımdan kaldırma henüz duyurmadı. Anında iletme bildirimleri mevcut kullanıcılarınız için bölme, emin olmak için mevcut GCM projenizle ilişkilendirerek yeni bir Firebase projesi oluşturun. Bu, gizli dizileri FCM SDK'ları ve kitaplıkları ile uygulamanızın yeni kullanıcıların yanı sıra, GCM SDK'ları ve kitaplıkları ile uygulamanızı daha eski sürümlerini çalıştıran kullanıcılar için işe yeni Firebase garanti eder.

**S:** Yeni FCM özellikleri ve şemaları Bildirimlerim için ne zaman kullanabilirim?

**C:** Bizim API ve SDK'lar, Azure.microsoft.com'a – güncelleştirme yayımlama sonra bir şey, önümüzdeki aylarda olmasını bekliyoruz.
