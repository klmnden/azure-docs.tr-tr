---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Comm100 canlı sohbet | Microsoft Docs'
description: Azure Active Directory ve Comm100 canlı sohbet arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 203c082275dc75a7dcf948eb42a383300955f355
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60281092"
---
# <a name="tutorial-azure-active-directory-integration-with-comm100-live-chat"></a>Öğretici: Azure Active Directory tümleştirmesiyle Comm100 canlı sohbet

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Comm100 canlı sohbet öğrenin.
Azure AD ile tümleştirme Comm100 canlı sohbet ile aşağıdaki avantajları sağlar:

* Comm100 canlı sohbet erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak Comm100 canlı sohbet için (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Comm100 canlı sohbet ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Comm100 canlı sohbet çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Comm100 canlı sohbet destekler **SP** tarafından başlatılan

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Galeriden Comm100 canlı sohbet ekleme

Azure AD'de Comm100 canlı sohbet, tümleştirmesini yapılandırmak için Comm100 canlı sohbet Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Comm100 canlı sohbet eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Comm100 canlı sohbet**seçin **Comm100 canlı sohbet** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Canlı sohbet Comm100 sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Comm100 adlı bir test kullanıcı tabanlı canlı sohbet ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Comm100 canlı sohbet arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Comm100 canlı sohbet ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Comm100 canlı sohbet çoklu oturum açmayı yapılandırma](#configure-comm100-live-chat-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Test kullanıcısı Comm100 canlı sohbet oluşturma](#create-comm100-live-chat-test-user)**  - Comm100 Canlı kullanıcı Azure AD gösterimini bağlı sohbet Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Comm100 canlı sohbet ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Comm100 canlı sohbet** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Comm100 canlı sohbet etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Oturum açma URL değeri, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek oturum açma URL ile güncelleştirir.

5. Uygulama Comm100 canlı sohbet, belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda kullanarak talep Düzenle **düzenleme simgesi** veya talep kullanarak **Ekle yeni talep**SAML belirteci özniteliği yukarıdaki görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin: 

    | Ad |  Kaynak özniteliği|
    | ---------------| --------------- |
    |   e-posta    | User.Mail |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Comm100 canlı sohbet kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-comm100-live-chat-single-sign-on"></a>Comm100 canlı sohbet çoklu oturum açmayı yapılandırın

1. Bir başka web tarayıcı penceresinde Comm100 canlı sohbet bir güvenlik yöneticisi olarak oturum açın.

1. Sayfanın üst sağ tarafında tıklayın **hesabım**.

   ![Myaccount Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

1. Menü Sol taraftan tıklayın **güvenlik** ve ardından **aracı çoklu oturum açma**.

   ![Güvenlik Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

1. Üzerinde **aracı çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

   ![Güvenlik Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

   a. İlk vurgulanan bağlantıyı kopyalayın ve yapıştırın **oturum açma URL'si** metin kutusunda **Comm100 canlı sohbet etki alanı ve URL'ler** bölümü Azure portalı.

   b. İçinde **SAML SSO URL** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız.

   c. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

   d. Tıklayın **bir dosya seçin** base-64 karşıya yüklemek için Azure portalından içine yüklediğiniz sertifika kodlanmış **sertifika**.

   e. Tıklayın **Değişiklikleri Kaydet**

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

Bu bölümde, Azure çoklu oturum açma Comm100 canlı sohbet için erişim izni verme kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Comm100 canlı sohbet**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Comm100 canlı sohbet**.

    ![Uygulamalar listesinde Comm100 canlı sohbet bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-comm100-live-chat-test-user"></a>Comm100 canlı sohbet test kullanıcısı oluşturma

Comm100 canlı sohbet oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Comm100 canlı sohbet sağlanması gerekir. Comm100 canlı sohbet sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Comm100 canlı sohbet bir güvenlik yöneticisi olarak oturum açın.

2. Sayfanın üst sağ tarafında tıklayın **hesabım**.

    ![Myaccount Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. Menü Sol taraftan tıklayın **aracıları** ve ardından **yeni aracı**.

    ![Aracı Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. Üzerinde **yeni aracı** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yeni aracı Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon\@contoso.com**.

    b. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    c. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **simon**.

    d. İçinde **görünen adı** metin gibi kullanıcının görünen adını girin **Britta simon**

    e. İçinde **parola** metin kutusuna bir parola yazın.

    f. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Comm100 canlı sohbet kutucuğa tıkladığınızda, otomatik olarak Comm100 Canlı SSO'yu ayarlama sohbet için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

