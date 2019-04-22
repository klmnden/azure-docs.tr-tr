---
title: 'Öğretici: Görev ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve TAS. arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 129b6e69-e3b4-41d7-9ab5-a2ddd0068f76
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: 11ba7cd9be59f13081bdc5a543c52ca5f9d66e09
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59272504"
---
# <a name="tutorial-azure-active-directory-integration-with-tas"></a>Öğretici: Azure Active Directory Tümleştirmesi ile GÖR

Bu öğreticide, Azure Active Directory (Azure AD) ile görevi tümleştirme konusunda bilgi edinin.
Görev Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* TAS. erişimi, Azure AD'de denetleyebilirsiniz
* Otomatik olarak (çoklu oturum açma) görevi için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile görevi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik görevi çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* GÖREVİ destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-tas-from-the-gallery"></a>Galeriden görev ekleme

Azure AD'de görevi tümleştirmesini yapılandırmak için görevi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden görev eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **görevi**seçin **görevi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde GÖR](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma görevi adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı görevi arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma görevi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[GÖREVİ çoklu oturum açmayı yapılandırma](#configure-tas-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[GÖREVİ test kullanıcısı oluşturma](#create-tas-test-user)**  - kullanıcı Azure AD gösterimini bağlı görevi Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile görevi yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **görevi** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir görevi etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://taseu.combtas.com/<DOMAIN>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://taseu.combtas.com/<ENVIRONMENTNAME>/AssertionService.aspx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir görevi etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://taseu.combtas.com/<DOMAIN>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu, öğreticinin ilerleyen bölümlerinde açıklanan gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirir. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **görevi ayarlayın** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-tas-single-sign-on"></a>GÖREVİ tek oturum açmayı yapılandırın

1. Bir başka web tarayıcı penceresinde görevi yönetici olarak oturum açın.

2. Menü sol tarafında tıklayarak **ayarları** gidin **yönetici** ve ardından **yönetme çoklu oturum açmayı**.

    ![GÖREVİ yapılandırma](./media/tas-tutorial/configure01.png)

3. Üzerinde **yönetme çoklu oturum açmayı** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![GÖREVİ yapılandırma](./media/tas-tutorial/configure02.png)

    a. İçinde **adı** metin ortam adınızı yazın.
    
    b. Seçin **SAML2** olarak **kimlik doğrulama türü**.

    c. İçinde **URL'si girin** metin değerini yapıştırın **oturum açma URL'si** hangi Azure portaldan kopyaladığınız.

    d. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini kopyalayın ve ardından yapıştırın **girin sertifika** kutusu.

    e. İçinde **yeni IP** metin kutusuna IP adresini yazın.

    >[!NOTE]
    > İlgili kişi [görevi Destek ekibine](mailto:support@combtas.com) IP adresini almak için.

    f. Kopyalama **çoklu oturum açma** URL'sini yapıştırın **tanımlayıcı (varlık kimliği)** ve **oturum açma URL'si** textbox'ın **temel SAML yapılandırma** içinde Azure Portalı'nı tıklatın. Url büyük/küçük harfe duyarlıdır ve bir eğik çizgi (/) ile bitmelidir lütfen unutmayın.

    g. Kopyalama **onaylama hizmet** kurulumunda url yapıştırın ve sayfa **yanıt URL'si** textbox'ın **temel SAML yapılandırma** Azure portalında.

    h. Tıklayın **Ekle SSO satır**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için TAS. erişim vererek Britta Simon etkinleştir

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **görevi**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **görevi**.

    ![Uygulamalar listesinde görev bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-tas-test-user"></a>GÖREVİ test kullanıcısı oluşturma

Bu bölümde, Britta Simon TAS. içinde adlı bir kullanıcı oluşturma Çalışmak [görevi Destek ekibine](mailto:support@combtas.com) görevi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli görevi kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama görevi için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

