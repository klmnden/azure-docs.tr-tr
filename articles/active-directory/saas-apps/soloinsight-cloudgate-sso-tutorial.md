---
title: 'Öğretici: Soloinsight CloudGate SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Soloinsight CloudGate SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9263c241-85a4-4724-afac-0351d6275958
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2dc6dcbcdee8df93f34cf4d68b5e08453554bc8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67090003"
---
# <a name="tutorial-integrate-soloinsight-cloudgate-sso-with-azure-active-directory"></a>Öğretici: Soloinsight CloudGate SSO Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Soloinsight CloudGate SSO tümleştirme öğreneceksiniz. Soloinsight CloudGate SSO, Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Soloinsight CloudGate SSO erişimi, Azure AD'de denetler.
* Otomatik olarak Soloinsight CloudGate SSO için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Soloinsight CloudGate SSO çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Soloinsight CloudGate SSO'yu destekler **SP** SSO başlattı.

## <a name="adding-soloinsight-cloudgate-sso-from-the-gallery"></a>Galeriden Soloinsight CloudGate SSO ekleme

Azure AD'de Soloinsight CloudGate SSO tümleştirmesini yapılandırmak için Soloinsight CloudGate SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Soloinsight CloudGate SSO** arama kutusuna.
1. Seçin **Soloinsight CloudGate SSO** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO Soloinsight CloudGate adlı bir test kullanıcı kullanarak SSO ile test etme **Britta Simon**. Çalışmak SSO için Soloinsight CloudGate SSO, Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO Soloinsight CloudGate SSO ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Soloinsight CloudGate SSO'yu yapılandırarak](#configure-soloinsight-cloudgate-sso)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Soloinsight CloudGate SSO test kullanıcısı oluşturma](#create-soloinsight-cloudgate-sso-test-user)**  bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Soloinsight CloudGate SSO sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Soloinsight CloudGate SSO** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    1. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.sigateway.com/login`

    1. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<SUBDOMAIN>.sigateway.com/process/sso`

   > [!NOTE]
   > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra açıklanan tanımlayıcısı ile güncelleştirme **yapılandırma Soloinsight CloudGate SSO çoklu oturum açma** öğreticinin bölümünde.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **Soloinsight CloudGate SSO'yu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-soloinsight-cloudgate-sso"></a>Soloinsight CloudGate SSO yapılandırma

1. İçinde Soloinsight CloudGate SSO yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum Soloinsight CloudGate SSO** Soloinsight CloudGate SSO uygulamaya yönlendirir. Burada, Soloinsight CloudGate SSO ile oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-8 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Soloinsight CloudGate SSO el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve Soloinsight CloudGate SSO şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Azure portalında temel SAML yapılandırılırken yapıştırılmasına değerleri almak için kimlik bilgilerinizi kullanarak CloudGate Web portalında oturum açın ardından şu yolda bulunabilir SSO ayarlarına erişmek **Giriş > Yönetim > Sistem Ayarları > Genel**.

    ![CloudGate SSO ayarları](./media/soloinsight-cloudgate-sso-tutorial/sso-main-settings.png)

5. **SAML tüketici URL'si**

    * Bağlantıları karşı kopyalama **Saml tüketici URL** ve **tekrar yönlendirme URL'sini** alanları ve bunları Azure portalında yapıştırın **temel SAML yapılandırma** bölümü **Tanımlayıcı (varlık kimliği)** ve **yanıt URL'si** sırasıyla alanları.

        ![SAMLIdentifier](./media/soloinsight-cloudgate-sso-tutorial/saml-identifier.png)

6. **SAML imzalama sertifikası**

    * Listeleri SAML imzalama sertifikası Azure portalından indirilen sertifika (Base64) dosyasının kaynağına gidin ve sağ tıklayın. Seçin **Düzenle not defteri ++ ile** listeden seçeneği. 

        ![SAMLcertificate](./media/soloinsight-cloudgate-sso-tutorial/certificate-file.png)

    * İçeriği sertifika (Base64) not defteri ++ dosyasına kopyalayın.

        ![Sertifika kopyalama](./media/soloinsight-cloudgate-sso-tutorial/certificate-copy.png)

    * İçerik CloudGate Web portalı SSO ayarlarında yapıştırmak **sertifika** alan ve düğmesine tıklayın.

        ![Sertifika portalı](./media/soloinsight-cloudgate-sso-tutorial/certificate-portal.png)

7. **Varsayılan grubu**

    * Seçin **Business Admin** aşağı açılan listesinden **varsayılan grup** CloudGate Web portalında seçeneği

        ![Varsayılan grubu](./media/soloinsight-cloudgate-sso-tutorial/default-group.png)

8. **AD tanımlayıcısı ve oturum açma URL'si**

    * Kopyalanan **oturum açma URL'si** Azure portalından **Soloinsight CloudGate SSO'yu ayarlama** yapılandırmalarıdır CloudGate Web portalı SSO ayarları bölümünde girilmelidir.

    * Yapıştırma **oturum açma URL'si** CloudGate Web portalında Azure portalından bağlantı **AD oturum açma URL'si** alan.

    * Yapıştırma **Azure AD tanımlayıcısı** CloudGate Web portalında Azure portalından bağlantı **AD tanımlayıcısı** alan

        ![Ad oturum açma](./media/soloinsight-cloudgate-sso-tutorial/ad-login.png)

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Soloinsight CloudGate SSO erişimi vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Soloinsight CloudGate SSO**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-soloinsight-cloudgate-sso-test-user"></a>Soloinsight CloudGate SSO test kullanıcısı oluşturma

Bir test kullanıcısı oluşturmak için Seç **çalışanlar** ana menüden CloudGate Web portalı ve yeni Ekle çalışan formu doldurun. Test kullanıcıya atanacak yetki düzeyi **Business Admin** tıklayarak **Oluştur** sonra tüm gerekli alanları doldurulur.

![Çalışan test](./media/soloinsight-cloudgate-sso-tutorial/employee-test.png)

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Soloinsight CloudGate SSO kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Soloinsight CloudGate SSO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)