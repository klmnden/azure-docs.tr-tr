---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Envi MMIS | Microsoft Docs'
description: Azure Active Directory ve Envi MMIS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff60e378e900d618cfc07f53959aa2d64518353c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103363"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Öğretici: Envi MMIS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Envi MMIS tümleştirme konusunda bilgi edinin.
Azure AD ile Envi MMIS tümleştirme ile aşağıdaki avantajları sağlar:

* Envi MMIS erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Envi MMIS için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Envi MMIS yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Envi MMIS çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Envi MMIS destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-envi-mmis-from-the-gallery"></a>Galeriden Envi MMIS ekleme

Azure AD'de Envi MMIS tümleştirmesini yapılandırmak için Envi MMIS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Envi MMIS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Envi MMIS**seçin **Envi MMIS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Envi MMIS](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Envi MMIS ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Envi MMIS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Envi MMIS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Envi MMIS çoklu oturum açmayı yapılandırma](#configure-envi-mmis-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Envi MMIS test kullanıcısı oluşturma](#create-envi-mmis-test-user)**  - kullanıcı Azure AD gösterimini bağlı Envi MMIS Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Envi MMIS yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Envi MMIS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Envi MMIS etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Envi MMIS etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.<CUSTOMER DOMAIN>.com/Account`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Envi MMIS istemci Destek ekibine](mailto:support@ioscorp.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **Envi MMIS kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-envi-mmis-single-sign-on"></a>Envi MMIS çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Envi MMIS sitenize yönetici olarak oturum açın.

2. Tıklayarak **My Domain** sekmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure1.png)

3. **Düzenle**‘ye tıklayın.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure2.png)

4. Seçin **uzak kimlik doğrulaması kullan** onay kutusunu seçip **HTTP yeniden yönlendirme** gelen **kimlik doğrulama türü** açılır.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure3.png)

5. Seçin **kaynakları** sekmesine ve ardından **meta verilerini karşıya yükleme**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure4.png)

6. İçinde **meta verilerini karşıya yükleme** açılan, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure5.png)

    a. Seçin **dosya** seçeneğini **karşıya gelen** açılır.

    b. Azure portalından indirilen meta veri dosyası seçerek karşıya **dosya simgesini**.

    c. **Tamam**’a tıklayın.

7. İndirilen meta veri dosyasını karşıya yükledikten sonra alanları otomatik olarak doldurulacak. Tıklayın **güncelleştirme**

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Envi MMIS erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Envi MMIS**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Envi MMIS**.

    ![Uygulamalar listesinde Envi MMIS bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından'a tıklayın **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-envi-mmis-test-user"></a>Envi MMIS test kullanıcısı oluşturma

Envi MMIS için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Envi MMIS sağlanması gerekir. Envi MMIS söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Envi MMIS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **kullanıcı listesi** sekmesi.

    ![Çalışan Ekle](./media/envimmis-tutorial/user1.png)

3. Tıklayın **Kullanıcı Ekle** düğmesi.

    ![Çalışan Ekle](./media/envimmis-tutorial/user2.png)

4. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/envimmis-tutorial/user3.png)

    a. İçinde **kullanıcı adı** Britta Simon hesap kullanıcı adı türü metin ister **brittasimon\@contoso.com**.
    
    b. İçinde **ad** metin türü adı BrittaSimon ister **Britta**.

    c. İçinde **Soyadı** BrittaSimon Soyadı türünü metin ister **Simon**.

    d. Kullanıcının bir başlık girin **başlık** TextBox.
    
    e. İçinde **e-posta adresi** metin Britta Simon hesap türü e-posta adresi ister **brittasimon\@contoso.com**.

    f. İçinde **SSO kullanıcı adı** Britta Simon hesap kullanıcı adı türü metin ister **brittasimon\@contoso.com**.

    g. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Envi MMIS kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Envi MMIS için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

