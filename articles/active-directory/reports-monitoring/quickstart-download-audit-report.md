---
title: Hızlı Başlangıç Azure portalı kullanarak denetim raporu indirme | Microsoft Docs
description: Azure portalı kullanarak bir denetim raporunun nasıl indirileceğini öğrenme
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6cbea49fe39c92c8a2fc50e501cb4ef5cff74b1
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436685"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir denetim raporu indir

Bu hızlı başlangıçta, bir CSV dosyası denetim günlükleri, son 24 saat boyunca kiracınız için indirme öğrenin. En fazla 250.000 kayıtlarını Azure portalından indirebilirsiniz. Kayıtları çoğu tarafından sıralanır varsayılan olarak, en son 250.000 kayıtları almak için son. 

## <a name="prerequisites"></a>Önkoşullar

Gerekenler:

* Bir Azure Active Directory kiracısı. 
* İçinde olan bir kullanıcı **Güvenlik Yöneticisi**, **güvenlik okuyucusu**, veya **genel yönetici** kiracının rol. Ayrıca, kiracıdaki tüm kullanıcılar kendi denetim günlüklerine erişebilir.

## <a name="quickstart-download-an-audit-report"></a>Hızlı Başlangıç: Denetim raporunu indirme

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol gezinti bölmesinden **Azure Active Directory**’yi seçin ve **Dizini değiştir** düğmesini kullanarak etkin dizininizi seçin.
3. Panodan **Azure Active Directory**’yi ve sonra **Denetim günlükleri**’ni seçin. 
4. **Tarih aralığı** filtre açılır listesinden **son 24 saat**’i seçin ve sonra **Uygula**’yı seçerek son 24 saatteki denetim günlüklerini görüntüleyin. 
5. Seçin **indirme** düğmesini seçme **CSV** dosyası olarak biçimlendirmek ve filtrelenen kayıtlar içeren bir CSV dosyasını indirmek için bir dosya adı belirtin. 

![Raporlama](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory portaldaki oturum açma etkinlik raporları](concept-sign-ins.md)
* [Azure Active Directory raporlama elde tutma](reference-reports-data-retention.md)
* [Azure Active Directory raporlama gecikmeleri](reference-reports-latencies.md)
