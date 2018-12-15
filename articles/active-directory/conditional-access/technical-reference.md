---
title: Azure Active Directory koşullu erişim ayarları başvurusu | Microsoft Docs
description: Desteklenen ayarlar genel bir bakış, Azure Active Directory koşullu erişim ilkesinde alın.
services: active-directory.
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 46dfb96df7b16fe03bd5c2c69fd9e2e33b04bbd2
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408587"
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

Koşullu erişim ilkeleriyle birlikte nasıl kullanıcılarınızın erişim denetimi, [bulut uygulamaları](conditions.md#cloud-apps). Koşullu erişim ilkesi yapılandırdığınızda, en az bir bulut uygulaması seçmeniz gerekir. 

![İlkeniz için bulut uygulamalarını seçin](./media/technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft bulut uygulamaları

Microsoft'tan aşağıdaki bulut uygulamaları için koşullu erişim ilkesi atayabilirsiniz:

- Azure Information Protection - [daha fazla bilgi edinin](/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint (OneDrive iş ve Project Online içerir) çevrimiçi

- Microsoft Power BI 

- Azure DevOps

- Microsoft Teams


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

- macOS


![İstemci işletim sistemi için erişim ilkesi bağlayın](./media/technical-reference/41.png)





## <a name="client-apps-condition"></a>İstemci uygulamaları koşulu 

Koşullu erişim ilkenizi yapılandırabilirsiniz [istemci uygulamaları](conditions.md#client-apps) erişim denemesi başlattı istemci uygulaması İlkesi bağlamak için koşul. İstemci uygulamaları koşul vermek veya istemci uygulamaları aşağıdaki türlerden erişim denemesi yapıldığında erişimi engellemek için ayarlayın:

- Tarayıcı
- Mobil uygulamalar ve Masaüstü uygulamaları

![İstemci uygulamaları için erişimi denetleme](./media/technical-reference/03.png)

### <a name="supported-browsers"></a>Desteklenen tarayıcılar 

Koşullu erişim ilkenizi seçtiğiniz **tarayıcılar** istemci uygulaması olarak.

![Desteklenen tarayıcılar için erişimi denetleme](./media/technical-reference/05.png)

Bu ayar tüm tarayıcılarla çalışır. Ancak, bir cihaz uyumlu gereksinim gibi bir cihaz ilkeyi karşılamak için aşağıdaki işletim sistemleri ve tarayıcılar desteklenir:


| İşletim Sistemi                     | Tarayıcılar                            | Destek     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, Microsoft Edge, Chrome     | ![İşaretli][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome           | ![İşaretli][1] |
| Windows 7              | Internet Explorer, Chrome           | ![İşaretli][1] |
| iOS                    | Safari, Intune yönetilen tarayıcı      | ![İşaretli][1] |
| Android                | Chrome, Intune yönetilen tarayıcı      | ![İşaretli][1] |
| Windows Phone          | Microsoft Edge, Internet Explorer             | ![İşaretli][1] |
| Windows Server 2016    | Microsoft Edge, Internet Explorer             | ![İşaretli][1] |
| Windows Server 2016    | Chrome                              | Çok yakında |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| macOS                  | Chrome, Safari                      | ![İşaretli][1] |



#### <a name="chrome-support"></a>Chrome desteği

Chrome için destek **Windows 10 Creators Update (sürüm 1703)** veya daha sonra yükleme [bu uzantı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Bu uzantı Chrome tarayıcısına otomatik olarak dağıtmak için aşağıdaki kayıt defteri anahtarı oluşturun:

|    |    |
|--- | ---|
|Yol | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
|Ad | 1 |
|Tür | REG_SZ (dize) |
|Veriler | ppnbnpeolgkicgegkbkbjmhlideopiji; https://clients2.google.com/service/update2/crx

Chrome için destek **7 ve Windows 8.1**, aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
|--- | ---|
|Yol | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
|Ad | 1 |
|Tür | REG_SZ (dize) |
|Veriler | {"deseni": "https://device.login.microsoftonline.com","filter": {"ISSUER": {"CN =": "MS-Kuruluş-erişim"}}}|

Bu tarayıcılar, cihaz kimlik doğrulaması, cihazın tanımlanması ve bir ilke karşı doğrulandı izin verme desteklemez. Tarayıcı özel modda çalışıyorsa cihaz denetimi başarısız olur. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve Masaüstü istemcileri

Koşullu erişim ilkenizi seçtiğiniz **mobil uygulamalar ve masaüstü istemciler** istemci uygulaması olarak.


![Desteklenen mobil uygulama ve masaüstü istemciler için erişim denetimi](./media/technical-reference/06.png)


Bu ayar, aşağıdaki mobil uygulamalar ve masaüstü istemciler yapılan erişim denemesi üzerinde bir etkisi yoktur: 


|İstemci uygulamaları|Hedef hizmeti|Platform|
|---|---|---|
|Azure RemoteApp|Azure RemoteApp hizmeti|Windows 10, Windows 8.1, Windows 7, iOS, Android ve Mac OS X|
|Dynamics CRM uygulaması|Dynamics CRM|Windows 10, Windows 8.1, iOS ve Android|
|Takvim/posta/kişiler uygulaması, Outlook 2016'ın, Outlook 2013 |Office 365 Exchange Online|Windows 10|
|Uygulamalar için MFA ve konum ilkesi. Cihaz tabanlı ilkeler desteklenmez. |Herhangi bir uygulamalarım uygulama hizmeti|Android ve iOS|
|Microsoft Teams Hizmetleri - bu, Microsoft Teams ve tüm istemci uygulamaları - Windows Masaüstü, iOS, Android, WP ve web istemcisi destekleyen tüm hizmetleri denetler|Microsoft Teams|Windows 10, Windows 8.1, Windows 7, iOS, Android ve macOS |
|Office 2016 uygulamaları, Office 2013, OneDrive eşitleme istemcisini (bkz [notları](https://support.office.com/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7|
|Office 2016 uygulamaları, Evrensel Office uygulamaları, Office 2013, OneDrive eşitleme istemcisini (bkz [notları](https://support.office.com/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), Office grupları destek gelecek için planlanan, SharePoint uygulama destek gelecek için planlanan|Office 365 SharePoint Online|Windows 10|
|MacOS (Word, Excel, PowerPoint ve OneNote yalnızca) için Office 2016. OneDrive for Business desteği gelecek için planlanan|Office 365 SharePoint Online|Mac OS X|
|Office mobil uygulamaları|Office 365 SharePoint Online|Android, iOS|
|Office Yammer uygulaması|Office 365 Yammer|Windows 10, iOS, Android|
|Outlook 2016'ın (macOS için Office)|Office 365 Exchange Online|Mac OS X|
|Outlook 2016'ın, Outlook 2013, Skype Kurumsal|Office 365 Exchange Online|Windows 8.1, Windows 7|
|Outlook mobil uygulaması|Office 365 Exchange Online|Android, iOS|
|Power BI uygulaması|Power BI hizmeti|Windows 10, Windows 8.1, Windows 7, Android ve iOS|
|Skype Kurumsal|Office 365 Exchange Online|Android, IOS |
|Azure DevOps uygulama|Azure DevOps|Windows 10, Windows 8.1, Windows 7, iOS ve Android|


## <a name="support-for-legacy-authentication"></a>Eski kimlik doğrulaması için destek

Seçerek **diğer istemcilerin**, IMAP, MAPI, POP, SMTP ve modern kimlik doğrulama kullanmayan eski Office uygulamaları gibi posta protokollerini ile temel kimlik doğrulaması kullanan uygulamalar etkileyen bir koşul belirtebilirsiniz.  

![Diğer istemciler](./media/technical-reference/11.png)

Daha fazla bilgi için [istemci uygulamaları](conditions.md#client-apps).

## <a name="approved-client-app-requirement"></a>Onaylı istemci uygulama gereksinimi 

Koşullu erişim ilkenizi erişim onaylı istemci uygulama tarafından yapılması gereken seçilen bulut uygulamaları için çalışır gerektirebilir. 

![Onaylı istemci uygulamalar için erişim denetimi](./media/technical-reference/21.png)

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:


- Microsoft Intune yönetilen tarayıcı
- Microsoft PowerBI
- Microsoft faturalama
- Microsoft Başlatıcısı
- Microsoft Azure Information Protection
- Microsoft Excel
- Microsoft Kaizala 
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Kurumsal
- Microsoft StaffHub
- Microsoft Teams
- Microsoft Visio
- Microsoft Word
- Microsoft To-Do
- Microsoft Stream
- Microsoft Edge



**Açıklamalar**

- Onaylı istemci uygulamaları, Intune mobil uygulama yönetimi özelliğini destekler.

- **Onaylı istemci uygulaması gerektir** gereksinimi:

    - Yalnızca iOS ve Android için destekleyen [cihaz platformu koşul](#device-platforms-condition).


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim genel bakış için bkz. [Azure Active Directory'de koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md)
- Ortamınızda koşullu erişim ilkeleri yapılandırmak hazırsanız bkz [Azure Active Directory'de koşullu erişim için önerilen yöntemleri](best-practices.md).



<!--Image references-->
[1]: ./media/technical-reference/01.png


