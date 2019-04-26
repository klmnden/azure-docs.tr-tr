---
title: Kullanıcı gizliliği ve Azure Active Directory geçişli kimlik doğrulaması | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ve GDPR uyumluluğu ile ilgilidir.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, GDPR, gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1a7b740a6b248a12fa3d95f85f602ef7a8b2fa5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60242374"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Kullanıcı gizliliği ve Azure Active Directory geçişli kimlik doğrulaması


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış

Kişisel veri içerebilen aşağıdaki günlük türü, Azure AD geçişli kimlik doğrulaması oluşturur:

- Azure AD Connect izleme günlüğü dosyaları.
- Kimlik Doğrulama Aracısı izleme günlüğü dosyaları.
- Windows olay günlüğü dosyaları.

Kullanıcı gizliliği, geçişli kimlik doğrulaması için iki yolla geliştirme:

1.  İstek, bir kişi için verileri ayıklayın ve bu kişiden yüklemeleri veri kaldırın.
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Uygulamak ve sürdürmek daha kolay olduğu gibi ikinci seçeneği önerilir. Her günlük türü için yönergeler aşağıda verilmiştir:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect izleme günlük dosyalarını silin

İçeriğini denetlemek **%ProgramData%\AADConnect** klasörü ve delete izleme günlüğünü içeriği (**izleme -\*.log** dosyaları) Azure AD Connect'i yükleme veya 48 saat içinde bu klasörün ya da bu eylem, geçişli kimlik doğrulaması yapılandırmasını değiştirme veriler GDPR kapsamında oluşturabilir.

>[!IMPORTANT]
>Silme **PersistedState.xml** dosyası bu klasörde, bu dosya, Azure AD Connect'in önceki yükleme durumunu korumak için kullanılır ve bir yükseltme yüklemesi tamamlandığında kullanılır. Bu dosya hiçbir zaman bir kişiyle ilgili tüm verileri içerir ve hiçbir zaman silinmesi gerekir.

Gözden geçirin ve Windows Gezgini'ni kullanarak bu izleme günlük dosyalarını silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Betik bir dosyaya kaydet ". PS1 "uzantısı. Gerektiğinde bu betiği çalıştırın.

İlgili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi edinmek için bkz. [bu makalede](reference-connect-user-privacy.md).

### <a name="delete-authentication-agent-event-logs"></a>Kimlik doğrulama aracısı olayları Sil

Bu ürün oluşturabilir **Windows olay günlükleri**. Daha fazla bilgi için lütfen okuyun [bu makalede](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Geçişli kimlik doğrulaması Aracısı ile ilgili günlükleri görüntülemek için Aç **Olay Görüntüleyicisi'ni** altında sunucu ve uygulama **uygulama ve hizmet Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin** .

### <a name="delete-authentication-agent-trace-log-files"></a>Kimlik Doğrulama Aracısı izleme günlük dosyalarını silin

İçeriği düzenli olarak denetlemesi gerekir <strong>%ProgramData%\Microsoft\Azure AD Connect kimlik doğrulaması Agent\Trace\</ strong > ve 48 saatte bu klasörün içeriğini silin. 

>[!IMPORTANT]
>Kimlik Doğrulama Aracısı Hizmeti çalışıyorsa, geçerli günlük dosyası klasörü içinde silmek mümkün olacaktır değil. Yeniden denemeden önce hizmetini durdurun. Kullanıcı oturum açma hatalarını önlemek için zaten geçişli kimlik doğrulaması için yapılandırmış olmanız [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

Gözden geçirip Windows Gezgini'ni kullanarak bu dosyaları silebilir ya da gerekli eylemleri gerçekleştirmek için aşağıdaki betiği kullanabilirsiniz:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Bu komut dosyasını çalıştırmak için zamanlama 48 saatte şu adımları izleyin:

1.  Betik bir dosyaya kaydet ". PS1 "uzantısı.
2.  Açık **Denetim Masası** tıklayın **sistem ve güvenlik**.
3.  Altında **Yönetimsel Araçlar** başlığı tıklayın "**görevleri zamanlayın**".
4.  İçinde **Görev Zamanlayıcı**, sağ tıklayın "**görev zamanlama Kitaplığı**"ve tıklayın"**... temel görevi oluştur** ".
5.  Yeni görev için bir ad girin ve tıklayın **sonraki**.
6.  Seçin "**günlük**" için **görev tetikleyici** tıklatıp **sonraki**.
7.  Yinelenme iki gün olarak ayarlanmış ve tıklayın **sonraki**.
8.  Seçin "**programı Başlat**"'i tıklatın ve eylemi olarak **sonraki**.
9.  Türü "**PowerShell**" Program/komut kutusundaki ve etiketli kutusuna"**bağımsız değişkenler (isteğe bağlı) ekleme**", daha önce oluşturduğunuz bir komut dosyası için tam yolu girin ve ardından tıklayın **sonraki**.
10. Sonraki ekranda, görev oluşturmak üzere olduğunuz bir özetini gösterir. Değerleri doğrulayın ve tıklayın **son** görev oluşturmak için:
 
### <a name="note-about-domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri hakkında dikkat edin.

Denetim günlüğü etkinleştirilmişse, bu ürün, etki alanı denetleyicileri için güvenlik günlükleri oluşturabilir. Denetim ilkeleri yapılandırma hakkında daha fazla bilgi edinmek için bu okuma [makale](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
* [**Sorun giderme** ](tshoot-connect-pass-through-authentication.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
