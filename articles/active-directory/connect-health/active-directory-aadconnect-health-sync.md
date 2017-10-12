---
title: "Eşitleme için Azure AD Connect Health'i kullanma| Microsoft Belgeleri"
description: "Bu Azure AD Connect Health sayfasında, Azure AD Connect eşitlemesinin nasıl izleneceği ele alınmıştır."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b06338cb62cc458e7b097db36023f0746d4e969
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a>Azure AD Connect eşitlemesini Azure AD Connect Health ile izleme
Aşağıdaki belgeler Azure AD Connect Health ile Azure AD Connect’in (Eşitleme) izlenmesine özgüdür.  Azure Connect Health ile AD FS'yi izleme hakkında bilgi almak için bkz. [Azure AD Connect Health'i AD FS ile kullanma](active-directory-aadconnect-health-adfs.md). Ek olarak, Active Directory Etki Alanı Hizmetleri’ni Azure AD Connect Health ile izleme hakkında bilgi için bkz. [AD DS ile Azure AD Connect Health Kullanma](active-directory-aadconnect-health-adds.md).

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Eşitleme için Azure AD Connect Health uyarıları
Eşitleme için Azure AD Connect Health Uyarıları bölümünde etkin uyarıların listesi sağlanmıştır. Her uyarı için ilgili bilgiler, çözüm adımları ve ilgili belgelere yönelik bağlantılar verilmiştir. Etkin veya çözümlenmiş bir uyarıyı seçtiğinizde, alarmı çözümlemek için uygulayabileceğiniz adımların ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler içeren yeni bir dikey pencere görürsünüz. Ayrıca, geçmişte çözümlenen uyarılara ilişkin geçmiş verileri de görüntüleyebilirsiniz.

Bir uyarıyı seçtiğinizde, uyarıyı çözümlemek için uygulayabileceğiniz adımlar ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler alırsınız.

![Azure AD Connect eşitleme hatası](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Sınırlı Uyarı Değerlendirmesi
Azure AD Connect, varsayılan yapılandırmayı (örneğin, Öznitelik Filtrelemesi için varsayılan yapılandırma özel bir yapılandırma olarak değiştirildiyse) KULLANMIYORSA Azure AD Connect Health aracısı, Azure AD Connect ile ilgili hata olaylarını karşıya yüklemez.

Bu, uyarıların değerlendirilmesini hizmete göre sınırlar. Azure Portal'da hizmetinizin altında bu koşulu belirten bir başlık görürsünüz.

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/banner.png)

Bunu, "Ayarlar"a tıklayarak ve Azure AD Connect Health aracısının tüm hata günlüklerini karşıya yüklemesine izin vererek değiştirebilirsiniz.

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Eşitleme Öngörüsü
Yöneticiler, sık sık değişikliklerin Azure AD ile eşitlenmesi için gereken süreyi ve yapılan değişikliklerin miktarını öğrenmek ister. Bu özellik, aşağıdaki grafikler yoluyla bunların görselleştirilmesi için kolay bir yol sağlar:   

* Eşitleme işlemlerinin gecikme süresi
* Nesne Değişikliği eğilimi

### <a name="sync-latency"></a>Eşitleme Gecikme Süresi
Bu özellik, bağlayıcılara ilişkin eşitleme işlemlerinin (içeri aktarma, dışarı aktarma vb.) gecikme sürelerinin grafik eğilimini sağlar.  Bu, yalnızca işlemlerinizin gecikme süresini (çok sayıda değişikliğiniz varsa daha fazladır) anlamak için değil, aynı zamanda gecikme süresindeki daha fazla incelenmesi gerekebilecek anormalliklerin algılanması için de hızlı ve kolay bir yol sunar.

![Eşitleme Gecikme Süresi](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Varsayılan olarak, Azure AD bağlayıcısı için yalnızca "Dışarı Aktarma" işleminin gecikme süresi gösterilir.  Bağlayıcı üzerinde gerçekleşen diğer işlemleri görmek veya diğer bağlayıcılara ilişkin işlemleri görüntülemek için grafik üzerinde sağ tıklayın, Grafiği Düzenle’yi seçin veya “Gecikme Süresi Grafiğini Düzenle” düğmesine tıklayın ve belirli bir işlemi ve bağlayıcıları seçin.

### <a name="sync-object-changes"></a>Eşitleme Nesnesi Değişiklikleri
Bu özellik, değerlendirilen ve Azure AD'ye aktarılan değişiklik sayısının grafik eğilimini sağlar.  Günümüzde bu bilgileri eşitleme günlüklerinden toplamak kolay değil.  Bu grafik sayesinde ortamınızda oluşan değişiklik sayısını izlemenin yanı sıra, oluşan hataların görsel görünümünü de daha basit bir şekilde izleyebilirsiniz.

![Eşitleme Gecikme Süresi](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Nesne Düzeyinde Eşitleme Hata Raporu (Önizleme)
Bu özellik, kimlik verileri Azure AD Connect kullanılarak Windows Server AD ile Azure AD arasında eşitlenirken oluşabilecek eşitleme hataları hakkında bir rapor sağlar.

* Rapor, eşitleme istemcisi tarafından kaydedilen hataları kapsar (Azure AD Connect 1.1.281.0 veya üzeri)
* Eşitleme altyapısındaki son eşitleme işlemi sırasında oluşan hataları içerir. (Azure AD Bağlayıcısı üzerinde “dışarı aktarma”.)
* Raporun en son verileri içermesi için eşitlemeye yönelik Azure AD Connect Health aracısının gerekli uç noktalara giden bağlantısının olması gerekir.
* Rapor, eşitleme için Azure AD Connect Health aracısı tarafından yüklenen verileri kullanarak **30 dakikada bir güncelleştirilir**. Aşağıdaki temel özellikleri sağlar

  * Hataların kategorilere ayrılması
  * Kategoriye göre hatalı nesnelerin listesi
  * Hatalarla ilgili tüm veriler tek bir yerde bulunur
  * Çakışma nedeniyle hata oluşan Nesnelerin yan yana karşılaştırılması
  * Raporu CVS olarak indirme (yakında kullanıma sunulacak)

### <a name="categorization-of-errors"></a>Hataların Kategorilere Ayrılması
Rapor, mevcut eşitleme hatalarını aşağıdaki kategorilere ayırır:

| Kategori | Açıklama |
| --- | --- |
| Yinelenen Öznitelik |Azure AD Connect, bir Kiracıda benzersiz olması gereken ve Azure AD Connect’te bulunan bir veya daha fazla özniteliğin (proxyAddresses ve UserPrincipalName gibi) yinelenen değerleri ile nesneler oluşturmaya veya güncelleştirmeye çalıştığında oluşan hatalar. |
| Veri Uyuşmazlığı |Geçici eşleştirme, nesneleri eşleştiremeyerek eşitleme hatalarına neden olduğunda görülen hatalar. |
| Veri Doğrulama Hatası |UserPrincipalName gibi kritik özniteliklerde desteklenmeyen karakterler bulunması ve Azure AD’ye yazılmadan önce doğrulanamayan biçim hataları gibi geçersiz verilerden kaynaklanan hatalar. |
| Büyük Öznitelik |Bir veya daha fazla öznitelik izin verilen boyuttan, uzunluktan veya sayıdan daha büyük olduğunda görülen hatalar. |
| Diğer |Yukarıdaki kategorilere uymayan diğer tüm hatalar. Bu kategori, geri bildirimleriniz temel alınarak alt kategorilere ayrılacaktır. |

![Eşitleme Hata Raporu Özeti](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Eşitleme Hata Raporu Kategorileri](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Kategoriye göre hatalı nesnelerin listesi
Her kategori ayrıntılı olarak incelendiğinde ilgili kategoride hata içeren nesnelerin bir listesi görülebilir.
![Eşitleme Hata Raporu Listesi](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Hata Ayrıntıları
Hataların ayrıntılı görünümünde aşağıdaki veriler bulunur

* Dahil olan *AD Nesnesi* için tanımlayıcılar
* Dahil olan *Azure AD Nesnesi* tanımlayıcıları (geçerli olduğunda)
* Hata açıklaması ve düzeltme yolu
* İlgili makaleler

![Eşitleme Hata Raporu Ayrıntıları](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Hata raporunu CSV olarak indirme
"Dışarı aktar" düğmesini seçerek tüm hataların tüm ayrıntılarını içeren bir CSV dosyası indirebilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar
* [Eşitleme sırasında karşılaşılan Hataları giderme](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Yinelenen Öznitelik Dayanıklılığı](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)