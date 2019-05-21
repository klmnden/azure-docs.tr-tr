---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Velpic SAML | Microsoft Docs'
description: Azure Active Directory arasındaki Velpic SAML çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc8431e2c45cc3bfdfa08dd0078220ab2d290309
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65905395"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Öğretici: Velpic SAML ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Velpic SAML tümleştirme konusunda bilgi edinin.
Azure AD ile Velpic SAML tümleştirme ile aşağıdaki avantajları sağlar:

* Velpic SAML erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Velpic SAML oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Velpic SAML ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik Velpic SAML çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Velpic SAML destekler **SP** tarafından başlatılan

## <a name="adding-velpic-saml-from-the-gallery"></a>Galeriden Velpic SAML ekleme

Azure AD'de SAML Velpic tümleştirmesini yapılandırmak için Velpic SAML Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Velpic SAML eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Velpic SAML**seçin **Velpic SAML** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Velpic SAML](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Velpic SAML ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Velpic SAML ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Velpic SAML ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Velpic SAML çoklu oturum açmayı yapılandırma](#configure-velpic-saml-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Velpic SAML test kullanıcısı oluşturma](#create-velpic-saml-test-user)**  - kullanıcı Azure AD gösterimini bağlı Velpic SAML Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Velpic SAML ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Velpic SAML** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Velpic SAML etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<sub-domain>.velpicsaml.net`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://auth.velpic.com/saml/v2/<entity-id>/login`

    > [!NOTE]
    > Oturum açma URL'si Velpic SAML ekibi tarafından sağlanır ve tanımlayıcı değeri Velpic SAML tarafında SSO eklentisi yapılandırırken kullanılabilecek lütfen unutmayın. Bu değer Velpic SAML uygulama sayfasından kopyalayın ve kopyalayıp buraya yapıştırın gerekir.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Velpic SAML kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-velpic-saml-single-sign-on"></a>Velpic SAML çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Velpic SAML şirket sitenize yönetici olarak oturum açın.

2. Tıklayarak **Yönet** sekmesini ve Git **tümleştirme** bölümüne tıklayarak gereken **eklentileri** oturum açmak için yeni bir eklenti oluşturmak için.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_1.png)

3. Tıklayarak **'Eklentisi Ekle'** düğmesi.
    
    ![Eklentisi](./media/velpicsaml-tutorial/velpic_2.png)

4. Tıklayarak **SAML** kutucuğuna eklentisini ekleyin sayfasında.
    
    ![Eklentisi](./media/velpicsaml-tutorial/velpic_3.png)

5. Yeni SAML eklentisini adını girin ve tıklayın **'Ekle'** düğmesi.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_4.png)

6. Ayrıntılar aşağıdaki gibi girin:

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_5.png)

    a. İçinde **adı** metin SAML eklentisini adını yazın.

    b. İçinde **veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** , kopyalamanın **yapılandırma oturum açma** Azure portal'ın penceresi.

    c. İçinde **sağlayıcısı meta verileri yapılandırma** Azure portalından indirilen meta veri XML dosyasını karşıya yükleyin.

    d. Ayrıca yalnızca zaman etkinleştirerek sağlama SAML etkinleştirmeyi seçebilirsiniz **'Otomatik olarak yeni kullanıcılar oluşturma'** onay kutusu. Bir kullanıcı Velpic yok ve bu bayrağı etkin değil, Azure oturum açma işlemi başarısız olur. Bayrak etkinleştirilirse kullanıcının otomatik olarak Velpic oturum açma sırasında sağlanacaktır. 

    e. Kopyalama **çoklu oturum açma URL'si** metin kutusu ve Azure portalında yapıştırın.
    
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
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma için SAML Velpic erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Velpic SAML**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Velpic SAML**.

    ![Uygulamalar listesinde Velpic SAML bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-velpic-saml-test-user"></a>Velpic SAML test kullanıcısı oluşturma

Bu adım yalnızca zaman sağlama kullanıcı uygulamanın desteklediği gibi genellikle gerekli değildir. Otomatik kullanıcı hazırlama etkin değilse el ile kullanıcı oluşturma aşağıda açıklandığı gibi yapılabilir.

Velpic SAML şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları uygulayın:
    
1. Yönet sekmesini tıklatın ve kullanıcılar bölümüne gidin ve ardından kullanıcı eklemek için yeni düğmesine tıklayın.

    ![Kullanıcı Ekle](./media/velpicsaml-tutorial/velpic_7.png)

2. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin.

    ![kullanıcı](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. İçinde **ad** metin Britta ilk tür adı.

    b. İçinde **Soyadı** metin Simon son adını yazın.

    c. İçinde **kullanıcı adı** metin Britta simon'un kullanıcı adını yazın.

    d. İçinde **e-posta** metin kutusuna e-posta adresini yazın Brittasimon@contoso.com hesabı.

    e. Geri kalan bilgileri isteğe bağlı olduğundan, gerekirse doldurabilirsiniz.
    
    f. **KAYDET**'e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim paneli Velpic SAML kutucuğa tıkladığınızda, oturum açma sayfası Velpic SAML uygulamanın almanız gerekir. Görmelisiniz **'Oturumu, Azure AD'de oturum'** oturum açma sayfasında düğme.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_6.png)

1. Tıklayarak **'Oturumu, Azure AD'de oturum'** düğmesini için Velpic Azure AD'ye hesabınızı kullanarak oturum açın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

