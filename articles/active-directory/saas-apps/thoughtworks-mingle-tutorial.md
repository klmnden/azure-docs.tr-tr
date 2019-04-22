---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Thoughtworks Mingle | Microsoft Docs'
description: Azure Active Directory ve Thoughtworks Mingle arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 5acf02c021fdfd9f85eeb2b6b1c697ce92c48a15
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59259516"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Öğretici: Thoughtworks Mingle ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Thoughtworks Mingle öğrenin.
Azure AD ile tümleştirme Thoughtworks Mingle ile aşağıdaki avantajları sağlar:

* Thoughtworks Mingle erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Thoughtworks Mingle için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Thoughtworks Mingle ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Thoughtworks Mingle çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Thoughtworks Mingle destekler **SP** tarafından başlatılan

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Galeriden Thoughtworks Mingle ekleme

Thoughtworks Mingle tümleştirmesini Azure AD'de yapılandırmak için Thoughtworks Mingle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Thoughtworks Mingle eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Thoughtworks Mingle**seçin **Thoughtworks Mingle** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Thoughtworks Mingle](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı Thoughtworks Mingle ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili Thoughtworks Mingle kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Thoughtworks Mingle ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Thoughtworks Mingle çoklu oturum açmayı yapılandırma](#configure-thoughtworks-mingle-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Test kullanıcısı Thoughtworks Mingle oluşturma](#create-thoughtworks-mingle-test-user)**  - Thoughtworks kullanıcı Azure AD gösterimini bağlı Mingle Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Thoughtworks Mingle ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Thoughtworks Mingle** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Thoughtworks Mingle etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Thoughtworks Mingle istemci Destek ekibine](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Thoughtworks Mingle kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-thoughtworks-mingle-single-sign-on"></a>Yapılandırma Thoughtworks Mingle çoklu oturum açma

1. Oturum açın, **Thoughtworks Mingle** şirketinizin sitesi yöneticisi olarak.

2. Tıklayın **yönetici** sekmesini tıklatıp, ardından **SSO yapılandırma**.
   
    ![Yönetim sekmesi](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO yapılandırma")

3. İçinde **SSO yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SSO yapılandırma](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO yapılandırma")
    
    a. Meta veri dosyası karşıya yüklemek için tıklayın **dosya**. 

    b. Tıklayın **değişiklikleri kaydetmek**.

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

Bu bölümde, Azure çoklu oturum açma Thoughtworks Mingle için erişim izni verme kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Thoughtworks Mingle**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Thoughtworks Mingle**.

    ![Uygulamalar listesinde Thoughtworks Mingle bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-thoughtworks-mingle-test-user"></a>Thoughtworks Mingle test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmesi, Azure Active Directory kullanıcı adlarını kullanarak Thoughtworks Mingle uygulaması sağlanmalıdır. Thoughtworks Mingle söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Şirketinizin Thoughtworks Mingle sitesi için yönetici olarak oturum açın.

2. tıklayın **profili**.
   
    ![İlk projenizi](./media/thoughtworks-mingle-tutorial/ic785160.png "ilk projenizi")

3. Tıklayın **yönetici** sekmesine ve ardından **kullanıcılar**.
   
    ![Kullanıcılar](./media/thoughtworks-mingle-tutorial/ic785161.png "kullanıcılar")

4. Tıklayın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/thoughtworks-mingle-tutorial/ic785162.png "yeni kullanıcı")

5. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/ic785163.png "yeni kullanıcı")  
 
    a. Tür **oturum açma adı**, **görünen ad**, **Seç parola**, **parolayı onayla** geçerli bir Azure AD hesabı sağlamak istediğiniz ilgili metin kutularına. 

    b. Olarak **kullanıcı türü**seçin **tam kullanıcı**.

    c. Tıklayın **bu profil oluşturma**.

>[!NOTE]
>Herhangi diğer Thoughtworks Mingle kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri Thoughtworks Mingle tarafından sağlanan AAD kullanıcı hesapları sağlamak için.
> 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Thoughtworks Mingle kutucuğa tıkladığınızda, otomatik olarak SSO'yu ayarlama Thoughtworks Mingle için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

