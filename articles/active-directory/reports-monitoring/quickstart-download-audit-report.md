---
title: Hızlı Başlangıç Azure portalı kullanarak denetim raporu indirme | Microsoft Docs
description: Azure portalı kullanarak bir denetim raporunun nasıl indirileceğini öğrenme
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 09/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d6ecf62b46c25ec7cf4ab80219d1bc220e4eb4cc
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367415"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalı kullanarak denetim raporu indirme

Bu hızlı başlangıçta, son 24 saat boyunca kiracınız için denetim günlüklerinin nasıl indirileceğini öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

* Bir Azure Active Directory kiracısı. 
* Kiracı için Güvenlik Yöneticisi, Güvenlik Okuyucusu veya Genel Yönetici rolünde olan bir kullanıcı. Ayrıca, kiracıdaki tüm kullanıcılar kendi denetim günlüklerine erişebilir.

## <a name="quickstart-download-an-audit-report"></a>Hızlı Başlangıç: Denetim raporunu indirme

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol gezinti bölmesinden **Azure Active Directory**’yi seçin ve **Dizini değiştir** düğmesini kullanarak etkin dizininizi seçin.
3. Panodan **Azure Active Directory**’yi ve sonra **Denetim günlükleri**’ni seçin. 
4. **Tarih aralığı** filtre açılır listesinden **son 24 saat**’i seçin ve sonra **Uygula**’yı seçerek son 24 saatteki denetim günlüklerini görüntüleyin. 
5. **İndir** düğmesini seçerek, filtrelenen kayıtları içeren bir CSV dosyasını indirin. 

![Raporlama](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory portaldaki oturum açma etkinlik raporları](concept-sign-ins.md)
* [Azure Active Directory raporlama elde tutma](reference-reports-data-retention.md)
* [Azure Active Directory raporlama gecikmeleri](reference-reports-latencies.md)
