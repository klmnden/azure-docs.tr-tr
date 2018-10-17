---
title: Azure AD Power BI içerik paketini yükleme | Microsoft Docs
description: Azure AD Power BI içerik paketini yüklemeyi öğrenin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ba2784663a585aebb82ee149be3b54e73920f6dd
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816659"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Hızlı başlangıç: Azure Active Directory Power BI içerik paketini yükleme

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Azure Active Directory için Power BI içerik paketi, Active Directory ortamınızda gerçekleşen işlemlerle ilgili ayrıntılı içgörüler elde etmenizi sağlar. Önceden oluşturulmuş içerik paketini indirip Power BI tarafından sunulan zengin görselleştirme deneyiminden faydalanarak dizininizdeki tüm etkinlikleri raporlayabilirsiniz. Ayrıca kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle kolayca paylaşabilirsiniz. 

Bu hızlı başlangıçta içerik paketini yüklemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* Power BI hesabı. O365 veya Azure AD hesabınızı kullanabilirsiniz. 
* Azure AD kiracınızın kimliği. Dizininizi tanımlayan bu **Dizin Kimliği** değerine Azure portalın [özellikler sayfasından](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) erişebilirsiniz.
* Azure AD Premium (P1/P2) lisansı. 

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
