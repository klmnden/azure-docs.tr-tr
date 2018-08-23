---
title: Azure AD raporlama API'si ile çalışmaya başlama | Microsoft Docs
description: Nasıl Azure Active Directory raporlama API'SİYLE çalışmaya başlama
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7de7c87e5cf1747f4899f33d9e6b9cbf0bb430e1
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060731"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE çalışmaya başlama

Azure Active Directory ile çeşitli sağlar [raporları](overview-reports.md). Bu raporların verileri SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için çok yararlı olabilir. 

Azure AD raporlama API'sini kullanarak, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim elde edebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede ilgili API'si kullanarak raporlama verilerine erişmek için bir yol haritası sağlar.

Sorun yaşarsanız bkz [Azure Active Directory için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Önkoşullar

Betik kullanarak API erişimi üzerinde planlıyor olsanız bile raporlama API'sini erişmek için şunları yapmanız:

1. (Güvenlik okuyucusu, Güvenlik Yöneticisi, genel yönetici) Rolleri Ata
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplayın

Ayrıntılı yönergeler için bkz. [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md).

## <a name="apis-with-graph-explorer"></a>API'leri ile Graph Gezgini

Kullanabileceğiniz [explorer MSGraph](https://developer.microsoft.com/graph/graph-explorer) oturum açma işleminizi doğrulamak ve API veri denetleyebilirsiniz. Oturum açma düğmelerinin her ikisi de Graph Gezgini Arabiriminde kullanarak hesabınızda oturum açtığınızdan emin olun ve ayarlama **AuditLog.Read.All** ve **Directory.Read.All** gösterildiği kiracınız için izinleri.   

![Graph Gezgini](./media/concept-reporting-api/graph-explorer.png)

![UI izinleri değiştirme](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Azure AD raporlama API'si erişmek için sertifika kullanma 

Kullanıcı müdahalesi olmadan raporlama verilerini almak planlıyorsanız Azure AD raporlama API'si ile sertifikaları kullanır.

Ayrıntılı yönergeler için bkz. [sertifikalarla Azure AD raporlama API'sini kullanarak veri alma](tutorial-access-api-with-certificates.md).


## <a name="next-steps"></a>Sonraki adımlar

 * [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
 * [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
 * [Azure AD raporlama API'sini hatalarını giderme](troubleshoot-graph-api.md)


