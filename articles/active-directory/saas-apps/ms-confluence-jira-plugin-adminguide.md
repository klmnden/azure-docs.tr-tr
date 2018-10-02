---
title: Atlassian Jıra/Confluence Yönetici Kılavuzu - Azure Active Directory | Microsoft Docs
description: Atlassian Jıra ve Confluence, Azure Active Directory (Azure AD) ile kullanmak için yönetici kılavuzundaki...
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2018
ms.author: jeedes
ms.openlocfilehash: 49516523abdd927c3ae60235fcd74473689c6856
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019871"
---
# <a name="atlassian-jira-and-confluence-admin-guide-for-azure-active-directory"></a>Azure Active Directory için Atlassian Jıra ve Confluence Yönetici Kılavuzu

## <a name="overview"></a>Genel Bakış

Azure Active Directory (Azure AD) çoklu oturum açma (SSO) eklentisi, Atlassian Jıra ve Confluence Server tabanlı ürünler için oturum açmak için iş veya Okul hesabı kullanmak Microsoft Azure AD müşterilerin sağlar. SAML 2.0 tabanlı SSO uygular.

## <a name="how-it-works"></a>Nasıl çalışır?

Kullanıcıların, Atlassian Jıra ya da Confluence uygulamasında oturum açmak istediğinizde görürler **Azure AD ile oturum açma** oturum açma sayfasında düğme. Bu seçeneği belirlediğinizde Azure AD kuruluş oturum açma sayfasını (diğer bir deyişle, iş veya Okul hesabı) kullanarak oturum açmak için gerekli.

Kullanıcıların kimliği doğrulandıktan sonra uygulamaya oturum açabilir olmalıdır. Bunlar zaten kimliği ve iş veya Okul hesabı için parola ile kimlik doğrulaması, ardından, doğrudan uygulamaya oturum açın. 

Oturum açma Jıra ve Confluence ortamlarında çalışır. Jıra uygulamaya kullanıcılar oturum açtınız ve Confluence aynı tarayıcı penceresi açıldığında, bunlar bir uygulama için kimlik bilgilerini sağlamanız gerekmez. 

Kullanıcılar iş veya Okul hesabı altında Atlassian ürüne uygulamalarım aracılığıyla da alabilirsiniz. Bunlar kimlik bilgilerini istenmeden oturum açmanız.

> [!NOTE]
> Kullanıcı sağlamayı eklentisi yapılmaz.

## <a name="audience"></a>Hedef kitle

Jıra ve Confluence Yöneticiler eklentiyi Azure AD kullanarak SSO etkinleştirmek için kullanabilir.

## <a name="assumptions"></a>Varsayımlar

* Jıra ve Confluence HTTPS etkin örnekleridir.
* Kullanıcılar, Jıra veya Confluence zaten oluşturulmuştur.
* Kullanıcıların, Jıra veya Confluence atanan roller vardır.
* Yöneticiler eklentiyi yapılandırmak için gerekli bilgilere erişebilir.
* Jıra veya Confluence, şirket ağı dışında de kullanılabilir.
* Yalnızca şirket içi sürümü Jıra Confluence, eklenti çalışır.

## <a name="prerequisites"></a>Önkoşullar

Eklenti yüklemeden önce aşağıdaki bilgileri unutmayın:

* Jıra ve Confluence Windows 64-bit sürümü yüklenir.
* Jıra ve Confluence HTTPS etkin sürümleridir.
* Jıra ve Confluence, internet'te kullanılabilir.
* Jıra ve Confluence yönetici kimlik bilgileridir.
* Yönetici kimlik bilgileri, Azure AD için yerdesiniz demektir.
* Jıra ve Confluence WebSudo devre dışı bırakıldı.

## <a name="supported-versions-of-jira-and-confluence"></a>Jıra ve Confluence desteklenen sürümleri

Eklenti Jıra ve Confluence aşağıdaki sürümleri destekler:

* Jıra çekirdek ve yazılım: 6.0 için 7.8
* Jıra hizmet Masası: 3.0 3.2
* Confluence: 5.0 5.10

## <a name="installation"></a>Yükleme

Eklentiyi yüklemek için aşağıdaki adımları izleyin:

1. Jıra veya Confluence Örneğiniz için bir yönetici olarak oturum açın

2. Jıra/Confluence yönetim konsoluna giderek seçin **eklentileri**.

3. Atlassian Market'ten arama **Microsoft SAML SSO eklentisi**.

   Eklenti'nün uygun sürümüne arama sonuçlarında görünür.

4. Eklentiyi seçin ve evrensel Eklenti Yöneticisi'ni (UPM) yükler.

Eklenti yükledikten sonra görünür **kullanıcı yüklü eklentiler** bölümünü **Eklentileri Yönet**.

## <a name="plug-in-configuration"></a>Eklenti yapılandırması

Eklenti'ı kullanmaya başlamadan önce yapılandırmanız gerekir. Eklenti seçin **yapılandırma** düğmesini ve yapılandırma ayrıntılarını sağlamak.

Aşağıdaki görüntüde, Jıra hem Confluence yapılandırma ekranında gösterilmektedir:

![Eklenti yapılandırması ekran](./media/ms-confluence-jira-plugin-adminguide/jira.png)

*   **Meta veri URL'si**: Azure AD Federasyon meta verilerini almak için URL.

*   **Tanımlayıcıları**: İstek kaynağını doğrulamak için URL, Azure AD kullanır. Eşlendiği **tanımlayıcı** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen *< etki alanı: bağlantı noktası >*/.

*   **Yanıt URL'si**: SAML oturum açma başlatır, kimlik sağlayıcısı (IDP) içinde yanıt URL'si. Eşlendiği **yanıt URL'si** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen *< etki alanı: bağlantı noktası >*/plugins/servlet/saml/auth.

*   **Üzerinde oturum URL'si**: SAML oturum açma başlatır, Idp'nin oturum açma URL'si. Eşlendiği **oturum açma** Azure AD'de öğesi. Eklenti otomatik olarak bu URL https:// olarak türetilen *< etki alanı: bağlantı noktası >*/plugins/servlet/saml/auth.

*   **IDP varlık kimliği**: Idp'nizi kullanan varlık kimliği. Meta veri URL'sini çözümlendiğinde bu kutuyu doldurulur.

*   **Oturum açma URL'si**: oturum açma URL'SİNDEN geçirebilirsiniz. Bu kutuyu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.

*   **Oturum kapatma URL'si**: Idp'nizi gelen oturum kapatma URL'si. Bu kutuyu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.

*   **X.509 sertifikası**: bilgisayarınızı Idp'nin X.509 sertifikası. Bu kutuyu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.

*   **Oturum açma düğmesi adı**: kuruluşunuzun oturum açma sayfasında görmek için kullanıcıların istediği oturum açma düğmesi adı.

*   **SAML kullanıcı kimliği konumları**: Jıra veya Confluence kullanıcı kimliği SAML yanıtını bekleniyor burada konumu. İçinde yer alabileceği **Nameıd** veya özel öznitelik adı.

*   **Öznitelik adı**: kullanıcı kimliği beklenirken özniteliğinin adı.

*   **Giriş bölgesi bulmayı etkinleştirmek**: Şirket Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsa, yapmanız - tabanlı oturum açma - seçimi içinde.

*   **Etki alanı adı**: oturum açma AD FS bağlı ise, etki alanı adı.

*   **Çoklu oturum kapatma etkinleştirme**: seçimin ne zaman bir kullanıcı oturumu Jıra veya Confluence Azure AD oturumunu kapatmak istiyorsanız olun.

## <a name="troubleshooting"></a>Sorun giderme

* **Birden çok sertifika hataları karşılaşacağınız**: Azure AD'de oturum açın ve uygulamayı karşı kullanılabilir birden çok sertifikaları kaldırın. Bu yalnızca bir sertifika mevcut olduğundan emin olun.

* **Bir sertifika Azure AD'de dolmak üzere olduğu**: eklentiler, sertifikanın otomatik geçişi ilgileniriz. Bir sertifikanın süresi dolmak üzere olduğunda yeni bir sertifika etkin olarak işaretlenmelidir ve kullanılmayan sertifikaları silinmesi gerekir. Ne zaman bir kullanıcı bu senaryoda, eklenti öğesinden Jıra'ya oturum açmak çalışır ve yeni sertifikayı kaydeder.

* **WebSudo (devre dışı güvenli yönetici oturumu) devre dışı bırakmak istediğiniz**:

  * Jıra için güvenli yönetici oturumlarını (yönetim işlevleri erişmeden önce diğer bir deyişle, parola onayı) varsayılan olarak etkindir. Bu özelliği Jıra Örneğinizde kaldırmak istiyorsanız, aşağıdaki satırı jıra config.properties dosyanızda belirtin: `ira.websudo.is.disabled = true`

  * Confluence için adımları takip edin [Confluence Destek sitesi](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html).

* **Meta veri URL'sini tarafından doldurulmalıdır beklenen alan doldurulmamış**:

  * URL'nin doğru olup olmadığını denetleyin. Eşlenen doğru Kiracı ve uygulama kimliği, denetleyin

  * Bir tarayıcıda URL'sini girin ve Federasyon meta verileri XML alırsanız bkz.

* **Bir iç sunucu hatası**: yükleme günlük dizininde bulunan günlükler gözden geçirin. Azure AD SSO kullanarak oturum açmak kullanıcının çalışırken bir hata alıyorsanız, günlükler destek ekibi ile paylaşabilirsiniz.

* **Kullanıcı oturum açmaya çalıştığında bir "kullanıcı kimliği bulunamadı" hatası olduğundan**: kullanıcı kimliği Jıra veya Confluence oluşturun.

* **Azure AD'de "uygulama bulunamadı" hatası oluşuyor**: uygun URL'yi, Azure AD'de uygulamaya eşlendiği bakın.

* **Desteğe ihtiyacınız**: ulaşın [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Takım, 24-48 iş saati içinde yanıt verir.

  Azure portal kanal üzerinden Microsoft ile bir destek bileti de gönderebilirsiniz.

## <a name="plug-in-faq"></a>Eklenti hakkında SSS

Bu ilgili herhangi bir sorgu varsa, lütfen aşağıdaki SSS bölümüne başvurun eklenti.

### <a name="what-does-the-plug-in-do"></a>Eklenti do nedir?

Eklenti Atlassian Jıra (Jıra çekirdek, Jıra Software, Jıra hizmet Masası dahil) ve şirket içi yazılım Confluence için çoklu oturum açma (SSO) özelliği sağlar. Bir kimlik sağlayıcıyı (IDP) olarak Azure Active Directory (Azure AD) ile eklenti çalışır.

### <a name="which-atlassian-products-does-the-plug-in-work-with"></a>Atlassian ürünleri eklenti çalışma mu?

Jıra Confluence ve şirket içi sürümleriyle eklenti çalışır.

### <a name="does-the-plug-in-work-on-cloud-versions"></a>Bulut sürümlerinde eklenti çalışır?

Hayır. Eklenti destekler yalnızca şirket içi Jıra ve Confluence sürümleri.

### <a name="which-versions-of-jira-and-confluence-does-the-plug-in-support"></a>Jıra ve Confluence hangi sürümlerinin Eklenti desteği mu?

Eklenti aşağıdaki sürümlerini destekler:

* Jıra çekirdek ve yazılım: 6.0 için 7.8
* Jıra hizmet Masası: 3.0 3.2
* Confluence: 5.0 5.10

### <a name="is-the-plug-in-free-or-paid"></a>Eklenti olan ücretsiz veya Ücretli?

Bu ücretsiz bir eklentidir.

### <a name="do-i-need-to-restart-jira-or-confluence-after-i-deploy-the-plug-in"></a>Jıra veya Confluence eklenti dağıtabilirim sonra yeniden başlatmanız gerekiyor mu?

Yeniden başlatma gerekli değildir. Eklenti hemen kullanmaya başlayabilirsiniz.

### <a name="how-do-i-get-support-for-the-plug-in"></a>Nasıl destek eklentisi alabilirim?

Sizin için ulaşın [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) herhangi bir destek için gereken bu eklenti. Takım, 24-48 iş saati içinde yanıt verir.

Azure portal kanal üzerinden Microsoft ile bir destek bileti de gönderebilirsiniz.

### <a name="would-the-plug-in-work-on-a-mac-or-ubuntu-installation-of-jira-and-confluence"></a>Bir Mac veya Ubuntu yüklemeyi Jıra ve Confluence eklenti çalışmaları musunuz?

Eklenti yalnızca 64 bit Windows Server yüklemeleri Jıra ve Confluence üzerinde sınanmıştır.

### <a name="does-the-plug-in-work-with-idps-other-than-azure-ad"></a>Azure AD dışındaki Idp'yi ile eklenti çalışır?

Hayır. Bu, yalnızca Azure AD ile çalışır.

### <a name="what-version-of-saml-does-the-plug-in-work-with"></a>SAML sürümünü ile eklenti çalışır?

SAML 2.0 ile çalışır.

### <a name="does-the-plug-in-do-user-provisioning"></a>Eklentinin kullanıcı sağlamayı yarar?

Hayır. Eklenti yalnızca SAML 2.0 tabanlı SSO sağlar. Kullanıcının uygulamada SSO oturum açma önce sağlanması gerekir.

### <a name="does-the-plug-in-support-cluster-versions-of-jira-and-confluence"></a>Jıra ve Confluence Eklenti desteği küme sürümleri mu?

Hayır. Jıra Confluence ve şirket içi sürümleriyle eklenti çalışır.

### <a name="does-the-plug-in-work-with-http-versions-of-jira-and-confluence"></a>Jıra ve Confluence HTTP sürümleri ile eklenti çalışır?

Hayır. Yalnızca HTTPS özellikli yüklemeleri ile eklenti çalışır.