---
title: Azure Active Directory raporlama | Microsoft Docs
description: "Azure Active Directory raporlamasına genel bir bakış sağlar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 738c8f4a56586b87f03973ec258b0a3023affa60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting"></a>Azure Active Directory raporlama

Azure Active Directory raporlamasıyla, ortamınızın nasıl çalıştığıyla ilgili fikir edinebilirsiniz.  
Sağlanan verilerle:

- Uygulama ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayabilirsiniz
- Ortamınızın durumunu etkileyebilecek olası riskleri algılayabilirsiniz
- Kullanıcılarınızın işlerini yapmalarını engelleyen sorunları giderebilirsiniz  

Raporlama mimarisinin iki ana dayanağı vardır:

- Güvenlik raporları
- Etkinlik raporları

![Raporlama](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Güvenlik raporları

Azure Active Directory'deki güvenlik raporları kuruluşunuzun kimliklerini korumanıza yardımcı olur.  
Azure Active Directory'de iki tür güvenlik raporu vardır:

- **Riskli oldukları belirlenen kullanıcılar** - [Riskli oldukları belirlenen kullanıcılar güvenlik raporundan](active-directory-reporting-security-user-at-risk.md), gizliliği bozulmuş olabilecek kullanıcı hesaplarına genel bir bakış elde edersiniz.

- **Riskli oturum açma işlemleri** - [Riskli oturum açma işlemleri güvenlik raporuyla](active-directory-reporting-security-risky-sign-ins.md), kullanıcı hesabının meşru sahibi olmayan biri tarafından gerçekleştirilmiş olabilecek oturum açma işlemleriyle ilgili göstergeler elde edersiniz. 

**Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?**  
Azure Active Directory'nin tüm sürümlerinde size riskli oldukları belirlenen kullanıcılar ve riskli oturum açma işlemleri raporları sağlanır.  
Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- **Azure Active Directory Ücretsiz ve Temel sürümlerinde**, riskli olduğu belirlenen kullanıcıların ve riskli oturum açma işlemlerinin listesini zaten alırsınız. 

- **Azure Active Directory Premium 1** sürümü bu modeli genişleterek her raporda algılanmış olan temel risk olaylarından bazılarını incelemenize olanak tanır. 

- **Azure Active Directory Premium 2** sürümü temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.


## <a name="activity-reports"></a>Etkinlik raporları

Azure Active Directory'de iki tür etkinlik raporu vardır:

- **Denetim günlükleri** - [Denetim günlükleri etkinlik raporu](active-directory-reporting-activity-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.

- **Oturum açma işlemleri** - [Oturum açma işlemleri etkinlik raporuyla](active-directory-reporting-activity-sign-ins.md), denetim günlükleri raporunda bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.



**Denetim günlükleri raporu** uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar.
Sağlanan verilerin yararları arasında şu yaygın senaryolara çözüm getirmenize olanak tanımaları da sayılabilir:

- Kiracımda birisi yönetici grubuna erişim aldı. Ona kim erişim verdi? 

- Belirli bir uygulamayı yeni ekledim ve iyi çalışıp çalışmadığını bilmek istiyorum; bu nedenle uygulamada oturum açan kullanıcıların listesini öğrenmek istiyorum

- Kiracımda kaç parola sıfırlama işlemi yapıldığını bilmek istiyorum


**Denetim günlükleri raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?**  
Denetim günlükleri raporu, lisansınız olan özellikler için sağlanır. Belirli bir özelliğin lisansına sahipseniz, o özelliğin denetim günlüğü bilgilerine de erişebilirsiniz.

Diğer ayrıntılar için, [Azure Active Directory özellikleri ve becerileri](https://www.microsoft.com/cloud-platform/azure-active-directory-features) altında **Ücretsiz, Temel ve Premium sürümlerinin genel olarak sağlanan özelliklerini karşılaştırma** konusuna bakın.   



**Oturum açma işlemleri etkinlik raporu**, şöyle soruların yanıtlarını bulmanızı sağlar:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?


**Oturum açma işlemleri etkinlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?**  
Oturum açma işlemleri etkinlik raporuna erişebilmek için, kiracınızın ilişkili bir Azure AD Premium lisansı olması gerekir.


## <a name="programmatic-access"></a>Programlı erişim

Kullanıcı arabirimine ek olarak, Azure Active Directory raporlaması [programlı erişim](active-directory-reporting-api-getting-started-azure-portal.md) yoluyla da raporlama verileri sağlar. Bu raporların verileri SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için çok yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory'deki çeşitli rapor türleri hakkında daha fazla bilgi edinmek istiyorsanız, bkz.:

- [Riskli olarak işaretlenen kullanıcılar raporu](active-directory-reporting-security-user-at-risk.md)
- [Riskli oturum açma işlemleri raporu](active-directory-reporting-security-risky-sign-ins.md)
- [Denetim günlükleri raporu](active-directory-reporting-activity-audit-logs.md)
- [Oturum açma günlükleri raporu](active-directory-reporting-activity-sign-ins.md)

Raporlama API'sini kullanarak raporlama verilerine erişim hakkında daha fazla bilgi edinmek istiyorsanız, bkz: 

- [Azure Active Directory raporlama API’siyle çalışmaya başlama](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png