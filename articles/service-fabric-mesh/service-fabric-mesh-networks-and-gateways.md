---
title: Azure Service Fabric ağ giriş | Microsoft Docs
description: Ağların, ağ geçitleri ve Service Fabric Mesh içinde akıllı trafik yönlendirme hakkında bilgi edinin.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/26/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: b0e1047c5bbd7d8caaf2afd8b002be1c46837852
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811055"
---
# <a name="introduction-to-networking-in-service-fabric-mesh-applications"></a>Service Fabric Mesh uygulamalarda ağ giriş
Bu makalede, yük Dengeleyiciler farklı türleri, ağ ağ geçitleri ile uygulamalarınızı diğer ağlara nasıl bağlanacağını ve uygulamalarınızda Hizmetleri arasındaki trafik nasıl yönlendirileceğini açıklar.

## <a name="layer-4-vs-layer-7-load-balancers"></a>Katman 4 vs katman 7 yük Dengeleyiciler
Farklı katmanlarda, Yük Dengeleme yapılabilir [OSI modeli](https://en.wikipedia.org/wiki/OSI_model) ağ genellikle, 4 (L4) ve katman 7 (L7) katman.  Genellikle, yük Dengeleyiciler iki tür vardır:

- L4 bir yük dengeleyici ile hiçbir şekilde içerik paketlerinin paketlerin teslim ile ilgilidir ağ aktarım katmanında çalışır. Dengeleme ölçütü için IP adresleri ve bağlantı noktaları sınırlı yalnızca paket üst bilgilerini yük dengeleyici tarafından incelenir. Örneğin, bir istemci, yük dengeleyici için TCP bağlantısı sağlar. Yük Dengeleyici (doğrudan SYN için yanıt vererek) bağlantıyı sonlandırır, bir arka uç seçer ve (yeni SYN gönderir) arka uç için yeni bir TCP bağlantı kurar. L4 bir yük dengeleyici genellikle yalnızca L4 TCP/UDP bağlantı veya oturumun düzeyinde çalışır. Böylece Yük Dengeleme bayt geçici olarak yeniden yönlendirir ve bayt aynı oturumundan aynı arka uç Rüzgar emin olmanızı sağlar. L4 yük dengeleyici, taşıma bayt tüm uygulama ayrıntılarını farkında değil. Herhangi bir uygulama protokolü bayt olabilir.

- Bir L7 yük dengeleyici, her paket içerik oluşturulmasıyla uygulama katmanında çalışır. HTTP, HTTPS ve WebSockets gibi protokoller anlayan çünkü paket içeriği olup olmadığını denetler. Bu yük dengeleyici Gelişmiş yönlendirme gerçekleştirmenize olanak sağlar. Örneğin, bir istemci, yük dengeleyici tek bir HTTP/2 TCP bağlantı sağlar. Yük Dengeleyici, ardından iki arka uç bağlantı devam eder. İstemci, yük dengeleyiciye iki HTTP/2 akışları gönderdiğinde, bir akış bir arka uca gönderilen ve akış iki arka uç iki gönderilir. Bu nedenle, hatta çok farklı istek yükü olan istemciler çoğullama verimli bir şekilde arka uçlar arasında dengelenir. 

## <a name="networks-and-gateways"></a>Ağlar ve ağ geçitleri
İçinde [Service Fabric kaynak modeli](service-fabric-mesh-service-fabric-resources.md), bir ağ kaynak, bağımsız olarak, bağımlılık olarak başvurduğu bir uygulama veya hizmet kaynak ayrı ayrı dağıtılabilen bir kaynak değildir. İnternet'e açık olan ağ uygulamaları için oluşturmak üzere kullanılır. Birden çok farklı uygulamalar hizmetlerinden aynı ağın bir parçası olabilir. Bu özel ağ oluşturulur ve Service Fabric tarafından yönetilen ve bir Azure sanal ağı (VNET) değil. Uygulamaları dinamik olarak eklenebilir ve etkinleştirmek ve sanal AĞA bağlantı devre dışı bırakmak için ağ kaynağı kaldırıldı. 

Bir ağ geçidi, iki ağ arasında köprü kuracak şekilde kullanılır. Ağ geçidi kaynağı dağıtan bir [Envoy proxy](https://www.envoyproxy.io/) L4 yönlendirme için herhangi bir iletişim kuralı ve L7 Gelişmiş HTTP (S) uygulama yönlendirme için yönlendirme sağlar. Ağ geçidi, ağınızda bir dış ağ trafiği yönlendirir ve trafiği yönlendirmek için hizmet belirler.  Dış ağ, (temelde, genel internet) açık bir ağ veya, diğer Azure uygulamalarınızın ve kaynaklarınızın ile bağlanmanıza olanak tanıyan bir Azure sanal ağı olabilir. 

![Ağ ve ağ geçidi][Image1]

İle ağ kaynak oluşturulduğunda `ingressConfig`, genel bir IP ağ kaynağına atanır. Genel IP için ağ kaynağı ömrünü bağlanır.

Mesh uygulaması oluştururken, mevcut bir ağ kaynağına başvurmalıdır. Yeni genel bağlantı noktaları eklenebilir veya var olan bağlantı noktaları giriş yapılandırmasından kaldırılabilir. Uygulama kaynağı kendisine başvuran bir ağ kaynağı için bir silme başarısız olur. Uygulama silindiğinde, ağ kaynağı kaldırıldı.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-networks-and-gateways/NetworkAndGateway.png