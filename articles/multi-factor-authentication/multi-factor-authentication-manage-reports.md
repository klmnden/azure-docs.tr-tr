---
title: "Azure MFA için erişim ve kullanım raporları | Microsoft Docs"
description: "Azure multi-Factor Authentication özelliğinin - raporların nasıl kullanılacağı açıklanmaktadır."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
<<<<<<< HEAD
ms.openlocfilehash: a0ac1711b6bfb8f461cd775ed1f3409925643615
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
=======
ms.openlocfilehash: fb83e957a206bff29132973d2dd3e9a7b5f9f060
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication raporlarında

Azure çok faktörlü kimlik doğrulaması ve kuruluşunuzun Azure portalı üzerinden erişilebilir, tarafından kullanılan çeşitli raporlar sağlar. Aşağıdaki tabloda kullanılabilir raporları listeler:

| Rapor | Konum | Açıklama |
|:--- |:--- |:--- |
| Engellenen Kullanıcı Geçmişi | Azure AD > MFA sunucusu > Kullanıcı engelle/Engellemeyi Kaldır | Engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve sahtekarlık uyarıları | Azure AD > oturum açma işlemleri | Bilgiler, genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar; aynı zamanda belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |
| Atlanan Kullanıcı Geçmişi | Azure AD > MFA sunucusu > bir kerelik atlama | Bir kullanıcı için multi-Factor Authentication atlama isteklerinin geçmişini sağlar. |
| Sunucu durumu | Azure AD > MFA sunucusu > sunucu durumu | Çok faktörlü kimlik doğrulama sunucularının durumunu hesabınızla ilişkili görüntüler. |

## <a name="view-reports"></a>Raporları görüntüleme 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **MFA sunucusu**.
3. Görüntülemek istediğiniz raporu seçin.

   <center>![Bulut](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar için](end-user/multi-factor-authentication-end-user.md)
* [Where dağıtmak için](multi-factor-authentication-get-started.md)
