---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile LiquidFiles | Microsoft Docs'
description: Azure Active Directory ve LiquidFiles arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6514594d3119ebf8fab774c3e84c85e34bdfeaf4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097940"
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a>Öğretici: LiquidFiles ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LiquidFiles tümleştirme konusunda bilgi edinin.
Azure AD ile LiquidFiles tümleştirme ile aşağıdaki avantajları sağlar:

* LiquidFiles erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) LiquidFiles için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile LiquidFiles yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* LiquidFiles tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* LiquidFiles destekler **SP** tarafından başlatılan

## <a name="adding-liquidfiles-from-the-gallery"></a>Galeriden LiquidFiles ekleme

Azure AD'de LiquidFiles tümleştirmesini yapılandırmak için LiquidFiles Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden LiquidFiles eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **LiquidFiles**seçin **LiquidFiles** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde LiquidFiles](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma LiquidFiles adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının LiquidFiles ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma LiquidFiles ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[LiquidFiles çoklu oturum açmayı yapılandırma](#configure-liquidfiles-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[LiquidFiles test kullanıcısı oluşturma](#create-liquidfiles-test-user)**  - kullanıcı Azure AD gösterimini bağlı LiquidFiles Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile LiquidFiles yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **LiquidFiles** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![LiquidFiles etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YOUR_SERVER_URL>/saml/init`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<YOUR_SERVER_URL>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<YOUR_SERVER_URL>/saml/consume`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [LiquidFiles istemci Destek ekibine](https://www.liquidfiles.com/support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

6. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

7. Üzerinde **LiquidFiles kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-liquidfiles-single-sign-on"></a>LiquidFiles tek oturum açmayı yapılandırın

1. LiquidFiles şirketinizin sitesi için yönetici olarak oturum.

1. Tıklayın **çoklu oturum açma** içinde **yönetici > yapılandırma** menüsünde.

1. Üzerinde **çoklu oturum açma yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin

    ![Çoklu oturum açmayı yapılandırın](./media/liquidfiles-tutorial/tutorial_single_01.png)

    a. Olarak **tek oturum açma yöntemini**seçin **SAML 2**.

    b. İçinde **IDP'nin oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **IDP sertifika parmak izi** metin kutusu, yapıştırma **parmak İZİ** Azure Portalı'ndan kopyaladığınız değeri...

    e. Ad tanımlayıcı biçimi metin kutusunda, değeri yazın `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Authn bağlam metin kutusunda, değeri yazın `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport`.

    g. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için LiquidFiles erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **LiquidFiles**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **LiquidFiles**.

    ![Uygulamalar listesinde LiquidFiles bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-liquidfiles-test-user"></a>LiquidFiles test kullanıcısı oluşturma

Bu bölümün amacı LiquidFiles Britta Simon adlı bir kullanıcı oluşturmaktır. Kendiniz LiquidFiles uygulamanıza oturum açma önce bir kullanıcı olarak eklenmiş almak için LiquidFiles server yöneticinizle birlikte çalışın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli LiquidFiles kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama LiquidFiles için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

