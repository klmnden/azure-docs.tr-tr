---
title: 'Azure AD Connect eşitleme: Azure AD hizmet hesabını yönetme | Microsoft Docs'
description: Bu konuda, Azure AD hizmet hesabını geri yüklemek nasıl belgelenir.
services: active-directory
keywords: AADSTS70002, AADSTS50054, Azure AD Connect eşitleme Bağlayıcısı hizmeti hesabı parolasını sıfırlama
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
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f88318c87e29567b40b5eacf10f3b6f259adee8b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196367"
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect eşitleme: Azure AD hizmet hesabını yönetme
Azure AD Bağlayıcısı tarafından kullanılan hizmet hesabı, ücretsiz hizmet olduğu varsayılır. Kimlik bilgilerini sıfırlamanız gerekirse, bu konu, hakkındadır. Örneğin, bir genel yönetici tarafından hata varsa, PowerShell kullanarak hizmet hesabının parolasını sıfırlayın.

## <a name="reset-the-credentials"></a>Kimlik bilgilerini Sıfırla
Azure AD Bağlayıcısı üzerinde tanımlanan hizmet hesabı, Azure AD kimlik doğrulama sorunları nedeniyle iletişim kuramıyorsa, parolayı sıfırlayabilir.

1. Azure AD Connect eşitleme sunucusu için oturum açın ve PowerShell'i başlatın.
2. `Add-ADSyncAADServiceAccount` öğesini çalıştırın.  
   ![PowerShell cmdlet addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD genel yönetici kimlik bilgilerini sağlayın.

Bu cmdlet, hizmet hesabı parolasını sıfırlar ve hem de Azure AD eşitleme altyapısında güncelleştirin.

## <a name="known-issues-these-steps-can-solve"></a>Bu adımları bilinen sorunları çözme
Bu bölümde, bir kimlik bilgilerini sıfırlama Azure AD hizmet hesabı tarafından düzeltilen müşteriler tarafından bildirilen hataları listesidir.

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

