---
title: Azure MFA için erişim ve kullanım raporlarını | Microsoft Docs
description: Bu, Azure multi-Factor Authentication özelliğinin - raporların nasıl kullanılacağı açıklanır.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 146a86058adc73626e532f33e9fdbc83d9cf27e8
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049004"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor authentication'da raporları

Azure multi-Factor Authentication, Azure portalı üzerinden erişilebilir, kuruluşunuz tarafından kullanılan çeşitli raporlar sağlar. Aşağıdaki tablo, kullanılabilen raporları listeler:

| Rapor | Konum | Açıklama |
|:--- |:--- |:--- |
| Engellenen Kullanıcı Geçmişi | Azure AD > MFA sunucusu > kullanıcıları engelle/engelini kaldır | Engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve dolandırıcılık uyarıları | Azure AD > oturum açma işlemleri | Bilgi genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları sağlar. yanı sıra belirtilen tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini. |
| Şirket içi bileşenleri için kullanım | Azure AD > MFA sunucusu > Etkinlik Raporu | Bilgi genel kullanım için MFA NPS uzantısı ile ADFS, sağlar ve MFA sunucusu. |
| Atlanan Kullanıcı Geçmişi | Azure AD > MFA sunucusu > bir kerelik atlama | Bir kullanıcı için multi-Factor Authentication'ı atlama isteklerinin geçmişini sağlar. |
| Sunucu durumu | Azure AD > MFA sunucusu > sunucu durumu | Multi-Factor Authentication sunucularının durumunu hesabınızla ilişkili görüntüler. |

## <a name="view-reports"></a>Raporları görüntüleme 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **MFA sunucusu**.
3. Görüntülemek istediğiniz raporu seçin.

   <center>![Bulut](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki PowerShell kullanarak MFA için kaydolduğunu kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA için kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcılar için](../user-help/multi-factor-authentication-end-user.md)
* [Dağıtılacağı yeri](concept-mfa-whichversion.md)
