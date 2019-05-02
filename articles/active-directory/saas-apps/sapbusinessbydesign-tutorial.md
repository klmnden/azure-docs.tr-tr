---
title: 'Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Business ByDesign ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23669671c9aec2ebad8e03e06a0ea1b139214cad
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683849"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Business ByDesign tümleştirme konusunda bilgi edinin.
SAP Business ByDesign ile Azure AD tümleştirme ile aşağıdaki avantajları sağlar:

* SAP Business ByDesign erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için SAP Business ByDesign oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP Business ByDesign ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* SAP Business ByDesign çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP Business ByDesign destekler **SP** tarafından başlatılan

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign galeri ekleme

Azure AD'de SAP Business ByDesign tümleştirmesini yapılandırmak için SAP Business ByDesign Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Business ByDesign Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP Business ByDesign**seçin **SAP Business ByDesign** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![SAP Business ByDesign sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve adlı bir test kullanıcı tabanlı SAP Business ByDesign ile Azure AD çoklu oturum açmayı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAP Business ByDesign ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP Business ByDesign ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP Business ByDesign çoklu oturum açmayı yapılandırma](#configure-sap-business-bydesign-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP Business ByDesign test kullanıcısı oluşturma](#create-sap-business-bydesign-test-user)**  - kullanıcı Azure AD gösterimini bağlı olan SAP Business ByDesign Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

SAP Business ByDesign ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP Business ByDesign** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<servername>.sapbydesign.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<servername>.sapbydesign.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [SAP Business ByDesign istemci Destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. SAP Business ByDesign uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. Tıklayarak **Düzenle** düzenleme simgesi **ad tanımlayıcı değeri**.

    ![image](media/sapbusinessbydesign-tutorial/mail-prefix1.png)

7. Üzerinde **yönetmek, kullanıcı talepleri** bölümünde, aşağıdaki adımları gerçekleştirin: ![görüntüsü](media/sapbusinessbydesign-tutorial/mail-prefix2.png)

    a. Seçin **dönüştürme** olarak bir **kaynak**.

    b. İçinde **dönüştürme** açılan listesinden **ExtractMailPrefix()**.

    c. İçinde **parametresi 1** açılan listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin. Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı kullanmak istediğiniz ve öznitelik değeri içinde ExtensionAttribute2 depoladığınız seçerseniz, user.extensionattribute2 seçin.

    d. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

9. Üzerinde **SAP Business ByDesign kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sap-business-bydesign-single-sign-on"></a>SAP Business ByDesign tek oturum açmayı yapılandırın

1. SAP Business ByDesign portalınız yönetici haklarıyla oturum açın.

2. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.

3. Tıklayın **yeni kimlik sağlayıcısı** ve Azure portalından indirdiğiniz meta veri XML dosyasını seçin. Meta verileri içeri aktararak, sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikasını karşıya yükler.

    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

4. Eklenecek **onay belgesi tüketici hizmeti URL'si** SAML isteği seçin **onay belgesi tüketici hizmeti URL'si dahil**.

5. Tıklayın **çoklu oturum açmayı etkinleştirme**.

6. Yaptığınız değişiklikleri kaydedin.

7. Tıklayın **My sistem** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

8. İçinde **Azure AD oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

9. Kullanıcı kimliği ve parola veya SSO seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçim**.

10. İçinde **SSO URL** bölümünde bir çalışana sisteme oturum açma için kullanılması gereken URL'yi belirtin.
    URL'ye gönderilen çalışan açılan listesi için aşağıdaki seçenekler arasından seçim yapabilirsiniz:

    **SSO URL**

    Sistem, yalnızca normal sistem URL çalışana gönderir. Çalışan, SSO kullanarak oturum açma olamaz ve parolayı kullanın veya gerekir bunun yerine sertifika.

    **SSO URL'Sİ**

    Sistem SSO URL çalışana gönderir. Çalışan SSO kullanarak oturum açma belirleyebilir. Kimlik doğrulama isteği, IDP yönlendirilir.

    **Otomatik Seçim**

    SSO etkin değilse, sistemin normal sistem URL çalışana gönderir. SSO etkin olursa, sistem çalışan bir parolaya sahip olup olmadığını denetler. Bir parolası varsa, hem SSO URL hem de olmayan SSO URL çalışana gönderilir. Ancak, çalışan parolası yoksa, yalnızca SSO URL çalışana gönderilir.

11. Yaptığınız değişiklikleri kaydedin.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için SAP Business ByDesign erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP Business ByDesign**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Business ByDesign**.

    ![Uygulamalar listesini SAP Business ByDesign bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-business-bydesign-test-user"></a>SAP Business ByDesign test kullanıcısı oluşturma

Bu bölümde, Britta Simon SAP Business ByDesign adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [SAP Business ByDesign istemci Destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) SAP Business ByDesign platform kullanıcıları eklemek için. 

> [!NOTE]
> Nameıd değer SAP Business ByDesign platform kullanıcıadı alanıyla eşleşmelidir emin olun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP Business ByDesign kutucuğa tıkladığınızda, size otomatik olarak SAP Business ByDesign SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
