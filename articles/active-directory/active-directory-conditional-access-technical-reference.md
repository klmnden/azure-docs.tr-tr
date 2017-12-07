---
title: "Azure Active Directory koşullu erişim Teknik Başvurusu | Microsoft Docs"
description: "Azure Active Directory'de koşullu erişim denetimi kullanmayı öğrenin. Kullanıcıların kimliklerini doğrulamak ve uygulamanıza erişimi denetlemek için koşulları belirtin. Belirtilen koşullar karşılandığında, kullanıcıların kimlik doğrulaması ve uygulamanıza erişimi verilir."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 5fad793bcf9ac86c2a1bc67e74dfb62af9876100
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory koşullu erişim Teknik Başvurusu

Kullanabileceğiniz [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md) ince ayar yapmak için nasıl yetkili kullanıcılar, kaynaklara erişebilir.   

Bu makalede, destek bilgileri için bir koşullu erişim ilkesi için aşağıdaki yapılandırma seçeneklerini sağlar: 

- Bulut uygulamaları atamaları

- Cihaz platform koşulu 

- İstemci uygulamaları koşulu

- Onaylanmış istemci uygulama gereksinimi



## <a name="cloud-apps-assignments"></a>Bulut uygulamaları atamaları

Koşullu erişim ilkeleri ile nasıl kullanıcılarınızın erişim denetim, [bulut uygulamaları](active-directory-conditional-access-azure-portal.md#who). Koşullu erişim ilkesi yapılandırdığınızda, en az bir bulut uygulama seçmeniz gerekir. 

![İlkeniz için bulut uygulamaları seçin](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Microsoft bulut uygulamaları

Microsoft'tan aşağıdaki bulut uygulamaları için bir koşullu erişim ilkesi atayabilirsiniz:

- Azure Information Protection - [daha fazla bilgi edinin](https://docs.microsoft.com/information-protection/get-started/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Microsoft Dynamics 365

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint (OneDrive iş içerir) çevrimiçi

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

Koşullu erişim ilkenizi yapılandırdığınız [istemci uygulamaları](active-directory-conditional-access-azure-portal.md#client-apps) erişim girişiminde başlattı istemci uygulama ilkesi bağlamanın koşulu. İstemci vermek veya istemci uygulamalarının aşağıdaki türlerden erişim girişiminde bulunulduğunda erişimi engellemek için uygulamaların koşulu belirtin:

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
| Windows Phone          | Internet Explorer, sınır             | ![İşaretli][1] |
| Windows Server 2016    | Internet Explorer, sınır             | ![İşaretli][1] |
| Windows Server 2016    | Chrome                              | Çok yakında |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![İşaretli][1] |
| macOS                  | Chrome, Safari                      | ![İşaretli][1] |


> [!NOTE]
> Chrome desteği, Windows 10 oluşturucuları güncelleştirme (sürüm 1703) kullanmanız gerekir ya da daha sonra.<br>
> Yükleyebileceğiniz [bu uzantı](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Bu tarayıcılar tanımlanması ve bir ilke karşı doğrulanmış olanak tanır cihaz kimlik doğrulamasını destekler. Tarayıcı özel modda çalışıyorsa, aygıt denetimi başarısız olur. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve Masaüstü istemcileri

Koşullu erişim ilkenizi seçtiğiniz **mobil uygulamalar ve Masaüstü istemcileri** istemci uygulaması olarak.


![Desteklenen mobil uygulama veya Masaüstü istemcileri için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/06.png)


Bu ayar aşağıdaki mobil uygulamalar ve Masaüstü istemcileri yapılan erişim denemesi üzerinde bir etkisi vardır: 


|İstemci uygulamaları|Hedef hizmet|Platform|
|---|---|---|
|Azure RemoteApp|Azure uzak uygulama hizmeti|Windows 10, Windows 8.1, Windows 7, iOS, Android ve Mac OS X|
|Dynamics CRM uygulaması|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS ve Android|
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
|Power BI uygulaması. Android için Power BI uygulaması cihaz temelli koşullu erişim şu anda desteklemiyor.|Powerbı hizmeti|Windows 10, Windows 8.1, Windows 7 ve iOS|
|Skype Kurumsal|Office 365 Exchange Online|Android, IOS |
|Visual Studio Team Services uygulama|Visual Studio Team Services|Windows 10, Windows 8.1, Windows 7, iOS ve Android|



## <a name="approved-client-app-requirement"></a>Onaylanmış istemci uygulama gereksinimi 

Koşullu erişim ilkenizi bir erişim onaylanmış istemci uygulamadan yapılması gerekiyorsa seçili bulut uygulamalara girişiminde gerektirebilir. 

![Onaylanmış istemci uygulamaları için erişimi denetleme](./media/active-directory-conditional-access-technical-reference/21.png)

Bu ayar, aşağıdaki istemci uygulamalar için geçerlidir:


- Microsoft Azure Information Protection
- Microsoft Excel
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planlayıcısı
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Kurumsal
- Microsoft Teams
- Microsoft Visio
- Microsoft Word



**Açıklamalar**

- Onaylanmış istemci uygulamaları, Intune mobil uygulama yönetimi özelliğini destekler.

- **Onaylanmış istemci uygulaması gerektiren** gereksinimi:

    - Yalnızca iOS ve Android için destekler [cihaz platformu koşul](#device-platforms-condition).

    - Desteklemediği **tarayıcı** seçenek için [istemci uygulamaları koşul](#supported-browsers).
    
    - Yerine geçen **mobil uygulamalar ve Masaüstü istemcileri** seçenek için [istemci uygulamaları koşulu](#supported-mobile-apps-and-desktop-clients) bu seçeneği seçildiğinde.


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim genel bakış için bkz: [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).
- Ortamınızda koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için önerilen yöntemleri](active-directory-conditional-access-best-practices.md).



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png


