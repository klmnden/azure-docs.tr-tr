---
title: 'Öğretici: GitHub ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve GitHub arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25540d1f26fa6021ef05108f9743e77a6184f3b3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59426333"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Öğretici: GitHub ile Azure Active Directory Tümleştirme

Bu öğreticide, GitHub, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Github'da Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* GitHub erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak GitHub'ın (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

GitHub Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Oluşturulan bir GitHub kuruluşuna [GitHub Enterprise Cloud](https://help.github.com/articles/github-s-products/#github-enterprise), gerektiren [GitHub Enterprise faturalandırma planı](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* GitHub'ı destekleyen **SP** tarafından başlatılan

* GitHub'ı destekleyen [ **otomatik** kullanıcı sağlama](github-provisioning-tutorial.md)

## <a name="adding-github-from-the-gallery"></a>GitHub galeri ekleme

Azure AD'de GitHub tümleştirmesini yapılandırmak için GitHub galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden GitHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GitHub**seçin **GitHub.com** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde GitHub](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma GitHub adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili GitHub kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma GitHub ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[GitHub çoklu oturum açmayı yapılandırma](#configure-github-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[GitHub test kullanıcısı oluşturma](#create-github-test-user)**  - kullanıcı Azure AD gösterimini bağlı github'da Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile GitHub yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **GitHub** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![GitHub etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://github.com/orgs/<entity-id>/sso`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirmeniz gerekiyor. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. Bu değerleri almak için GitHub yönetici bölümüne gidin.

5. GitHub uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. GitHub uygulama bekliyor **NameIdentifier** ile eşlenecek **user.mail**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle** simgesi ve değişiklik öznitelik eşleme.

    ![image](common/edit-attribute.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **GitHub'ı ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-github-single-sign-on"></a>GitHub tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde GitHub kuruluş sitenize yönetici olarak oturum.

2. Gidin **ayarları** tıklatıp **güvenlik**

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_03.png)

3. Kontrol **etkinleştirme SAML kimlik doğrulaması** kutusu, çoklu oturum açmayı yapılandırma alanları açıklayacak. Ardından, çoklu oturum açma URL'si Azure AD yapılandırmasını güncelleştirmek için tek oturum açma URL değeri kullanın.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_13.png)

4. Aşağıdaki alanları yapılandırın:

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **veren** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikanın yapıştırın içeriğinize **ortak sertifika** metin.

    d. Tıklayarak **Düzenle** düzenleme simgesi **imza yöntemi** ve **Özet yöntemi** gelen **RSA SHA1** ve **SHA1**için **RSA-SHA256** ve **SHA256** aşağıda gösterildiği gibi.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

5. Tıklayarak **Test SAML yapılandırma** onaylamak için hiçbir doğrulama hataları veya SSO sırasında hata.

    ![Ayarlar](./media/github-tutorial/tutorial_github_config_github_06.png)

6. **Kaydet**’e tıklayın

> [!NOTE]
> Çoklu oturum açmayı github'da github'da belirli bir kuruluş için doğrular ve Github'daki kimliğini değiştirmez. Bu nedenle, kullanıcının github.com oturumunu dolmuşsa, GitHub'ın kimliği/parola ile çoklu oturum açma işlemi sırasında kimlik doğrulaması istenebilir.

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

Bu bölümde, GitHub için erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **GitHub**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **GitHub**.

    ![Uygulamalar listesinde GitHub bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-github-test-user"></a>GitHub test kullanıcısı oluşturma

Bu bölümün amacı, Github'da Britta Simon adlı bir kullanıcı oluşturmaktır. GitHub otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](github-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. GitHub şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **kişiler**.

    ![Kişiler](./media/github-tutorial/tutorial_github_config_github_08.png "kişiler")

3. Tıklayın **davet üye**.

    ![Kullanıcıları davet](./media/github-tutorial/tutorial_github_config_github_09.png "kullanıcılar davet edin")

4. Üzerinde **davet üye** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    a. İçinde **e-posta** metin Britta Simon hesabı e-posta adresini yazın.

    ![Kişileri davet edin](./media/github-tutorial/tutorial_github_config_github_10.png "kişileri davet edin")

    b. Tıklayın **Davet Gönder**.

    ![Kişileri davet edin](./media/github-tutorial/tutorial_github_config_github_11.png "kişileri davet edin")

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli GitHub kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama github'a oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
