---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Samanage | Microsoft Docs'
description: Azure Active Directory ve Samanage arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b00e27a0620212fbc09f8b7d91d8f5b9d0971c8f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105726"
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Öğretici: Samanage ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Samanage tümleştirme konusunda bilgi edinin.
Azure AD ile Samanage tümleştirme ile aşağıdaki avantajları sağlar:

* Samanage erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Samanage için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Samanage yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Samanage çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Samanage destekler **SP** tarafından başlatılan

## <a name="adding-samanage-from-the-gallery"></a>Galeriden Samanage ekleme

Azure AD'de Samanage tümleştirmesini yapılandırmak için Samanage Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Samanage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Samanage**seçin **Samanage** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Samanage](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Samanage adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Samanage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Samanage ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Samanage çoklu oturum açmayı yapılandırma](#configure-samanage-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Samanage test kullanıcısı oluşturma](#create-samanage-test-user)**  - kullanıcı Azure AD gösterimini bağlı Samanage Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Samanage yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Samanage** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Samanage etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Daha fazla ayrıntı başvurun [Samanage istemci Destek ekibine](https://www.samanage.com/support). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Samanage kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-samanage-single-sign-on"></a>Samanage tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Samanage şirket sitenize yönetici olarak oturum.

2. Tıklayın **Pano** seçip **Kurulum** sol gezinti bölmesinde.
   
    ![Pano](./media/samanage-tutorial/tutorial_samanage_001.png "Panosu")

3. Tıklayın **çoklu oturum açma**.
   
    ![Çoklu oturum açma](./media/samanage-tutorial/tutorial_samanage_002.png "çoklu oturum açma")

4. Gidin **SAML kullanarak bir oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kullanarak bir oturum açma](./media/samanage-tutorial/tutorial_samanage_003.png "SAML ile oturum açın")
 
    a. Tıklayın **SAML ile çoklu oturum açmayı etkinleştirme**.  
 
    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **Azure Ad tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.    
 
    c. Onayla **oturum açma URL'si** eşleşen **işareti bulunan URL'si** , **temel SAML yapılandırma** bölümü Azure Portalı'nda.
 
    d. İçinde **oturum kapatma URL'si** metin değeri girin **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
 
    e. İçinde **SAML veren** metin kutusu, türü kimlik sağlayıcınız uygulama kimliği URI'si ayarlayın.
 
    f. Not Defteri'nde Azure portalından indirilen, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **, kimlik sağlayıcısı x.509 sertifikası aşağıya yapıştırın** metin.
 
    g. Tıklayın **Samanage içinde yoksa, kullanıcılar oluşturma**.
 
    h. Tıklayın **güncelleştirme**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Samanage erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Samanage**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Samanage**.

    ![Uygulamalar listesinde Samanage bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-samanage-test-user"></a>Samanage test kullanıcısı oluşturma

Samanage için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Samanage sağlanması gerekir.  
Samanage söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Samanage şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **Pano** seçip **Kurulum** sol gezinti bölmesinde.
   
    ![Kurulum](./media/samanage-tutorial/tutorial_samanage_001.png "Kurulumu")

3. Tıklayın **kullanıcılar** sekmesi
   
    ![Kullanıcılar](./media/samanage-tutorial/tutorial_samanage_006.png "kullanıcılar")

4. Tıklayın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/samanage-tutorial/tutorial_samanage_007.png "yeni kullanıcı")

5. Tür **adı** ve **e-posta adresi** , önce sağlamak istediğiniz bir Azure Active Directory hesabının **kullanıcı oluşturma**.
   
    ![Kullanıcı oluşturma](./media/samanage-tutorial/tutorial_samanage_008.png "kullanıcı oluşturma")
   
   >[!NOTE]
   >Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin. Herhangi diğer Samanage kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri tarafından Samanage sağlamak için Azure Active Directory kullanıcı hesaplarını sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Samanage kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Samanage için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

