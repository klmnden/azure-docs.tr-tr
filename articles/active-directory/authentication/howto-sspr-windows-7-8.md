---
title: Windows 7 ve 8. 1 - Azure Active Directory, Azure AD Self Servis parola sıfırlama
description: Kullanarak Self Servis parola sıfırlama olanağı tanıma Windows 7 veya 8.1 oturum açma ekranında Parolayı unuttum
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57d3e955059724756eb7102c1b9fbbf55ed203ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414723"
---
# <a name="how-to-enable-password-reset-from-windows-7-8-and-81"></a>Nasıl yapılır: Parola sıfırlama Windows 7, 8 ve 8.1 etkinleştir

Self Servis parola sıfırlama (SSPR) etkin bir yönetici olarak, ancak kullanıcılar erişmek için bir tarayıcı penceresi alınamıyor çünkü bu kullanıcının parolasını sıfırlamak için yardım masasına çağırma tutmak [SSPR portalı](https://aka.ms/sspr). Windows 10 makineler için öğreticiyi kullanarak oturum açma ekranında "parolayı Sıfırla" bağlantısı etkinleştirebilirsiniz [Azure AD parola sıfırlama oturum açma ekranından](tutorial-sspr-windows.md), aşağıdaki yönergeleri sıfırlamak, Windows 7, 8 ve 8.1 kullanıcıları etkinleştirmenize yardımcı olur SSPR Windows oturum açma ekranında kullanarak parolalarını.

Aksine, Windows 10 makineler, Windows 7, 8 ve 8.1 makineler bir Azure AD etki alanına katılmış yok veya Active Directory etki alanına katılmış gereksinimi parola sıfırlama.

![Örneğin Windows 7 oturum açma ekranı ile "Parolanızı mı unuttunuz?" gösterilen bağlantı](media/howto-sspr-windows-7-8/windows-7-logon-screen.png)

## <a name="prerequisites"></a>Önkoşullar

* Azure AD self servis parola sıfırlama etkinleştirilmelidir.
* Düzeltme eki uygulama Windows 7 veya Windows 8.1 işletim sistemi.
* TLS 1.2 yönergeler kullanılarak etkin bulunan [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12).

> [!WARNING]
> TLS 1.2 etkinleştirilmelidir, otomatik olarak tam kümesi anlaşması.

## <a name="install"></a>Yükleme

1. Etkinleştirmek istediğiniz Windows sürümü için uygun yükleyiciyi indirin.

   1. Microsoft İndirme Merkezi'nde, yazılım kullanılabilir [https://aka.ms/sspraddin](https://aka.ms/sspraddin)

1. Yükleme ve yükleyiciyi çalıştırmak istediğiniz makinede oturum açın.
1. Yüklemeden sonra sistemin yeniden başlatılması önemle tavsiye edilir.
1. Yeniden başlatma işleminden sonra oturum açma ekranında bir kullanıcı seçin ve "parolanızı mı unuttunuz?" tıklayın Parola başlatmak için iş akışı sıfırlayın.
1. İş akışı parolanızı sıfırlamak için ekran aşağıdaki adımları tamamlayın.

![Örneğin Windows 7, "parolanızı mı unuttunuz?" tıklandı SSPR akışı](media/howto-sspr-windows-7-8/windows-7-sspr.png)

### <a name="silent-installation"></a>Sessiz yükleme

* Sessiz yüklemede komut "msiexec /i SsprWindowsLogon.PROD.msi /qn" kullanın.
* Sessiz kaldırma için komut "msiexec /x SsprWindowsLogon.PROD.msi /qn" kullanın.

## <a name="caveats"></a>Uyarılar

Önce "Parolanızı mı unuttunuz" bağlantıyı mümkün olmayacak SSPR'ye kaydolması gerekir.

![Ek güvenlik bilgileri, parolanızı sıfırlamak için gereklidir](media/howto-sspr-windows-7-8/windows-7-sspr-need-security-info.png)

Parolanızı sıfırlamak için Microsoft Authenticator uygulamasını bildirimleri ve kodları kullanarak bu ilk sürümde çalışmaz. Kullanıcıların, ilke gereksinimlerini alternatif yöntemler kayıtlı olması gerekir.

## <a name="troubleshooting"></a>Sorun giderme

Makinede ve Azure AD'de olayları günlüğe kaydedilir.

Azure AD olayları IP adresi ve parola sıfırlama oluştuğu ClientType hakkında bilgiler içerir.

![Örneğin Windows 7'yi parola sıfırlama Azure AD denetim günlüğünde](media/howto-sspr-windows-7-8/windows-7-sspr-azure-ad-audit-log.png)

Ek günlükler gerekiyorsa, ayrıntılı günlük kaydını etkinleştirmek için bir kayıt defteri anahtarı makinede değiştirilebilir. Yalnız sorun gidermek amacıyla ayrıntılı günlüğe yazmayı etkinleştirin.

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

* Ayrıntılı günlük kaydını etkinleştirmek için bir REG_DWORD oluşturun: "EnableLogging" ve 1 olarak ayarlayın.
* Ayrıntılı günlük kaydını devre dışı bırakmak için ' % s ' REG_DWORD "EnableLogging" 0 olarak değiştirin.

Windows 7, 8 ve 8.1 makinelerinizi bir proxy sunucusu veya güvenlik duvarı ise HTTPS trafiğini (443) passwordreset.microsoftonline.com izin verilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Windows 10 oturum açma ekranında parolalarını sıfırlama olanağı verir](tutorial-sspr-windows.md)
