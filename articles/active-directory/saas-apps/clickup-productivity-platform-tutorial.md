---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle ClickUp üretkenlik platformu | Microsoft Docs'
description: Azure Active Directory ve ClickUp üretkenlik platformu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06853edd-e8da-4ad2-a4e6-5555d592cee5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: 3244140999dc61560549db077d4c402b3986956b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59282653"
---
# <a name="tutorial-azure-active-directory-integration-with-clickup-productivity-platform"></a>Öğretici: Azure Active Directory tümleştirmesiyle ClickUp üretkenlik platformu

Bu öğreticide, Azure Active Directory (Azure AD) ile ClickUp üretkenlik platformu tümleştirme konusunda bilgi edinin.
ClickUp üretkenlik platformu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Erişimi ClickUp üretkenlik platformu olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) ClickUp üretkenlik platformu için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ClickUp üretkenlik platformu ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik ClickUp üretkenlik platformu çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* ClickUp üretkenlik platformu destekleyen **SP** tarafından başlatılan

## <a name="adding-clickup-productivity-platform-from-the-gallery"></a>Galeriden ClickUp üretkenlik platformu ekleme

Azure AD'de ClickUp üretkenlik platformu tümleştirmesini yapılandırmak için ClickUp üretkenlik platformu Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ClickUp üretkenlik platformu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ClickUp üretkenlik platformu**seçin **ClickUp üretkenlik platformu** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde ClickUp üretkenlik platformu](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ClickUp üretkenlik adlı bir test kullanıcısı ve erişim yönetimi ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının ClickUp üretkenlik platformu ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ClickUp üretkenlik platformu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[ClickUp üretkenlik platformu çoklu oturum açmayı yapılandırma](#configure-clickup-productivity-platform-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[ClickUp üretkenlik platformu test kullanıcısı oluşturma](#create-clickup-productivity-platform-test-user)**  - kullanıcı Azure AD gösterimini bağlı ClickUp üretkenlik platformu Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ClickUp üretkenlik platformu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **ClickUp üretkenlik platformu** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ClickUp üretkenlik platformu etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL yazın: `https://app.clickup.com/login/sso`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://api.clickup.com/v1/team/<team_id>/microsoft`

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Bu değer, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek tanımlayıcısı ile güncelleştirin.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-clickup-productivity-platform-single-sign-on"></a>ClickUp üretkenlik platformu çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde ClickUp üretkenlik platformu kiracınıza yönetici olarak oturum.

2. Tıklayarak **kullanıcı profili** seçip **ayarları**.

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/configure1.png)

3. Seçin **Microsoft**altında tek oturum açma (SSO) sağlayıcısı.

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/configure2.png)

4. Üzerinde **yapılandırma Microsoft Çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/configure3.png)

    a. Tıklayın **kopyalama** varlık kimliği değerini kopyalayın ve yapıştırın **tanımlayıcı (varlık kimliği)** metin kutusunda **temel SAML yapılandırma** bölümünde Azure portalında.
    
    b. İçinde **Azure Federasyon meta veri URL'si** metin Azure portalından kopyalamış olmanız ve ardından uygulama Federasyon meta verileri URL'sini değeri yapıştırın **Kaydet**.

5. Kurulumu tamamlamak için tıklatın **Microsoft Kurulumu tamamlamak için kimlik doğrulaması ile** ve microsoft hesabıyla kimlik doğrulaması.

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/configure4.png)

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

Bu bölümde, ClickUp üretkenlik platformu için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **ClickUp üretkenlik platformu**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ClickUp üretkenlik platformu**.

    ![Uygulamalar listesinde ClickUp üretkenlik platformu bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-clickup-productivity-platform-test-user"></a>ClickUp üretkenlik platformu test kullanıcısı oluşturma

1. Farklı bir web tarayıcı penceresinde ClickUp üretkenlik platformu kiracınıza yönetici olarak oturum.

2. Tıklayarak **kullanıcı profili** seçip **kullanıcılar**.

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/user1.png)

3. Tıklayın ve metin kutusu, kullanıcının e-posta adresi girin **davet**.

    ![ClickUp üretkenlik yapılandırma](./media/clickup-productivity-platform-tutorial/user2.png)

    > [!NOTE]
    > Kullanıcı bildirimi alırsınız ve he hesabını etkinleştirmek için daveti kabul etmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ClickUp üretkenlik platformu kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama ClickUp üretkenlik platformu için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

