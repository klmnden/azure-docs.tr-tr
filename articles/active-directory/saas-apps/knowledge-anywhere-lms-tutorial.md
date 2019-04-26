---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile her yerde bilgi LMS | Microsoft Docs'
description: Azure Active Directory ve her yerde bilgi LMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5cfa07b1-a792-4f0a-8c6f-1a13142193d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/14/2019
ms.author: jeedes
ms.openlocfilehash: f39952c74006964155fd23920c85506cac13a878
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60261915"
---
# <a name="tutorial-azure-active-directory-integration-with-knowledge-anywhere-lms"></a>Öğretici: Herhangi bir Bilgi Bankası LMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile her yerde bilgi LMS tümleştirme konusunda bilgi edinin.
Azure AD ile herhangi bir Bilgi Bankası LMS tümleştirme ile aşağıdaki avantajları sağlar:

* Herhangi bir Bilgi Bankası LMS erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak bilgi her yerde LMS için (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bilgi Bankası LMS ile herhangi bir Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Herhangi bir Bilgi Bankası LMS tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Herhangi bir Bilgi Bankası LMS destekler **SP ve IDP** tarafından başlatılan
* Herhangi bir Bilgi Bankası LMS destekler **zamanında** kullanıcı sağlama

## <a name="adding-knowledge-anywhere-lms-from-the-gallery"></a>Galeriden herhangi bir Bilgi Bankası LMS ekleme

Azure AD'de herhangi bir Bilgi Bankası LMS tümleştirmesini yapılandırmak için her yerde bilgi LMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden herhangi bir Bilgi Bankası LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **herhangi bir Bilgi Bankası LMS**seçin **herhangi bir Bilgi Bankası LMS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Bilgi Bankası LMS sonuç listesinde her yerde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma bilgi her yerde adlı bir test kullanıcı tabanlı LMS test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve her yerde bilgi LMS ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile her yerde bilgi LMS sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bilgi her yerde LMS çoklu oturum açmayı yapılandırma](#configure-knowledge-anywhere-lms-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Herhangi bir Bilgi Bankası LMS test kullanıcısı oluşturma](#create-knowledge-anywhere-lms-test-user)**  - bilgi herhangi bir kullanıcı Azure AD gösterimini bağlı LMS Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma bilgisi LMS ile her yerde yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **herhangi bir Bilgi Bankası LMS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Bilgi her yerde LMS etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CLIENTNAME>.knowledgeanywhere.com/`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<CLIENTNAME>.knowledgeanywhere.com/SSO/SAML/Response.aspx?<IDPNAME>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve öğreticinin sonraki bölümlerinde açıklanan yanıt URL'si ile güncelleştirin.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Bilgi her yerde LMS etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<CLIENTNAME>.knowledgeanywhere.com/`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [bilgi her yerde LMS istemci Destek ekibine](https://knowany.zendesk.com/hc/en-us/articles/360000469034-SAML-2-0-Single-Sign-On-SSO-Set-Up-Guide) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **bilgi LMS herhangi bir yerde ayarlanmış** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-knowledge-anywhere-lms-single-sign-on"></a>Bilgi Bankası herhangi bir LMS çoklu oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde herhangi bir Bilgi Bankası LMS Yönetici portalını açın.

2. SELECT deyiminde **Site** sekmesi.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure1.png)

3. SELECT deyiminde **SAML ayarlarını** sekmesi.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure2.png)

4. Tıklayarak **yeni Ekle**.

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure3.png)

5. Üzerinde **ekleme/güncelleştirme SAML ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Herhangi bir Bilgi Bankası LMS yapılandırma](./media/knowledge-anywhere-lms-tutorial/configure4.png)

    a. Kuruluşunuz göre IDP adını girin. İçin örnek:- `Azure`.

    b. İçinde **IDP varlık kimliği** metin kutusu, yapıştırma **Azure Ad tanımlayıcısı** Azure Portalı'ndan kopyaladığınız değeri.

    c. İçinde **IDP URL** metin kutusu, yapıştırma **oturum açma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    d. İndirilen sertifika dosyasını Not Defteri'ne Azure portalından açın, sertifika içeriğini kopyalayın ve yapıştırın **sertifika** metin.

    e. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    f. Seçin **ana Site** için açılır menüden **etki alanı**.

    g. Kopyalama **SP varlık kimliği** yapıştırın ve değer **tanımlayıcı** metin kutusu **temel SAML yapılandırma** bölümünde Azure portalında.

    h. Kopyalama **SP Response(ACS) URL** yapıştırın ve değer **yanıt URL'si** metin kutusu **temel SAML yapılandırma** bölümünde Azure portalında.

    i. **Kaydet**’e tıklayın.

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

Bu bölümde, her yerde bilgi LMS erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **herhangi bir Bilgi Bankası LMS**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **herhangi bir Bilgi Bankası LMS**.

    ![Uygulamalar listesini her yerde bilgi LMS bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-knowledge-anywhere-lms-test-user"></a>Herhangi bir Bilgi Bankası LMS test kullanıcısı oluşturma

Bu bölümde, Bilgi Bankası LMS içinde her yerden Britta Simon adlı bir kullanıcı oluşturuldu. Herhangi bir Bilgi Bankası LMS just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bilgi Bankası LMS içinde her yerden bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde herhangi bir Bilgi Bankası LMS kutucuğa tıkladığınızda, otomatik olarak bilgi her yerde SSO'yu ayarlama LMS için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)