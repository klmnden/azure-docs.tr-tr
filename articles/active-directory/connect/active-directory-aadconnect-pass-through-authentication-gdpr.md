---
title: Kullanıcı gizliliği ve Azure Active Directory doğrudan kimlik doğrulaması | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulama ve GDPR uyumluluk ile ilgilidir.
services: active-directory
keywords: Azure AD Connect doğrudan kimlik doğrulama, GDPR, gerekli bileşenleri Azure AD, SSO, çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2018
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 910eb5bdd1b9d4a2a27a27c89812584bb068bec0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Kullanıcı gizliliği ve Azure Active Directory doğrudan kimlik doğrulaması


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış

Azure AD doğrudan kimlik doğrulama EUII içerebilir aşağıdaki günlük türlerini oluşturur:

- Azure AD Connect izleme günlüğü dosyalarının.
- Kimlik Doğrulama Aracısı izleme günlüğü dosyalarının.
- Windows olay günlüğü dosyaları.

Doğrudan kimlik doğrulama için kullanıcı gizliliğini iki yolla erişilebilir:

1.  İstek, bir kişi için verileri ayıklamak ve yüklemeleri bu kişiden veri kaldırın.
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Uygulama ve Bakım daha kolay olduğu gibi ikinci seçeneği kesinlikle öneririz. Her bir günlük türü için yönergeler şunlardır:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect izleme günlük dosyalarını silin

İçeriğini denetlemek **%ProgramData%\AADConnect** klasörü ve delete izleme günlüğünü içeriği (**izleme -\*.log** dosyaları), bu klasörün yüklerken veya yükseltirken Azure AD Connect, 48 saat içinde ya da bu eylem olarak doğrudan kimlik doğrulama yapılandırmasını değiştirme GDPR tarafından kapsamdaki verileri oluşturabilir.

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

### <a name="delete-authentication-agent-event-logs"></a>Kimlik Doğrulama Aracısı olay günlüklerini silme

Bu ürün ayrıca oluşturabilirsiniz **Windows olay günlüklerini**. Daha fazla bilgi için lütfen okuyun [bu makalede](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Doğrudan kimlik doğrulama Aracısı ile ilgili günlükleri görüntülemek için açık **Olay Görüntüleyicisi'ni** altında onay ve sunucudaki uygulama **uygulama ve hizmet Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin** .

### <a name="delete-authentication-agent-trace-log-files"></a>Kimlik Doğrulama Aracısı izleme günlük dosyalarını silin

İçeriğini düzenli olarak denetlemesi gerekir **%ProgramData%\Microsoft\Azure AD Connect kimlik doğrulama Agent\Trace\**  ve her 48 saat bu klasörün içeriğini silin. 

>[!IMPORTANT]
>Kimlik Doğrulama Aracısı Hizmeti çalışıyorsa, klasöründeki geçerli günlük dosyasını silemedi olmaması. Yeniden denemeden önce hizmetini durdurun. Kullanıcı oturum açma hatalarını önlemek için önceden geçişli kimlik doğrulaması için yapılandırmanız [yüksek kullanılabilirlik](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

Gözden geçirin ve Windows Gezgini'ni kullanarak bu dosyaları silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki komut dosyasını kullanabilirsiniz:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Bu komut dosyasının çalışmasını zamanlamak için her 48 saat şu adımları izleyin:

1.  Bir dosyada komut dosyasını kaydetme ". PS1 "uzantısı.
2.  Açık **Denetim Masası** ve tıklayın **sistem ve güvenlik**.
3.  Altında **Yönetimsel Araçlar** başlığı tıklayın "**zamanlama görevleri**".
4.  İçinde **Görev Zamanlayıcı**, sağ tıklayın "**görev zamanlama Kitaplığı**"ve tıklayın"**... temel görev oluştur** ".
5.  Yeni görev için bir ad girin ve tıklayın **sonraki**.
6.  Seçin "**günlük**" için **görev tetiklemesi** tıklatıp **sonraki**.
7.  Yinelenme iki gün olarak ayarlayın ve tıklatın **sonraki**.
8.  Seçin "**bir programı başlatamayan**"'i tıklatın ve eylemi olarak **sonraki**.
9.  Türü "**PowerShell**" Program/komut dosyası kutusunda ve etiketli kutusuna"**bağımsız değişkenler (isteğe bağlı) eklemek**", daha önce oluşturduğunuz komut dosyasının tam yolunu girin ve ardından **İleri**.
10. Sonraki ekranda oluşturmak üzere olduğunuz görev bir özetini gösterir. Değerleri doğrulayın ve tıklayın **son** görevi oluşturmak için:
 
### <a name="note-about-domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri hakkında dikkat edin

Denetim günlüğü etkinleştirilirse, bu ürün, etki alanı denetleyicileri için güvenlik günlüklerini oluşturabilir. Denetim ilkeleri yapılandırma hakkında daha fazla bilgi için bu okuma [makale](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
