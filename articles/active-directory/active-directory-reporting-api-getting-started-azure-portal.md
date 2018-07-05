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
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 93532f4b0b2d527a4d5c79e2ee1b2810394b2f11
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442092"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE çalışmaya başlama

Azure Active Directory ile çeşitli sağlar [raporları](active-directory-reporting-azure-portal.md). Bu raporların verileri SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için çok yararlı olabilir. 

Azure AD raporlama API'sini kullanarak, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim elde edebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede ilgili API'si kullanarak raporlama verilerine erişmek için bir yol haritası sağlar.

Sorun yaşarsanız bkz [Azure Active Directory için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Önkoşullar

Betik kullanarak API erişimi üzerinde planlıyor olsanız bile raporlama API'sini erişmek için şunları yapmanız:

1. (Güvenlik okuyucusu, Güvenlik Yöneticisi, genel yönetici) Rolleri Ata
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplayın


 
Ayrıntılı yönergeler için bkz. [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md).

## <a name="apis-with-graph-explorer"></a>API'leri ile Graph Gezgini

Kullanabileceğiniz [explorer MSGraph](https://developer.microsoft.com/en-us/graph/graph-explorer) oturum açma işleminizi doğrulamak ve API veri denetleyebilirsiniz. Oturum açma düğmelerinin her ikisi de Graph Gezgini Arabiriminde kullanarak hesabınızda oturum açtığınızdan emin olun ve ayarlama **Tasks.ReadWrite** ve **Directory.ReadAll** gösterildiği kiracınız için izinleri.   

![Graph Gezgini](./media/active-directory-reporting-api-getting-started-azure-portal/graph-explorer.png)

![UI izinleri değiştirme](./media/active-directory-reporting-api-getting-started-azure-portal/modify-permissions.png)

## <a name="recommendation"></a>Öneri 

Kullanıcı müdahalesi olmadan raporlama verilerini alınırken planlıyorsanız, sertifikalarla Azure AD raporlama API'sini kullanmayı düşünmeniz gerekir.

Ayrıntılı yönergeler için bkz. [sertifikalarla Azure AD raporlama API'sini kullanarak veri alma](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Keşfedin

Bir raporlama API'lerini ilk izlenim alın:
   
   - [Denetim için API örneklerini kullanma](active-directory-reporting-api-audit-samples.md) 
 
   - [Oturum açma etkinliği raporunu API örneklerini kullanma](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Özelleştirme  

Kendi çözümünüzü oluşturun: 
   
   - [API Başvurusu denetim kullanma](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Oturum açma etkinliği raporunu API başvurusunu kullanma](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



