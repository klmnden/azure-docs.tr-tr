---
title: "Microsoft Azure Active Directory tek oturum açma eklentisi Yönetici Kılavuzu | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Microsoft Azure Active Directory tek oturum açma için JIRA arasında yapılandırmayı öğrenin."
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
ms.openlocfilehash: af949d1db8af37a534a16364f9f0763479c436e4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-admin-guide"></a>Microsoft Azure Active Directory tek oturum açma eklentisi Yönetici Kılavuzu

## <a name="table-of-contents"></a>İçindekiler

1. **[GENEL BAKIŞ](#overview)**
2. **[NASIL ÇALIŞIR?](#how-it-works)**
3. **[HEDEF KİTLE](#audience)**
4. **[VARSAYIMLAR](#assumptions)**
5. **[ÖNKOŞULLARI](#prerequisites)**
6. **[JIRA VE CONFLUENCE DESTEKLENEN SÜRÜMLERİ](#supported-versions-of-jira-and-confluence)**
7. **[YÜKLEME](#installation)**
8. **[EKLENTİSİ YAPILANDIRMA](#plugin-configuration)**
9. **[ALAN AÇIKLAMA İÇİN EKLENTİYİ YAPILANDIRMA EKRANINDA:](#field-explanation-for-add---on-configuration-screen:)**
10. **[SORUN GİDERME](#troubleshooting)**

## <a name="overview"></a>Genel Bakış

Microsoft Azure AD kullanıcı kuruluş adı ve parola Atlassian Jira kullanmayı müşterilere Bu eklentilerin etkinleştirin ve ürünleri Confluence Server tabanlı. SAML 2.0 uygulayan SSO tabanlı.

## <a name="how-it-works"></a>Nasıl çalışır?

Kullanıcıların Atlassian Jira veya Confluence uygulamaya oturum açmak istediğinizde, gördükleri **Azure AD ile oturum açma** oturum açma sayfasındaki düğmesi. Üzerinde'e tıkladığınızda, Azure AD kuruluş oturum açma sayfası ile oturum açmak için gereklidirler.

Kullanıcıların kimliği doğrulandıktan sonra uygulamaya oturum açabilmeniz olmalıdır. Bunlar zaten kuruluş kimliği ve parola doğrulandıktan sonra bunlar doğrudan uygulamasına oturum. Ayrıca, bu oturum açma JIRA ve Confluence çalışır unutmayın. Kullanıcılar, JIRA uygulamasına kaydedilir ve Confluence ise de açık aynı tarayıcı penceresinde, bir kez oturum açmak ve ihtiyaç duydukları diğer uygulaması için kimlik bilgilerini yeniden sağlamanız gerekir. Kullanıcılar ayrıca Atlassian ürüne Azure hesabı altında myapps aracılığıyla alabilir ve bunlar için kimlik bilgilerini istenmeden oturum açması.

> [!NOTE]
> Kullanıcı sağlamayı yapılmadı bu eklentiyi kullanarak.

## <a name="audience"></a>Hedef kitle

Kimlerin Azure AD kullanarak SSO'yu etkinleştirmek için bu eklentinin kullanmayı planladığınız JIRA ve Confluence admins.

## <a name="assumptions"></a>Varsayımlar

* HTTPS etkinleştirilmiş JIRA/Confluence örneğidir
* Kullanıcıların JIRA/Confluence içinde zaten oluşturulmuştur
* Kullanıcıların JIRA/Confluence atanan rol sahibi
* Yöneticiler eklentiyi yapılandırmak için gerekli bilgilere erişimi
* JIRA/Confluence da şirket ağının dışından kullanılabilir
* Yalnızca şirket içi sürümü ile JIRA ve confluence works ekleyin

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar, önce aşağıdaki Not eklenti yükleme işlemine devam:

* JIRA/Confluence Windows 64-bit sürümü yüklü
* HTTPS etkinleştirilmiş JIRA/Confluence sürümleri
* "Desteklenen sürümleri" bölümünde aşağıdaki eklenti için desteklenen sürüm unutmayın.
* JIRA/Confluence Internet üzerinde kullanılabilir.
* JIRA/Confluence için yönetici kimlik bilgileri
* Azure AD için yönetici kimlik bilgileri
* WebSudo JIRA ve confluence devre dışı bırakılmalıdır

## <a name="supported-versions-of-jira-and-confluence"></a>JIRA ve Confluence desteklenen sürümleri

Şimdi itibariyle JIRA ve Confluence aşağıdaki sürümleri desteklenir:

* JIRA çekirdek ve yazılım: 6.0 için 7.2.0
* JIRA Service Desk: 3.0 to 3.2
* Confluence: 5.0 5.10

## <a name="installation"></a>Yükleme

Yönetim Eklentisi yüklemek için aşağıda belirtilen adımları izlemelisiniz:

1. JIRA/Confluence Örneğiniz için bir yönetici olarak oturum açın
    
2. JIRA/Confluence Yönetimi'ne gidin ve eklentiler de tıklatın.
    
3. Atlassian Pazar yerden arama **Microsoft SAML SSO eklentisi**
 
4. Eklentinin uygun sürümünü aramada görüntülenir
 
5. Eklentiyi seçin ve UPM aynı yükler.
 
6. Eklenti yüklendikten sonra kullanıcı yüklenen eklentiler bölümünde yönetmek eklenti bölümünün görünür
 
7. Bunu kullanmaya başlamadan önce eklentiyi yapılandırmak zorunda.
 
8. Eklenti üzerinde tıklatın ve Yapılandır düğmesini bakın.
 
9. Yapılandırma girişleri sağlamak için tıklayın
    
## <a name="plugin-configuration"></a>Eklentisi yapılandırma

Aşağıdaki görüntüde eklentisi yapılandırma ekranında, hem JIRA hem de Confluence gösterir.
    
![Eklenti yapılandırması](./media/ms-confluence-jira-plugin-adminguide/jira.png)

### <a name="field-explanation-for-add-on-configuration-screen"></a>Alan açıklama için eklentiyi yapılandırma ekranında:

1.   Meta veri URL'si: Azure AD'den Federasyon meta verilerini almak için URL
 
2.   Tanımlayıcı: Azure AD tarafından istek kaynağını doğrulamak için kullanılır. Bu, Azure AD'de tanımlayıcısı öğesine eşler. Bu otomatik olarak https://<domain:port>/ eklentisi tarafından türetilmiş olur
 
3.   Yanıt URL'si: SAML oturum başlatmak için IDP kullanım yanıt URL'si. Bu, Azure AD içinde yanıt URL'si öğesine eşler. Bu otomatik olarak https://<domain:port>/plugins/servlet/saml/auth eklentisi tarafından türetilmiş olur
 
4.   Oturum açma URL'si: Kullanım oturum üzerinde URL'de SAML oturum başlatmak için IDP. Bu, Azure AD'de oturum açma öğesine eşler. Bu otomatik olarak https://<domain:port>/plugins/servlet/saml/auth eklentisi tarafından türetilmiş olur
 
5.   IDP varlık Tanıtıcısı: Varlık, IDP kullanan kimliği. Meta veri URL'sini çözümlendiğinde doldurulur.
 
6.   Oturum açma URL'si: Oturum açma URL'SİNDEN, IDP. Bu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
7.   Oturum kapatma URL'si:, IDP gelen oturum kapatma URL'si. Bu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
8.   X.509 sertifikası: IdP'ın X.509 sertifikası. Bu meta veri URL'sini çözümlenmiş olduğunda Azure AD'den doldurulur.
 
9.   : Oturum açma düğmesi kuruluşunuz görmek istediği oturum açma düğmesi adı. Bu metin, oturum açma ekranında oturum açma düğmesi kullanıcılara gösterilir.
 
10.   SAML kullanıcı kimliği konumları: Burada SAML yanıt olarak kullanıcı kimliği beklenmektedir. NameID veya bir özel öznitelik adı ya da olabilir. Bu kimliği JIRA/Confluence kullanıcı kimliği olması gerekir.
 
11.   Öznitelik adı: kullanıcı kimliği beklenirken özniteliğinin adı.
 
12.   Etkinleştirme giriş bölgesi bulma: Bu bayrağı olup olmadığını denetleyin ADFS tabanlı oturum açma kullanılarak şirket.
 
13.   Etki alanı adı: ADFS tabanlı oturum açma durumunda burada etki alanı adı sağlayın
 
14.   Çoklu oturum kapatma etkinleştirme: bir kullanıcı oturum açtığında JIRA/Confluence Azure AD'den oturumunuzu isterseniz, bu office denetleyin.

### <a name="troubleshooting"></a>Sorun giderme

* Birden çok sertifika hata alıyorsanız
    
    * Azure ad oturum açma ve uygulamayı karşı kullanılabilir birden çok sertifikaları kaldırın. Yalnızca bir sertifika mevcut olduğundan emin olun.

* Azure AD'de dolmak üzere sertifikasıdır.
    
    * Eklentiler sertifikanın otomatik geçişi dikkatli olun. Sertifika süresi için yeni sertifika etkin olarak işaretlenmelidir ve kullanılmayan sertifika silinmesi gerekir. Bir kullanıcı JIRA için bu senaryoda oturum açmaya çalıştığında, eklenti yeni sertifika getirir ve eklenti kaydedin.

* WebSudo (devre dışı güvenli yönetici oturumu) devre dışı bırakma
    
    * JIRA: oturumları (yönetim işlevleri erişmeden önce başka bir deyişle, parola onayı) varsayılan olarak etkin olan yönetici güvenli hale getirin. Bu JIRA örneğinde devre dışı bırakmak istiyorsanız, bu özellik, jira config.properties dosyasında aşağıdaki satırı belirterek devre dışı bırakabilirsiniz: "ira.websudo.is.disabled = true"
    
    * Confluence: aşağıdaki URL'de belirtilen sağlanan adımları izleyin https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html

* Meta veri URL'sini tarafından doldurulmalıdır beklenen alanlar doldurulmaz
    
    * URL'nin doğru olup olmadığını denetleyin. Uygulama kimliği ve doğru Kiracı eşlenen olmadığını denetleyin.
    
    * Tarayıcı URL'den isabet ve Federasyon meta verileri XML almak, bkz.

* İç sunucu hatası:
    
    * Yükleme günlükleri dizininde mevcut günlükleri geçer. Azure AD SSO kullanarak oturum açma kullanıcı çalışırken hata alıyorsanız, aşağıda bu belgede sağlanan destek bilgilerle günlükleri paylaşabilirsiniz.

* Kullanıcı Kimliği kullanıcı oturum açmaya çalıştığında hata bulunamadı
    
    * Kullanıcı JIRA/Confluence oluşturulmaz, böylece aynı oluşturun.

* Uygulama Azure AD içinde bulunamadı hatası
    
    * Uygun URL'yi uygulamaya Azure AD'de eşlenip eşlenmediğini bakın.

* Destek ayrıntıları: üzerinde bize ulaşın: [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 24-48 iş saatleri içinde yanıt vermemiz.
    
    * Azure portal kanal üzerinden Microsoft ile bir destek bileti de yükseltebilirsiniz.