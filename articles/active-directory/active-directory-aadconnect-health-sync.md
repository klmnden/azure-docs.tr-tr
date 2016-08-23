
<properties
    pageTitle="Eşitleme için Azure AD Connect Health'i kullanma| Microsoft Azure"
    description="Bu Azure AD Connect Health sayfasında, Azure AD Connect eşitlemesinin nasıl izleneceği ele alınmıştır."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="billmath"/>

# Eşitleme için Azure AD Connect Health'i kullanma
Aşağıdaki belgeler Azure AD Connect Health ile Azure AD Connect’in (Eşitleme) izlenmesine özgüdür.  Azure Connect Health ile AD FS'yi izleme hakkında bilgi almak için bkz. [Azure AD Connect Health'i AD FS ile kullanma](active-directory-aadconnect-health-adfs.md). Ek olarak, Active Directory Etki Alanı Hizmetleri’ni Azure AD Connect Health ile izleme hakkında bilgi için bkz. [AD DS ile Azure AD Connect Health Kullanma](active-directory-aadconnect-health-adds.md).

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/sync.png)

## Eşitleme için Azure AD Connect Health uyarıları
Eşitleme için Azure AD Connect Health Uyarıları bölümünde etkin uyarıların listesi sağlanmıştır. Her uyarı için ilgili bilgiler, çözüm adımları ve ilgili belgelere yönelik bağlantılar verilmiştir. Etkin veya çözümlenmiş bir uyarıyı seçtiğinizde, alarmı çözümlemek için uygulayabileceğiniz adımların ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler içeren yeni bir dikey pencere görürsünüz. Ayrıca, geçmişte çözümlenen uyarılara ilişkin geçmiş verileri de görüntüleyebilirsiniz.

Bir uyarıyı seçtiğinizde, uyarıyı çözümlemek için uygulayabileceğiniz adımlar ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler alırsınız.

![Azure AD Connect eşitleme hatası](./media/active-directory-aadconnect-health-sync/alert.png)

### Sınırlı Uyarı Değerlendirmesi
Azure AD Connect, varsayılan yapılandırmayı (örneğin, Öznitelik Filtrelemesi için varsayılan yapılandırma özel bir yapılandırma olarak değiştirildiyse) KULLANMIYORSA Azure AD Connect Health aracısı, Azure AD Connect ile ilgili hata olaylarını karşıya yüklemez. 

Bu, uyarıların değerlendirilmesini hizmete göre sınırlar. Azure Portal'da hizmetinizin altında bu koşulu belirten bir başlık görürsünüz.

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/banner.png)

Bunu, "Ayarlar"a tıklayarak ve Azure AD Connect Health aracısının tüm hata günlüklerini karşıya yüklemesine izin vererek değiştirebilirsiniz.

![Eşitleme için Azure AD Connect Health](./media/active-directory-aadconnect-health-sync/banner2.png)

## Eşitleme Öngörüsü
Eşitleme için Azure AD Connect Health'in en son sürümüne şu yeni özellikler eklendi:

- Eşitleme işlemlerinin gecikme süresi
- Nesne Değişikliği eğilimi

### Eşitleme Gecikme Süresi
Bu özellik, bağlayıcılara ilişkin eşitleme işlemlerinin (içeri aktarma, dışarı aktarma vb.) gecikme sürelerinin grafik eğilimini sağlar.  Bu, yalnızca işlemlerinizin gecikme süresini (çok sayıda değişikliğinizin olması çok iyidir) anlamak için değil, aynı zamanda gecikme süresindeki daha fazla incelenmesi gerekebilecek anomalilerin algılanması için de hızlı ve kolay bir yol sunar.

![Eşitleme Gecikme Süresi](./media/active-directory-aadconnect-health-sync/synclatency.png)

Varsayılan olarak, Azure AD bağlayıcısı için yalnızca "Dışarı Aktarma" işleminin gecikme süresi gösterilir.  Bağlayıcı üzerinde gerçekleşen daha fazla işlemi görmek veya diğer bağlayıcılara ilişkin işlemleri görüntülemek üzere grafik üzerinde sağ tıklayıp belirli işlemi ve bağlayıcıyı seçin.

### Eşitleme Nesnesi Değişiklikleri
Bu özellik, değerlendirilen ve Azure AD'ye aktarılan değişiklik sayısının grafik eğilimini sağlar.  Günümüzde bu bilgileri eşitleme günlüklerinden toplamak kolay değil.  Bu grafik sayesinde ortamınızda oluşan değişiklik sayısını izlemenin yanı sıra, oluşan hataların görsel görünümünü de daha basit bir şekilde izleyebilirsiniz.

![Eşitleme Gecikme Süresi](./media/active-directory-aadconnect-health-sync/syncobjectchanges.png)

## İlgili bağlantılar

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)




<!--HONumber=Aug16_HO1-->


