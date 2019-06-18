---
title: Azure Application Gateway’de uçtan uca SSL’yi etkinleştirme | Microsoft Docs
description: Bu sayfada, Application Gateway uçtan uca SSL desteği için genel bir bakış sunulmuştur.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: 856f23de8a8772255f570a923ecf1708dc819bb5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831879"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Application Gateway ile uçtan uca SSL’ye genel bakış

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak arka uç sunucularıyla şifrelenmemiş iletişim, bazı müşteriler için kabul edilebilir bir seçenek değildir. Bunun şifrelenmemiş iletişimin nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

## <a name="overview"></a>Genel Bakış

Uçtan uca SSL, gizli verileri arka uca şifrelenmiş olarak güvenli bir şekilde aktarmanızı sağlar ve bu sırada Application Gateway'in sunduğu 7. Katman yük dengeleme özelliklerinin avantajlarından yararlanmaya devam eder. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Uçtan uca SSL, BackendHTTPSetting’de protokol ayarı HTTPS yapılarak etkinleştirilir. Bu ayar, daha sonra arka uç havuzuna uygulanır. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bunun gibi sertifikaların kullanılabilmesi için, önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle güvenilir listeye alınmaları gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL hakkında bilgi edindikten sonra uçtan uca SSL kullanan uygulama ağ geçidi oluşturmak için [Uygulama ağ geçidinde uçtan uca SSL'yi etkinleştirme](application-gateway-end-to-end-ssl-powershell.md) bölümüne gidin.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
