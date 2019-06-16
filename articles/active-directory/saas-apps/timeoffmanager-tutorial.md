---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TimeOffManager | Microsoft Docs'
description: Azure Active Directory ve TimeOffManager arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 0d57b3ce4e378a366148b5c3eef6bc4d5d9eb96d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67088555"
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Öğretici: TimeOffManager ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TimeOffManager tümleştirme konusunda bilgi edinin.
Azure AD ile TimeOffManager tümleştirme ile aşağıdaki avantajları sağlar:

* TimeOffManager erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) TimeOffManager için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TimeOffManager yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik TimeOffManager çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* TimeOffManager destekler **IDP** tarafından başlatılan

* TimeOffManager destekler **zamanında** kullanıcı sağlama

## <a name="adding-timeoffmanager-from-the-gallery"></a>Galeriden TimeOffManager ekleme

Azure AD'de TimeOffManager tümleştirmesini yapılandırmak için TimeOffManager Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden TimeOffManager eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TimeOffManager**seçin **TimeOffManager** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde TimeOffManager](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TimeOffManager adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının TimeOffManager ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TimeOffManager ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[TimeOffManager çoklu oturum açmayı yapılandırma](#configure-timeoffmanager-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[TimeOffManager test kullanıcısı oluşturma](#create-timeoffmanager-test-user)**  - kullanıcı Azure AD gösterimini bağlı TimeOffManager Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile TimeOffManager yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **TimeOffManager** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TimeOffManager etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-reply.png)

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. Bu değeri alabilirsiniz **çoklu oturum açma ayarları sayfasına** daha sonra öğreticide veya kişi açıklanan [TimeOffManager Destek ekibine](https://www.purelyhr.com/contact-us). Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. TimeOffManager uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. Yukarıdaki için ayrıca TimeOffManager uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki: 

    | Ad | Kaynak özniteliği|
    | --- | --- |
    | firstName |User.givenName |
    | Soyadı |User.surname |
    | Email |User.Mail |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **TimeOffManager kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-timeoffmanager-single-sign-on"></a>TimeOffManager tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde TimeOffManager şirket sitenize yönetici olarak oturum açın.

2. Git **hesabı \> hesap seçeneklerini \> çoklu oturum açma ayarları**.
   
    ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795917.png "çoklu oturum açma ayarları")

3. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795918.png "çoklu oturum açma ayarları")
   
    a. Tüm sertifika içine yapıştırın, base-64 kodlanmış sertifika Not Defteri'nde açın ve içeriğini, panoya kopyalanmak **X.509 sertifikası** metin.
   
    b. İçinde **IDP veren** metin değerini yapıştırın **Azure AD tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **IDP uç nokta URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    d. Olarak **zorunlu SAML**seçin **Hayır**.
   
    e. Olarak **kullanıcılar otomatik olarak oluşturma**seçin **Evet**.
   
    f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    g. Tıklayın **değişiklikleri kaydetmek**.

4. İçinde **çoklu oturum açma ayarları** sayfasında, değerini kopyalayın **onay belgesi tüketici hizmeti URL'si** yapıştırın **yanıt URL'si** metin kutusu altında **temel SAML Yapılandırma** bölümü Azure Portalı'nda. 

      ![Çoklu oturum açma ayarları](./media/timeoffmanager-tutorial/ic795915.png "çoklu oturum açma ayarları")

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

Bu bölümde, Azure çoklu oturum açma kullanmak için TimeOffManager erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **TimeOffManager**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **TimeOffManager**.

    ![Uygulamalar listesinde TimeOffManager bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-timeoffmanager-test-user"></a>TimeOffManager test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı TimeOffManager oluşturulur. TimeOffManager just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı TimeOffManager içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!NOTE]
>Herhangi diğer TimeOffManager kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için TimeOffManager tarafından sağlanan.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TimeOffManager kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama TimeOffManager için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

