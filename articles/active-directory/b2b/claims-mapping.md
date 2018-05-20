---
title: B2B işbirliği kullanıcı talepleri, Azure Active Directory'de eşleme | Microsoft Docs
description: Azure Active Directory (Azure AD) B2B kullanıcılar için SAML belirtecinde verilen kullanıcı taleplerini özelleştirin.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/06/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f2617fb79fc20fcb385c7e2fc3596245ab18caba
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Eşleme Azure Active Directory B2B işbirliği kullanıcı talepleri

B2B işbirliği kullanıcılar için SAML belirtecinde verilen talepler özelleştiriliyor azure Active Directory (Azure AD) destekler. Kullanıcı uygulamaya doğruladığında, Azure AD benzersiz olarak tanımlayan kullanıcı hakkında bilgileri (veya talep) içeren uygulamaya bir SAML belirteci verir. Varsayılan olarak, bu kullanıcının kullanıcı adı, e-posta adresi, ad ve Soyadı içerir.

İçinde [Azure portal](https://portal.azure.com), görüntüleyebilir veya uygulama için SAML belirtecinde gönderilen talepleri düzenleyebilirsiniz. Ayarlara erişmek için seçin **Azure Active Directory** > **kurumsal uygulamalar** > çoklu oturum açma için yapılandırılmış uygulama > **çoklu oturum açma** . SAML belirteci ayarlarında bkz **kullanıcı öznitelikleri** bölümü.

![SAML belirteci öznitelikleri kullanıcı Arabiriminde gösterir](media/claims-mapping/view-claims-in-saml-token.png)

Neden SAML belirtecinde verilen talepler düzenlemek için gerekebilecek iki olası nedeni vardır:

1. Farklı bir dizi uygulama gerektiriyor URI'ler talep veya değerleri.

2. Uygulama Azure AD'de depolanan kullanıcı asıl adı (UPN) dışında bir şey olması NameIdentifier talep gerektirir.

Ekleme ve talep düzenleme hakkında daha fazla bilgi için bkz: [Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor](../develop/active-directory-saml-claims-customization.md).

B2B işbirliği için NameID ve UPN arası Kiracı eşleme kullanıcıları, güvenlik nedenleriyle engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- B2B işbirliği kullanıcı özellikleri hakkında daha fazla bilgi için bkz: [bir Azure Active Directory B2B işbirliği kullanıcının özelliklerini](user-properties.md).
- B2B işbirliği kullanıcılar için kullanıcı belirteçleri hakkında daha fazla bilgi için bkz: [Azure AD B2B işbirliği kullanıcı belirteçleri anlamak](user-token.md).

