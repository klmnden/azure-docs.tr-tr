---
title: "Application Gateway’de SSL İlkesi ve uçtan uca SSL’yı etkinleştirme | Microsoft Belgeleri"
description: "Bu sayfada, Application Gateway uçtan uca SSL desteği için genel bir bakış sunulmuştur."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: cb2b7bc626294e12c6e19647c1e787e1f671595b
ms.openlocfilehash: a49a93b11ab3e965ac1ddaec919bfcbf43381dee


---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Application Gateway’de SSL İlkesi ve uçtan uca SSL’yi etkinleştirme

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme/şifre çözme ek yükünden kurtulmasını sağlar. Ancak arka uç sunucularıyla şifrelenmemiş iletişim, bazı müşteriler için kabul edilebilir bir seçenek değildir. Bunun şifrelenmemiş iletişimin nedeni, güvenlik/uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için artık uçtan uca SSL şifrelemesini desteklemektedir.

## <a name="overview"></a>Genel Bakış

Uçtan uca SSL, gizli verileri arka uca şifrelenmiş olarak güvenli bir şekilde aktarmanızı sağlar ve bu sırada Application Gateway'in sunduğu 7. Katman yük dengeleme özelliklerinin avantajlarından yararlanmaya devam eder. Bu özelliklerin bazıları tanımlama bilgisi benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Uçtan uca SSL, BackendHTTPSetting’de protokol ayarı Https yapılarak etkinleştirilir. Bu ayar, daha sonra arka uç havuzuna uygulanır. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bunun gibi sertifikaların kullanılabilmesi için, önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle güvenilir listeye alınmaları gerekir.

## <a name="application-gateway-ssl-policy"></a>Application Gateway SSL İlkesi

Application Gateway, kullanıcı tarafından yapılandırılabilen SSL anlaşma ilkelerini destekler. Bu ilkeler, müşterilere uygulama ağ geçidindeki SSL bağlantıları üzerinde daha büyük denetim sağlar.

1. SSL 2.0 ve 3.0, tüm Application Gateway'ler için varsayılan olarak devre dışı bırakılır. Bunlar tamamen yapılandırılabilir değildir.
2. SSL ilkesi tanımı, şu üç protokolden herhangi birini devre dışı bırakma seçeneği sunar: **TLSv1\_0**, **TLSv1\_1**, **TLSv1\_2**.
3. Hiçbir SSL ilkesi tanımlanmazsa üçü de (TLSv1\_0, TLSv1\_1, TLSv1_2) etkinleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL ve SSL ilkesi hakkında bilgi edindikten sonra uçtan uca SSL kullanan uygulama ağ geçidi oluşturmak için [Uygulama ağ geçidinde uçtan uca SSL'yi etkinleştirme](application-gateway-end-to-end-ssl-powershell.md) bölümüne gidin.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png



<!--HONumber=Dec16_HO2-->


