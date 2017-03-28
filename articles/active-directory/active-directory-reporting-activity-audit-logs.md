---
title: "Azure Active Directory portalındaki denetim etkinlik raporları - önizleme | Microsoft Docs"
description: "Azure Active Directory portalı önizleme sürümündeki denetim etkinlik raporlarına giriş"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 9f1fd20c77b085a66487c83d8dcd902e8cc83e6d
ms.lasthandoff: 03/17/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Azure Active Directory portalındaki denetim etkinlik raporları - önizleme

Azure Active Directory [önizleme sürümündeki](active-directory-preview-explainer.md) raporlama ile ortamınızın nasıl çalıştığını belirlemek için gereken tüm bilgileri edinirsiniz.

Azure Active Directory'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı denetim etkinliklerine genel bakış sunmaktadır.
 
## <a name="audit-logs"></a>Denetim günlükleri

Azure Active Directory'deki denetim günlükleri uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar.

Azure portalda ilgili etkinlikleri denetlemeye yönelik üç ana kategori vardır:

- Kullanıcılar ve gruplar   

- Uygulamalar

- Dizin   

Denetim rapor etkinliklerinin tam listesi için [denetim raporu olaylarının listesi](active-directory-reporting-audit-events.md#list-of-audit-report-events) bölümüne bakın.


Tüm denetim verilerine giriş noktanız, **Azure Active Directory**’nin **Etkinlik** bölümünde bulunan **Denetim günlükleri** kısmıdır.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/61.png "Denetim günlükleri")

Denetim günlüğünde aktörleri (*kim*), etkinlikleri (*ne*) ve hedefleri gösteren bir liste görünümü vardır.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/345.png "Denetim günlükleri")

Liste görünümünde bir öğeye tıklayarak bu öğe hakkında daha fazla bilgi edinebilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/873.png "Denetim günlükleri")


## <a name="users-and-groups-audit-logs"></a>Kullanıcı ve gruplara yönelik denetim günlükleri

Kullanıcı ve grup tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

- Kullanıcılara hangi tür güncelleştirmeler uygulanmış?

- Kaç adet kullanıcı değiştirildi?

- Kaç adet parola değiştirildi?

- Bir yönetici bir dizinde neler yaptı?

- Eklenmiş olan gruplar hangileridir?

- Üyelik değişiklikleri olan gruplar var mı?

- Grubun sahipleri değişti mi?

- Bir grup veya kullanıcıya hangi lisanslar atanmış?

Yalnızca kullanıcı ve gruplarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kullanıcılar ve Gruplar**’ın **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/93.png "Denetim günlükleri")

## <a name="application-audit-logs"></a>Uygulama denetim günlükleri
Uygulama tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

* Eklenmiş veya güncelleştirilmiş olan uygulamalar hangileridir?
* Kaldırılmış olan uygulamalar hangileridir?
* Belirli bir uygulamaya ait bir hizmet ilkesi değiştirildi mi?
* Uygulamaların adları değiştirildi mi?
* Belirli bir uygulama için kim onay verdi?

Yalnızca uygulamalarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kurumsal uygulamalar**’ın **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/134.png "Denetim günlükleri")

## <a name="filtering-audit-logs"></a>Denetim günlüklerini filtreleme
Görüntülenen veri miktarını sınırlamak için, oturum açma işlemlerini aşağıdaki alanları kullanarak filtreleyebilirsiniz:

- Tarih ve saat

- Aktörün kullanıcı asıl adı

- Kategori

- Etkinlik kaynak türü

- Etkinlik

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/625.png "Denetim günlükleri")


**Kategori** filtresi, denetim raporunuzun kapsamını aşağıdaki kategorilere göre sınırlamanızı sağlar:

- Çekirdek Dizin

- Self Servis Parola Yönetimi

- Self Servis Grup Yönetimi

- Otomatik Parola Geçişi 

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/626.png "Denetim günlükleri")



**Etkinlik kaynak türü** listesinin içeriği bu dikey pencerenin giriş noktasına bağlıdır.  
Giriş noktanız Azure Active Directory ise, bu liste tüm olası etkinlik türlerini içerir:

- Dizin

- Kullanıcı

- Grup 

- Uygulama 

- İlke

- Cihaz


![Denetim](./media/active-directory-reporting-activity-audit-logs/627.png "Auditing")

Listedeki etkinliklerin kapsamı etkinlik türüne göre belirlenir.
Örneğin, **Etkinlik Türü** olarak **Kullanıcı** seçeneğini belirlediyseniz **Etkinlik** listesi yalnızca grupla ilgili etkinlikleri içerir.   

![Denetim](./media/active-directory-reporting-activity-audit-logs/628.png "Auditing")

Örneğin, **Etkinlik kaynağı türü** olarak **Grup** seçeneğini belirlediyseniz aşağıdaki **Etkinlik Kaynaklarına** göre filtreleme yapmanızı sağlayan ek bir filtreleme seçeneği sunulur:

- Azure AD

- O365


![Denetim](./media/active-directory-reporting-activity-audit-logs/629.png "Auditing")



Denetim raporlarına ait girişleri filtrelemenin başka bir yöntemi de belirli girdiler için arama gerçekleştirmektir.

![Denetim](./media/active-directory-reporting-activity-audit-logs/237.png "Auditing")




## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory Raporlama Kılavuzu](active-directory-reporting-guide.md).


