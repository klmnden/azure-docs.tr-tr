---
title: "B2B işbirliği kullanıcı talepleri, Azure Active Directory'de eşleme | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği için başvuru eşleme talepleri"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 439219087d0a5027c729e9d0e0ecb90bd2a9de3c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Eşleme Azure Active Directory B2B işbirliği kullanıcı talepleri

B2B işbirliği kullanıcılar için SAML belirtecinde verilen talepler özelleştiriliyor azure Active Directory (Azure AD) destekler. Kullanıcı uygulamaya doğruladığında, Azure AD benzersiz olarak tanımlayan kullanıcı hakkında bilgileri (veya talep) içeren uygulamaya bir SAML belirteci verir. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir. Görüntüleyin veya SAML belirteci öznitelikler sekmesinde altındaki uygulamaya gönderilen talepleri düzenleyin.

Neden SAML belirtecinde verilen talepler düzenlemek için gerekebilecek iki olası nedeni vardır.

1. Uygulama talep URI'ler farklı kümesi gerektiren veya talep değerleri için yazılmış

2. Uygulamanız Azure Active Directory'de depolanan kullanıcı asıl adı dışında bir şey olması NameIdentifier talep gerektirir.

  ![SAML belirteci görünüm talepleri](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Bu makalede talep özelleştirme ekleyin ve talep düzenleme hakkında daha fazla bilgi için kullanıma [Azure Active Directory'de önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor](develop/active-directory-saml-claims-customization.md). B2B işbirliği için NameID ve UPN arası Kiracı eşleme kullanıcıları, güvenlik nedenleriyle engellenir.


## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2bB işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
