---
title: Azure Active Directory portalındaki oturum açma etkinlik raporları | Microsoft Docs
description: Azure Active Directory portalındaki oturum açma etkinlik raporlarına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac65a9ac81bca942f9fcbe802fdbf8a0aa3f8248
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60288087"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki oturum açma etkinlik raporları

Azure Active Directory (Azure AD) raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma işlemleri** – yönetilen uygulamalar ve kullanıcı oturum açma etkinlikleri kullanımı hakkında bilgi.
    - **Denetim günlükleri** - [denetim günlükleri](concept-audit-logs.md) kullanıcılar ve Grup Yönetimi, yönetilen uygulamalar ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri sağlar.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - [riskli oturum açma](concept-risky-sign-ins.md) bir kullanıcı hesabının meşru sahibi olmayan biri tarafından gerçekleştirilmiş olabilecek bir oturum açma girişiminin göstergesidir.
    - **Risk için işaretlenen kullanıcılar** - [riskli kullanıcı](concept-user-at-risk.md) gizliliği bozulmuş olabilecek bir kullanıcı hesabının göstergesidir.

Bu konu, oturum açma işlemleri raporu genel bir bakış sağlar.

## <a name="prerequisites"></a>Önkoşullar

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi, güvenlik okuyucu ve rapor okuyucu rolleri
* Genel Yöneticiler
* Ayrıca, herhangi bir kullanıcı (Yönetici olmayanlar) kendi oturum açma etkinliklerine erişebilir 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Oturum açma etkinliğine erişebilmek için hangi Azure AD lisansınızın olması gerekir?
* Kiracınızın tüm oturum açma etkinliği raporunu görebilmeniz için kendisiyle ilişkili bir Azure AD Premium lisansı olması gerekir. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için. Yükseltme öncesinde tüm etkinlikleri veri yoksa, birkaç gün raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin gerektiğine dikkat edin.

## <a name="sign-ins-report"></a>Oturum açma işlemleri raporu

Kullanıcı oturum açma işlemleri raporu aşağıdaki soruların yanıtlarını sağlar:

* Belirli bir kullanıcının oturum açma düzeni nedir?
* Bir hafta içerisinde kaç kullanıcı oturum açtı?
* Bu açılan oturumların durumu nedir?

Oturum açma işlemleri raporu seçerek erişebilirsiniz **oturum açma işlemleri** içinde **etkinlik** bölümünü **Azure Active Directory** dikey penceresinde [Azureportalı](https://portal.azure.com). Bu en fazla bazı oturum açma kayıt Portalı'nda görünmesi için iki saat sürebileceğini unutmayın.

![Oturum açma etkinliği](./media/concept-sign-ins/61.png "oturum açma etkinliği")

> [!IMPORTANT]
> Yalnızca görüntüler oturum açma işlemleri raporu **etkileşimli** olan oturum açma işlemleri, burada bir el ile oturum açtığında, kullanıcı adı ve parolasını kullanarak oturum açma işlemleri. Etkileşimli olmayan oturum açma işlemleri, hizmetten hizmete kimlik doğrulaması gibi oturum açma işlemleri raporu görüntülenmez. 

Oturum açma günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Oturum açma tarihi
- İlgili kullanıcı
- Kullanıcının oturum açtığı uygulama
- Oturum açma durumu
- Risk algılama durumu
- Çok faktörlü kimlik doğrulaması (MFA) gereksiniminin durumu

![Oturum açma etkinliği](./media/concept-sign-ins/01.png "oturum açma etkinliği")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Oturum açma etkinliği](./media/concept-sign-ins/19.png "oturum açma etkinliği")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Oturum açma etkinliği](./media/concept-sign-ins/02.png "oturum açma etkinliği")

Daha ayrıntılı bilgi almak için liste görünümünde bir öğe seçin.

![Oturum açma etkinliği](./media/concept-sign-ins/03.png "oturum açma etkinliği")

> [!NOTE]
> Müşteriler artık tüm oturum açma raporları aracılığıyla koşullu erişim ilkeleri giderebilirsiniz. Tıklayarak **koşullu erişim** sekmesinde bir oturum açma kaydı için müşterilerin ayrıntıları oturum açma ve sonucu her ilke için uygulanan tüm ilkeler hakkında ayrıntılı bilgi ve koşullu erişim durumunu gözden geçirebilirsiniz.
> Daha fazla bilgi için [CA tüm oturum açma bilgileri hakkında sık sorulan sorular](reports-faq.md#conditional-access).

![Oturum açma etkinliği](./media/concept-sign-ins/ConditionalAccess.png "oturum açma etkinliği")


## <a name="filter-sign-in-activities"></a>Oturum açma etkinliklerini filtreleme

Raporlanan verileri kendinize uygun bir seviyeye gelecek şekilde daraltmak için aşağıdaki varsayılan alanları kullanarak oturum açma verilerini filtreleyebilirsiniz:

- Kullanıcı
- Uygulama
- Oturum açma durumu
- Koşullu Erişim
- Tarih

![Oturum açma etkinliği](./media/concept-sign-ins/04.png "oturum açma etkinliği")

**Kullanıcı** filtresi, önem verdiğiniz kullanıcının adını veya kullanıcı asıl adını (UPN) belirtmenize imkan tanır.

**Uygulama** filtresi, önem verdiğiniz uygulamanın adını belirtmenize imkan tanır.

**Oturum açma durumu** filtresi aşağıdakilerden birini seçmenize imkan tanır:

- Tümü
- Başarılı
- Hata

**Koşullu erişim** filtre oturum açma için CA ilke durumu seçmenize imkan tanır:

- Tümü
- Uygulanmadı
- Başarılı
- Hata

**Tarih** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 24 saat
- Özel zaman aralığı

Özel bir zaman çerçevesi seçerken başlangıç ve bitiş zamanını yapılandırabilirsiniz.

Oturum açma görünümüne başka alanlar eklerseniz bu alanlar filtre listesine otomatik olarak eklenir. Örneğin, listenize **İstemci Uygulama** alanını ekleyerek, aşağıdaki filtreleri ayarlamanıza olanak tanıyan başka bir filtre seçeneği de alırsınız:

- Tarayıcı      
- Exchange ActiveSync (desteklenir)               
- Exchange ActiveSync (desteklenmez)
- Diğer istemciler               
    - IMAP
    - MAPI
    - Eski Office istemcileri
    - POP
    - SMTP


![Oturum açma etkinliği](./media/concept-sign-ins/12.png "oturum açma etkinliği")


## <a name="download-sign-in-activities"></a>Oturum açma etkinliklerini indirme

Yapabilecekleriniz [oturum açma verilerini indirmek](quickstart-download-sign-in-report.md) dışında Azure portal ile çalışmak istiyorsanız. Tıklayarak **indirme** , en son 250.000 kayıtları bir CSV veya JSON dosyası oluşturmak için bir seçenek sağlar.  

![İndir](./media/concept-sign-ins/71.png "İndir")

> [!IMPORTANT]
> İndirebilirsiniz kayıt sayısı tarafından sınırlı [Azure Active Directory rapor saklama ilkeleri](reference-reports-data-retention.md).  


## <a name="sign-ins-data-shortcuts"></a>Oturum açma verileri kısayolları

Azure AD ek olarak, Azure portalında oturum açma verilerini için ek giriş noktaları sağlar:

- Kimlik güvenliği korumasına genel bakış
- Kullanıcılar
- Gruplar
- Kurumsal uygulamalar

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Kimlik güvenliği koruması kullanıcıların oturum açma verileri

Kullanıcı oturum açma grafiğinde **kimlik güvenliği koruması** genel bakış sayfası, belirli bir süre içinde tüm kullanıcılar için oturum açma işlemlerinin haftalık olarak toplanmış halini gösterir. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/concept-sign-ins/06.png "oturum açma etkinliği")

Oturum açma grafiğinde bir güne tıkladığınızda, o güne ait oturum açma etkinliklerinin genel bir açıklamasını alırsınız.

Oturum açma etkinlikleri listesinin her satırında aşağıdakiler gösterilir:

* Kim oturum açtı?
* Oturum açmanın hedefi hangi uygulamaydı?
* Oturum açma durumu nedir?
* Oturum açma MFA durumu nedir?

Bir öğeye tıklayarak oturum açma işlemi hakkında daha fazla bilgi alabilirsiniz:

- Kullanıcı Kimliği
- Kullanıcı
- Kullanıcı adı
- Uygulama Kimliği
- Uygulama
- İstemci
- Location
- IP adresi
- Tarih
- MFA Gerekli
- Oturum açma durumu

> [!NOTE]
> IP adresleri, bir IP adresi ve bilgisayarın bu adresine sahip fiziksel olarak bulunduğu yeri arasında kesin bağlantı yoktur şekilde verilir. Mobil sağlayıcıları ve VPN IP adresleri genellikle çok istemci cihaz gerçekten kullanıldığı gölgeden uzak olan merkezi havuzlarından sorunu olarak IP adreslerini eşleme karmaşık. Şu anda Azure AD raporlarında, IP adresi fiziksel bir konuma dönüştürme izlemeleri, kayıt defteri verisi, geriye doğru arama ve diğer bilgilere göre en iyi çaba ilkesi var.

**Kullanıcılar** sayfasında, **Etkinlik** bölümündeki **Oturum açma** öğesine tıklayarak tüm kullanıcı oturum açma işlemlerine eksiksiz bir genel bakış elde edebilirsiniz.

![Oturum açma etkinliği](./media/concept-sign-ins/08.png "oturum açma etkinliği")

## <a name="usage-of-managed-applications"></a>Yönetilen uygulamaların kullanımı

Oturum açma bilgilerinizin uygulama odaklı bir görünümüyle aşağıdakiler gibi sorular yanıtlanabilir:

* Uygulamalarımı kimler kullanıyor?
* Kuruluşunuzdaki en çok kullanılan ilk 3 uygulama nelerdir?
* Kısa bir süre önce bir uygulamayı kullanıma sundum. Uygulamanın durumu nedir?

Bu verilere giriş noktanız, **Kurumsal uygulamalar** altındaki **Genel Bakış** bölümünde bulunan kuruluşunuzda son 30 gün içinde en çok kullanılan ilk 3 uygulama raporudur.

![Oturum açma etkinliği](./media/concept-sign-ins/10.png "oturum açma etkinliği")

Uygulama kullanımı grafiği ilişkin haftalık toplanan belirli bir süre içinde ilk 3 uygulamalarınız için oturum açma işlemleri. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/concept-sign-ins/47.png "oturum açma etkinliği")

İsterseniz belirli bir uygulamaya odaklanabilirsiniz.

![Raporlama](./media/concept-sign-ins/single_spp_usage_graph.png "Reporting")

Uygulama kullanımı grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.

**Oturum açma işlemleri** seçeneği, size tüm uygulamalarınıza ait oturum açma olaylarına genel bir bakış sunar.

![Oturum açma etkinliği](./media/concept-sign-ins/11.png "oturum açma etkinliği")

## <a name="office-365-activity-logs"></a>Office 365 etkinlik günlükleri

Office 365 etkinlik günlüklerini görüntüleyebilirsiniz [Microsoft 365 Yönetim merkezini](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Olsa da Microsoft 365 Yönetim Merkezi yalnızca Office 365 ve Azure AD etkinlik günlükleri dizin kaynaklarının çoğunu paylaşır, Office 365 etkinlik günlüklerinin tam bir görünümünü sağlar. 

Office 365 etkinlik günlüklerini programlı olarak kullanarak da erişebilirsiniz [Office 365 Yönetim API'leri](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma etkinlik raporundaki hata kodları](reference-sign-ins-error-codes.md)
* [Azure AD veri bekletme ilkeleri](reference-reports-data-retention.md)
* [Azure AD rapor gecikmeleri](reference-reports-latencies.md)

