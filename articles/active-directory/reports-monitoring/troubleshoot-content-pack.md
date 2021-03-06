---
title: Sorun giderme Azure Active Directory etkinlik günlükleri içerik paketi hataları | Microsoft Docs
description: Bunları düzeltmek için hata iletileri Azure Active Directory etkinlik içerik paketini ve adımları bir listesini sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/07/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ee49ae56122fe596a4490914677d91d2f0348f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66807531"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Sorun giderme Azure Active Directory etkinlik günlükleri içerik paketi hataları 

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Azure Active Directory (Azure AD) Power BI içerik paketi ile çalışırken aşağıdaki hatalarla karşılaşırsanız çalıştırmak mümkündür: 

- [Yenileme başarısız oldu](troubleshoot-content-pack.md#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Veri alma çok uzun sürüyor](#data-import-is-too-slow) 

Bu makalede, olası nedenleri ve bu hataları düzeltme hakkında bilgi sağlar.
 
## <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hatanın nasıl kullanıma sunulur**: Power BI veya yenileme geçmişi başarısız durumunda e-posta. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanma kullanıcıların kimlik bilgilerini sıfırlama ancak içerik paketi bağlantı ayarlarında güncelleştirilmemiş hataları nedeniyle başarısız oldu yenileyin. | Power BI'da Azure AD etkinlik günlüklerini panoya karşılık gelen bulun (**Azure Active Directory etkinlik günlükleri**), yenileme zamanlama seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Yenileme, büyük veri kümelerini nedeniyle başarısız olabilir. | Şu anda Power BI ile Azure AD içerik paketini, yalnızca küçük veri kümeleri (kısa da 500,00 satırlar) bazı sınırlamaların Power BI hizmetinde zaman aşımı nedeniyle destekleyebilir. Büyük bir veri kümesini getirilecek çalıştığınız için azaltma hatalarla karşılaşırsanız veya yenileme zaman aşımı sorunları nedeniyle başarısız olursa, bu olabilir. Sorgu zaman diliminde azaltın ve yeniden deneyin.|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hatanın nasıl kullanıma sunulur**: Azure AD etkinlik günlükleri içerik paketine yeniden bağlandığınızda Power BI,. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan kullanıcının genel yönetici veya güvenlik okuyucusu veya güvenlik yöneticisi değil. | Genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi içerik paketlerine erişmek için bir hesap kullanın. |
| Kiracınızın Premium Kiracı değil veya dosya Premium lisansı ile en az bir kullanıcı yok. | [Bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>Veri içeri aktarma çok yavaş 
 
**Bu hatanın nasıl kullanıma sunulur**: İçerik paketiniz bağlandıktan sonra Power BI'da veri içeri aktarma işlemi için Azure AD etkinlik panonuzu hazırlamak başlar. günlükleri. Şu iletiyle karşılaşırsınız: **Veri alınıyor...**  herhangi bir gelişme olmadan.  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adımı her yerde birkaç dakika veya 30 dakika sürebilir. | İleti bir saat içinde Panonuzda gösterecek şekilde değişmezse [bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD raporlar için Power BI içerik Paketi'ni yüklemek](quickstart-install-power-bi-content-pack.md).
* [Power BI İçerik Paketi için Azure AD verilerinizi görselleştirmek için raporları kullanma](howto-power-bi-content-pack.md)
* [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md)
