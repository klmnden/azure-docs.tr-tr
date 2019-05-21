---
title: 'Öğretici: SilkRoad yaşam Suite ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SilkRoad yaşam Suite arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: bcad6232de7fa257b58fe6d84f2c2ff794b64589
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65902312"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Öğretici: SilkRoad yaşam Suite ile Azure Active Directory Tümleştirmesi

Bu öğreticide, SilkRoad yaşam Suite Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Azure AD ile SilkRoad yaşam Suite tümleştirme ile aşağıdaki avantajları sağlar:

* SilkRoad yaşam Suite erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) SilkRoad yaşam paketine oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SilkRoad yaşam Suite ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik SilkRoad yaşam Suite çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SilkRoad yaşam paketini destekleyen **SP** tarafından başlatılan

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Galeriden SilkRoad yaşam paketi ekleme

Azure AD'ye SilkRoad yaşam Suite tümleştirmesini yapılandırmak için SilkRoad yaşam Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SilkRoad yaşam paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SilkRoad yaşam Suite**seçin **SilkRoad yaşam Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SilkRoad yaşam paketi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SilkRoad yaşam adlı bir test kullanıcı tabanlı Suite ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının SilkRoad yaşam paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SilkRoad yaşam Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SilkRoad yaşam Suite çoklu oturum açmayı yapılandırma](#configure-silkroad-life-suite-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SilkRoad yaşam Suite test kullanıcısı oluşturma](#create-silkroad-life-suite-test-user)**  - SilkRoad yaşam paketindeki, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SilkRoad yaşam Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SilkRoad yaşam Suite** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Erişmenizi sağlayacak **hizmet sağlayıcısı meta veri dosyası** Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![image](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik temel SAML yapılandırma bölümünde doldurulur:

    ![image](common/sp-identifier-reply.png)

    > [!Note]
    > Varsa **tanımlayıcı** ve **yanıt URL'si** değerleri otomatik polulated alamıyorsanız, ardından değerleri ihtiyacınıza göre el ile girin.

    d. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.silkroad-eng.com/Authentication/`

5. Üzerinde **temel SAML yapılandırma** sahip değilse, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    ![SilkRoad yaşam Suite etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.silkroad-eng.com/Authentication/`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP`|
    | `https://<subdomain>.silkroad.com/Authentication/SP`|

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/`|
    | `https://<subdomain>.silkroad.com/Authentication/`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SilkRoad yaşam Suite istemci Destek ekibine](https://www.silkroad.com/locations/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **SilkRoad yaşam paketini ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-silkroad-life-suite-single-sign-on"></a>SilkRoad yaşam Suite çoklu oturum açmayı yapılandırın

1. SilkRoad şirketinizin sitesi için yönetici olarak oturum açın.

    > [!NOTE]
    > Microsoft Azure AD ile Federasyonu yapılandırma için SilkRoad yaşam Suite kimlik doğrulaması uygulamaya erişim elde etmek için lütfen SilkRoad desteği veya SilkRoad Hizmetleri temsilcinize başvurun.

1. Git **hizmet sağlayıcısı**ve ardından **Federasyon ayrıntıları**.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png)

1. Tıklayın **Federasyon meta verileri indirme**ve bilgisayarınızda meta veri dosyasını kaydedin. İndirilen Federasyon meta veri olarak kullanmak bir **hizmet sağlayıcısı meta veri dosyası** içinde **temel SAML yapılandırma** bölümünde Azure portalında.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png)

1. İçinde **SilkRoad** uygulama tıklayın **kimlik doğrulaması kaynakları**.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png) 

1. Tıklayın **kimlik doğrulaması kaynağı ekleme**.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png)

1. İçinde **kimlik doğrulaması kaynağı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png)
  
    a. Altında **seçeneği 2 - meta veri dosyası**, tıklayın **Gözat** Azure portalından indirilen meta veri dosyası karşıya yüklemek için.
  
    b. Tıklayın **oluşturma kimlik sağlayıcısı kullanarak dosya verilerini**.

1. İçinde **kimlik doğrulaması kaynakları** bölümünde **Düzenle**.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png)

1. Üzerinde **kimlik doğrulaması kaynağı Düzenle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png)

    a. Olarak **etkin**seçin **Evet**.

    b. İçinde **Entityıd** metin değerini yapıştırın **Azure AD tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **IDP açıklama** metin kutusuna bir açıklama yapılandırmanızı (örneğin: *Azure AD SSO*).

    d. İçinde **meta veri dosyası** metin kutusuna, karşıya yükleme **meta verileri** Azure portalından indirdiğiniz dosyası.
  
    e. İçinde **IDP adı** metin kutusu, bir yapılandırma için belirli bir ad yazın (örneğin: *Azure SP*).
  
    f. İçinde **oturum kapatma hizmeti URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    g. İçinde **oturum açma hizmeti URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    h. **Kaydet**’e tıklayın.

1. Diğer tüm kimlik doğrulaması kaynakları devre dışı bırakın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png)

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

Bu bölümde, Azure çoklu oturum açma SilkRoad yaşam paketine erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SilkRoad yaşam Suite**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SilkRoad yaşam Suite**.

    ![Uygulamalar listesinde SilkRoad yaşam Suite bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-silkroad-life-suite-test-user"></a>SilkRoad yaşam Suite test kullanıcısı oluşturma

Bu bölümde, Britta Simon SilkRoad yaşam paketindeki adlı bir kullanıcı oluşturun. Çalışmak [SilkRoad yaşam Suite istemci Destek ekibine](https://www.silkroad.com/locations/) SilkRoad yaşam Suite platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SilkRoad yaşam Suite kutucuğa tıkladığınızda, size otomatik olarak SilkRoad yaşam SSO'yu ayarlama Suite oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
