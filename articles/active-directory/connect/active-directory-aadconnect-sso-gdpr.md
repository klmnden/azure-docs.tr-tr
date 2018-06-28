---
title: Kullanıcı gizliliği ve sorunsuz Azure AD çoklu oturum açma | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) sorunsuz SSO ve GDPR uyumluluk ile ilgilidir.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri GDPR, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a4fc779cdfb177a9817049fd7b62b0014e141ce0
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34592417"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Kullanıcı gizliliği ve Azure AD sorunsuz çoklu oturum açma

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış


Azure AD sorunsuz SSO kişisel verileri içerebilir aşağıdaki günlük türünü oluşturur: 

- Azure AD Connect izleme günlüğü dosyalarının.

Kullanıcı gizliliği için sorunsuz SSO iki yolla geliştirme:

1.  İstek, bir kişi için verileri ayıklamak ve yüklemeleri bu kişiden veri kaldırın.
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Uygulama ve Bakım daha kolay olduğu gibi ikinci seçeneği kesinlikle öneririz. Her bir günlük türü için yönergelerine bakın:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect izleme günlük dosyalarını silin

İçeriğini denetlemek **%ProgramData%\AADConnect** klasörü ve delete izleme günlüğünü içeriği (**izleme -\*.log** dosyaları), bu klasörün yüklerken veya yükseltirken Azure AD Connect, 48 saat içinde ya da bu eylem olarak sorunsuz SSO yapılandırmasını değiştirme GDPR tarafından kapsamdaki verileri oluşturabilir.

>[!IMPORTANT]
>Silmeyin **PersistedState.xml** bu dosya, Azure AD Connect'in önceki yükleme durumunu korumak için kullanılır ve yükseltme yüklemesi tamamlandığında, kullanılabilir olarak bu klasörde dosya. Bu dosya hiçbir zaman bir kişiyle ilgili tüm verileri içerir ve hiçbir zaman silinmesi gerekir.

Gözden geçirin ve Windows Gezgini'ni kullanarak bu izleme günlük dosyalarını silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Bir dosyada komut dosyasını kaydetme ". PS1 "uzantısı. Gerektiğinde bu komut dosyasını çalıştırın.

İlişkili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi için bkz: [bu makalede](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri hakkında dikkat edin

Denetim günlüğü etkinleştirilirse, bu ürün, etki alanı denetleyicileri için güvenlik günlüklerini oluşturabilir. Denetim ilkeleri yapılandırma hakkında daha fazla bilgi için bu okuma [makale](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
