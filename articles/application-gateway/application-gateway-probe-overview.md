---
title: "Azure uygulama ağ geçidi için genel bakış izleme sistem durumu | Microsoft Docs"
description: "Azure uygulama ağ geçidi izleme özellikleri hakkında bilgi edinin"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: davidmu
ms.openlocfilehash: 83a0b1be1aba48146aa1aaedb36ad9d9d23f17d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Uygulama ağ geçidi sistem durumu izlemeye genel bakış

Varsayılan olarak Azure uygulama ağ geçidi arka uç havuzundaki tüm kaynakların durumunu izler ve herhangi bir kaynak havuzundan sağlıksız kabul otomatik olarak kaldırır. Uygulama ağ geçidi düzgün çalışmayan örnekleri izlemek devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmalarının yanıt sonra bunları sağlıklı arka uç havuzuna ekler. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir. Bu yapılandırma, araştırma müşteriler arka ucuna bağlanmak için kullanılmasını aynı bağlantı noktasını test ediyor sağlar.

![Uygulama ağ geçidi araştırma örneği][1]

Varsayılan araştırma sistem durumu izleme kullanmanın yanı sıra, uygulamanızın gereksinimlerine uyacak şekilde durumu araştırması de özelleştirebilirsiniz. Bu makalede, hem varsayılan hem de özel sistem durumu araştırmalarının ele alınmıştır.

> [!NOTE]
> Uygulama ağ geçidi alt ağı üzerinde bir NSG varsa, bağlantı noktası aralıkları 65503 65534 uygulama ağ geçidi alt ağı gelen trafik için açılmalıdır. Bu bağlantı noktaları, arka uç sistem çalışmak için API için gereklidir.

## <a name="default-health-probe"></a>Varsayılan durumu araştırması

Herhangi bir özel araştırma yapılandırma ayarladığınızda olmayan bir uygulama ağ geçidi varsayılan durumu araştırması otomatik olarak yapılandırır. İzleme davranışı, arka uç havuzu için yapılandırılmış IP adresleri için bir HTTP isteği yaparak çalışır. Arka uç http ayarları HTTPS için yapılandırılmış olması halinde varsayılan araştırmalar için araştırma HTTPS de arka uçlarını durumunu sınamak için kullanır.

Örneğin: bağlantı noktası 80 üzerinde HTTP ağ trafiği almak için arka uç sunucu A, B ve C kullanmak için uygulama ağ geçidi yapılandırması. Varsayılan sistem durumu izleme üç sunucu her 30 saniyede sağlıklı bir HTTP yanıtı için sınar. Sağlıklı bir HTTP yanıtı sahip bir [durum kodu](https://msdn.microsoft.com/library/aa287675.aspx) 200 399 arasındaki.

Sunucu A varsayılan araştırma denetimi başarısız olursa, isteğe bağlı olarak uygulama ağ geçidi, arka uç havuzundan kaldırır ve bu sunucuya akışı ağ trafiğini durur. Varsayılan araştırmasını her 30 saniyede bir sunucu için denetlemek hala devam eder. Sunucu, bir isteği başarıyla varsayılan durumu araştırması yanıt verir. Bu geri sağlıklı arka uç havuzuna eklenir ve sunucuya yeniden akan trafik başlatır.

### <a name="default-health-probe-settings"></a>Varsayılan durumu araştırma ayarları

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Araştırma URL'si |http://127.0.0.1:\<bağlantı noktası\>/ |URL yolu |
| aralığı |30 |Saniye cinsinden yoklama aralığı |
| Zaman aşımı |30 |Zaman aşımı saniye cinsinden araştırma |
| Sağlıksız durum eşiği. |3 |Yeniden deneme sayısı araştırma. Sağlıksız eşik arka arkaya araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenmiş. |

> [!NOTE]
> Bağlantı uç HTTP ayarları aynı bağlantı noktasıdır.

Varsayılan araştırma yalnızca http://127.0.0.1 arar:\<bağlantı noktası\> sistem durumunu belirlemek için. Bir özel URL'ye Git veya diğer herhangi bir ayarı değiştirmek için sistem durumu araştırma yapılandırmanız gerekiyorsa, aşağıdaki adımlarda açıklandığı gibi özel araştırmalara kullanmanız gerekir:

## <a name="custom-health-probe"></a>Özel durum araştırması

Özel araştırmalara sistem durumu izleme üzerinde daha ayrıntılı bir denetim olmasına izin verir. Özel araştırmalara kullanırken, yoklama aralığı, URL ve test etmek için yolu ve arka uç havuzu örneği sağlıksız olarak işaretleme önce kabul etmek için kaç tane başarısız yanıtları yapılandırabilirsiniz.

### <a name="custom-health-probe-settings"></a>Özel durum yoklama ayarları

Aşağıdaki tabloda bir özel durum araştırması özelliklerini için tanımları sağlar.

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırma adı. Bu ad, arka uç HTTP Ayarları'nda araştırma başvurmak için kullanılır. |
| Protokol |Araştırma göndermek için kullanılan protokol. Araştırma arka uç HTTP Ayarları'nda tanımlanan protokolünü kullanır |
| Host |Araştırma göndermek için ana bilgisayar adı. Geçerli yalnızca çok siteli uygulama ağ geçidi üzerinde yapılandırılmış, aksi takdirde '127.0.0.1' kullanın. Bu değer, VM ana bilgisayar adından farklıdır. |
| Yol |Araştırma göreli yolu. Geçerli yol başlar '/'. |
| aralığı |Aralığı saniye cinsinden araştırma. Bu değer arasında iki ardışık araştırmalar zaman aralığıdır. |
| Zaman aşımı |Zaman aşımı saniye cinsinden araştırma. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmazsa, araştırma başarısız olarak işaretlenir.  |
| Sağlıksız durum eşiği. |Yeniden deneme sayısı araştırma. Sağlıksız eşik arka arkaya araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenmiş. |

> [!IMPORTANT]
> Uygulama ağ geçidi tek bir site için yapılandırılmışsa, varsayılan olarak ana bilgisayar adı '127.0.0.1' içinde özel araştırma aksi belirtilmedikçe belirtilmesi gerekir.
> Özel bir araştırma gönderilir başvurusunu \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>. Kullanılan bağlantı noktası aynı bağlantı noktasını arka uç HTTP Ayarları'nda tanımlanan gibi olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama ağ geçidi durumunu izleme hakkında daha fazla öğrenme sonra yapılandırdığınız bir [özel durumu araştırması](application-gateway-create-probe-portal.md) Azure portalında veya [özel durumu araştırması](application-gateway-create-probe-ps.md) PowerShell ve Azure Resource Manager kullanarak dağıtım modeli.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
