---
title: Azure AD Connect Health'i AD FS ile Kullanma| Microsoft Belgeleri
description: Bu Azure AD Connect Health sayfasında, şirket içi AD FS altyapınızı nasıl izleyeceğiniz açıklanmıştır.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: mtillman
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2018
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d93207e6a5f0acabcf348981e799e801c39f48b
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278846"
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>AD FS'yi Azure AD Connect Health'i kullanarak izleme
Aşağıdaki belgeler, AD FS altyapınızın Azure AD Connect Health ile izlenmesine ilişkin belgelerdir. Azure AD Connect’i (Eşitleme) Azure AD Connect Health ile izleme hakkında bilgi için bkz. [Eşitleme için Azure AD Connect Health Kullanma](how-to-connect-health-sync.md). Ek olarak, Active Directory Etki Alanı Hizmetleri’ni Azure AD Connect Health ile izleme hakkında bilgi için bkz. [AD DS ile Azure AD Connect Health Kullanma](how-to-connect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>AD FS uyarıları
Azure AD Connect Health Uyarıları bölümünde etkin uyarıların listesi mevcuttur. Her uyarı için ilgili bilgiler, çözüm adımları ve ilgili belgelere yönelik bağlantılar verilmiştir.

Ek bilgiler, uyarıyı çözme adımları ve ilgili belgelerin bağlantılarını içeren yeni bir dikey pencere açmak için etkin veya çözümlenmiş bir uyarıya çift tıklayabilirsiniz. Ayrıca, geçmişte çözümlenen uyarılara ilişkin geçmiş verileri de görüntüleyebilirsiniz.

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>AD FS için Kullanım Analizi
Azure AD Connect Health Kullanım Analizi, federasyon sunucularınızın kimlik doğrulama trafiğini analiz eder. Birkaç ölçüm ve gruplandırmayı gösteren kullanım analizi dikey penceresini açmak için kullanım analizi kutusuna çift tıklayabilirsiniz.

> [!NOTE]
> Kullanım Analizini AD FS ile kullanabilmek için AD FS denetiminin etkin olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz. [AD FS için Denetimi Etkinleştirme](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report1.png)

Ek ölçümler seçmek, zaman aralığı belirtmek veya gruplandırmayı değiştirmek için kullanım analizi grafiğine sağ tıklayıp Grafiği Düzenle seçeneğini belirleyin. Ardından zaman aralığı belirtebilir, farklı bir ölçüm seçebilir ve gruplandırmayı değiştirebilirsiniz. Kimlik doğrulama trafiğinin dağılımını farklı "ölçümlere" göre görüntüleyebilir ve aşağıdaki bölümde açıklanan ilgili "gruplandırma ölçütü" parametrelerini kullanarak her ölçümü gruplandırabilirsiniz:

**Ölçüm: Toplam İstek Sayısı** - AD FS sunucuları tarafından işlenen toplam istek sayısı.

|Gruplandırma Ölçütü | Gruplandırma nedir ve ne işe yarar? |
| --- | --- |
| Tümü | Tüm AD FS sunucuları tarafından işlenen isteklerin toplam sayısını gösterir.|
| Uygulama | Tüm istekleri hedeflenen bağlı olan tarafa göre gruplandırır. Bu gruplandırma, hangi uygulamanın toplam trafiğin yüzde kaçını aldığını anlamak için kullanışlıdır. |
|  Sunucu |Tüm istekleri isteği işleyen sunucuya göre gruplandırır. Bu gruplandırma, toplam trafiğin yük dağıtımını anlamak için kullanışlıdır.
| Çalışma Alanına Katılım |Tüm istekleri çalışma alanına katılmış (bilinen) cihazlardan gelip gelmediğine göre gruplandırır. Bu gruplandırma, kaynaklarınıza kimlik altyapısı tarafından bilinmeyen cihazlar kullanılarak erişilip erişilmediğini anlamak için kullanışlıdır. |
|  Kimlik Doğrulama Yöntemi | Toplam istekleri kimlik doğrulamak için kullanılan kimlik doğrulama yöntemine göre gruplandırır. Bu gruplandırma, kimlik doğrulaması için kullanılan yaygın kimlik doğrulama yöntemini anlamak için kullanışlıdır. Olası kimlik doğrulama yöntemleri aşağıda verilmiştir <ol> <li>Windows Tümleşik Kimlik Doğrulaması (Windows)</li> <li>Forms Tabanlı Kimlik Doğrulaması (Forms)</li> <li>SSO (Çoklu Oturum Açma)</li> <li>X509 Sertifika Doğrulaması (Sertifika)</li> <br>Federasyon sunucuları SSO Tanımlama Bilgisi ile istek alırsa bu istek SSO (Çoklu Oturum Açma) olarak sayılır. Bu gibi durumlarda, tanımlama bilgisi geçerliyse kullanıcıdan kimlik bilgilerini sağlaması istenmez ve kullanıcı uygulamaya sorunsuz bir şekilde erişir. Federasyon sunucuları tarafından korunan birden fazla bağlı olan tarafınız varsa bu davranış yaygındır. |
| Ağ Konumu | Tüm istekleri kullanıcının ağ konumuna göre gruplandırır. İntranet veya extranet olabilir. Bu gruplandırma, trafiğin yüzde kaçının intranetten veya extranetten geldiğini anlamak için kullanışlıdır. |


**Ölçüm: Toplam Başarısız İstek Sayısı** - Federasyon hizmeti tarafından işlenen başarısız isteklerin toplam sayısı. (Bu ölçüm yalnızca Windows Server 2012 R2 için AD FS'de kullanılabilir)

|Gruplandırma Ölçütü | Gruplandırma nedir ve ne işe yarar? |
| --- | --- |
| Hata Türü | Önceden tanımlanmış hata türlerine göre hata sayısını gösterir. Bu gruplandırma, genel hata türlerini anlamak için kullanışlıdır. <ul><li>Hatalı Kullanıcı Adı veya Parola: Hatalı kullanıcı adı veya paroladan kaynaklanan hatalar.</li> <li>"Extranet Kilitleme": Extranet dışında bırakılan bir kullanıcıdan alınan isteklerden kaynaklanan hatalar </li><li> "Süresi Dolan Parola": Süresi dolmuş bir parolayla oturum açmaya çalışan kullanıcılardan kaynaklanan hatalar.</li><li>"Devre Dışı Hesap": Devre dışı bırakılmış bir hesap ile oturum açmaya çalışan kullanıcılardan kaynaklanan hatalar.</li><li>"Cihaz Kimlik Doğrulaması": Cihaz Kimlik Doğrulamasını kullanarak kimlik doğrulaması gerçekleştiremeyen kullanıcılardan kaynaklanan hatalar.</li><li>"Kullanıcı Sertifikası Kimlik Doğrulaması": Geçersiz bir sertifika nedeniyle kimlik doğrulaması gerçekleştiremeyen kullanıcılardan kaynaklanan hatalar.</li><li>"MFA": Multi-Factor Authentication kullanarak kimlik doğrulaması yapmakta başarısız olan kullanıcılardan kaynaklanan hatalar.</li><li>"Diğer Kimlik Bilgisi": "Sertifika Verme Yetkilendirmesi": Başarısız yetkilendirmelerden kaynaklanan hatalar.</li><li>"Sertifika Verme Temsilcisi": Sertifika verme temsilcisi hatalarından kaynaklanan hatalar.</li><li>"Belirteç Onayı": ADFS'nin üçüncü taraf Kimlik Sağlayıcısından gelen bir belirteci reddetmesinden kaynaklanan hatalar.</li><li>"Protokol": Protokol hatalarından kaynaklanan hatalar.</li><li>"Bilinmeyen": Farklı nedenlerden kaynaklananlar. Tanımlanan kategorilere uymayan diğer tüm hatalar.</li> |
| Sunucu | Hataları sunucuya göre gruplandırır. Bu gruplandırma sunucular genelindeki hata dağıtımını anlamak için kullanışlıdır. Düzensiz dağıtım, hatalı durumdaki bir sunucunun göstergesi olabilir. |
| Ağ Konumu | Hataları isteklerin ağ konumuna (intranet veya extranet) göre gruplandırır. Bu gruplandırma başarısız olan istek türlerini anlamak için kullanışlıdır. |
|  Uygulama | Hataları hedeflenen uygulamaya (bağlı olan taraf) göre gruplandırır. Bu gruplandırma hedeflenen hangi uygulamanın en çok hata sayısını gördüğünü anlamak için kullanışlıdır. |

**Ölçüm: Kullanıcı sayısı** - Kimlik doğrulamak için etkin olarak AD FS’yi kullanan ortalama benzersiz kullanıcı sayısı

|Gruplandırma Ölçütü | Gruplandırma nedir ve ne işe yarar? |
| --- | --- |
|Tümü |Bu ölçüm seçilen bir zaman dilimi içinde federasyon hizmeti kullanan kullanıcıların ortalama sayısını sağlar. Kullanıcılar gruplandırılmaz. <br>Ortalama, seçilen zaman dilimine bağlı olarak değişir. |
| Uygulama |Ortalama kullanıcı sayısını hedeflenen uygulamaya (bağlı olan taraf) göre gruplandırır. Bu gruplandırma kaç kullanıcının hangi uygulamayı kullanmakta olduğunu anlamak için kullanışlıdır. |

## <a name="performance-monitoring-for-ad-fs"></a>AD FS için Performans İzleme
Azure AD Connect Health Performans İzleme, ölçümlere ilişkin izleme bilgileri sağlar. İzleme kutusunu seçtiğinizde, ölçümlere ilişkin ayrıntılı bilgiler içeren yeni bir dikey pencere açılır.

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/perf1.png)

Dikey pencerenin üst kısmındaki Filtre seçeneğini işaretlediğinizde her bir sunucunun ölçümlerini görmek için sunucuya göre filtreleme yapabilirsiniz. Ölçümü değiştirmek için, izleme dikey penceresinin altındaki izleme grafiğine sağ tıklayıp Grafiği Düzenle’yi seçin (veya Grafiği Düzenle düğmesini seçin). Açılan yeni dikey pencerede, açılan menüden başka ölçümler seçebilir veya performans verilerini görüntülemek istediğiniz zaman aralığını belirleyebilirsiniz.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>Hatalı Kullanıcı Adı/Parola kullanarak oturum açamayan İlk 50 Kullanıcı
Bir AD FS sunucusunda gerçekleşen başarısız kimlik doğrulama isteğinin en yaygın nedenlerinden biri geçersiz kimlik bilgileri ile ilgili isteklerdir, yani yanlış kullanıcı adı veya şifre istekleridir. Genellikle karmaşık parolalar, unutulan parolalar veya yazım hatalar nedeniyle oluşur.

Ancak, AD FS sunucularınızın beklenmeyen sayıda isteği işlemesiyle sonuçlanabilen diğer nedenlerle de mevcuttur, örneğin: Kullanıcı kimlik bilgilerini önbelleğe alan bir uygulama ve kimlik bilgilerinin süresinin dolması ya da kötü amaçlı bir kullanıcının bir dizi iyi bilinen parola ile hesapta oturum açmaya çalışması. Bu iki örnek, isteklerde ani bir artışa neden olabilecek geçerli nedenlerdir.

ADFS için Azure AD Connect Health, geçersiz kullanıcı adı veya paroladan dolayı oturum açma denemeleri başarısız olan İlk 50 Kullanıcıya ilişkin bir rapor sağlar. Bu rapor, gruplardaki tüm AD FS sunucuları tarafından oluşturulan denetim olaylarının işlenmesiyle sağlanır.

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report1a.png)

Bu raporda şu bilgilere kolayca erişebilirsiniz:

* Son 30 gün içinde yanlış kullanıcı adı/parola ile başarısız olmuş isteklerin toplam sayısı
* Her gün hatalı kullanıcı adı/parola ile oturum açarak başarısız olan kullanıcıların ortalama sayısı.

Bu bölüme tıkladığınızda, ek ayrıntılı bilgi görüntüleyebileceğiniz ana rapor dikey penceresine yönlendirilirsiniz. Bu dikey pencere, yanlış kullanıcı adı veya parola ile yapılan isteklere ilişkin bir temel oluşturmaya yardımcı olan eğilim bilgileriyle birlikte grafik içerir. Ayrıca, geçen hafta boyunca en fazla başarısız girişim sayısına sahip ilk 50 kullanıcının listesini verir. Önceki haftanın ilk 50 kullanıcısı, hatalı parola artışlarını belirlemeye yardımcı olabilir.  

Grafta şu bilgiler yer alır:

* Günlük olarak hatalı kullanıcı adı/parola nedeniyle başarısız olan oturum açma işlemlerinin toplam sayısı.
* Günlük olarak oturum açmada başarısız olan benzersiz kullanıcıların toplam sayısı.
* Son istek için istemci IP adresi

![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report3a.png)

Raporda şu bilgiler yer alır:

| Rapor Öğesi | Açıklama |
| --- | --- |
| Kullanıcı Kimliği |Kullanılan kullanıcı kimliğini gösterir. Bu değer, kullanıcının ne yazdığıyla ilgilidir ve bazı durumlarda yanlış kullanıcı kimliği kullanılır. |
| Başarısız Denemeler |Belirli bir kullanıcı kimliğine ait başarısız denemelerin toplam sayısını gösterir. Tablo, en fazla deneme sayısından en aza doğru azalan bir düzende sıralanır. |
| Son Hata |Son hatanın oluştuğu andaki zaman damgasını gösterir. |
| Son Hata IP’si |Son hatalı istekteki İstemci IP adresini gösterir. Bu değerde birden fazla IP adresi görürseniz, kullanıcının son girişim isteği IP’si ile birlikte iletme istemci IP’sini içerebilir.  |

> [!NOTE]
> Bu rapor, her 12 saatte bir bu süre içinde toplanan yeni bilgilerle otomatik olarak güncelleştirilir. Bu nedenle raporda son 12 saat içinde gerçekleşen oturum açma denemeleri bulunmayabilir.
>
>

## <a name="risky-ip-report-public-preview"></a>Riskli IP Raporu (Genel Önizleme)
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

### <a name="what-is-in-the-report"></a>Raporun içeriği
Riskli IP raporundaki her bir öğe, belirlenmiş eşiği aşan başarısız AD FS oturum açma etkinlikleri hakkında toplu bilgiler gösterir. Şu bilgileri sağlar: ![Azure AD Connect Health Portalı](./media/how-to-connect-health-adfs/report4a.png)

| Rapor Öğesi | Açıklama |
| ------- | ----------- |
| Zaman Damgası | Algılama zaman penceresi başladığında Azure portalı yerel saatini temel alan zaman damgasını gösterir.<br /> Tüm günlük olaylar UTC saat diliminde gece yarısı oluşturulur. <br />Saatlik olayların zaman damgası saat başına yuvarlanır. Birinci etkinlik başlangıç saatini dışarı aktarılan dosyadaki "firstAuditTimestamp" içinde bulabilirsiniz. |
| Tetikleyici Türü | Algılama zaman penceresinin türünü gösterir. Toplama tetikleyici türleri saat veya gün başınadır. Bu türler, yüksek sıklıktaki bir deneme yanılma saldırısı ile deneme sayısının gün geneline dağıtıldığı yavaş bir saldırı arasında karşılaştırmalı algılamaya yardımcı olur. |
| IP Adresi | Hatalı parola veya extranet kilitleme oturum açma etkinlikleri olan tek riskli IP adresi. Bu bir IPv4 veya IPv6 adresi olabilir. |
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

### <a name="load-balancer-ip-addresses-in-the-list"></a>Listedeki Load Balancer IP adresleri
Yük dengeleyici, başarısız oturum açma etkinliklerini topladı ve uyarı eşiğine ulaştı. Yük dengeleyici IP adreslerini görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir. Lütfen, iletme istemci IP adresini geçirmek için yük dengeleyicinizi doğru şekilde yapılandırın. 

### <a name="download-risky-ip-report"></a>Riskli IP raporu indirme 
**İndirme** işlevi kullanılarak, son 30 gün içindeki tüm riskli IP adresi listesi Connect Health Portalından dışarı aktarılabilir. Dışarı aktarma sonucu, her bir algılama zaman penceresindeki tüm başarısız AD FS oturum açma girişimlerini içerir, böylece dışarı aktarma sonrasında filtrelemeyi özelleştirebilirsiniz. Dışarı aktarma sonucunda, portalda vurgulanan toplamaların yanı sıra her bir IP adresi için başarısız oturum açma etkinliklerine ilişkin daha fazla ayrıntı gösterilmektedir:

|  Rapor Öğesi  |  Açıklama  | 
| ------- | ----------- | 
| firstAuditTimestamp | Algılama zaman penceresi sırasında başarısız etkinlikler başlatıldığında ilk zaman damgasını gösterir.  | 
| lastAuditTimestamp | Algılama zaman penceresi sırasında başarısız etkinlikler sonlandırıldığında son zaman damgasını gösterir.  | 
| attemptCountThresholdIsExceeded | Geçerli etkinlikler uyarı eşiğini aşıyorsa gösterilen bayrak.  | 
| isWhitelistedIpAddress | IP adresine uyarı ve raporlama filtresi uygulanmışsa gösterilen bayrak. Özel IP adresleri (<i>10.x.x.x, 172.x.x.x ve 192.168.x.x</i>) ile Exchange IP adresleri filtrelenir ve True olarak işaretlenir. Özel IP adresi aralıkları görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir.  | 

### <a name="configure-notification-settings"></a>Bildirim Ayarlarını Yapılandırma
Raporun yönetim kişileri **Bildirim Ayarları** üzerinden güncelleştirilebilir. Varsayılan olarak, riskli IP uyarısı e-posta bildirimi kapalı durumdadır. “Başarısız etkinlik eşiği raporunu aşan IP adresleri için e-posta bildirimleri alın” altındaki düğmeyi açarak bildirimi etkinleştirebilir. Connect Health uygulamasındaki genel uyarı bildirimi ayarları gibi bu seçenek de riskli IP raporu hakkında belirlenmiş bildirim alıcısını özelleştirmenize olanak tanır. Ayrıca, değişikliği yaparken tüm genel yöneticilere bildirebilirsiniz. 

### <a name="configure-threshold-settings"></a>Eşik ayarlarını yapılandırma
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

### <a name="faq"></a>SSS
1. Neden raporda özel IP adresi aralıkları görüyorum?  <br />
Özel IP adresleri (<i>10.x.x.x, 172.x.x.x ve 192.168.x.x</i>) ile Exchange IP adresleri filtrelenir ve IP güvenilir listesinde True olarak işaretlenir. Özel IP adresi aralıkları görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir.

2. Neden raporda yük dengeleyici IP adreslerini görüyorum?  <br />
Yük dengeleyici IP adreslerini görüyorsanız, dış yük dengeleyicinizin Web Uygulaması Ara sunucusuna isteği geçirdiğinde istemci IP adresini göndermeme olasılığı yüksektir. Lütfen, iletme istemci IP adresini geçirmek için yük dengeleyicinizi doğru şekilde yapılandırın. 

3. IP adresini engellemek için ne yapmalıyım?  <br />
Tanımlanmış kötü amaçlı IP adreslerini güvenlik duvarına eklemeniz veya Exchange’de engellemeniz gerekir.   <br />

4. Neden bu raporda hiçbir öğe görmüyorum? <br />
   - Başarısız oturum açma etkinlikleri eşik ayarlarını aşmıyor. 
   - AD FS sunucu listenizde etkin bir "Sistem durumu hizmeti güncel değil" uyarısı olmadığından emin olun.  [Bu uyarıyla ilgili sorunları giderme](how-to-connect-health-data-freshness.md) hakkında daha fazla bilgi edinin.
   - AD FS gruplarınde denetimler etkin değildir.
 
5. Neden rapora erişim olmadığını görüyorum?  <br />
Genel Yönetici veya [Güvenlik Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) izni gereklidir. Erişim elde etmek için lütfen genel yöneticinize başvurun.


## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](how-to-connect-health-operations.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](how-to-connect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](reference-connect-health-version-history.md)
