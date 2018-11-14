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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fab94088d1d54012a955b0663b078d03b13d6299
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624921"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Azure portalında Etkinlik raporlarını bulma

Bu makalede, Azure portalında Azure Active Directory (Azure AD) Kullanıcı Etkinlik raporlarını bulma hakkında bilgi edinin.

## <a name="audit-logs-report"></a>Denetim günlükleri raporu

Denetim günlükleri raporu Bağlam tabanlı raporlama için tek bir görünümde uygulama etkinliklerini geçici olarak çeşitli raporlar birleştirir. Denetim günlükleri raporuna erişebilmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşeden dizininizi seçin ve ardından **Azure Active Directory** dikey penceresinde sol gezinti bölmesinden.
3. Seçin **denetim günlükleri** gelen **etkinlik** Azure Active Directory dikey bölümü. 

    ![Denetim günlükleri](./media/howto-find-activity-reports/482.png "Denetim günlükleri")

Denetim günlükleri raporu, aşağıdaki raporları birleştirir:

* Denetim raporu
* Parola sıfırlama etkinliği
* Parola sıfırlama kaydı etkinliği
* Self Servis Grup etkinliği
* Office 365 grubunu silmiş ad değişiklikleri
* Hesap sağlama etkinliği
* Parola geçişi durumu
* Hesap hazırlama hataları

### <a name="filtering-on-audit-logs"></a>Denetim günlüklerini filtreleme

Denetim verilerinin belirli bir kategorisi içinde belirterek erişmek için Denetim raporuna Gelişmiş filtreleme kullanabileceğiniz **etkinlik kategorisi** filtre. Örneğin, Self Servis parola sıfırlama için ilgili tüm etkinlikleri görüntülemek için seçin **Self Servis parola yönetimi** kategorisi. 

    ![Category options on the Filter Audit Logs page](./media/howto-find-activity-reports/06.png "Category options on the Filter Audit Logs page")

Etkinlik kategorileri şunlardır:

- Çekirdek Dizin
- Self Servis Parola Yönetimi
- Self Servis Grup Yönetimi
- Hesap Sağlama


## <a name="sign-ins-report"></a>Oturum açma işlemleri raporu 

**Oturum açma işlemleri** görünüm dahil tüm kullanıcı oturum açma işlemleri, hem de **uygulama kullanımı** rapor. Uygulama kullanım bilgileri de görüntüleyebilirsiniz **Yönet** bölümünü **kurumsal uygulamalar** genel bakış.

    ![Enterprise applications](./media/howto-find-activity-reports/484.png "Enterprise applications")

Oturum açma işlemleri raporu erişmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşeden dizininizi seçin ve ardından **Azure Active Directory** dikey penceresinde sol gezinti bölmesinden.
3. Seçin **oturum açma bilgilerini** gelen **etkinlik** Azure Active Directory dikey bölümü. 

    ![Oturum açma görünümüne](./media/howto-find-activity-reports/483.png "oturum açmaları görüntüleyin")


### <a name="filtering-on-application-name"></a>Uygulama adı filtresi

Kullanıcı adı veya uygulama adı üzerinde filtreleme yaparak, uygulama kullanımını hakkındaki ayrıntıları görüntülemek için oturum açma işlemleri raporu kullanabilirsiniz.

![Filtre olayları oturum açma sayfası](./media/howto-find-activity-reports/07.png "filtre olayları, oturum açma sayfası")

## <a name="security-reports"></a>Güvenlik raporları

### <a name="anomalous-activity-reports"></a>Anormal etkinlik raporları

Anormal Etkinlik raporlarını, Azure AD algılayan ve rapor güvenlikle ilgili risk olayları hakkında bilgi sağlar.

Aşağıdaki tablo liste Azure AD anormal etkinlik güvenlik raporları ve Azure portalında ilgili risk olayı türleri. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](concept-risk-events.md).  


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


### <a name="detected-risk-events"></a>Algılanan risk olayları

Algılanan risk olayları hakkında rapor erişebileceğiniz **güvenlik** bölümünü **Azure Active Directory** dikey penceresinde [Azure portalında](https://portal.azure.com). Algılanan risk olayları aşağıdaki raporlarda izlenir:   

- [Risk altındaki kullanıcılar](concept-user-at-risk.md)
- [Riskli oturum açma işlemleri](concept-risky-sign-ins.md)

    ![Güvenlik raporları](./media/howto-find-activity-reports/04.png "güvenlik raporları")

## <a name="next-steps"></a>Sonraki adımlar

* [Denetim günlükleri'ne genel bakış](concept-audit-logs.md)
* [Oturum açma işlemleri genel bakış](concept-sign-ins.md)
* [Riskli olaylara genel bakış](concept-risk-events.md)
