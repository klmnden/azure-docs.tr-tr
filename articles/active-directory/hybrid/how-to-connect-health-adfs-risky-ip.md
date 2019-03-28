---
title: Azure AD Connect Health ile AD FS riskli IP raporu | Microsoft Docs
description: Azure AD Connect Health AD FS riskli IP raporu açıklar.
services: active-directory
documentationcenter: ''
ms.reviewer: zhiweiwangmsft
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 49b93cb7852692e4dad65fcbd72cd749db1b16fb
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540916"
---
# <a name="risky-ip-report-public-preview"></a>Riskli IP raporu (genel Önizleme)
AD FS müşterileri, son kullanıcıların Office 365 gibi SaaS uygulamalarına erişmelerini sağlamak için İnternet’te parola kimlik doğrulama uç noktalarını kullanıma sunabilir. Bu durumda kötü bir aktör, bir son kullanıcı parolasını tahmin etmek ve uygulama kaynaklarına erişmek amacıyla AD FS sisteminize karşı oturum açma girişimlerinde bulunabilir. AD FS, Windows Server 2012 R2'de AD FS’den itibaren bu tür saldırıları önlemek için extranet hesap kilitleme işlevselliği sağlamaktadır. Daha düşük bir sürüm kullanıyorsanız, AD FS sisteminizi Windows Server 2016’ya yükseltmeniz kesinlikle önerilir. <br />

Ayrıca, tek bir IP adresinin birden fazla kullanıcıya karşı birden çok oturum açma girişiminde bulunması mümkündür. Böyle durumlarda kullanıcı başına deneme sayısı, AD FS’deki hesap kilitleme korumasına ilişkin eşiğin altında olabilir. Azure AD Connect Health artık bu durumu algılayıp durum gerçekleştiğinde yöneticileri bilgilendiren "Riskli IP raporu" özelliğini sağlamaktadır. Bu raporun başlıca yararları şunlardır: 
- Parola tabanlı başarısız girişimler eşiğini aşan IP adreslerini algılama
- Hatalı parola veya extranet kilitleme durumu nedeniyle başarısız olan oturum açma işlemlerini destekler
- Bu durum oluşur oluşmaz özelleştirilebilir e-posta ayarlarıyla yöneticileri uyarmak üzere e-posta bildirimi gönderme
- Bir kuruluşun güvenlik ilkesi ile eşleşen özelleştirilebilir eşik ayarları
- Otomasyon aracılığıyla çevrimdışı analiz ve diğer sistemlerle tümleştirme için indirilebilir raporlar

> [!NOTE]
> Bu raporu kullanmak için AD FS denetiminin etkin olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz. [AD FS için Denetimi Etkinleştirme](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Erişmek için önizleme, Genel Yönetici veya [Güvenlik Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) izni gereklidir.  
> 

## <a name="what-is-in-the-report"></a>Raporda nedir?
Başarısız oturum açma etkinliği istemci IP adresleri, Web uygulaması Proxy sunucuları toplanır. Riskli IP raporundaki her bir öğe, belirlenmiş eşiği aşan başarısız AD FS oturum açma etkinlikleri hakkında toplu bilgiler gösterir. Aşağıdaki bilgilere yer verilmiştir: ![Azure AD Connect Health portalı](./media/how-to-connect-health-adfs/report4a.png)

| Rapor Öğesi | Açıklama |
| ------- | ----------- |
| Zaman Damgası | Algılama zaman penceresi başladığında Azure portalı yerel saatini temel alan zaman damgasını gösterir.<br /> Tüm günlük olaylar UTC saat diliminde gece yarısı oluşturulur. <br />Saatlik olayların zaman damgası saat başına yuvarlanır. Birinci etkinlik başlangıç saatini dışarı aktarılan dosyadaki "firstAuditTimestamp" içinde bulabilirsiniz. |
| Tetikleyici Türü | Algılama zaman penceresinin türünü gösterir. Toplama tetikleyici türleri saat veya gün başınadır. Bu türler, yüksek sıklıktaki bir deneme yanılma saldırısı ile deneme sayısının gün geneline dağıtıldığı yavaş bir saldırı arasında karşılaştırmalı algılamaya yardımcı olur. |
| IP Adresi | Hatalı parola veya extranet kilitleme oturum açma etkinlikleri olan tek riskli IP adresi. Bu, bir IPv4 veya IPv6 adresi olabilir. |
| Hatalı Parola Hata Sayısı | Algılama zaman penceresi boyunca IP adresinden kaynaklanan Hatalı Parola hatalarının sayısı. Hatalı Parola hataları belirli kullanıcılar için birden çok kez gerçekleşebilir. Buna süresi dolan parolalar nedeniyle başarısız olan denemelerin dahil olmadığına dikkat edin. |
| Extranet Kilitleme Hatası Sayısı | Algılama zaman penceresi boyunca IP adresinden kaynaklanan Extranet Kilitleme hatalarının sayısı. Extranet Kilitleme hataları belirli kullanıcılar için birden çok kez gerçekleşebilir. Bu durum yalnızca AD FS’de Extranet Kilitleme yapılandırılmışsa (sürüm 2012 R2 veya üstü) görülür. <b>Not</b> Parola kullanarak extranet oturumu açmaya izin veriyorsanız bu özelliği açmanız kesinlikle önerilir. |
| Deneme Yapan Benzersiz Kullanıcı Sayısı | Algılama zaman penceresi boyunca IP adresinden deneme yapan benzersiz kullanıcı hesaplarının sayısı. Tek ullanıcı saldırı deseni ile çoklu kullanıcı saldırı desenini birbirinden ayırt etmeye yönelik bir mekanizma sağlar.  |

Örneğin, aşağıdaki rapor öğesi 28.02.2018 tarihinde 18 ile 19 arasındaki saat penceresinde <i>104.2XX.2XX.9</i> IP adresinin parola hatası içermediğini ve 284 extranet kilitleme hatası içerdiğini gösterir. Ölçütler dahilinde 14 benzersiz kullanıcı etkilenmiştir. Etkinlik olayı, belirlenen saatlik rapor eşiğini aşmıştır. 

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report4b.png)

> [!NOTE]
> - Rapor listesinde yalnızca belirlenen eşiği aşan etkinlikler gösterilecektir. 
> - Bu rapor en fazla 30 gün geriye kadar izlenebilir.
> - Bu uyarı raporu, Exchange IP adreslerini veya özel IP adreslerini göstermez. Bunlar yine de dışarı aktarma listesine eklenir. 
>

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report4c.png)

## <a name="load-balancer-ip-addresses-in-the-list"></a>Yük Dengeleyici IP adresleri listesinde
Yük dengeleyici, başarısız oturum açma etkinliklerini topladı ve uyarı eşiğine ulaştı. Yük dengeleyici IP adreslerini görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir. Lütfen, iletme istemci IP adresini geçirmek için yük dengeleyicinizi doğru şekilde yapılandırın. 

## <a name="download-risky-ip-report"></a>Riskli IP raporu indirme 
**İndirme** işlevi kullanılarak, son 30 gün içindeki tüm riskli IP adresi listesi Connect Health Portalından dışarı aktarılabilir. Dışarı aktarma sonucu, her bir algılama zaman penceresindeki tüm başarısız AD FS oturum açma girişimlerini içerir, böylece dışarı aktarma sonrasında filtrelemeyi özelleştirebilirsiniz. Dışarı aktarma sonucunda, portalda vurgulanan toplamaların yanı sıra her bir IP adresi için başarısız oturum açma etkinliklerine ilişkin daha fazla ayrıntı gösterilmektedir:

|  Rapor Öğesi  |  Açıklama  | 
| ------- | ----------- | 
| firstAuditTimestamp | Algılama zaman penceresi sırasında başarısız etkinlikler başlatıldığında ilk zaman damgasını gösterir.  | 
| lastAuditTimestamp | Algılama zaman penceresi sırasında başarısız etkinlikler sonlandırıldığında son zaman damgasını gösterir.  | 
| attemptCountThresholdIsExceeded | Geçerli etkinlikler uyarı eşiğini aşıyorsa gösterilen bayrak.  | 
| isWhitelistedIpAddress | IP adresine uyarı ve raporlama filtresi uygulanmışsa gösterilen bayrak. Özel IP adresleri (<i>10.x.x.x, 172.x.x.x ve 192.168.x.x</i>) ile Exchange IP adresleri filtrelenir ve True olarak işaretlenir. Özel IP adresi aralıkları görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir.  | 

## <a name="configure-notification-settings"></a>Bildirim ayarlarını yapılandırma
Raporun yönetim kişileri **Bildirim Ayarları** üzerinden güncelleştirilebilir. Varsayılan olarak, riskli IP uyarısı e-posta bildirimi kapalı durumdadır. “Başarısız etkinlik eşiği raporunu aşan IP adresleri için e-posta bildirimleri alın” altındaki düğmeyi açarak bildirimi etkinleştirebilir. Connect Health uygulamasındaki genel uyarı bildirimi ayarları gibi bu seçenek de riskli IP raporu hakkında belirlenmiş bildirim alıcısını özelleştirmenize olanak tanır. Ayrıca, değişikliği yaparken tüm genel yöneticilere bildirebilirsiniz. 

## <a name="configure-threshold-settings"></a>Eşik ayarlarını yapılandırma
Uyarı eşiği, Eşik Ayarları üzerinden güncelleştirilebilir. Başlangıç için, sistemin varsayılan olarak ayarlanmış bir eşiği vardır. Riskli IP raporu eşik ayarları dört kategoriye ayrılır:

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report4d.png)

| Eşik Öğesi | Açıklama |
| --- | --- |
| (Hatalı U/P + Extranet Kilitleme) / Gün  | Hatalı Parola sayısı ile Extranet Kilitleme sayısının **gün** başına toplamı daha fazla olduğunda etkinliği bildirme ve uyarı bildirimini tetikleme eşiği ayarı. |
| (Hatalı U/P + Extranet Kilitleme) / Saat | Hatalı Parola sayısı ile Extranet Kilitleme sayısının **saat** başına toplamı daha fazla olduğunda etkinliği bildirme ve uyarı bildirimini tetikleme eşiği ayarı. |
| Extranet Kilitleme / Gün | **Gün** başına Extranet Kilitleme sayısı daha fazla olduğunda etkinliği bildirme ve uyarı bildirimini tetikleme eşiği ayarı. |
| Extranet Kilitleme / Saat| **Saat** başına Extranet Kilitleme sayısı daha fazla olduğunda etkinliği bildirme ve uyarı bildirimini tetikleme eşiği ayarı. |

> [!NOTE]
> - Rapor eşiği değişikliği, ayar değişikliğinden bir saat sonra uygulanır. 
> - Bildirilmiş mevcut öğeler eşik değişikliğinden etkilenmez. 
> - Ortamınızda görülen olayların sayısının çözümlemeniz ve eşiği buna uygun şekilde ayarlamanız önerilir. 
>
>

## <a name="faq"></a>SSS
**Neden raporda özel IP adresi aralıkları görüyorum?**  <br />
Özel IP adresleri (<i>10.x.x.x, 172.x.x.x ve 192.168.x.x</i>) ile Exchange IP adresleri filtrelenir ve IP güvenilir listesinde True olarak işaretlenir. Özel IP adresi aralıkları görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir.

**Neden görüyorum yük dengeleyici IP adreslerini rapordaki?**  <br />
Yük dengeleyici IP adreslerini görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir. Lütfen, iletme istemci IP adresini geçirmek için yük dengeleyicinizi doğru şekilde yapılandırın. 

**IP adres bloğu için ne yapmalıyım?**  <br />
Tanımlanmış kötü amaçlı IP adreslerini güvenlik duvarına eklemeniz veya Exchange’de engellemeniz gerekir.   <br />

**Neden Bu raporda tüm öğeler görüyorum değil mi?** <br />
- Başarısız oturum açma etkinlikleri eşik ayarlarını aşmıyor.
- AD FS sunucu listenizde etkin bir "Sistem durumu hizmeti güncel değil" uyarısı olmadığından emin olun.  [Bu uyarıyla ilgili sorunları giderme](how-to-connect-health-data-freshness.md) hakkında daha fazla bilgi edinin.
- AD FS gruplarınde denetimler etkin değildir.

**Rapor erişimi neden görüyorum?**  <br />
Genel Yönetici veya [Güvenlik Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) izni gereklidir. Erişim elde etmek için lütfen genel yöneticinize başvurun.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](how-to-connect-health-agent-install.md)
