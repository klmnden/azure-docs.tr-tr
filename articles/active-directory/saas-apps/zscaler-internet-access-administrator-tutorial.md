---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler Internet erişimi Yöneticisi | Microsoft Docs'
description: Azure Active Directory ve Zscaler Internet erişimi Yöneticisi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ea555097-bf62-45dd-9b45-b75c50324a69
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b46b8644b9ba4f0dc6b0b97215a4a96b60d69c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086028"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-internet-access-administrator"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler Internet erişimi Yöneticisi

Bu öğreticide, Zscaler Internet erişimi Yöneticisi Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Zscaler Internet erişimi yönetici Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Zscaler için Internet erişimi yönetici erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Zscaler Internet erişimi yönetici oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zscaler Internet erişimi Yöneticisi ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Zscaler Internet erişimi Yöneticisi abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Zscaler Internet erişimi Yöneticisi'ni destekleyen **IDP** tarafından başlatılan

## <a name="adding-zscaler-internet-access-administrator-from-the-gallery"></a>Galeriden Zscaler Internet erişimi Yöneticisi ekleme

Azure AD'de Zscaler Internet erişimi Yöneticisi'nin tümleştirmesini yapılandırmak için Zscaler Internet erişimi Yöneticisi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler Internet erişimi yönetici eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Zscaler Internet erişimi Yöneticisi**seçin **Zscaler Internet erişimi Yöneticisi** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

     ![Sonuç listesinde Zscaler Internet erişimi Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zscaler Internet erişimi adlı bir test kullanıcı tabanlı Yöneticisi ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler Internet erişimi Yöneticisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler Internet erişimi yönetici ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Zscaler Internet erişimi yönetici çoklu oturum açmayı yapılandırma](#configure-zscaler-internet-access-administrator-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Zscaler Internet erişimi yönetici test kullanıcısı oluşturma](#create-zscaler-internet-access-administrator-test-user)**  - Zscaler Internet erişimi kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Zscaler Internet erişimi Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Zscaler Internet erişimi Yöneticisi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![Zscaler Internet erişimi yöneticisi etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna, ihtiyacınıza göre bir URL yazın:

    | |
    |--|
    | `https://admin.zscaler.net` |
    | `https://admin.zscalerone.net` |
    | `https://admin.zscalertwo.net` |
    | `https://admin.zscalerthree.net` |
    | `https://admin.zscloud.net` |
    | `https://admin.zscalerbeta.net` |

    b. İçinde **yanıt URL'si** metin kutusuna, ihtiyacınıza göre bir URL yazın:

    | |
    |--|
    | `https://admin.zscaler.net/adminsso.do` |
    | `https://admin.zscalerone.net/adminsso.do` |
    | `https://admin.zscalertwo.net/adminsso.do` |
    | `https://admin.zscalerthree.net/adminsso.do` |
    | `https://admin.zscloud.net/adminsso.do` |
    | `https://admin.zscalerbeta.net/adminsso.do` |

5. Zscaler Internet erişimi Yöneticisi uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Üzerinde **SAML sayfası ile çoklu oturum açmayı ayarlayın**, tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![Öznitelik bağlantısı](./media/zscaler-internet-access-administrator-tutorial/tutorial_zscaler-internet_attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad  | Kaynak özniteliği  |
    | ---------| ------------ |
    | Rol     | User.assignedroles |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./common/new-save-attribute.png)
    
    ![image](./common/new-attribute-details.png)

    b. Gelen **kaynak özniteliği** selelct öznitelik değeri listesi.

    c. **Tamam**’a tıklayın.

    d. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Lütfen tıklayın [burada](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de rol yapılandırma bilmek

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **Zscaler Internet erişimi Yöneticisi yedekleme kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-zscaler-internet-access-administrator-single-sign-on"></a>Zscaler Internet erişimi yönetici çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Zscaler Internet erişimi yönetim kullanıcı Arabirimi için oturum açın.

2. Git **Yönetim > Yönetici Yönetimi** Kaydet'e tıklayın ve aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-internet-access-administrator-tutorial/AdminSSO.png "Yönetim")

    a. Denetleme **SAML kimlik doğrulamasını etkinleştirme**.

    b. Tıklayın **karşıya**, içinde Azure portalından indirilen Azure SAML imzalama sertifikasını karşıya yüklemek için **ortak SSL sertifikası**.

    c. Ek güvenlik için isteğe bağlı olarak, ekleme **veren** SAML yanıtını veren doğrulamak için ayrıntıları.

3. Yönetici kullanıcı Arabirimi, aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-internet-access-administrator-tutorial/ic800207.png)

    a. Üzerine **etkinleştirme** ekranın sol menü.

    b. Tıklayın **etkinleştirme**.

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

Bu bölümde, Zscaler Internet erişimi Yöneticisi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Zscaler Internet erişimi Yöneticisi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Zscaler Internet erişimi Yöneticisi**.

    ![Uygulamalar listesinde Zscaler Internet erişimi yönetici bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-zscaler-internet-access-administrator-test-user"></a>Zscaler Internet erişimi yönetici test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Zscaler Internet erişimi Yöneticisi'nde adlı bir kullanıcı oluşturmaktır. Zscaler Internet erişimi, Just-ın-yönetici SSO için sağlama Time desteklemez. El ile bir yönetici hesabı oluşturmanız gerekir.
Bir yönetici hesabı oluşturma adımları için Zscaler belgelerine bakın:

https://help.zscaler.com/zia/adding-admins

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler Internet erişimi Yöneticisi kutucuğa tıkladığınızda, size otomatik olarak Zscaler Internet erişimi yönetici Arabirimine SSO'yu ayarlama için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
