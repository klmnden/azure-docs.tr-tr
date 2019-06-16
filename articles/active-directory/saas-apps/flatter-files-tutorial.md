---
title: 'Öğretici: Azure Active Directory Tümleştirmesi düzleştiren dosyalarla | Microsoft Docs'
description: Azure Active Directory ve düzleştiren dosyaları arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 9e4ba987393628af07f8a8a507f635047eb18cc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102550"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Öğretici: Azure Active Directory tümleştirmesiyle düzleştiren dosyaları

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme düzleştiren dosyalarını öğrenin.
Düzleştiren dosyaları Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Düzleştiren dosyalara erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak düzleştiren dosyaları (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi düzleştiren dosyalarıyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Tek oturum açma etkin abonelik düzleştiren dosyaları

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Dosyaları destekler düzleştiren **IDP** tarafından başlatılan

## <a name="adding-flatter-files-from-the-gallery"></a>Galeriden düzleştiren dosya ekleme

Azure AD'de düzleştiren dosyalarının tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden düzleştiren dosyaları eklemeniz gerekir.

**Galeriden düzleştiren dosyaları eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **düzleştiren dosyaları**seçin **düzleştiren dosyaları** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde düzleştiren dosyaları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma düzleştiren adlı bir test kullanıcı tabanlı dosyaları test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı düzleştiren dosyalarında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile düzleştiren dosyaları test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Düzleştiren dosyaları çoklu oturum açmayı yapılandırma](#configure-flatter-files-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Düzleştiren dosyaları test kullanıcısı oluşturma](#create-flatter-files-test-user)**  - kullanıcı Azure AD gösterimini bağlı düzleştiren dosyalarındaki Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma düzleştiren dosyalarıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **düzleştiren dosyaları** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![Çoklu oturum açma bilgileri düzleştiren dosyaları etki alanı ve URL'ler](common/preintegrated.png)

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **düzleştiren dosyaları Ayarla** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-flatter-files-single-sign-on"></a>Düzleştiren dosyaları çoklu oturum açmayı yapılandırın

1. Düzleştiren dosyaları uygulamanıza yönetici olarak oturum.

2. Tıklayın **PANO**. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

3. Tıklayın **ayarları**ve ardından üzerinde aşağıdaki adımları gerçekleştirin **şirket** sekmesinde: 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Seçin **SAML 2.0 kimlik doğrulaması için kullanmak**.
    
    b. Tıklayın **SAML'yi yapılandırmak**.

4. Üzerinde **SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırın](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. İçinde **etki alanı** metin kayıtlı etki alanınızı girin.
   
   > [!NOTE]
   > Bir kaydedilmiş bir etki alanı henüz kişi yoksa düzleştiren dosyalarınızı aracılığıyla destek [ support@flatterfiles.com ](mailto:support@flatterfiles.com). 
    
    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **oturum açma URL'si** kopyaladığınız Azure portal'ı oluşturur.
   
    c.  Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    d. Tıklayın **güncelleştirme**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, düzleştiren dosyalara erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **düzleştiren dosyaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **düzleştiren dosyaları**.

    ![Uygulamalar listesinde düzleştiren dosyaları bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-flatter-files-test-user"></a>Düzleştiren dosyaları test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon düzleştiren dosyalarında adlı bir kullanıcı oluşturmaktır.

**Britta Simon düzleştiren dosyalarında adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **düzleştiren dosyaları** şirketinizin sitesi yöneticisi olarak.

2. Sol taraftaki gezinti bölmesinde **ayarları**ve ardından **kullanıcılar** sekmesi.
   
    ![Düzleştiren dosyaları kullanıcı oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Tıklayın **kullanıcı ekleme**. 

4. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Düzleştiren dosyaları kullanıcı oluşturma](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. İçinde **ad** metin kutusuna **Britta**.
   
    b. İçinde **Soyadı** metin kutusuna **Simon**. 
   
    c. İçinde **e-posta adresi** metin kutusuna, Azure portalında Britta'nın e-posta adresini yazın.
   
    d. **Gönder**'e tıklayın.   


### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli düzleştiren dosyaları kutucuğa tıkladığınızda, size otomatik olarak düzleştiren SSO'yu ayarlama dosyaları için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

