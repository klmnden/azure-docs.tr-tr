---
title: Mobile Apps ve Mobile Services istemci ve sunucu SDK'sı sürüm | Microsoft Docs
description: İstemci SDK'ları listesi ve mobil hizmetler ve Azure Mobile Apps için sunucu SDK'sı sürümlerle uyumluluk
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: cfa6a363725c35083b32d6de1dd1371777f91907
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240297"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobile Apps ve Mobile Services istemci ve sunucu sürümü oluşturma
Azure Mobile Services'ın en son sürüm **Mobile Apps** Azure App Service özelliğidir.

Mobile Apps istemci ve sunucu SDK'ları başlangıçta mobil hizmetler de bağlıdır, ancak bunlar *değil* birbiriyle uyumlu.
Diğer bir deyişle, kullanmalısınız bir *Mobile Apps* istemci SDK'sı ile bir *Mobile Apps* sunucu SDK'sı ve benzer şekilde *mobil Hizmetler*. Bu sözleşme, istemci ve sunucu SDK'ları, tarafından kullanılan özel üst bilgi değeri aracılığıyla zorlanır `ZUMO-API-VERSION`.

Not: herhangi bir zamanda bu belgenin başvurduğu bir *mobil Hizmetler* arka uç, onu gerekmeyen gerekmez mobil hizmetler üzerinde barındırılması. App Service üzerinde herhangi bir kod değişikliği çalıştırmak için bir mobil hizmet geçirmek artık mümkündür, ancak hizmeti kullanmaya devam *mobil Hizmetler* SDK sürümleri.

Herhangi bir kod değişikliği App Service'e geçirme hakkında daha fazla bilgi için [bir Azure App Service mobil hizmete geçiş] makale bakın.

## <a name="header-specification"></a>Üst bilgi belirtimi
Anahtar `ZUMO-API-VERSION` HTTP üst bilgisi veya sorgu dizesi belirtilebilir. Bir sürüm dizesi biçiminde değerdir **x.y.z**.

Örneğin:

AL https://service.azurewebsites.net/tables/TodoItem

ÜST BİLGİLER: ZUMO-API-VERSION: 2.0.0

YAYINLA https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Sürüm denetimi dışında seçim yapma
Sürüm denetimini değerini ayarlayarak dışında iyileştirilmiş **true** uygulama ayarı için **MS_SkipVersionCheck**. Bu, web.config dosyasında ya da Azure portal'ın uygulama ayarları bölümündeki belirtin.

> [!NOTE]
> Mobile Services ve Mobile Apps, özellikle çevrimdışı eşitleme, kimlik doğrulaması ve anında iletme bildirimleri alanları arasındaki davranış değişiklikleri vardır. Yalnızca sürüm denetimini Bu davranış değişiklikleri uygulamanızın işlevleri kesmediğinden emin olmak için testi Tamamla sonra dışında iyileştirilmiş.

## <a name="2.0.0"></a>Azure Mobile Apps istemci ve sunucu
### <a name="MobileAppsClients"></a> Mobil *uygulamaları* istemci SDK'ları
Sürüm denetimi tanıtılmıştır istemci SDK'sı aşağıdaki sürümleri ile başlatma için **Azure Mobile Apps**:

| İstemci Platformu | Version | Sürüm üst bilgisi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>Mobil *uygulamaları* sunucu SDK'ları
Sürüm denetimi sunucusu SDK sürümleri aşağıdaki yer almaktadır:

| Sunucu platformu | SDK | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobile Apps arka uçları davranışı
| ZUMO-API-VERSION | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| x.y.z veya Null |True |200 - TAMAM |
| Null |Belirtilen false/değil |400 - bozuk istek |
| 1.x.y |Belirtilen false/değil |400 - bozuk istek |
| 2.0.0-2.x.y |Belirtilen false/değil |200 - TAMAM |
| 3.0.0-3.x.y |Belirtilen false/değil |400 - bozuk istek |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
