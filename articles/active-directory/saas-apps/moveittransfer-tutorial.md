---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Moveıt aktarımı - Azure AD tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Moveıt aktarımı - Azure AD tümleştirmesi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: d7020299bbd52f5e7ba22809847815cb04048cb6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259414"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Öğretici: Azure Active Directory tümleştirmesiyle Moveıt aktarımı - Azure AD tümleştirmesi

Bu öğreticide, Moveıt aktarımı - Azure Active Directory (Azure AD) ile Azure AD tümleştirmesi tümleştirmeyi öğrenin.
Tümleştirme Moveıt aktarımı - Azure AD ile Azure AD Tümleştirmesi ile aşağıdaki avantajları sağlar:

* Moveıt aktarımı - Azure AD tümleştirmesi erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Moveıt aktarımı - Azure AD hesaplarıyla (çoklu oturum açma) Azure AD tümleştirmesi için oturum açmış, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Moveıt aktarımı - Azure AD Tümleştirmesi ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Moveıt aktarımı - Azure AD tümleştirmesi çoklu oturum açmayı aboneliği etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Moveıt aktarımı - Azure AD tümleştirmesi destekler **SP** tarafından başlatılan

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Moveıt aktarımı - Azure AD tümleştirmesi galerisinden ekleme

Moveıt aktarımı - Azure AD, Azure AD tümleştirme tümleştirmesini yapılandırmak için Moveıt aktarımı - Azure AD tümleştirmesi galerisinden listenize yönetilen SaaS uygulamalarının eklemeniz gerekir.

**Galerisi, Azure AD tümleştirmesi Moveıt aktarımı - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Moveıt aktarımı - Azure AD tümleştirmesi**seçin **Moveıt aktarımı - Azure AD tümleştirmesi** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

     ![Moveıt aktarımı - sonuç listesinde Azure AD tümleştirmesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Moveıt aktarımı - adlı bir test kullanıcı tabanlı Azure AD Tümleştirmesi ile test etme **Britta Simon**.
İş, bir Azure AD kullanıcısı ile ilgili kullanıcı Moveıt transfer - arasında bir bağlantı ilişki için çoklu oturum açma için Azure AD tümleştirmesi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Moveıt aktarımı ile-test etmek için Azure AD tümleştirmesi, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Moveıt aktarımı - Azure AD tümleştirmesi çoklu oturum açmayı yapılandırma](#configure-moveit-transfer---azure-ad-integration-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Moveıt aktarımı - Azure AD tümleştirme test kullanıcısı oluşturma](#create-moveit-transfer---azure-ad-integration-test-user)**  - bir karşılığı Britta simon'un Moveıt aktarımı - kullanıcı Azure AD gösterimini bağlı olduğu Azure AD tümleştirmesi sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Moveıt aktarımı - Azure AD Tümleştirmesi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Moveıt aktarımı - Azure AD tümleştirmesi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değer otomatik olarak doldurulmuş alır **temel SAML yapılandırma** bölümü:

    ![Moveıt aktarımı - Azure AD tümleştirmesi etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://contoso.com`

    > [!NOTE]
    > **Oturum açma URL'si** değeri gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Moveıt aktarımı - Azure AD tümleştirmesi istemci desteği](https://community.ipswitch.com/s/support) değerini almak için takım. İndirebileceğiniz **hizmet sağlayıcısı meta veri dosyası** gelen **hizmet sağlayıcısı meta veri URL'si** anlatılan daha sonra **yapılandırma Moveıt Transfer - tek Azure AD tümleştirmesi Oturum açma** öğreticinin bölümünde. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Moveıt aktarımı - Azure AD tümleştirmesi ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-moveit-transfer---azure-ad-integration-single-sign-on"></a>Moveıt aktarımı - Azure AD tümleştirmesi çoklu oturum açmayı yapılandırın

1. Moveıt aktarımı Kiracı yönetici olarak oturum açın.

2. Sol gezinti bölmesinde **ayarları**.

    ![Ayarlar bölümünde bulunan uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

3. Tıklayın **tek oturum açma** altında olan bağlantı **kullanıcı kimlik doğrulama -> güvenlik ilkelerini**.

    ![Güvenlik ilkeleri üzerinde uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

4. Meta veri belgesini indirmek için meta veri URL'si bağlantıya tıklayın.

    ![Hizmet sağlayıcısı meta veri URL'si](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
   * Doğrulama **Entityıd** eşleşen **tanımlayıcı** içinde **temel SAML yapılandırma** bölümü.
   * Doğrulama **AssertionConsumerService** konumu URL ile eşleşen **yanıt URL'si** içinde **temel SAML yapılandırma** bölümü.
    
     ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

5. Tıklayın **kimlik sağlayıcısı Ekle** düğmesini yeni bir Federasyon kimlik sağlayıcısı ekleyin.

    ![Kimlik sağlayıcısı Ekle](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

6. Tıklayın **Gözat...**  Azure portalından indirilen meta veri dosyası seçmek için ardından **kimlik sağlayıcısı Ekle** indirilen dosyayı karşıya yüklemek için.

    ![SAML kimlik sağlayıcısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

7. Seçin "**Evet**" olarak **etkin** içinde **Federasyon kimlik sağlayıcı ayarları Düzenle...**  sayfasında ve tıklayın **Kaydet**.

    ![Federe kimlik sağlayıcı ayarları](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

8. İçinde **federe kimlik sağlayıcısı kullanıcı ayarlarını Düzenle** sayfasında, aşağıdaki eylemleri gerçekleştirin:
    
    ![Federe kimlik sağlayıcı ayarları Düzenle](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Seçin **SAML Nameıd** olarak **oturum açma adı**.
    
    b. Seçin **diğer** olarak **tam adı** ve **öznitelik adı** textbox değeri koyun: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Seçin **diğer** olarak **e-posta** ve **öznitelik adı** textbox değeri koyun: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Seçin **Evet** olarak **oturum açma hesabı otomatik olarak oluşturma**.
    
    e. Tıklayın **Kaydet** düğmesi.

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

Bu bölümde, Moveıt aktarımı - Azure AD tümleştirmesi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Moveıt aktarımı - Azure AD tümleştirmesi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Moveıt aktarımı - Azure AD tümleştirmesi**.

    ![Azure AD tümleştirmesi Moveıt aktarımı - uygulamalar listesindeki bağlantılardan](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-moveit-transfer---azure-ad-integration-test-user"></a>Moveıt aktarımı - Azure AD tümleştirme test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Moveıt aktarımı - Azure AD tümleştirmesi adlı bir kullanıcı oluşturmaktır. Moveıt aktarımı - Azure AD tümleştirmesi tam zamanında sağlama, etkin destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı Moveıt aktarımı - henüz mevcut değilse Azure AD tümleştirmesi erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Moveıt aktarımı - Azure AD tümleştirme istemci Destek ekibine](https://community.ipswitch.com/s/support).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Moveıt aktarımı - Azure AD tümleştirme kutucuğuna tıkladığınızda, erişim Paneli'nde, otomatik olarak Moveıt aktarımı - Azure AD tümleştirmesi SSO'yu ayarladığınız oturum. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

