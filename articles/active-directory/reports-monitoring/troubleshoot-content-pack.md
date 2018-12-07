---
title: Sorun giderme Azure Active Directory etkinlik günlükleri içerik paketi hataları | Microsoft Docs
description: Bunları düzeltmek için hata iletileri Azure Active Directory etkinlik içerik paketini ve adımları bir listesini sağlar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 65747da92a3cad770cd9d474d27645782f7306b9
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998726"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Sorun giderme Azure Active Directory etkinlik günlükleri içerik paketi hataları 

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Azure Active Directory (Azure AD) Power BI içerik paketi ile çalışırken aşağıdaki hatalarla karşılaşırsanız çalıştırmak mümkündür: 

- [Yenileme başarısız oldu](troubleshoot-content-pack.md#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Veri alma çok uzun sürüyor](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 

Bu makalede, olası nedenleri ve bu hataları düzeltme hakkında bilgi sağlar.
 
## <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hatanın nasıl kullanıma sunulur**: e-posta Power BI veya yenileme geçmişi başarısız durumda. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanma kullanıcıların kimlik bilgilerini sıfırlama ancak içerik paketi bağlantı ayarlarında güncelleştirilmemiş hataları nedeniyle başarısız oldu yenileyin. | Power BI'da Azure AD etkinlik günlüklerini panoya karşılık gelen bulun (**Azure Active Directory etkinlik günlükleri**), yenileme zamanlama seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Yenileme, temel alınan içerik paketindeki veri sorunları nedeniyle başarısız olabilir. | [Bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hatanın nasıl kullanıma sunulur**: içinde için Azure AD etkinlik bağlandığınızda, Powerbı, günlükleri içerik paketi. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan kullanıcının genel yönetici veya güvenlik okuyucusu veya güvenlik yöneticisi değil. | Genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi içerik paketlerine erişmek için bir hesap kullanın. |
| Kiracınızın Premium Kiracı değil veya dosya Premium lisansı ile en az bir kullanıcı yok. | [Bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>Veri içeri aktarma çok yavaş 
 
**Bu hatanın nasıl kullanıma sunulur**: Power BI'da içerik paketinizi bağlandıktan sonra verileri içeri aktarma işlemi başlar panonuzu hazırlamak için Azure AD etkinlik günlükleri. İletisini görürsünüz: **veri alınıyor...**  herhangi bir gelişme olmadan.  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adımı her yerde birkaç dakika veya 30 dakika sürebilir. | İleti bir saat içinde Panonuzda gösterecek şekilde değişmezse [bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD raporlar Power BI contect paketini yüklemeniz](quickstart-install-power-bi-content-pack.md).
* [Power BI İçerik Paketi için Azure AD verilerinizi görselleştirmek için raporları kullanma](howto-power-bi-content-pack.md)
* [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md)
