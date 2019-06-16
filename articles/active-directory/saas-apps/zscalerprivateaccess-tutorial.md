---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim (ZPA) | Microsoft Docs'
description: Zscaler özel erişim (ZPA) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0a1538f640bb4722eca1d4f3a80125837593bab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67085691"
---
# <a name="tutorial-integrate-zscaler-private-access-zpa-with-azure-active-directory"></a>Öğretici: Zscaler özel erişim (ZPA) Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim (ZPA) tümleştirme öğreneceksiniz. Zscaler özel erişim (ZPA) Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Erişimi için Zscaler özel erişim (ZPA) olan Azure AD'de denetler.
* Otomatik olarak Zscaler özel erişim (ZPA) ile Azure AD hesaplarına açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Zscaler özel erişim (ZPA) çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Zscaler özel erişim (ZPA) destekleyen **SP** SSO başlattı.

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler özel erişim (ZPA) ekleme

Azure AD'de, Zscaler özel erişim (ZPA) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden Zscaler özel erişim (ZPA) eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Zscaler özel erişim (ZPA)** arama kutusuna.
1. Seçin **Zscaler özel erişim (ZPA)** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile Zscaler özel erişim (adlı bir test kullanıcı kullanarak ZPA) test **Britta Simon**. Çalışmak SSO için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki Zscaler özel erişim (ZPA içinde) oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile Zscaler özel erişim (ZPA) sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Zscaler özel erişim (ZPA) yapılandırma](#configure-zscaler-private-access-zpa)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Zscaler özel erişim (ZPA) test kullanıcısı oluşturma](#create-zscaler-private-access-zpa-test-user)**  bir karşılığı Britta simon'un içinde Zscaler özel erişim (kullanıcı Azure AD gösterimini bağlı ZPA) sahip.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Zscaler özel erişim (ZPA)** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    1. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL yazın: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > **Oturum açma URL'si** değeri gerçek değil. Değeri, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Zscaler özel erişim (ZPA) istemci Destek ekibine](https://help.zscaler.com/zpa-submit-ticket) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **Federasyon meta verileri XML** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üzerinde **Zscaler özel erişim (ZPA ayarlama) kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-zscaler-private-access-zpa"></a>Zscaler özel erişimi (ZPA) yapılandırma

1. ' % S'yapılandırması içinde Zscaler özel erişim (ZPA) otomatikleştirmek için yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Zscaler özel erişim (ZPA)** Zscaler özel erişim (ZPA) uygulamaya yönlendirir. Burada, Zscaler özel erişim (ZPA içine) oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-6 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Zscaler özel erişim (ZPA) el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve oturum Zscaler özel erişim (ZPA) şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Menü Sol taraftan tıklayın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırmasını**.

    ![Zscaler özel erişim Yönetici Yönetimi](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-administration.png)

5. Sağ üst köşedeki, tıklayın **IDP Yapılandırması Ekle**. 

    ![Zscaler özel erişim Yöneticisi IDP](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-idp.png)

6. Üzerinde **IDP Yapılandırması Ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler özel erişim Yöneticisi seçin](./media/zscalerprivateaccess-tutorial/tutorial-zscaler-private-access-select.png)

    a. Tıklayın **Dosya Seç** indirdiğiniz Azure AD'de meta verileri dosyadan karşıya yüklemek için **IDP meta veri dosyasını karşıya yükleyin.** alan.

    b. Okuduğu **IDP meta verileri** Azure AD'den ve aşağıda gösterildiği gibi tüm alanları bilgileri doldurur.

    ![Zscaler özel erişim Yöneticisi yapılandırma](./media/zscalerprivateaccess-tutorial/config.png)

    c. Etki alanınızdan seçin **etki alanları** alan.
    
    d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Zscaler özel erişim (ZPA) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Zscaler özel erişim (ZPA)** .
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Zscaler özel erişim (ZPA) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Zscaler özel erişim (ZPA içinde) adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) Zscaler özel erişim (ZPA) platform kullanıcıları eklemek için.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Zscaler özel erişim (ZPA) kutucuğu seçtiğinizde, otomatik olarak için Zscaler özel erişim (SSO'yu ayarlama ZPA) oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)