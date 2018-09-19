---
title: Azure AD Connect Health'i - sistem sağlığı hizmeti verileri değil en fazla tarih uyarı | Microsoft Docs
description: Bu belgede nedenini "sistem sağlığı hizmeti verileri güncel değil" uyarısı ve nasıl giderileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 430ea5f0a6f737d7632a4352c24d893368b80558
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315101"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sistem sağlığı hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış
<li>Azure AD Connect Health, tüm veri noktalarına sunucudan iki saatlik almaz yeni veri uyarısı oluşturur. Uyarı başlığı **sistem sağlığı hizmeti verileri güncel değil**. </li>
<li>**Uyarı** Connect Health kısmi veri öğeleri sunucudan iki saat boyunca gönderilen almazsa durumu uyarı tetikler. Uyarı durumu, Kiracı yöneticisine e-posta bildirimlerini tetiklenmiyor </li>
<li>**Hata** Connect Health, iki saatlik sunucusundan onlara gönderilen veri öğeleri almazsa durumu uyarı tetikler. Hata durumu uyarı Tetikleyicileri e-posta bildirimleri için Kiracı Yöneticisi </li>

>[!IMPORTANT] 
> Bu uyarı Connect Health aşağıdaki [veri bekletme ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları 
* Azure portalına ve karşılamak emin [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements).
* Kullanım [test bağlantısı aracı](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) bağlantı sorunları bulmak için.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
