---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile her yerde bilgi LMS | Microsoft Docs'
description: Azure Active Directory ve her yerde bilgi LMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f44324bbdd5af6675dfb4f5664cbbde2627edfec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098554"
---
# <a name="tutorial-integrate-knowledge-anywhere-lms-with-azure-active-directory"></a>Öğretici: Bilgi Bankası, LMS herhangi bir Azure Active Directory ile tümleştirin.

Bu öğreticide, Azure Active Directory (Azure AD) ile her yerde bilgi LMS tümleştirme öğreneceksiniz. Herhangi bir Bilgi Bankası LMS Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Herhangi bir Bilgi Bankası LMS erişimi, Azure AD'de denetler.
* Otomatik olarak herhangi bir Bilgi Bankası LMS kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Bilgi Bankası LMS her yerden çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Herhangi bir Bilgi Bankası LMS destekler **SP** SSO ve destekler başlatılan **zamanında** kullanıcı sağlama.

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Galeriden herhangi bir Bilgi Bankası LMS ekleme

Azure AD'de herhangi bir Bilgi Bankası LMS tümleştirmesini yapılandırmak için her yerde bilgi LMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **herhangi bir Bilgi Bankası LMS** arama kutusuna.
1. Seçin **herhangi bir Bilgi Bankası LMS** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile bilgi her yerde adlı bir test kullanıcı kullanarak LMS test **b Simon**. Çalışmak SSO için Bilgi Bankası LMS içinde herhangi bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile her yerde bilgi LMS sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Herhangi bir Bilgi Bankası LMS yapılandırma](#configure-knowledge-anywhere-lms)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
5. **[Herhangi bir Bilgi Bankası LMS test kullanıcısı oluşturma](#create-knowledge-anywhere-lms-test-user)**  b Simon bir karşılığı bilgi herhangi bir kullanıcı Azure AD gösterimini bağlı LMS sağlamak için.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **herhangi bir Bilgi Bankası LMS** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    1. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    1. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve öğreticinin sonraki bölümlerinde açıklanan yanıt URL'si ile güncelleştirin.

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<CLIENTNAME>.knowledgeanywhere.com/`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [bilgi her yerde LMS istemci Destek ekibine](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **sertifika (Base64)** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **bilgi LMS herhangi bir yerde ayarlanmış** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-knowledge-anywhere-lms"></a>Bilgi Bankası LMS herhangi bir yapılandırma

1. İçinde herhangi bir Bilgi Bankası LMS yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **herhangi bir kurulum bilgisi LMS** herhangi bir Bilgi Bankası LMS uygulamaya yönlendirir. Buradan bilgi LMS her yerde oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-7 arası adımlar otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Herhangi bir Bilgi Bankası LMS el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve her yerde bilgi LMS şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. SELECT deyiminde **Site** sekmesi.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure1.png)

5. SELECT deyiminde **SAML ayarlarını** sekmesi.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure2.png)

6. Tıklayarak **yeni Ekle**.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure3.png)

7. Üzerinde **ekleme/güncelleştirme SAML ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure4.png)

    a. Kuruluşunuz göre IDP adını girin. İçin örnek:- `Azure`.

    b. İçinde **IDP varlık kimliği** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure Portalı'ndan kopyaladığınız değeri.

    c. İçinde **IDP URL** metin kutusu, yapıştırma **oturum açma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    d. İndirilen sertifika dosyasını Not Defteri'ne Azure portalından açın, sertifika içeriğini kopyalayın ve yapıştırın **sertifika** metin.

    e. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    f. Seçin **ana Site** için açılır menüden **etki alanı**.

    g. Kopyalama **SP varlık kimliği** yapıştırın ve değer **tanımlayıcı** metin kutusu **temel SAML yapılandırma** bölümünde Azure portalında.

    h. Kopyalama **SP Response(ACS) URL** yapıştırın ve değer **yanıt URL'si** metin kutusu **temel SAML yapılandırma** bölümünde Azure portalında.

    i. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, her yerde bilgi LMS erişim vererek Azure çoklu oturum açmayı kullanmak b Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **herhangi bir Bilgi Bankası LMS**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Herhangi bir Bilgi Bankası LMS test kullanıcısı oluşturma

Bu bölümde, Bilgi Bankası LMS içinde her yerden b Simon adlı bir kullanıcı oluşturuldu. Herhangi bir Bilgi Bankası LMS just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bilgi Bankası LMS içinde her yerden bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde herhangi bir Bilgi Bankası LMS kutucuğu seçtiğinizde, otomatik olarak bilgi her yerde SSO'yu ayarlama LMS için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)