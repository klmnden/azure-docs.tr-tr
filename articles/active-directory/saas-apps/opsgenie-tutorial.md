---
title: 'Öğretici: OpsGenie ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: OpsGenie ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: f395ae51d51e56668482ad6c8d446bdd31050fea
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59280183"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Öğretici: OpsGenie ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile OpsGenie tümleştirme konusunda bilgi edinin.
OpsGenie Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* OpsGenie erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) için OpsGenie ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi OpsGenie ile yapılandırma için aşağıdakiler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* OpsGenie çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* OpsGenie destekler **SP** tarafından başlatılan

## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie galeri ekleme

Azure AD'de OpsGenie tümleştirmesini yapılandırmak için OpsGenie Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OpsGenie eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **OpsGenie**seçin **OpsGenie** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde OpsGenie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı OpsGenie ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının OpsGenie ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma OpsGenie ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[OpsGenie çoklu oturum açmayı yapılandırma](#configure-opsgenie-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[OpsGenie test kullanıcısı oluşturma](#create-opsgenie-test-user)**  - kullanıcı Azure AD gösterimini bağlı OpsGenie Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma OpsGenie ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **OpsGenie** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![OpsGenie etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://app.opsgenie.com/auth/login`

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

6. Üzerinde **OpsGenie kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-opsgenie-single-sign-on"></a>OpsGenie tek oturum açmayı yapılandırın

1. Başka bir tarayıcı örneğinde açın ve ardından günlük OpsGenie yönetici olarak açın.

2. Tıklayın **ayarları**ve ardından **çoklu oturum açma** sekmesi.
   
    ![OpsGenie çoklu oturum açma](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

3. SSO'yu etkinleştirmek üzere işaretleyin **etkin**.
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

4. İçinde **sağlayıcısı** bölümünde **Azure Active Directory** sekmesi.
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

5. Azure Active Directory sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. İçinde **SAML 2.0 uç noktası** metin kutusu, yapıştırma **oturum açma URL'si**Azure portaldan kopyaladığınız değeri.
    
    b. İçinde **meta veri URL'si:** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.
    
    c. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümün OpsGenie için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **OpsGenie**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **OpsGenie**.

    ![Uygulamalar listesinde OpsGenie bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-opsgenie-test-user"></a>OpsGenie test kullanıcısı oluşturma

Amacı, bu bölümün OpsGenie içinde Britta Simon adlı bir kullanıcı oluşturmaktır. 

1. Bir web tarayıcısı penceresinde OpsGenie Kiracı yönetici olarak oturum açın.

2. Tıklayarak kullanıcıların listesine gidin **kullanıcılar** sol panelde.
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. Tıklayın **kullanıcı ekleme**.

4. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
    a. İçinde **e-posta** metin BrittaSimon türü e-posta adresi, Azure Active Directory'de ele.
   
    b. İçinde **tam adı** metin kutusuna **Britta Simon**.
   
    c. **Kaydet**’e tıklayın. 

>[!NOTE]
>Britta kendi profilinizi ayarlama yönergeleri içeren bir e-posta alır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli OpsGenie kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama OpsGenie için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

