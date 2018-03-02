---
title: "Azure AD Connect Health - sistem durumu hizmeti verileri değil tarih uyarı kadar | Microsoft Docs"
description: "Bu belgede nedenini \"sistem durumu hizmeti verileri güncel değil\" uyarı ve nasıl giderileceği açıklanmaktadır."
services: active-directory
documentationcenter: 
author: zhiweiw
manager: maheshu
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 45e88320a64770239c8f322212e12ca7826cd0da
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sistem sağlığı hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış
<li>Bunu tüm veri noktaları sunucudan iki saat almadığında azure AD Connect Health veri yeni uyarı oluşturur. Uyarı başlık **sistem sağlığı hizmeti verileri güncel değil**. </li>
<li>**Uyarı** durum uyarısı Connect Health için iki saat sunucusundan gönderilen kısmi veri öğeleri almazsa ateşlenir. Uyarı durumu Kiracı yöneticisine e-posta bildirimlerini tetiklemez </li>
<li>**Hata** durum uyarısı Connect Health için iki saat sunucusundan gönderilen tüm veri öğelerinin almazsa ateşlenir. Hata durumu uyarı Tetikleyicileri e-posta bildirimleri için Kiracı Yöneticisi </li>

>[!IMPORTANT] 
> Bu uyarı Connect Health aşağıdaki [veri bekletme ilkesi](active-directory-aadconnect-health-gdpr.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları 
* Git ve karşıladığından emin olun [gereksinimleri bölümüne](active-directory-aadconnect-health-agent-install.md#requirements).
* Kullanım [test bağlantısı aracı](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) bağlantı sorunları bulmak için.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
