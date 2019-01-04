---
title: Azure Application Gateway'de uçtan uca SSL'yi etkinleştirme
description: Bu makalede, Application Gateway uçtan uca SSL desteği bir genel bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2018
ms.author: amsriva
ms.openlocfilehash: fcb49f532d5dfcd340baf017bd55c69d4e81e0e6
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630691"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Application Gateway ile uçtan uca SSL’ye genel bakış

Application Gateway, arka uç sunucularına şifrelenmemiş olarak genellikle hangi trafik akışları sonra geçidinde SSL sonlandırmayı destekler. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak arka uç sunucularıyla şifrelenmemiş iletişim, bazı müşteriler için kabul edilebilir bir seçenek değildir. Bunun şifrelenmemiş iletişimin nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

Uçtan uca SSL, gizli verileri arka uca hala application Gateway'in sunduğu 7. Katman Yük Dengeleme özelliklerinin avantajlarından alma sağlarken şifrelenmiş güvenli bir şekilde aktarmanıza olanak sağlar. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Uçtan uca SSL etkin protokolünü ayarlayarak **Backendhttpsetting'de** HTTPS için ardından uygulandığı bir arka uç havuzuna. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bunun gibi sertifikaların kullanılabilmesi için önce önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle Güvenilenler listesinde olmalıdır.

> [!NOTE]
> Kimlik doğrulama sertifikası Kurulumu güvenilir, Azure App Service gibi Azure Hizmetleri için gerekli değildir.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>SSL v2 SKU ile uçtan uca

Kimlik doğrulama sertifikaları kullanım dışı ve uygulama ağ geçidi v2 SKU güvenilen kök sertifikalarda değiştirilmiştir. Bunlar kimlik doğrulama sertifikalarını bazı önemli farklar ile benzer şekilde çalışır:

- Ana bilgisayar adıyla arka uç HTTP ayarları, CN bilinen CA yetkilisi tarafından imzalanmış sertifikalar çalışmak uçtan uca SSL için herhangi bir ek adımı gerektirmez. 

   Örneğin, arka uç sertifikaları tanınmış bir CA tarafından verilen ve bir CN contoso.com olan ve arka uç http ayarı'nın ana bilgisayar adı alanı, contoso.com etki alanına de ayarlanır, sonra ek adımlar gereklidir. Protokol ayarı HTTPS için arka uç http ayarlayabilirsiniz ve sistem durumu araştırması ve veri yolu, SSL etkin olacaktır. Azure App Service veya diğer Azure web Hizmetleri, arka uç olarak kullanıyorsanız, bunlar da örtük olarak güvenilir ve uçtan uca SSL için başka bir adım gereklidir.
- Sertifika otomatik olarak imzalanan veya bilinmeyen Aracılar tarafından imzalanmış, ardından v2 SKU güvenilen kök sertifika uçtan uca SSL etkinleştirmek için tanımlanmalıdır. Uygulama ağ geçidi, yalnızca arka uçları, sunucu sertifikasının kök sertifikasını havuzuyla ilişkili arka uç http ayarı güvenilen kök sertifikaların listesi biriyle eşleşen ile iletişim kurar.
- Arka uç sunucusunun SSL sertifikası tarafından sunulan ortak ad (CN) ana bilgisayar arka uç http ayarlarında belirtilen ayarını eşleşiyorsa, kök sertifika eşleşme ek olarak, Application Gateway de doğrular. Application Gateway sunucu adı belirtme (SNI) uzantısına arka uç bir SSL bağlantısı kurmaya çalışırken arka uç http ayarlarında belirtilen konağa ayarlar.
- Varsa **arka uç adresi ana bilgisayar adını çekme** arka uç http ayarı bilgisindeki Host alanı yerine FQDN ve CN SNI başlığı her zaman arka uç havuzuna ayarlandıysa seçilen arka uç sunucusunda SSL sertifikası, FQDN ile eşleşmesi gerekir. Bu senaryoda, arka uç havuzu üyelerine IP'leri ile desteklenmiyor.
- Arka uç sunucusu sertifikalarının bir Base64 ile kodlanmış bir kök sertifika kök sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL hakkında daha fazla edindikten sonra Git [ve PowerShell kullanarak Application Gateway uçtan uca SSL'yi yapılandırma](application-gateway-end-to-end-ssl-powershell.md) uçtan uca SSL kullanan bir uygulama ağ geçidi oluşturmak için.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
