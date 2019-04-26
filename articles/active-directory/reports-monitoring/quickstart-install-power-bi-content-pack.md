---
title: Azure AD Power BI içerik paketini yükleme | Microsoft Docs
description: Azure AD Power BI içerik paketini yüklemeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69a69732d8cb42c248fa954ef9047e5876f40837
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60285913"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Hızlı Başlangıç: Azure Active Directory Power BI içerik paketi yükleyin

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Power BI İçerik Paketi için Azure Active Directory (Azure AD) ortamınızı raporlama verileri görselleştirme olanağı sağlar. Önceden oluşturulmuş içerik paketini indirip Power BI tarafından sunulan zengin görselleştirme deneyiminden faydalanarak dizininizdeki tüm etkinlikleri raporlayabilirsiniz. Ayrıca kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle kolayca paylaşabilirsiniz. 

Bu hızlı başlangıçta içerik paketini yüklemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* Power BI hesabı. O365 veya Azure AD hesabınızı kullanabilirsiniz. 
* Azure AD kiracınızın kimliği. Dizininizi tanımlayan bu **Dizin Kimliği** değerine Azure portalın [özellikler sayfasından](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) erişebilirsiniz.
* Azure AD Premium (P1/P2) lisansı. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için.

## <a name="install-azure-ad-power-bi-content-pack"></a>Azure AD Power BI içerik paketini yükleme 

1. Power BI hesabınızla [Power BI](https://app.powerbi.com/groups/me/getdata/services) oturumu açın. O365 veya Azure AD Hesabınızı kullanabilirsiniz.

2. **Uygulamalar** sayfasında **Azure Active Directory Etkinlik Günlükleri** araması yapın ve **Şimdi edinin**'i seçin. 

   ![Azure Active Directory Power BI İçerik Paketi](./media/quickstart-install-power-bi-content-pack/getitnow.png) 
    
3. Açılan pencerede Azure AD Kiracı Kimliğinizi girin, sorgu için gün sayısına **7** yazın ve **İleri**'yi seçin.
    
   ![Azure Active Directory Power BI İçerik Paketi](./media/quickstart-install-power-bi-content-pack/connect.png) 

4. Azure Active Directory Etkinlik günlükleri panonuz oluşturulduğunda seçin.

   ![Azure Active Directory Power BI İçerik Paketi](./media/quickstart-install-power-bi-content-pack/dashboard.png) 
    
## <a name="next-steps"></a>Sonraki adımlar

* [Power BI içerik paketini kullanma](howto-power-bi-content-pack.md).
* [İçerik paketi hatalarını düzeltme](troubleshoot-content-pack.md).
