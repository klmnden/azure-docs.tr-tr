---
title: Azure Active Directory koşullu erişim ayarları başvurusu | Microsoft Docs
description: Bir Azure Active Directory koşullu erişim ilkesinde desteklenen ayarlar özetini alın.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 07/10/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 535842989ef49ee13a5ddee7c4349a3b819f741c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797851"
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Azure Active Directory koşullu erişim ayarları başvurusu

Kullanabileceğiniz [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md) nasıl yetkili kullanıcıların denetlemek için kaynaklarınıza erişebilirsiniz.

Bu makalede, destek bilgileri için bir koşullu Erişim İlkesi'nde aşağıdaki yapılandırma seçeneklerini sağlar:

- Bulut uygulamaları atama
- Cihaz platformu koşulu
- İstemci uygulamaların durumu
- Onaylı istemci uygulaması gereksinimi

Aradığınız bilgiler bu değilse, bu makalenin sonunda bir yorum yazın.

## <a name="cloud-apps-assignments"></a>Bulut uygulamaları atama

Koşullu erişim ilkeleriyle birlikte nasıl kullanıcılarınızın erişim denetimi, [bulut uygulamaları](conditions.md#cloud-apps-and-actions). Koşullu erişim ilkesi yapılandırdığınızda, en az bir bulut uygulaması seçmeniz gerekir. 

![İlkeniz için bulut uygulamalarını seçin](./media/technical-reference/09.png)

### <a name="microsoft-cloud-applications"></a>Microsoft bulut uygulamaları

Microsoft'tan aşağıdaki bulut uygulamaları için koşullu erişim ilkesi atayabilirsiniz:

- Azure Analysis Services
- Azure DevOps
- Azure SQL veritabanı ve veri ambarı - [daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- Dynamics CRM Online
- Microsoft Application Insights Analytics
- Microsoft Azure Information Protection - [daha fazla bilgi edinin](https://docs.microsoft.com/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- Microsoft Azure Yönetimi - [daha fazla bilgi edinin](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)
- Microsoft Azure abonelik yönetimi
- Microsoft Cloud App Security
- Erişim denetimi Portalı Microsoft Commerce araçları
- Microsoft Commerce araçları kimlik doğrulama hizmeti
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- Microsoft Intune kaydı
- Microsoft Planner
- Microsoft Power BI
- Microsoft PowerApps
- Bing içinde Microsoft arama
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Office 365 Yammer
- Office inceleyin
- Office Sway
- Outlook Groups
- Project Online
- Skype Kurumsal Çevrimiçi Sürüm
- Sanal özel ağ (VPN)
- Visual Studio App Center
- Windows Defender ATP

### <a name="other-applications"></a>Diğer uygulamalar

Microsoft bulut uygulamalarının yanı sıra aşağıdaki türde bulut uygulamaları için koşullu erişim ilkesi atayabilirsiniz:

- Azure AD'ye bağlı uygulamalara
- Önceden tümleştirilmiş Federasyon yazılım olarak hizmet (SaaS) uygulaması
- Parola çoklu oturum açma (SSO) kullanan uygulamalar
- Satır iş kolu uygulamaları
- Azure AD uygulama ara sunucusu kullanan uygulamalar

## <a name="device-platform-condition"></a>Cihaz platformu koşulu

Bir koşullu erişim ilkesini bir istemcide ilke işletim sistemine bağlamak için cihaz platformu koşul yapılandırabilirsiniz. Azure AD koşullu erişim, aşağıdaki cihaz platformlarını destekler:

- Android
- iOS
- Windows Phone
- Windows
- Mac OS

![İstemci işletim sistemi için erişim ilkesi bağlayın](./media/technical-reference/41.png)

## <a name="client-apps-condition"></a>İstemci uygulamaları koşulu

Koşullu erişim ilkenizi yapılandırabilirsiniz [istemci uygulamaları](conditions.md#client-apps) erişim denemesi başlattı istemci uygulaması İlkesi bağlamak için koşul. İstemci uygulamaları koşul vermek veya istemci uygulamaları aşağıdaki türlerden erişim denemesi yapıldığında erişimi engellemek için ayarlayın:

- Browser
- Mobil uygulamalar ve Masaüstü uygulamaları

![İstemci uygulamaları için erişimi denetleme](./media/technical-reference/03.png)

### <a name="supported-browsers"></a>Desteklenen tarayıcılar

Koşullu erişim ilkenizi seçtiğiniz **tarayıcılar** istemci uygulaması olarak.

![Desteklenen tarayıcılar için erişimi denetleme](./media/technical-reference/05.png)

Bu ayar tüm tarayıcılarla çalışır. Ancak, bir cihaz uyumlu gereksinim gibi bir cihaz ilkeyi karşılamak için aşağıdaki işletim sistemleri ve tarayıcılar desteklenir:

| OS                     | Tarayıcılar                                      |
| :--                    | :--                                           |
| Windows 10             | Internet Explorer, Microsoft Edge, Chrome     |
| Windows 8 / 8.1        | Internet Explorer, Chrome                     |
| Windows 7              | Internet Explorer, Chrome                     |
| iOS                    | Safari, Microsoft Edge, Intune Managed Browser |
| Android                | Chrome, Microsoft Edge, Intune Managed Browser |
| Windows Phone          | Microsoft Edge, Internet Explorer             |
| Windows Server 2016    | Microsoft Edge, Internet Explorer             |
| Windows Server 2016    | Chrome                                        |
| Windows Server 2012 R2 | Internet Explorer, Chrome                     |
| Windows Server 2008 R2 | Internet Explorer, Chrome                     |
| Mac OS                  | Chrome, Safari                                |

#### <a name="why-do-i-see-a-certificate-prompt-in-the-browser"></a>Tarayıcıda iste sertifika neden görüyorum

Windows 7, iOS, Android ve macOS cihaz Azure AD'ye kaydedildiğinde, sağlanan bir istemci sertifikası kullanarak cihazı Azure AD'ye tanımlar.  Bir kullanıcı ilk kez bir tarayıcıdan oturum açtığında kullanıcıya sertifikayı seçmek için istenir. Kullanıcının Tarayıcıyı kullanmadan önce bu sertifikayı seçmesi gerekir.

#### <a name="chrome-support"></a>Chrome desteği

Chrome için destek **Windows 10 Creators Update (sürüm 1703)** veya daha sonra yükleme [Windows 10 hesapları uzantısı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). Koşullu erişim ilkesi, cihaz belirli ayrıntıları gerektirdiğinde bu gerekli bir uzantısıdır.

Bu uzantı Chrome tarayıcısına otomatik olarak dağıtmak için aşağıdaki kayıt defteri anahtarı oluşturun:

|    |    |
| --- | --- |
| `Path` | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
| Name | 1\. |
| Type | REG_SZ (String) |
| Data | ppnbnpeolgkicgegkbkbjmhlideopiji; https://clients2.google.com/service/update2/crx |

Chrome için destek **7 ve Windows 8.1**, aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
| --- | --- |
| `Path` | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
| Name | 1 |
| Type | REG_SZ (String) |
| Data | {"deseni": "https://device.login.microsoftonline.com","filter": {"ISSUER": {"CN =": "MS-Kuruluş-erişim"}}} |

Bu tarayıcılar, cihaz kimlik doğrulaması, cihazın tanımlanması ve bir ilke karşı doğrulandı izin verme desteklemez. Tarayıcı özel modda çalışıyorsa cihaz denetimi başarısız olur.

### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve Masaüstü istemcileri

Koşullu erişim ilkenizi seçtiğiniz **mobil uygulamalar ve masaüstü istemciler** istemci uygulaması olarak.

![Desteklenen mobil uygulama ve masaüstü istemciler için erişim denetimi](./media/technical-reference/06.png)

Bu ayar, aşağıdaki mobil uygulamalar ve masaüstü istemciler yapılan erişim denemesi üzerinde bir etkisi yoktur:

| İstemci uygulamaları | Hedef hizmeti | Platform |
| --- | --- | --- |
| Dynamics CRM uygulaması | Dynamics CRM | Windows 10, Windows 8.1, iOS ve Android |
| Takvim/posta/kişiler uygulaması, Outlook 2016'ın (modern kimlik doğrulaması ile) Outlook 2013| Office 365 Exchange Online | Windows 10 |
| Uygulamalar için MFA ve konum ilkesi. Cihaz tabanlı ilkeler desteklenmez.| Herhangi bir uygulamalarım uygulama hizmeti| Android ve iOS |
| Microsoft Teams Hizmetleri - bu, Microsoft Teams ve tüm istemci uygulamaları - Windows Masaüstü, iOS, Android, WP ve web istemcisi destekleyen tüm hizmetleri denetler | Microsoft Teams | Windows 10, Windows 8.1, Windows 7, iOS, Android ve macOS |
| Office 2016 uygulamaları, Office 2013 (modern kimlik doğrulaması ile) OneDrive eşitleme istemcisini (bkz [notları](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)) | Office 365 SharePoint Online | Windows 8.1, Windows 7 |
| Office 2016 uygulamaları, Evrensel Office uygulamaları, Office 2013 (modern kimlik doğrulaması ile), OneDrive eşitleme istemcisini (bkz [notları](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), Office grupları destek gelecek için planlanan, SharePoint uygulama destek gelecek için planlanan | Office 365 SharePoint Online | Windows 10 |
| Office 2016 (Word, Excel, PowerPoint, OneNote). OneDrive for Business desteği gelecek için planlanan| Office 365 SharePoint Online| Mac OS|
| Office 2019| Office 365 SharePoint Online | Windows 10, macOS |
| Office mobil uygulamaları | Office 365 SharePoint Online | Android, iOS |
| Office Yammer uygulaması | Office 365 Yammer | Windows 10, iOS, Android |
| Outlook 2019 | Office 365 SharePoint Online | Windows 10, macOS |
| Outlook 2016'ın (macOS için Office) | Office 365 Exchange Online | Mac OS |
| Outlook 2016, Outlook 2013 (modern kimlik doğrulaması ile) Skype Kurumsal'a (ile modern kimlik doğrulaması) | Office 365 Exchange Online | Windows 8.1, Windows 7 |
| Outlook mobil uygulaması | Office 365 Exchange Online | Android, iOS |
| Power BI uygulaması | Power BI hizmeti | Windows 10, Windows 8.1, Windows 7, Android ve iOS |
| Skype Kurumsal | Office 365 Exchange Online| Android, IOS |
| Visual Studio Team Services ile uygulama | Visual Studio Team Services | Windows 10, Windows 8.1, Windows 7, iOS ve Android |

## <a name="support-for-legacy-authentication"></a>Eski kimlik doğrulaması için destek

Seçerek **diğer istemcilerin**, IMAP, MAPI, POP, SMTP ve modern kimlik doğrulama kullanmayan eski Office uygulamaları gibi posta protokollerini ile temel kimlik doğrulaması kullanan uygulamalar etkileyen bir koşul belirtebilirsiniz.  

![Diğer istemciler](./media/technical-reference/11.png)

Daha fazla bilgi için [istemci uygulamaları](conditions.md#client-apps).

## <a name="approved-client-app-requirement"></a>Onaylı istemci uygulama gereksinimi

Koşullu erişim ilkenizi erişim onaylı istemci uygulama tarafından yapılması gereken seçilen bulut uygulamaları için çalışır gerektirebilir. 

![Onaylı istemci uygulamalar için erişim denetimi](./media/technical-reference/21.png)

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Managed Browser
- Microsoft faturalama
- Microsoft Kaizala
- Microsoft Başlatıcısı
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Kurumsal
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer

**Açıklamalar**

- Onaylı istemci uygulamaları, Intune mobil uygulama yönetimi özelliğini destekler.
- **Onaylı istemci uygulaması gerektir** gereksinimi:
   - Yalnızca iOS ve Android için destekleyen [cihaz platformu koşul](#device-platform-condition).

## <a name="app-protection-policy-requirement"></a>Uygulama koruma İlkesi gereksinimi 

Koşullu erişim ilkenizi erişim, seçili bulut uygulamaları için kullanılabilir olmadan önce bir uygulama koruma İlkesi işlem istemci uygulamada mevcut olması gerekebilir. 

![Uygulama koruma İlkesi ile erişimi denetleme](./media/technical-reference/22.png)

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:

- Microsoft OneDrive
- Microsoft Outlook

**Açıklamalar**

- Uygulama koruma ilkesi için uygulama koruma İlkesi ile Intune mobil uygulama yönetimi özelliği destekler.
- **Uygulama koruma İlkesi gerekli** gereksinimleri:
    - Yalnızca iOS ve Android için destekleyen [cihaz platformu koşul](#device-platform-condition).

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim genel bakış için bkz. [Azure Active Directory'de koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md)
- Ortamınızda koşullu erişim ilkeleri yapılandırmak hazırsanız bkz [Azure Active Directory'de koşullu erişim için önerilen uygulamalar](best-practices.md).

<!--Image references-->
[1]: ./media/technical-reference/01.png
