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
ms.topic: article
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 3ced4d7be9864f16b4fbd7a91ae916e1b4f65d18
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177925"
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

* [Azure AD Connect eşitleme: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

