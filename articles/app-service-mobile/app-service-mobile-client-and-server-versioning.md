---
title: "İstemci ve sunucu SDK sürüm mobil uygulamaları ve Mobile Services | Microsoft Docs"
description: "İstemci SDK'ları listesi ve Mobile Services ve Azure mobil uygulamalar sunucusu SDK sürümleriyle uyumluluk"
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 37bf36af535eb9b5c8b0ba38434b71f1a6686811
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Mobil uygulamaları ve Mobile Services istemci ve sunucu sürüm oluşturma
Azure Mobile Services en son sürümü **Mobile Apps** Azure uygulama hizmeti özelliğidir.

Mobile Apps istemci ve sunucu SDK başlangıçta Mobile Services de dayanır, ancak bunlar *değil* birbiriyle uyumlu.
Diğer bir deyişle, kullanmanız gereken bir *Mobile Apps* istemci SDK'sı ile bir *Mobile Apps* sunucusu SDK ve benzer şekilde *Mobile Services*. Bu sözleşme istemci ve sunucu SDK'ları, tarafından kullanılan bir özel üstbilgi değeri aracılığıyla zorlanır `ZUMO-API-VERSION`.

Not: her bu belgenin başvurduğu bir *Mobile Services* arka uç, onu mutlaka gerekmez Mobile Services üzerinde barındırılması. Kod değişiklikleri uygulama hizmeti çalıştırmak için bir mobil hizmet geçirmek şimdi mümkündür, ancak hizmet kullanmaya devam *Mobile Services* SDK sürümleri.

Kod değişiklikleri App Service'e geçirme hakkında daha fazla bilgi için bkz [mobil hizmet Azure App Service'e geçirme].

## <a name="header-specification"></a>Üstbilgi belirtimi
Anahtar `ZUMO-API-VERSION` HTTP üstbilgisi veya sorgu dizesi olarak belirtilebilir. Formunda bir sürüm dizesi değeridir **x.y.z**.

Örneğin:

Https://Service.azurewebsites.NET/Tables/TodoItem Al

ÜSTBİLGİLERİ: ZUMO-API-VERSION: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Sürüm denetimi dışında kullanmama
Sürüm değerine ayarlayarak denetimini dışında seçebilirsiniz **true** uygulama ayarı için **MS_SkipVersionCheck**. Bu, web.config dosyasında ya da Azure portal'ın uygulama ayarları bölümündeki belirtin.

> [!NOTE]
> Mobile Services ve özellikle de çevrimdışı eşitleme, kimlik doğrulaması ve anında iletme bildirimleri alanlarında Mobile Apps arasındaki davranış değişiklikleri mevcuttur. Yalnızca sürüm bu davranış değişiklikleri uygulamanızın işlevselliği bozmadığını emin olmak için testi Tamamla sonra denetimini dışında tercih.
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Tüm sürümler için Uyumluluk özeti
Tüm istemci ve sunucu türleri arasındaki uyumluluk grafik gösterir. Bir arka uç ya da mobil sınıflandırılır **Hizmetleri** ya da mobil **uygulamaları** kullandığı SDK sunucuya göre.

|  | **Mobil Hizmetler** Node.js veya .NET | **Mobil uygulamaları** Node.js veya .NET |
| --- | --- | --- |
| [Mobile Services istemcileri] |Tamam |Hata\* |
| [Mobile Apps istemcileri] |Hata\* |Tamam |

\*Bu belirterek denetlenebilir **MS_SkipVersionCheck**.

<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile Services İstemcisi ve sunucusu
Aşağıdaki tabloda istemci SDK'ları ile uyumlu **Mobile Services**.

Not: Mobile Services istemci SDK'ları *sağlamadığı* bir üstbilgi değeri göndermek `ZUMO-API-VERSION`. Hizmet bu üst bilgi veya sorgu dizesi değerini alırsa açıkça yukarıda açıklandığı gibi out çevirdiniz sürece bir hata döndürülür.

### <a name="MobileServicesClients"></a>Mobil *Hizmetleri* istemci SDK'ları
| İstemci Platformu | Sürüm | Version üstbilgi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |yok |
| iOS |[2.2.2](http://aka.ms/gc6fex) |yok |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |yok |
| HTML |[1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |yok |

### <a name="mobile-services-server-sdks"></a>Mobil *Hizmetleri* sunucusu SDK
| Sunucu platformu | Sürüm | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[WindowsAzure.MobileServices.Backend.* sürüm 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |** Hiçbir sürüm üst bilgisi ** |
| Node.js |(yakında) |**Hiçbir sürüm üst bilgisi** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Mobile Services arka uçlarını davranışı
| ZUMO-API-VERSION | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| Belirtilmedi |Herhangi biri |200 - TAMAM |
| Herhangi bir değer |True |200 - TAMAM |
| Herhangi bir değer |Belirtilen false/değil |400 - Hatalı istek |

## <a name="2.0.0"></a>Azure Mobile Apps istemci ve sunucu
### <a name="MobileAppsClients"></a>Mobil *uygulamaları* istemci SDK'ları
Sürüm denetimi sunulmuştur istemci SDK aşağıdaki sürümleriyle başlangıç için **Azure Mobile Apps**:

| İstemci Platformu | Sürüm | Version üstbilgi değeri |
| --- | --- | --- |
| Yönetilen istemci (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobil *uygulamaları* sunucusu SDK
Sürüm denetimi server SDK sürümleri aşağıdaki eklenmiştir:

| Sunucu platformu | SDK | Kabul edilen sürüm üst bilgisi |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure-mobile-uygulamalar)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobile Apps arka uçlarını davranışı
| ZUMO-API-VERSION | MS_SkipVersionCheck değeri | Yanıt |
| --- | --- | --- |
| x.y.z ya da Null |True |200 - TAMAM |
| Null |Belirtilen false/değil |400 - Hatalı istek |
| 1.x.y |Belirtilen false/değil |400 - Hatalı istek |
| 2.0.0-2.x.y |Belirtilen false/değil |200 - TAMAM |
| 3.0.0-3.x.y |Belirtilen false/değil |400 - Hatalı istek |

## <a name="next-steps"></a>Sonraki Adımlar
* [mobil hizmet Azure App Service'e geçirme]

[Mobile Services istemcileri]: #MobileServicesClients
[Mobile Apps istemcileri]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[mobil hizmet Azure App Service'e geçirme]: app-service-mobile-migrating-from-mobile-services.md
