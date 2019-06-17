---
title: Azure AD Bağlayıcısı hesap parolası değiştirme | Microsoft Docs
description: Bu konuda Azure AD Bağlayıcısı hesabını geri yüklemek nasıl belgelenir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d68c190b51b9bbb5faf21e8ea75b07d1a82005e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64571406"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Azure AD Bağlayıcısı hesap parolasını değiştirme
Azure AD Bağlayıcısı hesabı, ücretsiz hizmet olduğu varsayılır. Kimlik bilgilerini sıfırlamanız gerekirse, bu konu, hakkındadır. Örneğin, bir genel yönetici tarafından hata varsa PowerShell kullanarak hesabın parolasını sıfırlayın.

## <a name="reset-the-credentials"></a>Kimlik bilgilerini Sıfırla
Azure AD Bağlayıcısı hesabı Azure AD kimlik doğrulama sorunları nedeniyle iletişim kuramazsa, parolayı sıfırlayabilir.

1. Azure AD Connect eşitleme sunucusu için oturum açın ve PowerShell'i başlatın.
2. `Add-ADSyncAADServiceAccount` öğesini çalıştırın.  
   ![PowerShell cmdlet addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD genel yönetici kimlik bilgilerini sağlayın.

Bu cmdlet, hizmet hesabı parolasını sıfırlar ve hem de Azure AD eşitleme altyapısında güncelleştirin.

## <a name="known-issues-these-steps-can-solve"></a>Bu adımları bilinen sorunları çözme
Bu bölümde, bir kimlik bilgilerini sıfırlama Azure AD Bağlayıcısı hesabı düzeltilmiş olan müşteriler tarafından bildirilen hataları listesidir.

- - -
Olay 6900  
Sunucu, bir parola değiştirme bildirimi işlenirken beklenmeyen bir hatayla karşılaştı:  
AADSTS70002: Kimlik bilgileri doğrulanırken hata. AADSTS50054: Kimlik doğrulaması için eski parola kullanıldı.

- - -
Event 659  
Parola İlkesi eşitleme yapılandırması alınırken hata oluştu. Microsoft.IdentityModel.Clients.activedirectory.adalserviceexception:  
AADSTS70002: Kimlik bilgileri doğrulanırken hata. AADSTS50054: Kimlik doğrulaması için eski parola kullanıldı.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

