---
title: "Azure AD Connect: Sorunsuz çoklu oturum açma - GDPR uyumluluk | Microsoft Docs"
description: Bu makalede, Azure Active Directory (Azure AD) sorunsuz SSO ve GDPR uyumluluk ile ilgilidir.
services: active-directory
keywords: "Azure AD, SSO, gerekli bileşenleri GDPR, Azure AD Connect nedir çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: billmath
ms.openlocfilehash: 0c7ed376accb1eed01106358491e925d3b8126c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-ad-seamless-single-sign-on-gdpr-compliance"></a>Azure AD sorunsuz çoklu oturum açma: GDPR uyumluluk

## <a name="overview"></a>Genel Bakış

Mayıs 2018, Avrupa gizlilik yasalarına içinde [genel veri koruma düzenleme (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), etkili olması son. GDPR şirketler, devlet dairesi, kar kaybı olmayan ve diğer kuruluşlardan teklif mal ve hizmet Avrupa Birliği (AB) ya da, kişilere toplamak ve analiz etmek için AB Satışlar bağlı veri yeni kuralları uygular. Bulunduğunuz olsun GDPR geçerlidir. 

Microsoft Ürün ve hizmetlerini bugün GDPR gereksinimlerini karşılamanıza yardımcı olmak kullanılabilir. Microsoft Privacy İlkesi hakkında daha fazla bilgiyi [Güven Merkezi](https://www.microsoft.com/trustcenter).

Azure AD sorunsuz SSO EUII içerebilir aşağıdaki günlük türünü oluşturur:

- Azure AD Connect izleme günlüğü dosyalarının.

GDPR uyumluluk sorunsuz SSO için iki yolla erişilebilir:

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

- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
