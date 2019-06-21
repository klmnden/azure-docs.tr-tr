---
title: 'Öğretici: Yaptığımız yolu ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory arasında yaptığımız gibi çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: faa23f61e5a213c492a7fb51bfc5b108e5c77946
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310399"
---
# <a name="tutorial-integrate-way-we-do-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile yaptığımız şekilde tümleştirin

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme gibi yaptığımız öğreneceksiniz. Yaptığımız gibi Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Erişimi yaptığımız gibi Azure AD'de denetler.
* Otomatik olarak yol yapmamız için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Size çoklu oturum açma (SSO) yolu abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin.

* Yaptığımız biçimini destekler **SP** tarafından başlatılan
* Yaptığımız biçimini destekler **zamanında** kullanıcı sağlama

## <a name="adding-way-we-do-from-the-gallery"></a>Yaptığımız gibi galeri ekleme

Yaptığımız gibi Azure AD'de tümleştirmesini yapılandırmak için yaptığımız gibi galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **yaptığımız gibi** arama kutusuna.
1. Seçin **yaptığımız gibi** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve test etme yolu kullanarak adlı bir test kullanıcısı olan yaptığımız ile Azure AD SSO **B.Simon**. Çalışmak SSO için yol yaptığımız içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile yaptığımız gibi sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Yol biz yapmak SSO'yu yapılandırarak](#configure-way-we-do-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Biz kullanıcı test yolu oluşturma](#create-way-we-do-test-user)**  - bir karşılığı Britta simon'un şekilde kullanıcı Azure AD gösterimini bağlı bunu sağlamak için.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **yaptığımız gibi** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (ham)** seçip **indirme**sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificateraw.png)

1. Üzerinde **şekilde yaptığımız kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-way-we-do-sso"></a>SSO yaptığımız şekilde yapılandırın

1. Yaptığımız gibi içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

1. Tarayıcı uzantısı ekledikten sonra tıklayın **Kurulum şekilde yaptığımız** yaptığımız gibi uygulamaya yönlendirir. Burada, yol yaptığımız oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-6 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Yaptığımız gibi el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve yaptığımız gibi şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Tıklayın **kişi simgesi** yaptığımız gibi herhangi bir sayfasında sağ üst köşesinde, ardından **hesabı** açılır menüdeki.

    ![Yol yaptığımız hesabı](./media/waywedo-tutorial/tutorial_waywedo_account.png)

1. Tıklayın **menüsü simgesi** anında iletme Gezinti menüsüne ve ardından açmak için **çoklu oturum açma**.

    ![Yol yaptığımız tek](./media/waywedo-tutorial/tutorial_waywedo_single.png)

1. Üzerinde **tek oturum açma Kurulumu** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yol yaptığımız Kaydet](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Tıklayın **üzerinde çoklu oturum açmayı etkinleştirmek** geç **Evet** çoklu oturum açmayı etkinleştirmek için.

    b. İçinde **tek oturum açma adı** metin adınızı girin.

    c. İçinde **varlık kimliği** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure portaldan kopyaladığınız.

    d. İçinde **SAML SSO URL** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Tıklayarak sertifikayı karşıya yüklemek **seçin** düğmesinin yanındaki **sertifika**.

    f. **İsteğe bağlı ayarlar** -
    
    * Parolaları - etkinleştirme bu seçeneği devre dışı bırakıldığında, böylece kullanıcıların yalnızca çoklu oturum açma kullanabilirsiniz normal parola şekilde yapmamız için çalışır.

    * Otomatik sağlamayı - etkinleştirme etkinleştirildiğinde, oturum açma için kullanılan e-posta adresine otomatik olarak yaptığımız gibi kullanıcıların listesine karşılaştırılır. Yaptığımız gibi etkin bir kullanıcı e-posta adresi ile eşleşmiyorsa, oturum açma, eksik bilgileri isteyen kişi için otomatik olarak yeni bir kullanıcı hesabı ekler.

      > [!NOTE]
      > Çoklu oturum açma ile eklenen kullanıcılar genel kullanıcılar olarak eklenir ve sistemde bir rolü atanmaz. Bir yönetici, Git ve bir düzenleyici veya yönetici olarak güvenlik rolü değiştirebilirsiniz ve ayrıca bir veya birden çok kuruluş şeması roller atayabilirsiniz.

    g. Tıklayın **Kaydet** ayarlarınızı kalıcı hale getirmek için.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma yolu yapmamız için erişim izni verme kullanmak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **yaptığımız gibi**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-way-we-do-test-user"></a>Yaptığımız gibi test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı yolu yaptığımız içinde oluşturulur. Yol yaptığımız just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı yolu yaptığımız içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com).

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde yaptığımız gibi kutucuğu seçtiğinizde, otomatik olarak yol biz SSO'yu ayarlama yapmak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)