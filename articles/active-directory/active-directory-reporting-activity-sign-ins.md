---
title: Azure Active Directory portalındaki oturum açma etkinlik raporları | Microsoft Docs
description: Azure Active Directory portalındaki oturum açma etkinlik raporlarına giriş
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/17/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d9c2f210204122947c24eb42f643450537f3b9a8
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232550"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki oturum açma etkinlik raporları

[Azure portalında](https://portal.azure.com) Azure Active Directory (Azure AD) raporlama özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Azure Active Directory'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı oturum açma etkinliklerine genel bakış sunmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi, Güvenlik Okuyucusu veya Rapor Okuyucusu rolündeki kullanıcılar
* Genel Yöneticiler
* Tüm kullanıcılar (yönetici olmayan) kendi oturum açma etkinliklerine erişebilirler 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Oturum açma etkinliğine erişebilmek için hangi Azure AD lisansınızın olması gerekir?
* Oturum açma etkinliği raporunun tamamını görebilmek için kiracınız ile ilişkili bir Azure AD Premium lisansınızın olması gerekir


## <a name="sign-in-activities"></a>Oturum açma etkinlikleri

Kullanıcı oturum açma raporu tarafından sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

* Belirli bir kullanıcının oturum açma düzeni nedir?
* Bir hafta içerisinde kaç kullanıcı oturum açtı?
* Bu açılan oturumların durumu nedir?

Tüm oturum açma etkinliği verilerine ilk giriş noktanız, **Azure Active**’in Etkinlik bölümündeki **Oturum açma işlemleri** kısmıdır.


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/61.png "oturum açma etkinliği")


Oturum açma günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Oturum açma tarihi
- İlgili kullanıcı
- Kullanıcının oturum açtığı uygulama
- Oturum açma durumu
- Risk algılama durumu
- Çok faktörlü kimlik doğrulaması (MFA) gereksiniminin durumu 

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/01.png "oturum açma etkinliği")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/19.png "oturum açma etkinliği")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/02.png "oturum açma etkinliği")

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/03.png "oturum açma etkinliği")


## <a name="filter-sign-in-activities"></a>Oturum açma etkinliklerini filtreleme

Raporlanan verileri kendinize uygun bir seviyeye gelecek şekilde daraltmak için aşağıdaki varsayılan alanları kullanarak oturum açma verilerini filtreleyebilirsiniz:

- Kullanıcı
- Uygulama
- Oturum açma durumu
- Risk algılama durumu
- Tarih


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/04.png "oturum açma etkinliği")

**Kullanıcı** filtresi, önem verdiğiniz kullanıcının adını veya kullanıcı asıl adını (UPN) belirtmenize imkan tanır. 

**Uygulama** filtresi, önem verdiğiniz uygulamanın adını belirtmenize imkan tanır.  

**Oturum açma durumu** filtresi aşağıdakilerden birini seçmenize imkan tanır:

- Tümü 
- Başarılı
- Hata

**Algılanan Risk** filtresi aşağıdakilerden birini seçmenize imkan tanır:

- Tümü
- Evet
- Hayır 


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


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/12.png "oturum açma etkinliği")


> [!TIP] 
> Varsayılan filtrelere ek olarak, oturum açma işlemlerinize eklediğiniz tüm ilave filtreler filtre alanlarına dönüşür.


## <a name="download-sign-in-activities"></a>Oturum açma etkinliklerini indirme

Azure portalının dışında çalışmak istiyorsanız oturum açma etkinlik verilerini indirebilirsiniz. Azure portalında İndir düğmesine ek olarak verilerinizi indirmek için bir betik oluşturma seçeneği sunulur.  

![İndir](./media/active-directory-reporting-activity-sign-ins/71.png "İndir")

**İndir**’e tıkladığınızda en son 5 bin kaydı içeren bir CSV dosyası oluşturulur. Daha fazla esneklik gerekiyorsa betik çözümünü kullanabilirsiniz. Tıklatarak **betik** ayarladığınız tüm filtreleri içeren bir PowerShell komut dosyası oluşturur. Karşıdan yükleme ve bu komut dosyası çalıştırma **Yönetici modu** CSV dosyasını oluşturmak için. Teknik uygulamaya ek olarak, indirebileceğiniz kayıt sayısı aynı zamanda [Azure Active Directory rapor saklama ilkeleri](active-directory-reporting-retention.md) ile kısıtlanır.  



## <a name="sign-in-activities-shortcuts"></a>Oturum açma etkinlikleri kısayolları

Azure portalı, Azure Active Directory’ye ek olarak oturum açma etkinliği verileri için fazladan iki giriş noktası sağlar:

- Kimlik güvenliği korumasına genel bakış
- Kullanıcılar
- Gruplar
- Kurumsal uygulamalar


### <a name="users-sign-ins-activities"></a>Kullanıcıların oturum açma etkinlikleri

Kullanıcı oturum açma raporu tarafından sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?



Bu verilere giriş noktanız, **kimlik güvenliği koruması** genel bakış sayfasındaki kullanıcı oturum açma grafiğidir. Kullanıcı oturum açma grafiği, belirli bir zaman dönemi içerisinde tüm kullanıcılara ait oturum açma işlemlerinin haftalık olarak toplanmış halini gösterir. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/06.png "oturum açma etkinliği")

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
- Konum
- IP adresi
- Tarih
- MFA Gerekli
- Oturum açma durumu

 
**Kullanıcılar** sayfasında, **Etkinlik** bölümündeki **Oturum açma** öğesine tıklayarak tüm kullanıcı oturum açma işlemlerine eksiksiz bir genel bakış elde edebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/08.png "oturum açma etkinliği")




## <a name="usage-of-managed-applications"></a>Yönetilen uygulamaların kullanımı

Oturum açma bilgilerinizin uygulama odaklı bir görünümüyle aşağıdakiler gibi sorular yanıtlanabilir:

* Uygulamalarımı kimler kullanıyor?
* Kuruluşunuzdaki en çok kullanılan ilk 3 uygulama nelerdir?
* Kısa bir süre önce bir uygulamayı kullanıma sundum. Uygulamanın durumu nedir?

Bu verilere giriş noktanız, **Kurumsal uygulamalar** sayfasının **Genel Bakış** bölümünde bulunan *son 30 günde kuruluşunuzdaki en iyi 3 uygulama* raporudur.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/10.png "oturum açma etkinliği")

Belirli bir zaman döneminde en çok kullanılan ilk 3 uygulamanızda oturum açma işlemlerine ilişkin haftalık toplanan uygulama kullanımı grafiği. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/47.png "oturum açma etkinliği")

İsterseniz belirli bir uygulamaya odaklanabilirsiniz.


![Raporlama](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Uygulama kullanımı grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.




**Oturum açma işlemleri** seçeneği, size tüm uygulamalarınıza ait oturum açma olaylarına genel bir bakış sunar.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/11.png "oturum açma etkinliği")



## <a name="next-steps"></a>Sonraki adımlar

Oturum açma etkinliği hata kodları hakkında daha fazla bilgi edinmek isterseniz, bkz. [Azure Active Directory portalındaki oturum açma işlemleri etkinlik raporu hata kodları](active-directory-reporting-activity-sign-ins-errors.md).

