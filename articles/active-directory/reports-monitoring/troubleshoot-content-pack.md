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
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: eafbe25a5a0fa9182030304e9142a6013c9fb29b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061109"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Sorun giderme Azure Active Directory etkinlik günlükleri içerik paketi hataları 


Azure Active Directory önizlemesi için Power BI içerik paketi ile çalışırken aşağıdaki hatalarla karşılaşırsanız çalıştırmak mümkündür: 

- [Yenileme başarısız oldu](troubleshoot-content-pack.md#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Veri alma çok uzun sürüyor](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Bu makalede, olası nedenleri ve bu hataları düzeltme hakkında bilgi sağlar.
 
## <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hatanın nasıl kullanıma sunulur**: e-posta Power BI veya yenileme geçmişi başarısız durumda. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanma kullanıcıların kimlik bilgilerini sıfırlama ancak içerik paketi bağlantı ayarlarında güncelleştirilmemiş hataları nedeniyle başarısız oldu yenileyin. | Power bı'da Azure Active Directory etkinlik günlükleri panoya (Azure Active Directory etkinlik günlükleri) karşılık gelen bulun, yenileme zamanlama seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Yenileme, temel alınan içerik paketindeki veri sorunları nedeniyle başarısız olabilir. | Bir destek bileti çıkartın. Daha fazla bilgi için [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hatanın nasıl kullanıma sunulur**: de Azure Active Directory etkinlik günlükleri (Önizleme) içerik paketine bağlanırken Powerbı,. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan kullanıcının genel yönetici veya güvenlik okuyucusu veya güvenlik yöneticisi değil | Genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi'ı içerik paketlerini erişimi olan bir hesap kullanın. |
| Kiracınızın Premium Kiracı değil veya dosya Premium lisansı ile en az bir kullanıcı yok. | Bir destek bileti çıkartın. Daha fazla bilgi için [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Veri alma çok uzun sürüyor 
 
**Bu hatanın nasıl kullanıma sunulur**: Power BI'da içerik paketinizi bağlandıktan veri içeri aktarma işlemi başlar panonuz için Azure Active Directory etkinlik günlüğü hazırlamak. İletisini görürsünüz: "*... veri alma* "  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adımı her yerde 30 dakika ile birkaç dakika sürebilir. | Yalnızca sabırlı olun. İleti bir saat içinde Panonuzda gösterecek şekilde değişmezse, Lütfen bir destek bileti oluşturun. Daha fazla bilgi için [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory önizlemesi için Power BI içerik Paketi'ni yüklemek için tıklayın [burada](https://powerbi.microsoft.com/blog/azure-active-directory-meets-power-bi/).


