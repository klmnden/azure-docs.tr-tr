---
title: Azure AD Connect yönetim Aracısı - nedir Azure AD Connect | Microsoft Docs
description: Eşitleme ve Azure AD ile şirket içi ortamınızı izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e33143626e136523b4af086e841b92e9ad30fa86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60295033"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Azure AD Connect Yönetim Aracısı nedir? 
Azure AD Connect yönetim Aracısı, Azure Active Directory bir Azure Active Directory Connect sunucusunda yüklü olan Connect, yeni bir bileşenidir. Bir Microsoft destek mühendisi olarak destek servis talebi açtığınızda sorunlarını gidermek için yardımcı olan Active Directory ortamınızdaki belirli verileri toplamak için kullanılır.

Azure Active Directory'den veri eşitleme ortamından istenen verileri alır ve Azure Active Directory'ye gönderir yüklendiğinde, belirli istekler için Azure AD Connect yönetim Aracısı bekler Microsoft Burada sunulan desteği Mühendislik.

Azure AD Connect yönetim Aracısı ortamınızdan alan bilgilerin herhangi bir yolla depolanmaz - yalnızca araştırma ve Azure Active Directory Connect sorun gidermeye yardımcı olmak üzere Microsoft destek mühendisine için görüntülenir açtığınız ilgili destek talebi

## <a name="how-is-the-azure-ad-connect-admin-agent-installed-on-the-azure-ad-connect-server"></a>Nasıl Azure AD Connect yönetim Aracısı Azure AD Connect sunucusunda yüklü mü? 
Yönetim Aracısı yüklendikten sonra aşağıdaki iki yeni program sunucunuzun Denetim Masası'ndaki "Program Ekle/Kaldır" listesinde görürsünüz: 

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent1.png)

Azure AD Connect yüklemenizi kritik bir parçası olduğu gibi programları XML'deki yüklemek veya kaldırmayın.

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Hangi verileri eşitleme Hizmetim için Microsoft servis mühendisi gösterilir?
Bir destek talebi, Microsoft destek mühendisine açtığınızda, belirli bir kullanıcı, ilgili verileri Active Directory'de, Active Directory Bağlayıcısı alanına Azure Active Directory Connect sunucusunda Azure Azure Active Directory Bağlayıcı alanında görebilirsiniz Active Directory Connect sunucusu ve Azure Active Directory Connect sunucusunda meta veri deposu.

Microsoft destek mühendisine, herhangi bir veri sisteminizde değerini değiştiremez ve parolaları göremez.

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Verilerim erişmek için Microsoft destek mühendisine ne istemez miyiz? 
 
Bir destek çağrısı için verilerinize erişmek için Microsoft servis mühendisi istemiyorsanız, bunu aşağıda açıklandığı gibi hizmet yapılandırma dosyasını değiştirerek devre dışı bırakabilirsiniz: 

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