---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Appraisd | Microsoft Docs'
description: Azure Active Directory ve Appraisd arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08453928ab000cf906c451fa6c1cd619a00ee4ca
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561192"
---
# <a name="tutorial-integrate-appraisd-with-azure-active-directory"></a>Öğretici: Appraisd Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Appraisd tümleştirme öğreneceksiniz. Appraisd Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Appraisd erişimi, Azure AD'de denetler.
* Otomatik olarak Appraisd için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Appraisd çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Appraisd destekler **SP ve IDP** SSO başlattı.

## <a name="adding-appraisd-from-the-gallery"></a>Galeriden Appraisd ekleme

Azure AD'de Appraisd tümleştirmesini yapılandırmak için Appraisd Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Appraisd** arama kutusuna.
1. Seçin **Appraisd** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO kullanarak adlı bir test kullanıcı Appraisd ile test etme **b Simon**. Çalışmak SSO için Appraisd içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Appraisd sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Appraisd yapılandırma](#configure-appraisd)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
5. **[Appraisd test kullanıcısı oluşturma](#create-appraisd-test-user)**  b Simon bir karşılığı kullanıcı Azure AD gösterimini bağlı Appraisd sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Appraisd** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamanın önceden yapılandırılmış olduğu ve gerekli URL'ler zaten Azure ile önceden doldurulur. Kullanıcının erişmesi Kaydet düğmesine tıklayarak yapılandırmayı kaydetmek ve aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **ek URL'lerini ayarlayın**.

    b. İçinde **geçiş durumu** metin kutusuna bir URL yazın: `<TENANTCODE>`

    c. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > Bu öğreticinin ilerleyen bölümlerinde açıklanan Appraisd SSO yapılandırma sayfasında gerçek oturum açma URL'si ve geçiş durumu değer elde edin.

1. Appraisd uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Appraisd uygulama bekliyor **NameIdentifier** ile eşlenecek **user.mail**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle** simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **Appraisd kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-appraisd"></a>Appraisd yapılandırın

1. Yüklemeniz gerekiyor Appraisd içinde yapılandırmasını otomatik hale getirmenizi **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Appraisd** Appraisd uygulamaya yönlendirir. Burada, Appraisd oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-7 arası adımlar otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Appraisd el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Appraisd şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Üst sayfanın sağ tıklayın **ayarları** simgesine ve ardından gidin **yapılandırma**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

5. Menü Sol taraftan tıklayarak **SAML çoklu oturum açma**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

6. Üzerinde **SAML 2.0 çoklu oturum açmayı yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopyalama **varsayılan geçiş durumu** yapıştırın ve değer **geçiş durumu** metin kutusunda **temel SAML yapılandırma** Azure portalında.

    b. Kopyalama **hizmet tarafından başlatılan oturum açma URL'si** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **temel SAML yapılandırma** Azure portalında.

7. Altında aynı sayfayı aşağı kaydırın **kullanıcıları tanımlama**, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, Azure portal'ı seçin ve kopyalanan **Kaydet**.

    b. İçinde **kimlik sağlayıcısını veren URL'si** metin değerini yapıştırın **Azure AD tanımlayıcısı**, Azure portal'ı seçin ve kopyalanan **Kaydet**.

    c. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusuna ve tıklatın **Kaydet**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B. Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, B. Appraisd için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Appraisd**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-appraisd-test-user"></a>Appraisd test kullanıcısı oluşturma

Azure AD etkinleştirmek için Appraisd için kullanıcıların oturum bunların Appraisd sağlanması gerekir. Appraisd içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Appraisd bir güvenlik yöneticisi olarak oturum açın.

2. Üst sayfanın sağ tıklayın **ayarları** simgesine ve ardından gidin **Yönetim Merkezi**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Sayfanın üst kısmındaki araç çubuğunda **kişiler**, ardından gidin **yeni kullanıcı ekleme**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Üzerinde **yeni kullanıcı ekleme** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)

    a. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin `B. Simon@contoso.com`.

    d. **Kullanıcı ekle**'ye tıklayın.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Appraisd kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Appraisd için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
