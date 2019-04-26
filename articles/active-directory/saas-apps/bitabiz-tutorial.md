---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile BitaBIZ | Microsoft Docs'
description: Azure Active Directory ve BitaBIZ arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19e46c77d3204cc6cd037e5ef8252aa7598d624d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60289251"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Öğretici: BitaBIZ ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BitaBIZ tümleştirme konusunda bilgi edinin.
Azure AD ile BitaBIZ tümleştirme ile aşağıdaki avantajları sağlar:

* BitaBIZ erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) BitaBIZ için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BitaBIZ yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik BitaBIZ çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* BitaBIZ destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-bitabiz-from-the-gallery"></a>Galeriden BitaBIZ ekleme

Azure AD'de BitaBIZ tümleştirmesini yapılandırmak için BitaBIZ Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden BitaBIZ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **BitaBIZ**seçin **BitaBIZ** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde BitaBIZ](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma BitaBIZ adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının BitaBIZ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma BitaBIZ ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[BitaBIZ çoklu oturum açmayı yapılandırma](#configure-bitabiz-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[BitaBIZ test kullanıcısı oluşturma](#create-bitabiz-test-user)**  - kullanıcı Azure AD gösterimini bağlı BitaBIZ Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile BitaBIZ yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **BitaBIZ** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![BitaBIZ etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.bitabiz.com/<instanceId>`

    > [!NOTE]
    > Yalnızca gösterimi için yukarıdaki URL'deki değerdir. Öğreticinin ilerleyen bölümlerinde açıklanan gerçek tanımlayıcı değerini güncelleştirin.

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![image](common/both-preintegrated-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın:  `https://www.bitabiz.com/dashboard`

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **BitaBIZ kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-bitabiz-single-sign-on"></a>BitaBIZ tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde BitaBIZ kiracınıza yönetici olarak oturum.

2. Tıklayarak **Kurulum yönetici**.

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings1.png)

3. Tıklayarak **Microsoft tümleştirmeler** altında **değer Ekle** bölümü.

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings2.png)

4. Bölümüne kaydırın **Microsoft Azure AD (etkinleştir çoklu oturum açma)** ve aşağıdaki adımları uygulayın:

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings3.png)

    a. Değeri Şuradan Kopyala: **varlık kimliği (Azure AD'de "tanımlayıcı")** metin kutusuna yapıştırın **tanımlayıcı** metin üzerinde **temel SAML yapılandırma** bölümünde Azure portalında . 

    b. İçinde **Azure AD çoklu oturum açma hizmeti URL'si** metin kutusu, yapıştırma **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **Azure AD SAML varlık kimliği** metin kutusu, yapıştırma **Azure Ad tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **Azure AD imzalama sertifikası (şifreli Base64)** metin.

    e. İş e-posta etki alanınızı ekleme mycompany.com, diğer bir deyişle, ad **etki alanı adı** SSO bu e-posta etki alanı ile şirketinizdeki kullanıcılara atamak için metin (zorunlu).

    f. İşareti **SSO etkin** BitaBIZ hesabı.

    g. Tıklayın **Azure AD'ye yapılandırmayı kaydetmek** kaydedin ve SSO yapılandırma etkinleştirme.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için BitaBIZ erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **BitaBIZ**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **BitaBIZ**.

    ![Uygulamalar listesinde BitaBIZ bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-bitabiz-test-user"></a>BitaBIZ test kullanıcısı oluşturma

BitaBIZ için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların BitaBIZ sağlanması gerekir.  
BitaBIZ söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. BitaBIZ şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **Kurulum yönetici**.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/settings1.png)

3. Tıklayarak **kullanıcı ekleme** altında **kuruluş** bölümü.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user1.png)

4. Tıklayın **Ekle yeni çalışan**.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user2.png)

5. Üzerinde **Ekle yeni çalışan** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user3.png)

    a. İçinde **ad** metin kutusu, kullanıcının Britta gibi ilk tür adı.

    b. İçinde **Soyadı** metin son Simon gibi kullanıcı adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. Bir tarih seçin **gününü**.

    e. Kullanıcı için ayarlanabilir diğer zorunlu olmayan kullanıcı öznitelikleri vardır. Lütfen [çalışan Kurulum Doc](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) daha fazla ayrıntı için.

    f. Tıklayın **Kaydet çalışan**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli BitaBIZ kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama BitaBIZ için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
