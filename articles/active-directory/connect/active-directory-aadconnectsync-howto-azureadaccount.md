---
title: "Azure AD Connect eşitleme: Azure AD hizmet hesabı yönetme | Microsoft Docs"
description: "Bu konuda Azure AD hizmet hesabını geri yüklemek nasıl belgelenir."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, Azure AD Connect eşitleme Connector hizmet hesabı parolasını sıfırlama"
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 096b14f8e64ac288fe6d3956658a4b738993cea9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect eşitleme: Azure AD hizmet hesabı yönetme
Azure AD Bağlayıcısı tarafından kullanılan hizmet hesabını hizmeti boş olması beklenir. Kimlik bilgilerini sıfırlamanız gerekirse, bu konu, ilgilidir. Örneğin, bir genel yönetici yanlışlıkla varsa PowerShell kullanarak hizmet hesabı parolasını sıfırlayın.

## <a name="reset-the-credentials"></a>Kimlik bilgilerini sıfırlama
Azure AD Bağlayıcısı üzerinde tanımlı hizmet hesabı Azure AD kimlik doğrulama sorunları nedeniyle bağlantı kuramıyorsa, parolayı sıfırlayabilirsiniz.

1. Azure AD Connect eşitleme sunucusunda oturum açın ve PowerShell'i başlatın.
2. `Add-ADSyncAADServiceAccount` öğesini çalıştırın.  
   ![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD genel yönetici kimlik bilgilerini sağlayın.

Bu cmdlet, hizmet hesabı parolasını sıfırlar ve Azure AD hem de eşitleme altyapısı güncelleştirin.

## <a name="known-issues-these-steps-can-solve"></a>Bu adımları çözebilir bilinen sorunlar
Bu bölümde, sıfırlama Azure AD hizmeti hesabı kimlik bilgileri tarafından düzeltilen müşteriler tarafından bildirilen hataları listesidir.

- - -
Olay 6900  
Sunucu, bir parola değişikliği bildirimi işlenirken beklenmeyen bir hatayla karşılaştı:  
AADSTS70002: Hata doğrulama kimlik bilgileri. AADSTS50054: Eski parola kimlik doğrulaması için kullanılır.

- - -
Olay 659  
Parola İlkesi eşitleme yapılandırması alınırken hata oluştu. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Hata doğrulama kimlik bilgileri. AADSTS50054: Eski parola kimlik doğrulaması için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

