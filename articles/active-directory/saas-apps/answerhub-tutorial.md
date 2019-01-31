---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AnswerHub | Microsoft Docs'
description: Azure Active Directory ve AnswerHub arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.openlocfilehash: 41a1eef4ff417890114addcac00e2df3e49dc529
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453272"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Öğretici: AnswerHub ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AnswerHub tümleştirme konusunda bilgi edinin.
Azure AD ile AnswerHub tümleştirme ile aşağıdaki avantajları sağlar:

* AnswerHub erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) AnswerHub için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AnswerHub yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik AnswerHub çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AnswerHub destekler **SP** tarafından başlatılan

## <a name="adding-answerhub-from-the-gallery"></a>Galeriden AnswerHub ekleme

Azure AD'de AnswerHub tümleştirmesini yapılandırmak için AnswerHub Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AnswerHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AnswerHub**seçin **AnswerHub** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde AnswerHub](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma AnswerHub adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının AnswerHub ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AnswerHub ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[AnswerHub çoklu oturum açmayı yapılandırma](#configure-answerhub-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[AnswerHub test kullanıcısı oluşturma](#create-answerhub-test-user)**  - kullanıcı Azure AD gösterimini bağlı AnswerHub Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile AnswerHub yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **AnswerHub** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AnswerHub etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company>.answerhub.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company>.answerhub.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [AnswerHub istemci Destek ekibine](mailto:success@answerhub.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **AnswerHub kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-answerhub-single-sign-on"></a>AnswerHub tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde AnswerHub şirket sitenize yönetici olarak oturum.

    > [!NOTE]
    > AnswerHub yapılandırırken Yardım gerekirse başvurun [AnswerHub'ın Destek ekibine](mailto:success@answerhub.com.).

2. Git **Yönetim**.

3. Tıklayın **kullanıcı ve grup** sekmesi.

4. Sol taraftaki gezinti bölmesinde, **sosyal ayarları** bölümünde **SAML Kurulumu**.

5. Tıklayın **IDP yapılandırma** sekmesi.

6. Üzerinde **IDP yapılandırma** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![SAML Kurulumu](./media/answerhub-tutorial/ic785172.png "SAML Kurulumu")  
  
    a. İçinde **IDP'nin oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
  
    b. İçinde **IDP oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    c. İçinde **IDP ad tanımlayıcı biçimi** metin kullanıcı tanımlayıcısı olarak seçili Azure portalında aynı değeri girin **kullanıcı öznitelikleri** bölümü.
  
    d. Tıklayın **anahtarlara ve sertifikalara**.

7. Üzerinde **anahtarlara ve sertifikalara** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Anahtarlara ve sertifikalara](./media/answerhub-tutorial/ic785173.png "anahtarlar ve sertifikalar")  

    a. Not Defteri'nde, Azure portalından indirilen, base-64 kodlanmış sertifika açın içeriğini sizin panoya kopyalayın ve yapıştırın kendisine **IDP ortak anahtarı (x 509 biçimi)** metin.
  
    b. **Kaydet**’e tıklayın.

8. Üzerinde **IDP yapılandırma** sekmesinde **Kaydet**.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için AnswerHub erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **AnswerHub**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **AnswerHub**.

    ![Uygulamalar listesinde AnswerHub bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-answerhub-test-user"></a>AnswerHub test kullanıcısı oluşturma

AnswerHub için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların AnswerHub sağlanması gerekir. AnswerHub söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **AnswerHub** şirketinizin sitesi yöneticisi olarak.

2. Git **Yönetim**.

3. Tıklayın **kullanıcıları ve grupları** sekmesi.

4. Sol taraftaki gezinti bölmesinde, **Kullanıcıları Yönet** bölümünde **oluşturun veya içeri aktarma kullanıcı** ve ardından **kullanıcıları ve grupları**.

   ![Kullanıcıları ve grupları](./media/answerhub-tutorial/ic785175.png "kullanıcıları ve grupları")

5. Tür **e-posta adresi**, **kullanıcıadı** ve **parola** ilgili metin kutularına sağlayın ve ardından istediğinizgeçerlibirAzureActiveDirectoryhesabı **Kaydet**.

> [!NOTE]
> Herhangi diğer AnswerHub kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak AnswerHub tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AnswerHub kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama AnswerHub için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

