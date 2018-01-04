---
title: "Azure Active Directory portalındaki oturum açma etkinlik raporları | Microsoft Docs"
description: "Azure Active Directory portalındaki oturum açma etkinlik raporlarına giriş"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 36ff13f94e94df5b5be3645d3c9bd0cdca5cd6f9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
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

## <a name="pre-requisite"></a>Önkoşul

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi veya Güvenlik Okuyucusu rolündeki kullanıcılar
* Genel Yöneticiler
* Tüm kullanıcılar (yönetici olmayan) kendi oturum açma etkinliklerine erişebilirler 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Oturum açma etkinliğine erişebilmek için hangi Azure AD lisansınızın olması gerekir?
* Oturum açma etkinliği raporunun tamamını görebilmek için kiracınız ile ilişkili bir Azure AD Premium lisansınızın olması gerekir


## <a name="signs-in-activities"></a>Oturum açma etkinlikleri

Kullanıcı oturum açma raporu tarafından sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

* Belirli bir kullanıcının oturum açma düzeni nedir?
* Bir hafta içerisinde kaç kullanıcı oturum açtı?
* Bu açılan oturumların durumu nedir?

Tüm oturum açma etkinliği verilerine ilk giriş noktanız, **Azure Active**’in Etkinlik bölümündeki **Oturum açma işlemleri** kısmıdır.


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/61.png "oturum açma etkinliği")


Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- İlgili kullanıcı
- Kullanıcının oturum açtığı uygulama
- Oturum açma durumu
- Oturum açma zamanı

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/41.png "oturum açma etkinliği")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/19.png "oturum açma etkinliği")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/42.png "oturum açma etkinliği")

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları öğrenebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/43.png "oturum açma etkinliği")


## <a name="filtering-sign-in-activities"></a>Oturum açma etkinliklerini filtreleme

Raporlanan verileri kendinize uygun bir seviyeye gelecek şekilde daraltmak için aşağıdaki alanları kullanarak oturum açma verilerini filtreleyebilirsiniz:

- Zaman aralığı
- Kullanıcı
- Uygulama
- İstemci
- Oturum açma durumu

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/44.png "oturum açma etkinliği")


**Zaman aralığı** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 24 saat
- Özel

Özel bir zaman çerçevesi seçerken başlangıç ve bitiş zamanını yapılandırabilirsiniz.

**Kullanıcı** filtresi, önem verdiğiniz kullanıcının adını veya kullanıcı asıl adını (UPN) belirtmenize imkan tanır.

**Uygulama** filtresi, önem verdiğiniz uygulamanın adını belirtmenize imkan tanır.

**İstemci** filtresi, önem verdiğiniz cihazla ilgili bilgileri belirtmenize imkan tanır.

**Oturum açma durumu** filtresi, aşağıdaki filtrelerden birini seçmenize imkan tanır:

- Tümü
- Başarılı
- Hata


## <a name="sign-in-activities-shortcuts"></a>Oturum açma etkinlikleri kısayolları

Azure portalı, Azure Active Directory’ye ek olarak oturum açma etkinliği verileri için fazladan iki giriş noktası sağlar:

- Kullanıcılar ve gruplar
- Kurumsal uygulamalar


### <a name="users-and-groups-sign-ins-activities"></a>Kullanıcı ve grupların oturum açma etkinlikleri

Kullanıcı oturum açma raporu tarafından sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?



Bu verilere giriş noktanız, **Kullanıcılar ve gruplar** altındaki **Genel Bakış** bölümünde bulunan kullanıcı oturum açma grafiğidir.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/45.png "oturum açma etkinliği")

Kullanıcı oturum açma grafiği, belirli bir zaman dönemi içerisinde tüm kullanıcılara ait oturum açma işlemlerinin haftalık olarak toplanmış halini gösterir. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/46.png "oturum açma etkinliği")

Oturum açma grafiğinde bir güne tıkladığınızda, o güne ait oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/41.png "oturum açma etkinliği")

Oturum açma etkinlikleri listesindeki her satır, seçili oturum açma hakkında aşağıdakiler gibi ayrıntılı bilgiler sağlar:

* Kim oturum açtı?
* İlgili UPN neydi?
* Oturum açmanın hedefi hangi uygulamaydı?
* Oturum açmanın IP adresi nedir?
* Oturum açmanın durumu neydi?

**Oturum açma işlemleri** seçeneği, size tüm kullanıcı oturum açma işlemlerinin genel bir görünümünü sunar.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/51.png "oturum açma etkinliği")



## <a name="usage-of-managed-applications"></a>Yönetilen uygulamaların kullanımı

Oturum açma bilgilerinizin uygulama odaklı bir görünümüyle aşağıdakiler gibi sorular yanıtlanabilir:

* Uygulamalarımı kimler kullanıyor?
* Kuruluşunuzdaki en çok kullanılan ilk 3 uygulama nelerdir?
* Kısa bir süre önce bir uygulamayı kullanıma sundum. Uygulamanın durumu nedir?

Bu verilere giriş noktanız, **Kurumsal uygulamalar** altındaki **Genel Bakış** bölümünde bulunan kuruluşunuzda son 30 gün içinde en çok kullanılan ilk 3 uygulama raporudur.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/64.png "oturum açma etkinliği")

Belirli bir zaman döneminde en çok kullanılan ilk 3 uygulamanızda oturum açma işlemlerine ilişkin haftalık toplanan uygulama kullanımı grafiği. Zaman dönemi için varsayılan süre 30 gündür.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/47.png "oturum açma etkinliği")

İsterseniz belirli bir uygulamaya odaklanabilirsiniz.


![Raporlama](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Uygulama kullanımı grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/48.png "oturum açma etkinliği")


**Oturum açma işlemleri** seçeneği, size tüm uygulamalarınıza ait oturum açma olaylarına genel bir bakış sunar.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins/49.png "oturum açma etkinliği")



## <a name="next-steps"></a>Sonraki adımlar

Oturum açma etkinliği hata kodları hakkında daha fazla bilgi edinmek isterseniz, bkz. [Azure Active Directory portalındaki oturum açma işlemleri etkinlik raporu hata kodları](active-directory-reporting-activity-sign-ins-errors.md).

