---
title: 'Öğretici: G Suite ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: G Suite ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d48977c60fb4a13e1fc0dbd294fa4e7708f1cd5d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878977"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Öğretici: G Suite ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile G Suite tümleştirme konusunda bilgi edinin.
G Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* G Suite erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) G Suite için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

G Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir G Suite çoklu oturum açma etkin
- Google Apps aboneliği veya Google Cloud Platform abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz. Bu belge, yeni kullanıcı çoklu oturum açma deneyimini kullanarak oluşturuldu. Kurulum, hala eski kullanıyorsanız, farklı olarak görünecektir. G Suite uygulamanın çoklu oturum açma ayarlarını yeni deneyim etkinleştirebilirsiniz. Git **Azure AD, kurumsal uygulamalar**seçin **G Suite**seçin **çoklu oturum açma** ve ardından **yeni deneyimimizi deneyin**.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

1. **S: Bu tümleştirme, Azure AD ile tümleştirme Google Cloud Platform SSO destekliyor mu?**

    Y: Evet. Google Cloud Platform ve Google Apps, aynı kimlik doğrulaması platformunun paylaşın. Bu nedenle GCP tümleştirme yapmak için Google Apps ile SSO yapılandırmanız gerekir.

2. **S: Chromebooks ve diğer Chrome cihazları Azure AD çoklu oturum açma ile uyumludur?**
  
    Y: Evet, kullanıcılar, Azure AD kimlik bilgilerini kullanarak Chromebook cihazlarını oturum açabilir. Bkz. Bu [G Suite Destek makalesi](https://support.google.com/chrome/a/answer/6060880) neden hakkında bilgi için kimlik bilgilerini iki kez kullanıcılardan.

3. **S: Çoklu oturum açma etkinleştirirseniz, kullanıcıların Google Classroom, GMail, Google Drive, YouTube ve benzeri gibi tüm Google ürün oturum açmak için Azure AD kimlik bilgilerini kullanmanız mümkün olacak mı?**

    Y: Evet, bağlı olarak [hangi G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) etkinleştirmek veya kuruluşunuz için devre dışı bırakmak seçin.

4. **S: Yalnızca bir alt kümesini G Suite Kullanıcılarım için çoklu oturum açmayı etkinleştirebilir?**

    Y: Hayır, çoklu oturum açmayı üzerinde hemen kapatma kendi Azure AD kimlik bilgileriyle kimlik doğrulaması tüm G Suite kullanıcılarınız gerektirir. G Suite sahip birden çok kimlik sağlayıcı desteklemediğinden, G Suite ortamınız için kimlik sağlayıcısı ya da Azure AD olabilir veya Google--ikisi aynı anda.

5. **S: Bir kullanıcı Windows oturum açtıysa, otomatik olarak G Suite için bir parola girmeniz istenir alma olmadan kimlik doğrulaması misiniz?**

    Y: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Windows 10 cihazları oturum [Azure Active Directory Join](../device-management-introduction.md). Alternatif olarak, kullanıcıların etki alanına katılmış bir şirket içi Active Directory'ye Azure ad çoklu oturum açma için etkinleştirilmiş olan Windows cihazları oturum bir [Active Directory Federasyon Hizmetleri (AD FS)](../hybrid/plan-connect-user-signin.md) dağıtım. Azure AD arasında çoklu oturum açmayı etkinleştirmek için aşağıdaki öğreticiye adımları gerçekleştirmek iki seçenek de gerektirir ve G Suite.

6. **S: "Geçersiz e-posta" hata iletisi alıyorum olduğunda ne yapmalıyım?**

    Y: Bu kurulumu için e-posta özniteliği, kullanıcıların oturum açabilmesi için gereklidir. Bu öznitelik el ile ayarlanamaz.

    E-posta özniteliği geçerli bir Exchange lisansına sahip herhangi bir kullanıcı için otomatik doldurulur. Kullanıcı e-posta etkin değilse, erişim vermek için bu öznitelik almak uygulama gereksinimleriniz değiştikçe bu hata alınır.

    Bir yönetici hesabıyla portal.office.com tıklayarak Yönetim merkezinde, abonelikler, faturalama, Office 365 aboneliğinizi seçin ve ardından Ata seçeneğine tıklayın, kullanıcılara seçin aboneliğini denetleyin ve sağ bölmede, tıklayarak istediğiniz kullanıcıların gidebilirsiniz lisansları düzenleyin.

    O365 lisansı atandıktan sonra uygulanması bazı dakika sürebilir. Bundan sonra user.mail öznitelik girdiğinizde olur ve sorunun çözülmesi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* G Suite destekler **SP** tarafından başlatılan
* G Suite destekler  **[otomatik kullanıcı hazırlama](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)**

## <a name="adding-g-suite-from-the-gallery"></a>G Suite galeri ekleme

Azure AD'de G Suite tümleştirmesini yapılandırmak için G Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**G Suite Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **G Suite**seçin **G Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde G Suite](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı G Suite ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve G Suite ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma G Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[G Suite çoklu oturum açmayı yapılandırma](#configure-g-suite-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[G Suite test kullanıcısı oluşturma](#create-g-suite-test-user)**  - kullanıcı Azure AD gösterimini bağlı G Suite Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma G Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **G Suite** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölüm için yapılandırmak istiyorsanız **Gmail** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için.

5. Üzerinde **temel SAML yapılandırma** bölüm için yapılandırmak istiyorsanız **Google Cloud Platform** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için.

6. G Suite uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **benzersiz kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak G Suite bu kullanıcının e-posta adresi ile eşlenmesini bekliyor. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın.

    ![image](common/edit-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği |
    | ---------------| --------------- |
    | Benzersiz Kullanıcı Tanımlayıcısı | User.Mail |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **G Suite kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-g-suite-single-sign-on"></a>G Suite çoklu oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve oturum [G Suite Yönetici Konsolu](https://admin.google.com/) yönetici hesabınızı kullanarak.

2. Tıklayın **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **diğer denetimler** ekranın alt kısmındaki menü.

    ![Güvenlik'e tıklayın.][10]

3. Üzerinde **güvenlik** sayfasında **çoklu oturum açmayı (SSO) ayarlayın.**

    ![SSO tıklayın.][11]

4. Şu yapılandırma değişiklikleri gerçekleştirin:

    ![SSO yapılandırma][12]

    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan G Suite'te, değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **sayfa oturum kapatma URL'si** alan G Suite'te, değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **değiştirme parola URL'si** alan G Suite'te, değerini yapıştırın **değiştirme parola URL'si** , Azure Portalı'ndan kopyaladığınız.

    e. G Suite'teki için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Seçin **belirli bir etki alanı gönderici kullanın**.

    g. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, G Suite erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **G Suite**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **G Suite**.

    ![Uygulamalar listesini G Suite bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-g-suite-test-user"></a>G Suite test kullanıcısı oluşturma

Bu bölümün amacı, G Suite yazılımı Britta Simon adlı bir kullanıcı oluşturmaktır. G Suite otomatik sağlamayı, varsayılan olarak etkin olan destekler. Bu bölümde hiçbir şey yoktur. Bir kullanıcı, G Suite yazılımı zaten yoksa, G Suite yazılımı erişmeye çalıştığında yeni bir tane oluşturulur.

> [!NOTE]
> Sağlama Azure AD'de çoklu oturum açmayı test etmeden önce açık durumda değil, kullanıcı G Suite içinde zaten mevcut olduğundan emin olun.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google Destek ekibine](https://www.google.com/contact/).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli G Suite kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama G suite'te oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Kullanıcı sağlamayı yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png
