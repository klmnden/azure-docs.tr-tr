---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iQualify LMS | Microsoft Docs'
description: Azure Active Directory ve LMS iQualify arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 95c24f74e9af4443db994a6655a82108de18efdd
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280154"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Öğretici: İQualify LMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iQualify LMS tümleştirme konusunda bilgi edinin.
Azure AD ile iQualify LMS tümleştirme ile aşağıdaki avantajları sağlar:

* İQualify LMS erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak iQualify LMS (çoklu oturum açma) için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi LMS iQualify ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik iQualify LMS çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* LMS iQualify destekler **SP ve IDP** tarafından başlatılan
* LMS iQualify destekler **zamanında** kullanıcı sağlama

## <a name="adding-iqualify-lms-from-the-gallery"></a>Galeriden iQualify LMS ekleme

Azure AD'de iQualify LMS tümleştirmesini yapılandırmak için iQualify LMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iQualify LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iQualify LMS**seçin **iQualify LMS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![sonuç listesinde iQualify LMS](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma iQualify LMS tabanlı adlı bir test kullanıcı ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının iQualify ilgili kullanıcı arasında bir bağlantı ilişki LMS kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iQualify LMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[İQualify LMS çoklu oturum açmayı yapılandırma](#configure-iqualify-lms-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[İQualify LMS test kullanıcısı oluşturma](#create-iqualify-lms-test-user)**  - Britta Simon iQualify kullanıcı Azure AD gösterimini bağlı LMS içinde bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma LMS iQualify ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **iQualify LMS** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/`|
    | Test ortamı: `https://<yourorg>.iqualify.io`|

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Test ortamı: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/login` |
    | Test ortamı: `https://<yourorg>.iqualify.io/login` |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [iQualify LMS istemci Destek ekibine](https://www.iqualify.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. İQualify LMS uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

7. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği|
    | --- | --- |
    | e-posta | User.userPrincipalName |
    | first_name | User.givenName |
    | Soyadı | User.surname |
    | person_id | "özniteliği" |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    > [!Note]
    > **Person_id** özniteliği **isteğe bağlı**

8. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Üzerinde **LMS iQualify kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-iqualify-lms-single-sign-on"></a>İQualify LMS çoklu oturum açmayı yapılandırın

1. Yeni bir tarayıcı penceresi açın ve ardından iQualify ortamınıza bir yönetici olarak oturum açın.

1. Oturum açtıktan sonra sağ üst köşedeki avatarınız tıklayın ve ardından tıklayarak **hesap ayarları**

    ![Hesap ayarları](./media/iqualify-tutorial/setting1.png)

1. Hesap ayarları alanında Şerit menüsünde sol tıklayın ve tıklayarak **TÜMLEŞTİRMELERİ**

    ![TÜMLEŞTİRMELER](./media/iqualify-tutorial/setting2.png)

1. TÜMLEŞTİRMELER altında tıklayarak **SAML** simgesi.

    ![SAML simgesi](./media/iqualify-tutorial/setting3.png)

1. İçinde **SAML kimlik doğrulaması ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulama ayarları](./media/iqualify-tutorial/setting4.png)

    a. İçinde **SAML çoklu oturum açma hizmeti URL'si** kutusu, yapıştırma **oturum açma URL'si** değer Azure AD uygulama yapılandırması penceresinde kopyalanır.

    b. İçinde **SAML oturum kapatma URL'si** kutusu, yapıştırma **oturum kapatma URL'si** değer Azure AD uygulama yapılandırması penceresinde kopyalanır.

    c. İndirilen sertifika dosyasını Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **ortak sertifika** kutusu.

    d. İçinde **oturum açma düğmesi ETİKETİNİN** oturum açma sayfasında görüntülenecek düğme için bir ad girin.

    e. **KAYDET**'e tıklayın.

    f. Tıklayın **güncelleştirme**.

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

Bu bölümde, Azure çoklu oturum açma iQualify LMS erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **iQualify LMS**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iQualify LMS**.

    ![İQualify LMS uygulamalar listesinde bağlayın.](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-iqualify-lms-test-user"></a>İQualify LMS test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı LMS iQualify içinde oluşturulur. iQualify LMS just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı LMS iQualify içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

İQualify LMS erişim Paneli'nde kutucuğuna tıkladığınızda, oturum açma sayfası iQualify LMS uygulamanızın almanız gerekir. 

   ![oturum açma sayfası](./media/iqualify-tutorial/login.png) 

Tıklayın **oturum Azure AD'de oturum** düğmesini otomatik olarak oturum açma iQualify LMS uygulamanıza.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)