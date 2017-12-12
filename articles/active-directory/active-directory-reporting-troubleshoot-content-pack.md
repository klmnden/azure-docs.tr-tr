---
title: "İçerik Paketi hataları günlüğe Azure Active Directory etkinliği sorunlarını giderme | Microsoft Docs"
description: "Bunları düzeltmek için hata iletileri adımları ve Azure Active Directory etkinliği içerik paketi bir listesini sağlar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d1c1698b1d94131d4f54878306b020c12dc577f2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>İçerik Paketi hataları günlüklerini Azure Active Directory etkinliği sorunlarını giderme 


Azure Active Directory Önizleme için Power BI içerik paketi ile çalışırken, aşağıdaki hatalarla karşılaşırsanız çalıştırmak mümkündür: 

- [Yenileme başarısız oldu](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Veri alma çok uzun sürüyor](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Bu konu, olası nedenleri ve bu hataları düzeltme hakkında bilgi sağlar.
 
## <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hatanın nasıl ortaya**: e-posta Power BI veya başarısız durumu yenileme geçmişi. 


| Nedeni | Düzeltme yapma |
| ---   | ---        |
| İçerik Paketi bağlanan kullanıcıların kimlik bilgilerini sıfırlama ancak bağlantı ayarları güncelleştirilmemiş hataları neden olabilir hatası Yenile içerik paketinin. | Power bı'da Azure Active Directory etkinliği günlükleri panoya (Azure Active Directory etkinliği günlükleri) karşılık gelen bulun, yenileme zamanlaması seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Bir yenileme, temel alınan içerik paketiyle veri sorunları nedeniyle başarısız olabilir. | Bir destek bileti dosya. Daha fazla ayrıntı için bkz: [Azure Active Directory için destek alma](active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hatanın nasıl ortaya**: Power Azure Active Directory etkinliği günlükleri (Önizleme) İçerik Paketi bağlanırken BI'da. 

| Nedeni | Düzeltme yapma |
| ---   | ---        |
| Bağlanan kullanıcının bir genel yönetici veya bir güvenlik okuyucu veya bir güvenlik yöneticisine olduğu | Genel yönetici veya güvenlik okuyucu veya bir güvenlik Yöneticisi'ı içerik paketleri erişimi olan bir hesap kullanın. |
| Kiracı Premium Kiracı değil veya Premium lisansı dosya en az bir kullanıcı yok. | Bir destek bileti dosya. Daha fazla ayrıntı için bkz: [Azure Active Directory için destek alma](active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Veri alma çok uzun sürüyor 
 
**Bu hatanın nasıl ortaya**: Power BI'da içerik paketiniz bağlandıktan sonra veri alma işlemi başlatır panonuz Azure Active Directory etkinlik günlüğü için hazırlamak. İletisini görürsünüz: "*... veri alma* "  

| Nedeni | Düzeltme yapma |
| ---   | ---        |
| Kiracı boyutuna bağlı olarak, bu adımı her yerden birkaç dakika 30 dakika olarak ele geçirebilir. | Yalnızca endişelenmeyin. Lütfen ileti bir saat içinde panonuz gösterecek şekilde değişmezse bir destek bileti dosya. Daha fazla ayrıntı için bkz: [Azure Active Directory için destek alma](active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory Önizleme için Power BI içerik paketi yüklemek için tıklatın [burada](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).


