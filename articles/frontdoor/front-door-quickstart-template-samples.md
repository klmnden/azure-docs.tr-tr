---
title: Azure Resource Manager şablonu örnekleri - Azure Front Door Service | Microsoft Docs
description: Azure Front Door Service için Azure Resource Manager şablonu örnekleri
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2018
ms.author: sharadag
ms.openlocfilehash: c0dccdd38f4b945df3702178e7351db58687bf82
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883905"
---
# <a name="azure-resource-manager-deployment-model-templates-for-front-door"></a>Front Door için Azure Resource Manager dağıtım modeli şablonları

Aşağıdaki tabloda, Azure Front Door Service için Azure Resource Manager dağıtım modeli şablonlarının bağlantıları yer alır. 

| | |
| ---| ---|
| [Temel Front Door oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-basic)| Tek bir arka uçla temel Front Door yapılandırması oluşturur. |
| [Birden çok arka uçla, arka uç havuzuyla ve URL tabanlı yönlendirmeyle Front Door oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-multiple-backends)| Arka uç havuzundaki birden çok arka uç için ve URL yolu temelinde arka uç havuzları arasında yük dengeleme yapılandırmasına sahip bir Front Door oluşturur. |
| [Front Door ile HTTPS (Front Door yönetilen sertifikası) protokolüyle özel bir etki alanı ekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-custom-domain)| Front Door'unuza özel bir etki alanı ekleyin ve DigiCert yoluyla oluşturulan bir Front Door yönetilen sertifikasıyla bu etki alanı için HTTPS trafiğini etkinleştirin. |
| [Coğrafi filtreleme ile ön kapısı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering)| Belirli ülkelerden gelen trafiğe izin veren/engelleyen bir Front Door oluşturun. |
| [Front Door'da arka uçlarınız için Durum Yoklamalarını denetleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-health-probes)| Yoklama yolunu ve ayrıca yoklamaların gönderileceği zaman aralıklarını güncelleştirerek durum yoklaması ayarlarını değiştirmek için Front Door'unuzu güncelleştirin. |
| [Etkin/Beklemede arka uç yapılandırmasıyla Front Door oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-priority-lb)| Etkin/Beklemede uygulama topolojisi için önceliklere dayalı yönlendirme gösteren, başka bir deyişle varsayılan olarak tüm trafiği birincil (en yüksek öncelikli) arka uca gönderen ve arka uç kullanılamaz duruma gelene kadar buna devam eden bir Front Door oluşturur. |
| [Belirli yollar için önbelleğin etkinleştirildiği bir Front Door oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-create-caching)| Tanımlanan yönlendirme yapılandırması için önbelleğin etkinleştirildiği, böylelikle iş yükünüz için tüm statik varlıkların önbelleğe alındığı bir Front Door oluşturur. |
| [Front Door konak adlarınız için Oturum Benzeşimi'ni yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-session-affinity) | Ön uç konağınız için oturum benzeşimini etkinleştirmek, böylelikle aynı kullanıcı oturumundan daha sonra gelen trafiği aynı arka ucu göndermek üzere Front Door'u güncelleştirir. |
| [Front Door'unuz için özel SSL sertifikasıyla (HTTPS için) özel etki alanı ekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-custom-byoc) | Özel SSL sertifikasıyla Front Door için özel bir etki alanı ekler; buna Kendi Sertifikanızı Getirin senaryosu da denir. |
| [Front Door'u IP beyaz listesi veya kara listesi için yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)| Front Door'u istemci IP'lerini kullanıp özel erişim denetiminden yararlanarak belirli istemci IP'lerinin trafiğini kısıtlayacak şekilde yapılandırır. |
| [Belirli http parametrelere sahip eylem için ön kapı yapılandırın](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-http-params)| Front Door'u http parametrelerini kullanıp erişim denetimi için özel kurallardan yararlanarak gelen istekteki http parametreleri temelinde belirli trafiklere izin verecek veya engelleyecek şekilde yapılandırır. |
| [Ön kapısı, oran sınırlandırma yapılandırın](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-rate-limiting)| Front Door'u belirli bir ön uç konağı için gelen trafiğin hızını sınırlandıracak şekilde yapılandırır. |
| | |

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.