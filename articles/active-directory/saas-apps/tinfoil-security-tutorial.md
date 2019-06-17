---
title: 'Öğretici: TINFOIL SECURITY ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: TINFOIL SECURITY ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 627a324c580661220712a0598a996844fac0c169
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088595"
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Öğretici: TINFOIL SECURITY ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TINFOIL SECURITY tümleştirme konusunda bilgi edinin.
TINFOIL SECURITY Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* TINFOIL SECURITY erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için TINFOIL SECURITY oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

TINFOIL SECURITY ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* TINFOIL SECURITY çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* TINFOIL SECURITY destekler **IDP** tarafından başlatılan

## <a name="adding-tinfoil-security-from-the-gallery"></a>TINFOIL SECURITY galeri ekleme

Azure AD'de TINFOIL SECURITY tümleştirmesini yapılandırmak için TINFOIL SECURITY Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**TINFOIL SECURITY Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TINFOIL SECURITY**seçin **TINFOIL SECURITY** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![TINFOIL SECURITY sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TINFOIL SECURITY adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı TINFOIL Security arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TINFOIL SECURITY ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[TINFOIL güvenlik çoklu oturum açmayı yapılandırma](#configure-tinfoil-security-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[TINFOIL SECURITY test kullanıcısı oluşturma](#create-tinfoil-security-test-user)**  - kullanıcı Azure AD gösterimini bağlı TINFOIL Security Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

TINFOIL SECURITY ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **TINFOIL SECURITY** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleşik olarak herhangi bir adımı gerçekleştirmek.

    ![TINFOIL güvenlik etki alanı ve URL'ler tek oturum açma bilgileri](common/preintegrated.png)

5. TINFOIL SECURITY uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

        ![image](common/edit-attribute.png)

6. Ayrıca yukarıdaki için TINFOIL SECURITY uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Kaynak özniteliği |
    | ------------------- | -------------|
    | Hesap Kimliği | UXXXXXXXXXXXXX |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. İçinde **kaynak özniteliği** metin kutusuna, daha sonra öğreticide alacak Yapıştır hesap kimliği değeri.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. İçinde **SAML imzalama sertifikası** bölümünde **Düzenle** açmak için düğmeyi **SAML imzalama sertifikası** iletişim.

    ![SAML imzalama sertifikası Düzenle](common/edit-certificate.png)

8. İçinde **SAML imzalama sertifikası** bölümünde, kopya **parmak izi** ve bilgisayarınıza kaydedin.

    ![Parmak izi değerini kopyalayın](common/copy-thumbprint.png)

9. Üzerinde **TINFOIL güvenliği'kurmak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-tinfoil-security-single-sign-on"></a>TINFOIL güvenlik çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde TINFOIL SECURITY şirket sitenize yönetici olarak oturum açın.

2. Üst araç çubuğunda tıklatın **hesabım**.
   
    ![Pano](./media/tinfoil-security-tutorial/ic798971.png "Panosu")

3. Tıklayın **güvenlik**.
   
    ![Güvenlik](./media/tinfoil-security-tutorial/ic798972.png "güvenlik")

4. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/tinfoil-security-tutorial/ic798973.png "çoklu oturum açma")
   
    a. Seçin **etkinleştirme SAML**.
   
    b. Tıklayın **el ile yapılandırma**.
   
    c. İçinde **SAML gönderme URL'sini** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız
   
    d. İçinde **SAML sertifikası parmak izi** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü.
  
    e. Kopyalama **bilgisayarınızı hesap kimliği** değeri ve değer yapıştırın **öznitelik değeri** metin kutusunun altında **öznitelik Ekle** bölümü Azure Portalı'nda.
   
    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için TINFOIL SECURITY erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **TINFOIL SECURITY**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **TINFOIL SECURITY**.

    ![TINFOIL SECURITY bağlantıya uygulamalar listesi](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-tinfoil-security-test-user"></a>TINFOIL SECURITY test kullanıcısı oluşturma

TINFOIL SECURITY oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların TINFOIL SECURITY sağlanması gerekir. TINFOIL SECURITY söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Sağlanan kullanıcı almak için aşağıdaki adımları gerçekleştirin:**

1. Kullanıcı bir kurumsal hesap parçasıysa yapmanız [TINFOIL SECURITY Destek ekibine başvurun](https://www.tinfoilsecurity.com/contact) oluşturduğunuz kullanıcı hesabı.

1. TINFOIL SECURITY SaaS normal bir kullanıcı kullanıcı ise daha sonra kullanıcı ortak çalışan herhangi bir kullanıcının siteler ekleyebilirsiniz. Bu, yeni bir TINFOIL SECURITY kullanıcı hesabı oluşturmak için bir davet belirtilen e-posta göndermek için bir işlemi tetikler.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir TINFOIL SECURITY kullanıcı hesabı oluşturma araçları veya TINFOIL SECURITY tarafından sağlanan API'leri kullanabilirsiniz.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TINFOIL SECURITY kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama TINFOIL SECURITY için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

