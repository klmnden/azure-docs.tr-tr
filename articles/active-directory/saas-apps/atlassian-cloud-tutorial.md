---
title: 'Öğretici: Atlassian bulut ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Atlassian bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66b8b9076c0a4b1fddda4ab0bcfe9f104d7dcf8a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191115"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Öğretici: Atlassian bulut ile Azure Active Directory Tümleştirme

Bu öğreticide, Atlassian bulut Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Atlassian bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Atlassian Bulutuna erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Atlassian buluta oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Atlassian Bulutla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Atlassian bulut çoklu oturum açma abonelik etkin.
* Güvenlik onaylama işlemi biçimlendirme dili (SAML) çoklu oturum açma için Atlassian bulut ürünleri etkinleştirmek için Atlassian erişimi ayarlamak gerekir. Daha fazla bilgi edinin [Atlassian erişim]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Atlassian bulutun desteklediği **SP ve IDP** tarafından başlatılan

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian bulut galeri ekleme

Azure AD'de Atlassian bulut tümleştirmesini yapılandırmak için Atlassian bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Atlassian bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Atlassian bulut**seçin **Atlassian bulut** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Atlassian bulut](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırdığınız ve Azure AD çoklu oturum açmayı test Atlassian bulutla adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Atlassian bulutta arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Atlassian Bulutu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Atlassian bulut çoklu oturum açmayı yapılandırma](#configure-atlassian-cloud-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Atlassian bulut test kullanıcısı oluşturma](#create-atlassian-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Atlassian bulutta Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Atlassian Bulutla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Atlassian bulut** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![[Uygulama adı] Etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-relay.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://auth.atlassian.com/saml/<unique ID>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **geçiş durumu** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Yukarıdaki değerleri gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'si. Bu gerçek değerleri erişmenizi sağlayacak **Atlassian bulut SAML Yapılandırması** daha sonra açıklanan ekranını **yapılandırma Atlassian bulut çoklu oturum açma** Öğreticisi.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![[Uygulama adı] Etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Önceki oturum açma URL değeri, gerçek değil. Değeri, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Atlassian bulut istemci Destek ekibine](https://support.atlassian.com/) bu değeri alınamıyor.

6. Atlassian bulut uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Atlassian bulut uygulaması bekliyor **NameIdentifier** ile eşlenecek **user.mail**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle** simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **Atlassian bulut kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-atlassian-cloud-single-sign-on"></a>Atlassian bulut çoklu oturum açmayı yapılandırın

1. Atlassian bulut içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Atlassian bulut** Atlassian bulut uygulamaya yönlendirir. Burada, Atlassian bulut hizmetlerinde oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-7 arası adımlar otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Atlassian bulut el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Atlassian bulut şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Çoklu oturum açmayı yapılandırmak için geçmeden önce etki alanınızı doğrulayın gerekir. Daha fazla bilgi için [Atlassian etki alanı doğrulaması](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belge.

5. Sol bölmede seçin **SAML çoklu oturum açma**. Zaten yapmadıysanız, Atlassian Identity Manager'a abone olun.

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

6. İçinde **ekleme SAML yapılandırma** penceresinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız.

    b. İçinde **kimlik sağlayıcısı SSO URL** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız.

    c. Açık bir .txt dosyasında Azure portalından indirilen sertifikayı kopyalamanız değeri (olmadan *başlamak sertifika* ve *End Certıfıcate* satırları), ardından yapıştırın **genel X509 Sertifika** kutusu.

    d. Tıklayın **yapılandırmayı kaydetmek**.

7. Doğru URL'leri ayarladığınızdan emin olmak için aşağıdakileri yaparak Azure AD ayarlarını güncelleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. SAML penceresindeki kopyalamak **SP kimliği** ve ardından Azure portalında, Atlassian Bulut'un altında **temel SAML yapılandırma**, yapıştırın **tanımlayıcı** kutusu.

    b. SAML penceresindeki kopyalamak **SP onay belgesi tüketici hizmeti URL'si** ve ardından Azure portalında, Atlassian Bulut'un altında **temel SAML yapılandırma**, yapıştırın **yanıt URL'si**kutusu. Oturum açma URL'si Atlassian bulut Kiracı URL'sidir.

    > [!NOTE]
    > Güncelleştirdikten sonra varolan bir müşteri olmadığınızı **SP kimliği** ve **SP onay belgesi tüketici hizmeti URL'si** Azure portalında, değerler **Evet, yapılandırmayıgüncelleştirme**. Yeni bir müşteri iseniz, bu adımı atlayabilirsiniz.

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

Bu bölümde, Atlassian buluta erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Atlassian bulut**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-atlassian-cloud-test-user"></a>Atlassian bulut test kullanıcısı oluşturma

Atlassian buluta oturum açın, aşağıdakileri yaparak Atlassian bulutta el ile kullanıcı hesapları sağlamak Azure AD kullanıcılarının etkinleştirmek için:

1. İçinde **Yönetim** bölmesinde **kullanıcılar**.

    ![Atlassian bulut kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Bir kullanıcı Atlassian bulutta oluşturmak için Seç **davet kullanıcı**.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. İçinde **e-posta adresi** kutusuna kullanıcının e-posta adresini girin ve ardından uygulama erişimi atayın.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Kullanıcıya bir e-posta davetiyesi göndermek için seçin **kullanıcıları davet**. Kullanıcıya bir davet e-postası gönderilir ve kullanıcı daveti kabul ettikten sonra sistemde etkin.

> [!NOTE]
> Ayrıca toplu-seçerek kullanıcılar oluşturma **Toplu oluşturma** düğmesine **kullanıcılar** bölümü.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Atlassian bulut kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Atlassian buluta oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
   
