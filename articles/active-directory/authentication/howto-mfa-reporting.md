---
title: Azure MFA için erişim ve kullanım raporları | Microsoft Docs
description: Azure multi-Factor Authentication özelliğinin - raporların nasıl kullanılacağı açıklanmaktadır.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 4eb91e37331a5af064d2af0e937eb071d805688f
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097888"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication raporlarında

Azure çok faktörlü kimlik doğrulaması ve kuruluşunuzun Azure portalı üzerinden erişilebilir, tarafından kullanılan çeşitli raporlar sağlar. Aşağıdaki tabloda kullanılabilir raporları listeler:

| Rapor | Konum | Açıklama |
|:--- |:--- |:--- |
| Engellenen Kullanıcı Geçmişi | Azure AD > MFA sunucusu > Kullanıcı engelle/Engellemeyi Kaldır | Engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve sahtekarlık uyarıları | Azure AD > oturum açma işlemleri | Bilgiler, genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar; aynı zamanda belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |
| Şirket içi bileşenleri için kullanım | Azure AD > MFA sunucusu > Etkinlik Raporu | ADFS, NPS uzantısı aracılığıyla MFA için genel kullanımı bilgi sağlayan ve MFA sunucusu. |
| Atlanan Kullanıcı Geçmişi | Azure AD > MFA sunucusu > bir kerelik atlama | Bir kullanıcı için multi-Factor Authentication atlama isteklerinin geçmişini sağlar. |
| Sunucu durumu | Azure AD > MFA sunucusu > sunucu durumu | Çok faktörlü kimlik doğrulama sunucularının durumunu hesabınızla ilişkili görüntüler. |

## <a name="view-reports"></a>Raporları görüntüleme 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **MFA sunucusu**.
3. Görüntülemek istediğiniz raporu seçin.

   <center>![Bulut](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar için](end-user/current/multi-factor-authentication-end-user.md)
* [Where dağıtmak için](concept-mfa-whichversion.md)
