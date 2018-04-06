---
title: Azure Active Directory SSO'ya eklenti için Yönetici Kılavuzu | Microsoft Docs
description: Çoklu oturum açma Azure Active Directory ile Jira/Confluence arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: d34ff6021816c73fb064a3ce73b7fcf3ae22dbd1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="admin-guide-for-the-azure-active-directory-sso-plug-in"></a>Azure Active Directory SSO'ya eklenti için Yönetici Kılavuzu

## <a name="overview"></a>Genel Bakış

Azure Active Directory (Azure AD) çoklu oturum açma (SSO) eklentisi Microsoft Azure AD müşterilerinin Atlassian Jira ve Confluence Server tabanlı ürünler için oturum açmak için iş veya Okul hesabı kullanmasını sağlar. SAML 2.0 tabanlı SSO uygular.

## <a name="how-it-works"></a>Nasıl çalışır?

Kullanıcıların Atlassian Jira veya Confluence uygulamada oturum açmak istediğinizde, gördükleri **Azure AD ile oturum açma** oturum açma sayfasındaki düğmesi. Bunlar seçtiğinizde, Azure AD kuruluş oturum açma sayfası (diğer bir deyişle, iş veya Okul hesabı) kullanarak oturum açmak için gerekli.

Kullanıcıların kimliği doğrulandıktan sonra uygulamaya oturum açabilir olmalıdır. Bunlar zaten kimliği ve iş veya Okul hesabı için parola doğrulanır, sonra bunlar doğrudan uygulamaya oturum açın. 

Oturum açma works Jira ve Confluence arasında. Kullanıcıların Jira uygulamaya imzalanmış ve Confluence aynı bir tarayıcı penceresi açıldığında, bunlar bir uygulama için kimlik bilgilerini sağlamanız gerekmez. 

Kullanıcılar ayrıca iş veya Okul hesabı altında Atlassian ürüne My uygulamaları aracılığıyla elde edebilirsiniz. Bunlar için kimlik bilgilerini istenmeden oturum açmanız.

> [!NOTE]
> Kullanıcı sağlamayı eklentisi yapılmamaktadır.

## <a name="audience"></a>Hedef kitle

Jira ve Confluence admins eklenti Azure AD kullanarak SSO'yu etkinleştirmek için kullanabilirsiniz.

## <a name="assumptions"></a>Varsayımlar

* Jira ve Confluence HTTPS etkinleştirilmiş örnekleridir.
* Kullanıcılar, zaten Jira veya Confluence oluşturulur.
* Kullanıcıların Jira veya Confluence atanan rolleri sahiptir.
* Yöneticileri eklentiyi yapılandırmak için gerekli bilgilere erişebilirsiniz.
* Jira veya Confluence da şirket ağının dışından kullanılabilir.
* Yalnızca şirket içi sürümü ile Jira ve Confluence eklenti çalışır.

## <a name="prerequisites"></a>Önkoşullar

Eklenti yüklemeden önce aşağıdaki bilgileri unutmayın:

* Jira ve Confluence Windows 64-bit sürümü yüklenir.
* Jira ve Confluence sürümleri HTTPS etkin değildir.
* Jira ve Confluence Internet üzerinde kullanılabilir.
* Yerinde Jira ve Confluence için yönetici kimlik bilgileridir.
* Yönetici kimlik bilgileri Azure AD için verilmiştir.
* WebSudo Jira ve Confluence devre dışı bırakılır.

## <a name="supported-versions-of-jira-and-confluence"></a>Jira ve Confluence desteklenen sürümleri

Eklenti Jira ve Confluence aşağıdaki sürümlerini destekler:

* Jira çekirdek ve yazılım: 6.0 için 7.2.0
* Jira hizmet masasına: 3.0 3.2 için
* Confluence: 5.0 5.10

## <a name="installation"></a>Yükleme

Eklentisini yüklemek için aşağıdaki adımları izleyin:

1. Jira veya Confluence Örneğiniz için bir yönetici olarak oturum açın
    
2. Jira/Confluence yönetim konsoluna gidin ve seçin **eklentileri**.
    
3. Atlassian marketten arama **Microsoft SAML SSO eklentisi**.
 
   Eklenti uygun sürümünü arama sonuçlarında görünür.
 
5. Eklentiyi seçin ve evrensel eklentisi Yöneticisi (UPM) yükler.
 
Eklenti yüklendikten sonra görünür **kullanıcı yüklü eklentileri** bölümünü **Eklentileri Yönet**.
    
## <a name="plug-in-configuration"></a>Eklenti yapılandırması

Eklenti kullanmaya başlamadan önce yapılandırmanız gerekir. Eklenti seçin **yapılandırma** düğmesine tıklayın ve yapılandırma ayrıntılarını sağlamak.

Aşağıdaki resimde, hem Jira hem de Confluence yapılandırma ekranında gösterilmektedir:
    
![Eklenti yapılandırması ekran](./media/ms-confluence-jira-plugin-adminguide/jira.png)

*   **Meta veri URL'sini**: Azure AD'den Federasyon meta verilerini almak için URL.
 
*   **Tanımlayıcıları**: URL, Azure AD kullanır isteğin kaynak doğrulamak için. Eşlendiği **tanımlayıcısı** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen*< etki alanı: bağlantı noktası >*/.
 
*   **Yanıt URL'si**: SAML oturum açma başlatır, kimlik sağlayıcıyı (IDP) içinde yanıt URL'si. Eşlendiği **yanıt URL'si** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen*< etki alanı: bağlantı noktası >*/plugins/servlet/saml/auth.
 
*   **Oturum üzerinde URL'si**: SAML oturum açma başlatır, IDP oturum açma URL'si. Eşlendiği **oturum açma** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen*< etki alanı: bağlantı noktası >*/plugins/servlet/saml/auth.
 
*   **IDP varlık kimliği**:, IDP kullanan varlık kimliği. Meta veri URL'sini çözüldüğünde, bu kutu doldurulur.
 
*   **Oturum açma URL'si**: oturum açma URL'SİNDEN, IDP. Bu kutu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
*   **Oturum kapatma URL'si**:, IDP oturum kapatma URL'den. Bu kutu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
*   **X.509 sertifikası**: bilgisayarınızı IDP'ın X.509 sertifikası. Bu kutu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
*   **Oturum açma düğmesi adı**: kullanıcıların oturum açma sayfasında görmesini, kuruluşunuzda Oturum Aç düğmesini adı.
 
*   **SAML kullanıcı kimliği konumları**: Burada Jira veya Confluence kullanıcı kimliği beklenmektedir SAML yanıtta konumu. İçinde olabileceği **NameID** veya bir özel öznitelik adı.
 
*   **Öznitelik adı**: kullanıcı kimliği beklenirken özniteliğinin adı.
 
*   **Giriş bölgesi bulmayı etkinleştirmek**: şirketin Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsa, yapmanız - tabanlı oturum - seçimi içinde.
 
*   **Etki alanı adı**: oturum açma AD FS tabanlı ise, etki alanı adı.
 
*   **Çoklu oturum kapatma etkinleştirme**: seçimin ne zaman bir kullanıcı oturumu kapattığında Jira veya Confluence Azure AD'den oturumu kapatmak istiyor olun.

## <a name="troubleshooting"></a>Sorun giderme

* **Birden çok sertifika hatası alıyorsunuz**: Azure AD ile oturum açın ve uygulamayı karşı kullanılabilir birden çok sertifikaları kaldırın. Bu yalnızca bir sertifika bulunduğundan emin olun.

* **Bir sertifika Azure AD'de dolmak üzeredir**: eklentileri dikkatli olun sertifikanın otomatik geçişi. Bir sertifika dolmak üzere olduğunda, yeni bir sertifika etkin olarak işaretlenmelidir ve kullanılmayan sertifikaları silinmesi gerekir. Ne zaman bir kullanıcı bu senaryoda, eklenti öğesinden Jira için oturum açmaya çalıştığında ve yeni sertifikayı kaydeder.

* **WebSudo (devre dışı güvenli yönetici oturumu) devre dışı bırakmak istediğiniz**:
    
  * Jira için güvenli yönetici oturumları (yönetim işlevleri erişmeden önce başka bir deyişle, parola onayı) varsayılan olarak etkinleştirilir. Bu özelliği Jira örneğinizi kaldırmak istiyorsanız, aşağıdaki satırı jira config.properties dosyanızda belirtin: `ira.websudo.is.disabled = true`
    
  * Confluence için adımları izleyin [Confluence Destek sitesi](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html).

* **Meta veri URL'sini tarafından doldurulmalıdır beklenen alanlar doldurulmamış**:
    
  * URL'nin doğru olup olmadığını denetleyin. Doğru Kiracı ve uygulama kimliğini eşledikten olmadığını denetleyin
    
  * Bir tarayıcıda URL'sini girin ve Federasyon meta veri XML alırsanız bkz.

* **Bir iç sunucu hatası**: yükleme günlük dizininde günlüklerini gözden geçirin. Azure AD SSO kullanarak oturum açmak kullanıcı çalışırken hata alıyorsanız, günlükleri destek ekibi ile paylaşabilirsiniz.

* **Kullanıcı oturum açmaya çalıştığında "kullanıcı kimliği bulunamadı" hatası olan**: Jira veya Confluence kullanıcı kimliği oluşturun.

* **Azure AD'de bir "uygulama bulunamadı" hatası var.**: uygun URL'yi uygulamaya Azure AD'de eşlenip eşlenmediğini bakın.

* **Desteğe ihtiyacınız**: ulaşmak [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Takım 24-48 iş saatleri içinde yanıt verir.
    
  Azure portal kanal üzerinden Microsoft ile bir destek bileti de yükseltebilirsiniz.