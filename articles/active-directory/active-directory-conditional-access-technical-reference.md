---
title: Azure Active Directory koşullu erişim ayarları başvurusu | Microsoft Docs
description: Desteklenen ayarlar genel bir bakış Azure Active Directory koşullu erişim ilkesinde alın.
services: active-directory.
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 341142222fb1440cf5118e6d10f5a254e73d53d9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Azure Active Directory koşullu erişim ayarları başvurusu

Kullanabileceğiniz [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md) nasıl yetkili kullanıcıların denetlemek için kaynaklara erişebilir.   

Bu makalede, destek bilgileri için bir koşullu Erişim İlkesi'nde aşağıdaki yapılandırma seçeneklerini sağlar: 

- Bulut uygulamaları atamaları

- Cihaz platform koşulu 

- İstemci uygulamaları koşulu

- Onaylanmış istemci uygulama gereksinimi


Aradığınız bilgiler bu değilse, lütfen bu makalenin sonunda bir yorum bırakın.

## <a name="cloud-apps-assignments"></a>Bulut uygulamaları atamaları

Koşullu erişim ilkeleri ile nasıl kullanıcılarınızın erişim denetim, [bulut uygulamaları](active-directory-conditional-access-conditions.md#cloud-apps). Koşullu erişim ilkesi yapılandırdığınızda, en az bir bulut uygulama seçmeniz gerekir. 

![İlkeniz için bulut uygulamaları seçin](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft bulut uygulamaları

Microsoft'tan aşağıdaki bulut uygulamaları için bir koşullu erişim ilkesi atayabilirsiniz:

- Azure Information Protection - [daha fazla bilgi edinin](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint (OneDrive iş ve Project Online içerir) çevrimiçi

- Microsoft Power BI 

- Microsoft Visual Studio Team Services

- Microsoft Teams


### <a name="other-applications"></a>Diğer uygulamalar 

Microsoft bulut uygulamalarının yanı sıra bulut uygulamaları aşağıdaki türleri için bir koşullu erişim ilkesi atayabilirsiniz:

- Azure AD bağlı uygulamalar

- Önceden tümleştirilmiş Federasyon olarak yazılım hizmet (SaaS) uygulaması

- Parola çoklu oturum açma (SSO) kullanan uygulamalar

- Satır iş kolu uygulamaları

- Azure AD uygulama proxy'si kullanan uygulamalar


## <a name="device-platform-condition"></a>Cihaz platform koşulu

Bir koşullu erişim ilkesi, bir istemcide ilke işletim sistemine bağlamanın cihaz platformu koşul yapılandırabilirsiniz. Azure AD koşullu erişim, aşağıdaki cihaz platformlarını destekler:

- Android

- iOS

- Windows Phone

- Windows

- macOS


![İstemci işletim sistemi için erişim ilkesi bağlayın](./media/active-directory-conditional-access-technical-reference/41.png)





## <a name="client-apps-condition"></a>İstemci uygulamaları koşulu 

Koşullu erişim ilkenizi yapılandırdığınız [istemci uygulamaları](active-directory-conditional-access-conditions.md#client-apps) erişim girişiminde başlattı istemci uygulama ilkesi bağlamanın koşulu. İstemci vermek veya istemci uygulamalarının aşağıdaki türlerden erişim girişiminde bulunulduğunda erişimi engellemek için uygulamaların koşulu belirtin:

- Tarayıcı
- Mobil uygulamalar ve Masaüstü uygulamaları

![İstemci uygulamaları için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Desteklenen tarayıcılar 

Koşullu erişim ilkenizi seçtiğiniz **tarayıcılar** istemci uygulaması olarak.

![Desteklenen tarayıcılar için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/05.png)

Bu ayarı tüm tarayıcılarla çalışır. Ancak, bir uyumlu aygıt gereksinim gibi bir cihaz İlkesi karşılamak için aşağıdaki işletim sistemleri ve tarayıcılar desteklenir:


| İşletim Sistemi                     | Tarayıcılar                            | Destek     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, kenar, Chrome     | ![İşaretli][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome           | ![İşaretli][1] |
| Windows 7              | Internet Explorer, Chrome           | ![İşaretli][1] |
| iOS                    | Safari, Intune yönetilen tarayıcı      | ![İşaretli][1] |
| Android                | Chrome, Intune yönetilen tarayıcı      | ![İşaretli][1] |
| Windows Phone          | Internet Explorer, Edge             | ![İşaretli][1] |
| Windows Server 2016    | Internet Explorer, Edge             | ![İşaretli][1] |
| Windows Server 2016    | Chrome                              | Çok yakında |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| macOS                  | Chrome, Safari                      | ![İşaretli][1] |



#### <a name="chrome-support"></a>Chrome desteği

Chrome için destek **Windows 10 oluşturucuları güncelleştirme (sürüm 1703)** veya daha sonra yükleme [bu uzantı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Bu uzantı Chrome tarayıcı otomatik olarak dağıtmak için aşağıdaki kayıt defteri anahtarı oluşturun:

|    |    |
|--- | ---|
|Yol | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
|Ad | 1 |
|Tür | REG_SZ (dize) |
|Veriler | ppnbnpeolgkicgegkbkbjmhlideopiji;https://clients2.google.com/service/update2/crx

Chrome için destek **7 ve Windows 8.1**, aşağıdaki kayıt defteri anahtarı oluşturun:

|    |    |
|--- | ---|
|Yol | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
|Ad | 1 |
|Tür | REG_SZ (dize) |
|Veriler | {"düzeni": "https://device.login.microsoftonline.comfiltre" ",": {"Veren": {"CN =": "MS Kuruluş erişim"}}}|

Bu tarayıcılar tanımlanması ve bir ilke karşı doğrulanmış olanak tanır cihaz kimlik doğrulamasını destekler. Tarayıcı özel modda çalışıyorsa, aygıt denetimi başarısız olur. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve Masaüstü istemcileri

Koşullu erişim ilkenizi seçtiğiniz **mobil uygulamalar ve Masaüstü istemcileri** istemci uygulaması olarak.


![Desteklenen mobil uygulama veya Masaüstü istemcileri için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/06.png)


Bu ayar aşağıdaki mobil uygulamalar ve Masaüstü istemcileri yapılan erişim denemesi üzerinde bir etkisi vardır: 


|İstemci uygulamaları|Hedef hizmet|Platform|
|---|---|---|
|Azure RemoteApp|Azure uzak uygulama hizmeti|Windows 10, Windows 8.1, Windows 7, iOS, Android ve Mac OS X|
|Dynamics CRM uygulaması|Dynamics CRM|Windows 10, Windows 8.1, iOS ve Android|
|Takvim/posta/kişiler uygulama, Outlook 2016 (modern kimlik doğrulaması ile) Outlook 2013|Office 365 Exchange Online|Windows 10|
|MFA ve konum İlkesi uygulamalar için. Cihaz tabanlı ilkeleri desteklenmez. |Herhangi bir My uygulamaları uygulama hizmeti|Android ve iOS|
|Microsoft ekipleri Hizmetleri - bu Microsoft Teams ve tüm alt istemci uygulamaları - Windows Masaüstü, iOS, Android, WP ve web istemcisi destekleyen tüm hizmetleri denetler|Microsoft Teams|Windows 10, Windows 8.1, Windows 7, iOS, Android ve macOS |
|Office 2016 uygulamaları, Office 2013 (modern kimlik doğrulaması ile) OneDrive eşitleme istemci (bkz [notları](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7|
|Office 2016 uygulamalar, Evrensel Office uygulamaları, Office 2013 (modern kimlik doğrulaması ile), OneDrive eşitleme istemcisi (bkz [notları](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), Office grupları destek gelecek için planlanan, SharePoint uygulama destek gelecek için planlanan|Office 365 SharePoint Online|Windows 10|
|MacOS (Word, Excel, PowerPoint, yalnızca OneNote) için Office 2016. OneDrive iş desteğine gelecek için planlanan|Office 365 SharePoint Online|Mac OS X|
|Office mobil uygulamaları|Office 365 SharePoint Online|Android, iOS|
|Office Yammer uygulaması|Office 365 Yammer|Windows 10, iOS, Android|
|Outlook 2016 (Office macOS için)|Office 365 Exchange Online|Mac OS X|
|Outlook 2016, Outlook 2013 (modern kimlik doğrulaması ile) Skype Kurumsal (modern kimlik doğrulaması)|Office 365 Exchange Online|Windows 8.1, Windows 7|
|Outlook mobil uygulama|Office 365 Exchange Online|Android, iOS|
|Power BI uygulaması|Powerbı hizmeti|Windows 10, Windows 8.1, Windows 7, Android ve iOS|
|Skype Kurumsal|Office 365 Exchange Online|Android, IOS |
|Visual Studio Team Services uygulama|Visual Studio Team Services|Windows 10, Windows 8.1, Windows 7, iOS ve Android|



## <a name="approved-client-app-requirement"></a>Onaylanmış istemci uygulama gereksinimi 

Koşullu erişim ilkenizi bir erişim onaylanmış istemci uygulamadan yapılması gerekiyorsa seçili bulut uygulamalara girişiminde gerektirebilir. 

![Onaylanmış istemci uygulamaları için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/21.png)

Bu ayar, aşağıdaki istemci uygulamalar için geçerlidir:


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
- Microsoft Planlayıcısı
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Kurumsal
- Microsoft StaffHub
- Microsoft Teams
- Microsoft Visio
- Microsoft Word



**Açıklamalar**

- Onaylanmış istemci uygulamaları, Intune mobil uygulama yönetimi özelliğini destekler.

- **Onaylanmış istemci uygulaması gerektiren** gereksinimi:

    - Yalnızca iOS ve Android için destekler [cihaz platformu koşul](#device-platforms-condition).


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim genel bakış için bkz: [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).
- Ortamınızda koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için önerilen yöntemleri](active-directory-conditional-access-best-practices.md).



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


