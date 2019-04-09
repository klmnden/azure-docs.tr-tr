---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile PureCloud Genesys tarafından | Microsoft Docs'
description: Azure Active Directory ve Genesys tarafından PureCloud arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 764ad8f1ca19238e1986d1d187d19c405963a832
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267404"
---
# <a name="tutorial-azure-active-directory-integration-with-purecloud-by-genesys"></a>Öğretici: Azure Active Directory tarafından Genesys PureCloud ile tümleştirmesi

Bu öğreticide, PureCloud Genesys tarafından Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
PureCloud Genesys tarafından Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* PureCloud Genesys tarafından erişebilir, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak için PureCloud tarafından Genesys (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi PureCloud Genesys tarafından ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik PureCloud tarafından Genesys çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Genesys tarafından PureCloud destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Galeriden PureCloud Genesys tarafından ekleme

Azure AD'de Genesys tarafından PureCloud tümleştirmesini yapılandırmak için Genesys tarafından PureCloud Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden PureCloud Genesys tarafından eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Genesys tarafından PureCloud**seçin **Genesys tarafından PureCloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Genesys tarafından PureCloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile PureCloud Genesys adlı bir test kullanıcı tabanlı olarak test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı tarafından Genesys PureCloud ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma PureCloud Genesys tarafından ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[PureCloud tarafından Genesys çoklu oturum açmayı yapılandırma](#configure-purecloud-by-genesys-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[PureCloud Genesys test kullanıcısı tarafından oluşturma](#create-purecloud-by-genesys-test-user)**  - kullanıcı Azure AD gösterimini bağlı Genesys tarafından PureCloud Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma PureCloud Genesys tarafından ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Genesys tarafından PureCloud** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![PureCloud bilgileriyle Genesys etki alanı ve URL'ler tek oturum açma](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna, bölgenize göre bir URL yazın:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. İçinde **yanıt URL'si** metin kutusuna, bölgenize göre bir URL yazın:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml` |

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![PureCloud bilgileriyle Genesys etki alanı ve URL'ler tek oturum açma](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna, bölgenize göre bir URL yazın:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

6. PureCloud Genesys uygulama tarafından özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

7. Yukarıdaki için ayrıca PureCloud Genesys uygulama tarafından SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği|
    | ---------------| --------------- |
    | Email | User.userprinicipalname |
    | OrganizationName | `Your organization name` |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **PureCloud Genesys tarafından ayarlanmış** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-purecloud-by-genesys-single-sign-on"></a>PureCloud Genesys çoklu oturum açma ile yapılandırma

1. Bir başka web tarayıcı penceresinde PureCloud tarafından Genesys yönetici olarak oturum açın.

2. Tıklayarak **yönetici** üst gidin **çoklu oturum açma** altında **tümleştirmeler**.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure01.png)

3. Geçiş **ADFS/Azure AD(Premium)** sekmesini tıklatın ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Tıklayın **Gözat** base-64 karşıya yüklemek için Azure portalından içine yüklediğiniz sertifika kodlanmış **ADFS sertifika**.

    b. İçinde **ADFS veren URI** metin değerini yapıştırın **Azure AD tanımlayıcısı** hangi Azure portaldan kopyaladığınız.

    c. İçinde **hedef URI** metin değerini yapıştırın **oturum açma URL'si** hangi Azure portaldan kopyaladığınız.

    d. İçin **bağlı olan taraf tanımlayıcısı** değeri, Azure portalına gitmek için ihtiyacınız **Genesys tarafından PureCloud** uygulama tümleştirme sayfası, tıklayarak **özellikleri** sekmesi ve kopyalama **uygulama kimliği** değeri. Yapıştırın **bağlı olan taraf tanımlayıcısı** metin. 

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. **Kaydet**’e tıklayın   

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

Bu bölümde, Azure çoklu oturum açma için PureCloud Genesys tarafından erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Genesys tarafından PureCloud**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Genesys tarafından PureCloud**.

    ![Uygulamalar listesinde Genesys bağlantısıyla PureCloud](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-purecloud-by-genesys-test-user"></a>PureCloud tarafından Genesys test kullanıcısı oluşturma

PureCloud Genesys tarafından oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların PureCloud Genesys tarafından sağlanması gerekir. Genesys tarafından PureCloud içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Genesys tarafından PureCloud için oturum açın.

2. Tıklayarak **yönetici** üst gidin **kişiler** altında **kişileri ve izinleri**.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure03.png)

3. Kişiler sayfasında tıklayarak **Kişi Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure04.png)

4. Üzerinde **kuruluş Ekle kişilere** açılır penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. İçinde **tam adı** metin kutusunda, gibi kullanıcı adını girin **Brittasimon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **brittasimon\@contoso.com**.
    
    c. **Oluştur**’a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

PureCloud tarafından erişim panelinde Genesys kutucuğa tıkladığınızda, size otomatik olarak PureCloud için SSO'yu ayarlama Genesys tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

