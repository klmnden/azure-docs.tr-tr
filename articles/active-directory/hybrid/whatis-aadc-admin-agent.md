---
title: Azure AD Connect yönetim Aracısı - nedir Azure AD Connect | Microsoft Docs
description: Eşitleme ve Azure AD ile şirket içi ortamınızı izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36ab3fff4294b4cda3d1554ef2761d3f4acaca35
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687242"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Azure AD Connect Yönetim Aracısı nedir? 
Azure AD Connect yönetim Aracısı, Azure Active Directory, bir Azure Active Directory Connect sunucusunda yüklenebilen Connect, yeni bir bileşenidir. Bir Microsoft destek mühendisi olarak destek servis talebi açtığınızda sorunlarını gidermek için yardımcı olan Active Directory ortamınızdaki belirli verileri toplamak için kullanılır. 

>[!NOTE]
>Yönetici aracı olmayan yüklenir ve varsayılan olarak etkindir.  Destek çalışmalarıyla yardımcı olmak için veri toplamak için aracı yüklemeniz gerekir.

Azure Active Directory'den veri eşitleme ortamından istenen verileri alır ve Azure Active Directory'ye gönderir yüklendiğinde, belirli istekler için Azure AD Connect yönetim Aracısı bekler Microsoft Burada sunulan desteği Mühendislik. 

Azure AD Connect yönetim Aracısı ortamınızdan alan bilgilerin herhangi bir yolla depolanmaz - yalnızca araştırma ve Azure Active Directory Connect sorun gidermeye yardımcı olmak üzere Microsoft destek mühendisine için görüntülenir Azure AD Connect yönetim Aracısı açtığınız ilgili destek talebi varsayılan olarak Azure AD Connect sunucusunda yüklü değil. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>Azure AD Connect sunucusunda Azure AD Connect yönetim Aracısı yükleme 
Azure AD Connect yönetim Aracısı ikili dosyaları AAD Connect sunucusunda yerleştirilir. Aracıyı yüklemek için aşağıdakileri yapın: 



1. PowerShell'i Yönetici modunda açın 
2. Uygulamanın bulunduğu cd "C:\Program Files\Microsoft Azure Active Directory Connect\SetupFiles" olduğu dizine gidin 
3. AADConnectAdminAgentSetup.exe uygulamayı çalıştırın 
 
İstendiğinde, Azure AD genel yönetici kimlik bilgilerinizi girin. 

>[!NOTE]
>Burada kimlik bilgileriniz birden çok kez istenir bilinen bir sorun yoktur. Bu, sonraki sürümde düzeltilecektir.

Aracı yüklendikten sonra aşağıdaki iki yeni program sunucunuzun Denetim Masası'ndaki "Program Ekle/Kaldır" listesinde görürsünüz: 

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Hangi verileri eşitleme Hizmetim için Microsoft servis mühendisi gösterilir? 
Destek talebinde bulunun Microsoft desteği mühendisine, belirli bir kullanıcı, ilgili verileri Active Directory'de, Active Directory Bağlayıcısı alanına Azure Active Directory Connect sunucusunda Azure Azure Active Directory Bağlayıcı alanında görebilirsiniz Active Directory Connect sunucusu ve Azure Active Directory Connect sunucusunda meta veri deposu. 

Microsoft destek mühendisine, herhangi bir veri sisteminizde değerini değiştiremez ve parolaları göremez. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Verilerim erişmek için Microsoft destek mühendisine ne istemez miyiz? 
Bir destek çağrısı için verilerinize erişmek için Microsoft servis mühendisi istemiyorsanız Aracısı yüklendikten sonra aşağıda açıklandığı gibi hizmet yapılandırma dosyasını değiştirerek işlevselliği devre dışı bırakabilirsiniz: 

1.  Açık **C:\Program Files\Microsoft Azure AD Connect yönetim Agent\AzureADConnectAdministrationAgentService.exe.config** Defteri'nde.
2.  Devre dışı **UserDataEnabled** aşağıda gösterildiği gibi ayarlar. Varsa **UserDataEnabled** ayarı var ve true ve false olarak ayarlanır. Bir ayar yoksa, bu ayarı aşağıda gösterildiği gibi ekleyin.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.  Yapılandırma dosyasını kaydedin.
4.  Aşağıda gösterildiği gibi Azure AD Connect yönetim Aracısı hizmetini yeniden başlatın

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
