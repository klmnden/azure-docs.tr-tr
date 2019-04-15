---
title: 'Öğretici: Sugar CRM ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Sugar CRM ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 2dea1dcd2f6ecef580d65a95d1227380901213eb
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565529"
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Öğretici: Sugar CRM ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Sugar CRM tümleştirme konusunda bilgi edinin.
Sugar CRM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Sugar CRM erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Sugar CRM için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Sugar CRM ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Sugar CRM çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Sugar CRM destekler **SP** tarafından başlatılan

## <a name="adding-sugar-crm-from-the-gallery"></a>Sugar CRM galeri ekleme

Azure AD'de Sugar CRM tümleştirmesini yapılandırmak için Sugar CRM Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Sugar CRM Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Sugar CRM**seçin **Sugar CRM** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sugar CRM sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Sugar adlı bir test kullanıcı tabanlı CRM test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Sugar CRM'de arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Sugar CRM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Sugar CRM çoklu oturum açmayı yapılandırma](#configure-sugar-crm-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Sugar CRM test kullanıcısı oluşturma](#create-sugar-crm-test-user)**  - kullanıcı Azure AD gösterimini bağlı Sugar CRM'de Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Sugar CRM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Sugar CRM** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Sugar CRM etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://<companyname>.sugarondemand.com`|
    | `https://<companyname>.trial.sugarcrm`|

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Sugar CRM istemci Destek ekibine](https://support.sugarcrm.com/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Sugar CRM ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sugar-crm-single-sign-on"></a>Sugar CRM çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Sugar CRM şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **yönetici**.

    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "yönetici")

1. İçinde **Yönetim** bölümünde **parola yönetimi**.

    ![Yönetim](./media/sugarcrm-tutorial/ic795889.png "Yönetim")

1. Seçin **SAML kimlik doğrulamasını etkinleştirme**.

    ![Yönetim](./media/sugarcrm-tutorial/ic795890.png "Yönetim")

1. İçinde **SAML kimlik doğrulaması** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulaması](./media/sugarcrm-tutorial/ic795891.png "SAML kimlik doğrulaması")  

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
  
    b. İçinde **SLO URL** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
  
    c. Tüm sertifika içine yapıştırın, base-64 kodlanmış sertifika Not Defteri'nde açın ve içeriğini, panoya kopyalanmak **X.509 sertifikası** metin.
  
    d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Sugar CRM için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Sugar CRM**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Sugar CRM**.

    ![Uygulamalar listesinde Sugar CRM bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sugar-crm-test-user"></a>Sugar CRM test kullanıcısı oluşturma

Sugar CRM'de oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Sugar CRM'ye sağlanması gerekir. Sugar CRM söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Sugar CRM** şirketinizin sitesi yöneticisi olarak.

1. Git **yönetici**.

    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "yönetici")

1. İçinde **Yönetim** bölümünde **kullanıcı yönetimi**.

    ![Yönetim](./media/sugarcrm-tutorial/ic795893.png "Yönetim")

1. Git **kullanıcılar \> yeni kullanıcı oluşturma**.

    ![Yeni kullanıcı oluşturma](./media/sugarcrm-tutorial/ic795894.png "yeni kullanıcı oluşturma")

1. Üzerinde **kullanıcı profili** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/sugarcrm-tutorial/ic795895.png "yeni kullanıcı")

    * Tür **kullanıcı adı**, **Soyadı**, ve **e-posta adresi** ilgili metin kutularına halinde geçerli bir Azure Active Directory kullanıcı.
  
1. Olarak **durumu**seçin **etkin**.

1. Parola sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/sugarcrm-tutorial/ic795896.png "yeni kullanıcı")

    a. Parolayı ilgili metin kutusuna yazın.

    b. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer Sugar CRM kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Sugar CRM tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Sugar CRM kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Sugar CRM için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

