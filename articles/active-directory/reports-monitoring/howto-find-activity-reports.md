---
title: Azure portalında Azure Active Directory Kullanıcı Etkinlik raporlarını bulma | Microsoft Docs
description: Azure Active Directory kullanıcı etkinlik raporları Azure portalında nerede olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 182537d6f07b624f2395f591681ed4596579bde0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42059520"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Azure portalında Etkinlik raporlarını bulma

Bu makalede, sizi Azure portalında Azure Active Directory Kullanıcı Etkinlik raporlarını bulma açıklanmaktadır.

## <a name="activity-and-integrated-app-reports"></a>Etkinlik ve tümleşik uygulama raporları

Bağlam tabanlı Azure portalında raporlama için var olan raporların tek bir görünümde birleştirilir. Tek bir API temel alınan veri görünümüne sağlar.

Bu görünümde görmek için **Azure Active Directory** dikey altında **etkinlik**seçin **denetim günlükleri**.

![Denetim günlükleri](./media/howto-find-activity-reports/482.png "Denetim günlükleri")

Aşağıdaki raporlar bu görünümde birleştirilir:

* Denetim raporu
* Parola sıfırlama etkinliği
* Parola sıfırlama kaydı etkinliği
* Self Servis Grup etkinliği
* Office 365 grubunu silmiş ad değişiklikleri
* Hesap sağlama etkinliği
* Parola geçişi durumu
* Hesap hazırlama hataları


Uygulama kullanım raporu geliştirilmiştir ve dahildir **oturum açma işlemleri** görünümü. Bu görünümde görmek için **Azure Active Directory** dikey altında **etkinlik**seçin **oturum açma**.

![Oturum açma görünümüne](./media/howto-find-activity-reports/483.png "oturum açmaları görüntüleyin")

**Oturum açma işlemleri** görünümü, tüm kullanıcı oturum açma işlemleri içerir. Uygulama kullanım bilgilerini almak için bu bilgileri kullanabilirsiniz. Uygulama kullanım bilgileri de görüntüleyebilirsiniz **kurumsal uygulamalar** genel bakış bölmesinde **Yönet** bölümü.

![Kurumsal uygulamalar](./media/howto-find-activity-reports/484.png "kurumsal uygulamalar")

## <a name="access-a-specific-report"></a>Belirli bir rapora erişme

Azure portalında tek bir görünüm sunar, ancak Ayrıca belirli raporları arayabilirsiniz.

### <a name="audit-logs"></a>Denetim günlükleri

Azure portalında, müşteri geri bildirimleri doğrultusunda, Gelişmiş filtreleme istediğiniz verilere erişmek için kullanabilirsiniz. Bir filtre kullanabileceğiniz bir *etkinlik kategorisi*, Azure AD'de oturum farklı etkinlik türleri listelenir. Aradığınız ne sonuçları daraltmak için bir kategori seçebilirsiniz.

Örneğin, yalnızca Self Servis parola sıfırlama için ilgili etkinliklere ilgileniyorsanız, seçebileceğiniz **Self Servis parola yönetimi** kategorisi. Gördüğünüz kategorileri içinde çalışmakta olduğunuz kaynak temel alır.  

![Filtre denetim günlükleri sayfasında kategori seçenekleri](./media/howto-find-activity-reports/06.png "filtre denetim günlükleri sayfasında kategori seçenekleri")

Etkinlik kategorileri şunlardır:

- Çekirdek Dizin
- Self Servis Parola Yönetimi
- Self Servis Grup Yönetimi
- Hesap Sağlama

### <a name="application-usage"></a>Uygulama kullanımı

Tüm uygulamalar için veya tek bir uygulama için uygulama kullanımı hakkında ayrıntılar görünümünde için **etkinlik**seçin **oturum açma**. Sonuçları daraltmak için kullanıcı adı veya uygulama adı filtre uygulayabilirsiniz.

![Filtre olayları oturum açma sayfası](./media/howto-find-activity-reports/07.png "filtre olayları, oturum açma sayfası")

### <a name="security-reports"></a>Güvenlik raporları

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD anormal etkinlik raporları

Azure AD anormal etkinlik güvenlik raporları birleştirilmiş genel bir görünüm ile sağlamak için. Bu görünüm, Azure AD algılamak ve raporlama tüm risk güvenlikle ilgili olayları gösterir.

Aşağıdaki tablo liste Azure AD anormal etkinlik güvenlik raporları ve Azure portalında ilgili risk olayı türleri.

| Azure AD anormal Etkinlik Raporu |  Kimlik koruması risk olayı türü|
| :--- | :--- |
| Sızan kimlik bilgilerine sahip kullanıcılar | Sızdırılan kimlik bilgileri |
| Düzensiz oturum açma etkinliği | Alışılmadık konumlara imkansız seyahat |
| Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri | Bulaşma olan cihazlardan oturum açma işlemleri|
| Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Anonim IP adreslerinden oturum açma işlemleri |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri | Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |
| - | Alışılmadık konumlardan oturum açma işlemleri |

Aşağıdaki Azure AD anormal etkinlik güvenlik raporları olarak Azure portalında risk olayları dahil değildir:

* Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
* Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri

Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](concept-risk-events.md).  


#### <a name="detected-risk-events"></a>Algılanan risk olayları

Azure portalında, algılanan risk olayları hakkında rapor üzerinde erişebilirsiniz **Azure Active Directory** dikey altında **güvenlik**. Algılanan risk olayları aşağıdaki raporlarda izlenir:   

- Risk altındaki kullanıcılar
- Riskli Oturum Açma İşlemleri

![Güvenlik raporları](./media/howto-find-activity-reports/04.png "güvenlik raporları")

Güvenlik raporları hakkında daha fazla bilgi için bkz:

- [Risk altındaki kullanıcılar güvenlik raporu Azure Active Directory portalındaki](concept-user-at-risk.md)
- [Azure Active Directory portalındaki riskli oturum açma işlemleri raporu](concept-risky-sign-ins.md)


Görüntülenecek **uygulama kullanımı** raporunda **Azure Active Directory** dikey altında **Yönet**seçin **kurumsal uygulamalar**, ve ardından **oturum açma**.


![Kurumsal uygulamaları oturum açma işlemleri raporu](./media/howto-find-activity-reports/199.png)

## <a name="next-steps"></a>Sonraki adımlar

Raporlamaya genel bir bakış için bkz. [Azure Active Directory raporlama](overview-reports.md).
