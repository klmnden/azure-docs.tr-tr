---
title: "Azure MFA için erişim ve kullanım raporları | Microsoft Docs"
description: "Azure multi-Factor Authentication özelliğinin - raporların nasıl kullanılacağı açıklanmaktadır."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 76a13467fff23ad62a857a53e0e31865b1a9fe81
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication raporlarında

Azure çok faktörlü kimlik doğrulaması ve kuruluşunuz tarafından kullanılan çeşitli raporlar sağlar. Bu rapor multi-Factor Authentication Yönetim Portalı erişilebilir. Aşağıdaki tabloda kullanılabilir raporları listeler:

| Rapor | Açıklama |
|:--- |:--- |
| Kullanım |Kullanım raporları bilgileri görüntüler genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları. |
| Sunucu Durumu |Bu rapor, çok faktörlü kimlik doğrulaması için hesabınızla ilişkili sunucularının durumunu görüntüler. |
| Engellenen Kullanıcı Geçmişi |Bu raporlar engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini görüntüleyin. |
| Atlanan Kullanıcı Geçmişi |Çok faktörlü kimlik doğrulaması için bir kullanıcının telefon numarası atlama isteklerinin geçmişini gösterir. |
| Sahtekarlık Uyarısı |Belirttiğiniz tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini gösterir. |
| Sıraya Alındı |Sıraya alınan işleme ve durumlarını listeler raporları. Rapor tamamlandığında indirme veya raporu görüntülemek için bir bağlantı sağlanır. |

## <a name="view-reports"></a>Raporları görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar** > **multi-Factor Kimlik doğrulama**.
3. Altında **çok faktörlü kimlik doğrulaması**seçin **hizmet ayarları**. Altında altındaki **Gelişmiş ayarları ve raporları görüntüleme Yönet**seçin **Portal'a Git**.
4. Azure multi-Factor Authentication Yönetim Portalı'nda, istediğiniz raporu seçin **bir raporu görüntülemek** sol gezinti bölümünde.

<center>![Bulut](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell kullanarak MFA'ya kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Ek Kaynaklar**

* [Kullanıcılar için](end-user/multi-factor-authentication-end-user.md)
* [MSDN'deki Azure çok faktörlü kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dn249471.aspx)
