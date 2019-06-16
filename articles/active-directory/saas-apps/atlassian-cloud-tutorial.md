---
title: 'Öğretici: Atlassian bulut ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Atlassian bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdbfb07b73d591bbab64f38e8c986fb1b7e072a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106580"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Öğretici: Atlassian bulut Azure Active Directory ile tümleştirme

Bu öğreticide, Atlassian bulut Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Atlassian bulut Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Atlassian Bulutuna erişimi olan Azure AD'de denetler.
* Otomatik olarak Atlassian buluta, Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Atlassian bulut çoklu oturum açma (SSO) abonelik etkin.
* Güvenlik onaylama işlemi biçimlendirme dili (SAML) çoklu oturum açma için Atlassian bulut ürünleri etkinleştirmek için Atlassian erişimi ayarlamak gerekir. Daha fazla bilgi edinin [Atlassian erişim]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Atlassian bulutun desteklediği **SP ve IDP** tarafından başlatılan

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian bulut galeri ekleme

Azure AD'de Atlassian bulut tümleştirmesini yapılandırmak için Atlassian bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Atlassian bulut** arama kutusuna.
1. Seçin **Atlassian bulut** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO Atlassian adlı bir test kullanıcı kullanarak Bulutu ile test etme **B.Simon**. Çalışmak SSO için Atlassian buluttaki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO Atlassian bulutla sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Atlassian bulut SSO'yu yapılandırarak](#configure-atlassian-cloud-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak B.Simon etkinleştirmek için.
5. **[Atlassian bulut test kullanıcısı oluşturma](#create-atlassian-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Atlassian bulutta B.Simon bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Atlassian bulut** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki alanlar için değerleri girin:

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://auth.atlassian.com/saml/<unique ID>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **geçiş durumu** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Yukarıdaki değerleri gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve yanıt URL'si. Bu gerçek değerleri erişmenizi sağlayacak **Atlassian bulut SAML Yapılandırması** daha sonra açıklanan ekranını **yapılandırma Atlassian bulut çoklu oturum açma** Öğreticisi.

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Atlassian bulut Yönetici portalı oturum açma için örnek değerini yapıştırın.

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. Atlassian bulut uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Atlassian bulut uygulaması bekliyor **NameIdentifier** ile eşlenecek **user.mail**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle** simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **Atlassian bulut kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-atlassian-cloud-sso"></a>Atlassian bulut SSO yapılandırma

1. Atlassian bulut içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Atlassian bulut** Atlassian bulut uygulamaya yönlendirir. Burada, Atlassian bulut hizmetlerinde oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-7 arası adımlar otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Atlassian bulut el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Atlassian bulut şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Çoklu oturum açmayı yapılandırmak için geçmeden önce etki alanınızı doğrulayın gerekir. Daha fazla bilgi için [Atlassian etki alanı doğrulaması](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belge.

5. Sol bölmede seçin **güvenlik** > **SAML çoklu oturum açma**. Zaten yapmadıysanız, Atlassian Identity Manager'a abone olun.

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

6. İçinde **ekleme SAML yapılandırma** penceresinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız.

    b. İçinde **kimlik sağlayıcısı SSO URL** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız.

    c. Açık bir .txt dosyasında Azure portalından indirilen sertifikayı kopyalamanız değeri (olmadan *başlamak sertifika* ve *End Certıfıcate* satırları), ardından yapıştırın **genel X509 Sertifika** kutusu.

    d. Tıklayın **yapılandırmayı kaydetmek**.

7. Doğru URL'leri ayarladığınızdan emin olmak için aşağıdakileri yaparak Azure AD ayarlarını güncelleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. SAML penceresindeki kopyalamak **SP kimliği** ve ardından Azure portalında, Atlassian Bulut'un altında **temel SAML yapılandırma**, yapıştırın **tanımlayıcı** kutusu.

    b. SAML penceresindeki kopyalamak **SP onay belgesi tüketici hizmeti URL'si** ve ardından Azure portalında, Atlassian Bulut'un altında **temel SAML yapılandırma**, yapıştırın **yanıt URL'si**kutusu. Oturum açma URL'si Atlassian bulut Kiracı URL'sidir.

    > [!NOTE]
    > Güncelleştirdikten sonra varolan bir müşteri olmadığınızı **SP kimliği** ve **SP onay belgesi tüketici hizmeti URL'si** Azure portalında, değerler **Evet, yapılandırmayıgüncelleştirme**. Yeni bir müşteri iseniz, bu adımı atlayabilirsiniz.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Atlassian buluta erişim vererek, Azure çoklu oturum açma kullanılacak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Atlassian bulut**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-atlassian-cloud-test-user"></a>Atlassian bulut test kullanıcısı oluşturma

Atlassian buluta oturum açın, aşağıdakileri yaparak Atlassian bulutta el ile kullanıcı hesapları sağlamak Azure AD kullanıcılarının etkinleştirmek için:

1. İçinde **Yönetim** bölmesinde **kullanıcılar**.

    ![Atlassian bulut kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

2. Bir kullanıcı Atlassian bulutta oluşturmak için Seç **davet kullanıcı**.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

3. İçinde **e-posta adresi** kutusuna kullanıcının e-posta adresini girin ve ardından uygulama erişimi atayın.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

4. Kullanıcıya bir e-posta davetiyesi göndermek için seçin **kullanıcıları davet**. Kullanıcıya bir davet e-postası gönderilir ve kullanıcı daveti kabul ettikten sonra sistemde etkin.

> [!NOTE]
> Ayrıca toplu-seçerek kullanıcılar oluşturma **Toplu oluşturma** düğmesine **kullanıcılar** bölümü.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Atlassian bulut kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Atlassian buluta oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)