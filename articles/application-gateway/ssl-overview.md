---
title: Azure Application Gateway'de uçtan uca SSL'yi etkinleştirme
description: Bu makalede, Application Gateway uçtan uca SSL desteği bir genel bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 8/6/2018
ms.author: amsriva
ms.openlocfilehash: 4575bed18697a5661d58dc350c24a9497f7c46ff
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578822"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Application Gateway ile uçtan uca SSL’ye genel bakış

Application Gateway, arka uç sunucularına şifrelenmemiş olarak genellikle hangi trafik akışları sonra geçidinde SSL sonlandırmayı destekler. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak arka uç sunucularıyla şifrelenmemiş iletişim, bazı müşteriler için kabul edilebilir bir seçenek değildir. Bunun şifrelenmemiş iletişimin nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

Uçtan uca SSL, gizli verileri arka uca şifrelenmiş olarak güvenli bir şekilde aktarmanızı sağlar ve bu sırada Application Gateway'in sunduğu 7. Katman yük dengeleme özelliklerinin avantajlarından yararlanmaya devam eder. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Uçtan uca SSL etkin protokolünü ayarlayarak **Backendhttpsetting'de** HTTPS için ardından uygulandığı bir arka uç havuzuna. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bunun gibi sertifikaların kullanılabilmesi için önce önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle Güvenilenler listesinde olmalıdır.

> [!NOTE]
> Kimlik doğrulama sertifikası Kurulumu güvenilir, Azure Web Apps gibi Azure Hizmetleri için gerekli değildir.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL hakkında bilgi edindikten sonra uçtan uca SSL kullanarak uygulama ağ geçidi oluşturmak için [Azure portal kullanarak SSL sonlandırmayla uygulama ağ geçidi yapılandırma](create-ssl-portal.md) bölümüne gidin.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
