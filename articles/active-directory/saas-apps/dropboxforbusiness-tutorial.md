---
title: 'Öğretici: İş için Dropbox ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve iş için Dropbox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d55ae4077b3ec14cb8dc2226714b094574ed9522
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898973"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Öğretici: İş için Dropbox ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iş için Dropbox tümleştirme konusunda bilgi edinin.
İş için Dropbox'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* İş Dropbox erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak dropbox'a iş (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iş için Dropbox ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik iş çoklu oturum açma için Dropbox etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* İş desteklediği için Dropbox **SP** tarafından başlatılan

* İş desteklediği için Dropbox **zamanında** kullanıcı sağlama

## <a name="adding-dropbox-for-business-from-the-gallery"></a>İş için Dropbox galeri ekleme

Azure AD'de iş için Dropbox tümleştirmesini yapılandırmak için iş için Dropbox Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**İş için Dropbox Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iş için Dropbox**seçin **iş için Dropbox** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde iş için Dropbox](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve iş adlı bir test kullanıcı tabanlı için Azure AD çoklu oturum açma Dropbox ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve iş için dropbox'ta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile iş için Dropbox sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Dropbox iş çoklu oturum açma için yapılandırma](#configure-dropbox-for-business-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Dropbox için iş test kullanıcısı oluşturma](#create-dropbox-for-business-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı iş için dropbox'ta bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

İş için Dropbox ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **iş için Dropbox** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Dropbox için iş etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.dropbox.com/sso/<id>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir değer yazın: `Dropbox`

    > [!NOTE]
    > Önceki oturum açma URL değeri, gerçek değeri değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek oturum açma URL'si ile değerini güncelleştirir.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **iş için Dropbox ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-dropbox-for-business-single-sign-on"></a>İş çoklu oturum açma için Dropbox'ı yapılandırma

1. Çoklu oturum açmayı yapılandırma **iş için Dropbox** yan, iş Kiracı ve oturum için Dropbox'üzerinde iş Kiracı için Dropbox için gidin.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769509.png "çoklu oturum açmayı yapılandırın")

2. Tıklayarak **kullanıcı simgesi** seçip **ayarları** sekmesi.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure1.png "çoklu oturum açmayı yapılandırın")

3. Sol taraftaki gezinti bölmesinde **Yönetici Konsolu**.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure2.png "çoklu oturum açmayı yapılandırın")

4. Üzerinde **Yönetici Konsolu**, tıklayın **ayarları** sol gezinti bölmesinde.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure3.png "çoklu oturum açmayı yapılandırın")

5. Seçin **çoklu oturum açma** altındaki **kimlik doğrulaması** bölümü.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure4.png "çoklu oturum açmayı yapılandırın")

6. İçinde **çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure5.png "çoklu oturum açmayı yapılandırın")

    a. Seçin **gerekli** için açılan listeden bir seçenek olarak **çoklu oturum açma**.

    b. Tıklayarak **oturum açma URL'si ekleme** ve **oturum açma kimlik sağlayıcısı URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portal ve ardından kopyaladığınızdeğeri **Bitti**.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure6.png "çoklu oturum açmayı yapılandırın")

    c. Tıklayın **sertifikayı karşıya yükle**ve ardından göz atın, **Base64 olarak kodlanmış sertifika dosyası** Azure portalından indirilen.

    d. Tıklayarak **bağlantıyı Kopyala** kopyalanan değerine yapıştırın **oturum açma URL'si** textbox'ın **iş etki alanı ve URL'ler için Dropbox** bölümü Azure portalı.

    e. **Kaydet**’e tıklayın.

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

Bu bölümde, iş için dropbox'a erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **iş için Dropbox**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **iş için Dropbox**.

    ![Uygulamalar listesini iş bağlantısı için Dropbox](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-dropbox-for-business-test-user"></a>Dropbox için iş test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı, iş için dropbox'ta oluşturulur. İş için Dropbox just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. İş için dropbox'ta bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde iş kutucuk için Dropbox'ı tıklattığınızda, otomatik olarak dropbox'a SSO'yu ayarlama iş için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

