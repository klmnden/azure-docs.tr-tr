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
ms.reviewer: alexwe
ms.openlocfilehash: 77d6742faadfaf3d7afccfbe888b910c80278737
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication raporlarında

Azure çok faktörlü kimlik doğrulaması ve kuruluşunuz tarafından kullanılan çeşitli raporlar sağlar. Bu rapor multi-Factor Authentication Yönetim Portalı erişilebilir. Aşağıdaki tabloda kullanılabilir raporları listeler:

| Rapor | Açıklama |
|:--- |:--- |
| Kullanım |Kullanım raporları bilgileri görüntüler genel kullanımı, kullanıcı özeti ve kullanıcı ayrıntıları. |
| Sunucu durumu |Bu rapor, çok faktörlü kimlik doğrulaması için hesabınızla ilişkili sunucularının durumunu görüntüler. |
| Engellenmiş kullanıcı geçmişi |Bu raporlar engelleme veya kullanıcıların engelini kaldırma isteklerinin geçmişini görüntüleyin. |
| Atlanmış kullanıcı geçmişi |Çok faktörlü kimlik doğrulaması için bir kullanıcının telefon numarası atlama isteklerinin geçmişini gösterir. |
| Sahtekarlık Uyarısı |Belirttiğiniz tarih aralığı içinde gönderilen sahtekarlık uyarısı geçmişini gösterir. |
| Sıraya alınmış |Sıraya alınan işleme ve durumlarını listeler raporları. Rapor tamamlandığında indirme veya raporu görüntülemek için bir bağlantı sağlanır. |

## <a name="view-reports"></a>Raporları görüntüleme

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Solda, Active Directory'yi seçin.
3. Kimlik doğrulama sağlayıcıları kullanmadığınıza bağlı olarak bu iki seçenekten birini izleyin:
   * **Seçenek 1**: çok faktörlü kimlik doğrulama sağlayıcıları sekmesini tıklatın. MFA sağlayıcınızı tıklatıp **Yönet** altındaki düğmesini.
   * **Seçenek 2**: dizininizi seçin ve Git **yapılandırma** sekmesi. Çok faktörlü kimlik doğrulaması bölümünün altında **Hizmet ayarlarını yönet**'i seçin. MFA hizmeti ayarları sayfasının en altında Git portal bağlantısına tıklayın.
4. Azure multi-Factor Authentication Yönetim Portalı'nda, istediğiniz raporu seçin **bir raporu görüntülemek** sol gezinti bölümünde.

<center>![Bulut](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell raporlama

Aşağıdaki Powershell kullanarak MFA'ya kayıtlı kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Aşağıdaki Powershell kullanarak MFA'ya kayıtlı değil kullanıcıları belirleyin.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Ek Kaynaklar**

* [Kullanıcılar için](end-user/multi-factor-authentication-end-user.md)
* [MSDN'deki Azure çok faktörlü kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dn249471.aspx)
