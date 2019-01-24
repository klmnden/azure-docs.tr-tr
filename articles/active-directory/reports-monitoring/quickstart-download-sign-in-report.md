---
title: Hızlı Başlangıç Azure portalı kullanarak bir oturum açma raporunu indirme | Microsoft Docs
description: Azure portalı kullanarak bir oturum açma raporunun nasıl indirileceğini öğrenme
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7b16fb718e689eec8ea016b513d866390b2328e0
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815574"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir oturum açma raporunu indirin

Bu hızlı başlangıçta, son 24 saat boyunca kiracınız için oturum açma verilerinin nasıl indirileceğini öğreneceksiniz. En fazla 5000 kayıtlarını Azure portalından indirebilirsiniz. Kayıtları çoğu tarafından sıralanır varsayılan olarak, en son 5000 kayıt alabilmeniz son. 

## <a name="prerequisites"></a>Önkoşullar

Gerekenler:

* Oturum açma etkinlik raporunu görüntülemek için Premium lisansına sahip Azure Active Directory kiracısı. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için. Yükseltme öncesinde tüm etkinlikleri veri yoksa, birkaç gün raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin gerektiğine dikkat edin.
* İçinde olan bir kullanıcı **Güvenlik Yöneticisi**, **güvenlik okuyucusu**, **rapor okuyucu** veya **genel yönetici** kiracının rol. Ayrıca, kiracıdaki tüm kullanıcılar kendi oturum açma işlemlerine erişebilir.

## <a name="quickstart-download-a-sign-in-report"></a>Hızlı Başlangıç: Oturum açma raporunu indirme

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol gezinti bölmesinden **Azure Active Directory**’yi seçin ve **Dizini değiştir** düğmesini kullanarak etkin dizininizi seçin.
3. Panodan **Azure Active Directory**’yi ve sonra **Oturum açma işlemleri**’ni seçin. 
4. **Tarih** filtre açılır listesinden **son 24 saat**’i seçin ve sonra **Uygula**’yı seçerek son 24 saatteki oturum açma işlemlerini görüntüleyin. 
5. **İndir** düğmesini seçerek, filtrelenen kayıtları içeren bir CSV dosyasını indirin. 

![Raporlama](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory portaldaki oturum açma etkinlik raporları](concept-sign-ins.md)
* [Azure Active Directory raporlama elde tutma](reference-reports-data-retention.md)
* [Azure Active Directory raporlama gecikmeleri](reference-reports-latencies.md)
