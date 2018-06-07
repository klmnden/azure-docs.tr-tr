---
title: Azure AD raporlama API'si ile çalışmaya başlama | Microsoft Docs
description: Azure Active raporlama API'si ile dizinde nereden başlayacaksınız
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: df0b672a07575a0d26fff89c90008043d02818dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589109"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active raporlama API'si Directory ile çalışmaya başlama

Azure Active Directory ile çeşitli sağlar [raporları](active-directory-reporting-azure-portal.md). Bu raporların verileri SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için çok yararlı olabilir. 

Azure AD raporlama API'si kullanılarak programlı bir dizi API REST tabanlı üzerinden verilere erişebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede ilgili API'yi kullanarak raporlama veri erişimi için bir yol haritası ile sağlar.

Sorunla çalıştırırsanız, bkz: [Azure Active Directory için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>Önkoşullar

Bir komut dosyası kullanarak API erişme planlıyorsanız bile raporlama API erişmek için aktarmanız gerekir:

1. (Güvenlik Okuyucu, Güvenlik Yöneticisi, genel yönetici) rollerini atama
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplayın


 
Ayrıntılı yönergeler için bkz: [Azure Active Directory'ı Raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md).


## <a name="recommendation"></a>Öneri 

Kullanıcı müdahalesi olmadan raporlama veri alınırken planlıyorsanız, Azure AD raporlama API'si ile sertifikaları kullanmayı düşünün.

Ayrıntılı yönergeler için bkz: [Azure AD raporlama API'si ile sertifika kullanımı Veri Al](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Keşfedin

Raporlama API'leri bir ilk izlenim alın:
   
   - [Denetim API'si örnekleri kullanma](active-directory-reporting-api-audit-samples.md) 
 
   - [Oturum açma etkinliği raporu API örnekleri kullanma](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Özelleştirme  

Kendi çözümü oluşturun: 
   
   - [API Başvurusu denetim kullanma](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Oturum açma etkinliği raporu API başvurusunu kullanma](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



