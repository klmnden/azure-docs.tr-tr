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
ms.openlocfilehash: 56c5e0582afe55dcd63aa056817898d3d4942419
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859082"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobile Apps ve Mobile Services istemci ve sunucu sürümü oluşturma
Azure Mobile Services'ın en son sürüm **Mobile Apps** Azure App Service özelliğidir.

Mobile Apps istemci ve sunucu SDK'ları başlangıçta mobil hizmetler de bağlıdır, ancak bunlar *değil* birbiriyle uyumlu.
Diğer bir deyişle, kullanmalısınız bir *Mobile Apps* istemci SDK'sı ile bir *Mobile Apps* sunucu SDK'sı ve benzer şekilde *mobil Hizmetler*. Bu sözleşme, istemci ve sunucu SDK'ları, tarafından kullanılan özel üst bilgi değeri aracılığıyla zorlanır `ZUMO-API-VERSION`.

Not: herhangi bir zamanda bu belgenin başvurduğu bir *mobil Hizmetler* arka uç, onu gerekmeyen gerekmez mobil hizmetler üzerinde barındırılması. App Service üzerinde herhangi bir kod değişikliği çalıştırmak için bir mobil hizmet geçirmek artık mümkündür, ancak hizmeti kullanmaya devam *mobil Hizmetler* SDK sürümleri.

Herhangi bir kod değişikliği App Service'e geçirme hakkında daha fazla bilgi için bkz [Bir mobil hizmet, Azure App Service'e geçirme].

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
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Tüm sürümler için Uyumluluk özeti
Aşağıdaki grafik, tüm istemci ve sunucu türleri arasındaki uyumluluk gösterir. Bir arka uç ya da mobil sınıflandırılır **Hizmetleri** ya da mobil **uygulamaları** sunucuya göre kullandığı SDK'sı.

|  | **Mobil Hizmetler** Node.js veya .NET | **Mobil uygulamalar** Node.js veya .NET |
| --- | --- | --- |
| [Mobil hizmetler istemciler] |Tamam |Hata\* |
| [Mobile Apps istemciler] |Hata\* |Tamam |

\*Bu belirterek denetlenebilir **MS_SkipVersionCheck**.

<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is https://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobil hizmetler istemci ve sunucu
Aşağıdaki tabloda istemci SDK'ları ile uyumludur **mobil Hizmetler**.

Not: Mobil hizmetler istemci SDK'ları *olmayan* bir üstbilgi değerini göndermek `ZUMO-API-VERSION`. Hizmet bu üst bilgi veya sorgu dizesi değerini alırsa açıkça yukarıda açıklanan şekilde dışarı bayraklarımızın sürece bir hata döndürülür.

### <a name="MobileServicesClients"></a> Mobil *Hizmetleri* istemci SDK'ları
| İstemci Platformu | Version | Sürüm üst bilgisi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |yok |
| iOS |[2.2.2](https://aka.ms/gc6fex) |yok |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |yok |
| HTML |[1.2.7](https://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |yok |

### <a name="mobile-services-server-sdks"></a>Mobil *Hizmetleri* sunucu SDK'ları
| Sunucu platformu | Version | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[WindowsAzure.MobileServices.Backend.* sürüm 1.0.x kullanılır](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |**Sürüm üst bilgisi yok** |
| Node.js |(çok yakında) |**Sürüm üst bilgisi yok** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Mobil hizmetler arka uçları davranışı
| ZUMO-API-VERSION | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| Belirtilmedi |Herhangi biri |200 - TAMAM |
| Herhangi bir değer |True |200 - TAMAM |
| Herhangi bir değer |Belirtilen false/değil |400 - bozuk istek |

## <a name="2.0.0"></a>Azure Mobile Apps istemci ve sunucu
### <a name="MobileAppsClients"></a> Mobil *uygulamaları* istemci SDK'ları
Sürüm denetimi tanıtılmıştır istemci SDK'sı aşağıdaki sürümleri ile başlatma için **Azure Mobile Apps**:

| İstemci Platformu | Version | Sürüm üst bilgisi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

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

## <a name="next-steps"></a>Sonraki Adımlar
* [Bir mobil hizmet, Azure App Service'e geçirme]

[Mobil hizmetler istemciler]: #MobileServicesClients
[Mobile Apps istemciler]: #MobileAppsClients


[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
[Bir mobil hizmet, Azure App Service'e geçirme]: app-service-mobile-migrating-from-mobile-services.md
