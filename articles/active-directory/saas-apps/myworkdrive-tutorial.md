---
title: 'Öğretici: MyWorkDrive ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve MyWorkDrive arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.openlocfilehash: 0c3eb72abc90347c8e18a2f56a5d4756ecd80f3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096330"
---
# <a name="tutorial-integrate-myworkdrive-with-azure-active-directory"></a>Öğretici: MyWorkDrive Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile MyWorkDrive tümleştirme öğreneceksiniz. MyWorkDrive Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* MyWorkDrive erişimi, Azure AD'de denetler.
* Otomatik olarak MyWorkDrive için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* MyWorkDrive çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. MyWorkDrive destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-myworkdrive-from-the-gallery"></a>MyWorkDrive galeri ekleme

Azure AD'de MyWorkDrive'nın tümleştirmesini yapılandırmak için MyWorkDrive Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **MyWorkDrive** arama kutusuna.
1. Seçin **MyWorkDrive** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO MyWorkDrive kullanarak adlı bir test kullanıcı ile test etme **Britta Simon**. Çalışmak SSO için MyWorkDrive bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO MyWorkDrive ile'test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[MyWorkDrive SSO'yu yapılandırarak](#configure-myworkdrive-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[MyWorkDrive test kullanıcısı oluşturma](#create-myworkdrive-test-user)**  - kullanıcı Azure AD gösterimini bağlı MyWorkDrive Britta simon'un bir karşılığı vardır.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **MyWorkDrive** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak istiyorsanız, sayfa **IDP** modunda başlatılan şu alan için değerleri girin:

    İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<SERVER.DOMAIN.COM>/Account/Login-saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kendi şirketin MyWorkDrive sunucu konak name:e.g girin.
    > 
    > Yanıt URL'si: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Oturum açma URL'si:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > İlgili kişi [MyWorkDrive Destek ekibine](mailto:support@myworkdrive.com) kendi ana bilgisayar adı ve bu değerler için SSL sertifikası ayarlama hakkında şüpheleriniz varsa.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** panonuza.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-myworkdrive-sso"></a>MyWorkDrive SSO yapılandırma

1. Farklı bir web tarayıcı penceresinde MyWorkDrive için bir güvenlik yöneticisi olarak oturum açın.

2. MyWorkDrive sunucuda yönetim paneli tıklayarak **Kurumsal** ve aşağıdaki adımları gerçekleştirin:

    ![Yönetici](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Etkinleştirme **SAML/ADFS SSO**.

    b. Seçin **SAML - Azure AD**

    c. İçinde **Azure uygulama Federasyon meta verileri URL'sini** metin değerini yapıştırın **uygulama Federasyon meta verileri URL'sini** hangi Azure portaldan kopyaladığınız.

    d. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Ek bilgileri İnceleme için [MyWorkDrive Azure AD'ye destek makalesi](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için MyWorkDrive erişim vererek Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **MyWorkDrive**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-myworkdrive-test-user"></a>MyWorkDrive test kullanıcısı oluşturma

Bu bölümde, Britta Simon MyWorkDrive adlı bir kullanıcı oluşturun. Çalışmak [MyWorkDrive Destek ekibine](mailto:support@myworkdrive.com) MyWorkDrive platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde MyWorkDrive kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama MyWorkDrive için'oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)