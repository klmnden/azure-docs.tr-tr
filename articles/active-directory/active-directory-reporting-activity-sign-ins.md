---
title: "Azure Active Directory portalındaki oturum açma etkinlik raporları - önizleme | Microsoft Docs"
description: "Azure Active Directory portalı önizleme sürümündeki oturum açma etkinlik raporlarına giriş"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 78617dc7e3a4b6eb4fc32d32b6850b8c0832d6d8
ms.openlocfilehash: 9819c5f6a3aea53664d86e3a23b25946c0f2b731
ms.lasthandoff: 02/22/2017


---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal---preview"></a>Azure Active Directory portalındaki oturum açma etkinlik raporları - önizleme

Azure Active Directory [önizleme sürümündeki](active-directory-preview-explainer.md) raporlama ile ortamınızın nasıl çalıştığını belirlemek için gereken tüm bilgileri edinirsiniz.

Azure Active Directory'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı oturum açma etkinliklerine genel bakış sunmaktadır.

## <a name="signs-in-activities"></a>Oturum açma etkinlikleri

Kullanıcı oturum açma raporu tarafından sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

* Belirli bir kullanıcının oturum açma düzeni nedir?
* Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
* Bu açılan oturumların durumu nedir?

Bu verilere giriş noktanız, **Kullanıcılar ve gruplar** altındaki **Genel Bakış** bölümünde bulunan kullanıcı oturum açma grafiğidir.

 ![Raporlama](./media/active-directory-reporting-activity-sign-ins/05.png "Reporting")

Kullanıcı oturum açma grafiği, belirli bir zaman dönemi içerisinde tüm kullanıcılara ait oturum açma işlemlerinin haftalık olarak toplanmış halini gösterir. Zaman dönemi için varsayılan süre 30 gündür.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/02.png "Reporting")

Oturum açma grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/03.png "Reporting")

Oturum açma etkinlikleri listesindeki her satır, seçili oturum açma hakkında aşağıdakiler gibi ayrıntılı bilgiler sağlar:

* Kim oturum açtı?
* İlgili UPN neydi?
* Oturum açmanın hedefi hangi uygulamaydı?
* Oturum açmanın IP adresi nedir?
* Oturum açmanın durumu neydi?

## <a name="usage-of-managed-applications"></a>Yönetilen uygulamaların kullanımı

Oturum açma bilgilerinizin uygulama odaklı bir görünümüyle aşağıdakiler gibi sorular yanıtlanabilir:

* Uygulamalarımı kimler kullanıyor?
* Kuruluşunuzdaki en çok kullanılan ilk 3 uygulama nelerdir?
* Kısa bir süre önce bir uygulamayı kullanıma sundum. Uygulamanın durumu nedir?

Bu verilere giriş noktanız, **Kurumsal uygulamalar** altındaki **Genel Bakış** bölümünde bulunan kuruluşunuzda son 30 gün içinde en çok kullanılan ilk 3 uygulama raporudur.

 ![Raporlama](./media/active-directory-reporting-activity-sign-ins/06.png "Reporting")

Belirli bir zaman döneminde en çok kullanılan ilk 3 uygulamanızda oturum açma işlemlerine ilişkin haftalık toplanan uygulama kullanımı grafiği. Zaman dönemi için varsayılan süre 30 gündür.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/78.png "Reporting")

İsterseniz belirli bir uygulamaya odaklanabilirsiniz.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Uygulama kullanımı grafiğinde bir güne tıkladığınızda, oturum açma etkinliklerinin ayrıntılı bir listesini alırsınız.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/top_app_sign_ins.png "Reporting")

**Oturum açma işlemleri** seçeneği, size tüm uygulamalarınıza ait oturum açma olaylarına genel bir bakış sunar.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/85.png "Reporting")

Sütun seçiciyi kullanarak, görüntülenmesini istediğiniz veri alanlarını seçebilirsiniz.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/column_chooser.png "Reporting")

## <a name="filtering-sign-ins"></a>Oturum açma işlemlerini filtreleme
Görüntülenen veri miktarını sınırlamak için, oturum açma işlemlerini aşağıdaki alanları kullanarak filtreleyebilirsiniz:

* Tarih ve saat 
* Kullanıcı asıl adı
* Uygulama adı
* İstemci adı
* Oturum açma durumu

![Raporlama](./media/active-directory-reporting-activity-sign-ins/293.png "Reporting")

Oturum açma etkinliklerine ait girişleri filtrelemenin başka bir yöntemi de belirli girdiler için arama gerçekleştirmektir.
Arama yöntemi, oturum açma işlemlerinin kapsamı olarak belirli **kullanıcıları**, **grupları** veya **uygulamaları** seçmenize olanak tanır.

![Raporlama](./media/active-directory-reporting-activity-sign-ins/84.png "Reporting")


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory Raporlama Kılavuzu](active-directory-reporting-guide.md).


