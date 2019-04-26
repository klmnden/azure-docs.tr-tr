---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HighGear | Microsoft Docs'
description: Azure Active Directory ve HighGear arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 55dcd2fb-96b7-46ec-9e69-eee71c535773
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 873bc340d738704418310e22c34b0042f71a96bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60276338"
---
# <a name="tutorial-azure-active-directory-integration-with-highgear"></a>Öğretici: HighGear ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile HighGear tümleştirme öğrenebilirsiniz.
Azure AD ile HighGear tümleştirme ile aşağıdaki avantajları sağlar:

* HighGear erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) HighGear için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile HighGear yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Bir kuruluş veya sınırsız lisans HighGear sistemiyle

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test etme konusunda bilgi edinebilirsiniz.

* HighGear destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-highgear-from-the-gallery"></a>Galeriden HighGear ekleme

Azure AD'de HighGear tümleştirmesini yapılandırmak için HighGear Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden HighGear eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **HighGear**seçin **HighGear** sonuç paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde HighGear](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma HighGear sisteminizi adlı bir test kullanıcı ile test etme nasıl edinebilirsiniz **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı HighGear sisteminizde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma HighGear sisteminizle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[HighGear çoklu oturum açmayı yapılandırma](#configure-highgear-single-sign-on)**  - HighGear uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[HighGear test kullanıcısı oluşturma](#create-highgear-test-user)**  - kullanıcı Azure AD gösterimini bağlı HighGear Britta simon'un bir karşılığı vardır. 
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirme konusunda bilgi edinebilirsiniz.

Azure AD çoklu oturum açma HighGear sisteminizle yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **HighGear** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![HighGear etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusunda, değerini yapıştırın **hizmet sağlayıcısı varlık kimliği** çoklu oturum açma ayarları sayfasında HighGear sisteminizdeki alan.

    ![Hizmet sağlayıcısı varlık kimliği alanı](media/highgear-tutorial/service-provider-entity-id-field.png)
    
    > [!NOTE]
    > HighGear sisteminize çoklu oturum açma ayarları sayfasına erişmek için oturum açmanız. Oturum açtınız sonra Yönetim sekmesinde HighGear üzerinde fareyi hareket ettirin ve çoklu oturum açma ayarları menü öğesini tıklatın.
    
    ![Çoklu oturum açma ayarları menü öğesi](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

    b. İçinde **yanıt URL'si** metin kutusunda, değerini yapıştırın **onaylama tüketici hizmeti (ACS) URL'si** HighGear sisteminizde çoklu oturum açma ayarları sayfasından.

    ![Onaylama tüketici hizmeti (ACS) URL'si](media/highgear-tutorial/assertion-consumer-service-url-field.png)

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

     ![HighGear etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

     İçinde **oturum açma URL'si** metin kutusunda, değerini yapıştırın **hizmet sağlayıcısı varlık kimliği** çoklu oturum açma ayarları sayfasında HighGear sisteminizdeki alan. (Bu varlığı da SP tarafından başlatılan oturum açma için kullanılacak olan HighGear sisteme temel URL'sini kimliğidir.)

    ![Hizmet sağlayıcısı varlık kimliği alanı](media/highgear-tutorial/service-provider-entity-id-field.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile Güncelleştir **çoklu oturum açma ayarları** HighGear sisteminizde sayfası. Yardıma ihtiyacınız varsa lütfen iletişime geçin [HighGear Destek ekibine](mailto:support@highgear.com).

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınıza kaydedin. Çoklu oturum açma yapılandırması daha sonraki bir adımda gerekir.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **HighGear kümesi** bölümünde, aşağıdaki URL'ler konumunu not edin.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum açma URL'si. Adım 2 altında bu değeri ihtiyacınız olacak **yapılandırma HighGear çoklu oturum açma** aşağıda.

    b. Azure AD tanımlayıcısı. Bu değeri 3. adım altında ihtiyacınız olacak **yapılandırma HighGear çoklu oturum açma** aşağıda.

    c. Oturum kapatma URL'si. Adım 4 altında bu değeri ihtiyacınız olacak **yapılandırma HighGear çoklu oturum açma** aşağıda.

### <a name="configure-highgear-single-sign-on"></a>HighGear tek oturum açmayı yapılandırın

HighGear için çoklu oturum açmayı yapılandırmak için lütfen HighGear sisteminize oturum açın. Oturum açtınız sonra Yönetim sekmesinde HighGear üzerinde fareyi hareket ettirin ve çoklu oturum açma ayarları menü öğesini tıklatın.

![Çoklu oturum açma ayarları menü öğesi](media/highgear-tutorial/single-sign-on-settings-menu-item.png)

1. İçinde **kimlik sağlayıcı adı**, oturum açma sayfasındaki HighGear'ın çoklu oturum açma düğmesi görünür kısa bir açıklama yazın. Örneğin: Azure AD

2. İçinde **çoklu oturum açma (SSO) URL'si** alan HighGear içinde değerini yapıştırın **oturum açma URL'si** yer alan **HighGear kümesi** Azure bölümünde.

3. İçinde **kimlik sağlayıcısı varlık kimliği** alan HighGear içinde değerini yapıştırın **Azure AD tanımlayıcısı** yer alan **HighGear kümesi** Azure bölümünde.

4. İçinde **çoklu oturum kapatma (SLO) URL'si** alan HighGear içinde değerini yapıştırın **oturum kapatma URL'si** yer alan **HighGear kümesi** Azure bölümünde.

5. Kaynağından indirdiğiniz sertifika Not Defteri'ni kullanarak **SAML imzalama sertifikası** Azure bölümünde. İndirilen **sertifika (Base64)** biçimi. Not Defteri'nden sertifika içeriğini kopyalayın ve yapıştırın **kimlik sağlayıcısı sertifikası** HighGear alanındaki.

6. E-posta [HighGear Destek ekibine](mailto:support@highgear.com) HighGear sertifikanızı istemek için. Bunlardan doldurun aldığınız yönergeleri **HighGear sertifika** ve **HighGear sertifika parolası** alanları.

7. Tıklayın **Kaydet** HighGear çoklu oturum açma yapılandırmanızı kaydetmek için düğme.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için HighGear erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **HighGear**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **HighGear**.

    ![Uygulamalar listesinde HighGear bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-highgear-test-user"></a>HighGear test kullanıcısı oluşturma

Çoklu oturum açma yapılandırmanızı test etme HighGear test kullanıcısı oluşturmak için lütfen HighGear sisteminize oturum açın.

1. Tıklayın **yeni kişi oluşturun** düğmesi.

    ![Yeni kişi oluştur düğmesi](media/highgear-tutorial/create-new-contact-button.png)

    Oluşturmak istediğiniz kişinin türünü seçmenize olanak tanıyan bir menü görünür.

2. Tıklayın **bireysel** HighGear kullanıcı oluşturmak için menü öğesi.

    Böylece yeni kullanıcı bilgilerini yazabilirsiniz bölme kullanıma sağ tarafta slayt.  
    ![Yeni kişi formu](media/highgear-tutorial/new-contact-form.png)

3. İçinde **adı** alanında, kişi için bir ad yazın. Örneğin: Britta Simon

4. Tıklayın **diğer seçenekler** menü ve select **hesap bilgileri** menü öğesi.

    ![Hesap bilgileri menü öğesine tıklandığında](media/highgear-tutorial/account-info-menu-item.png)

5. Ayarlama **oturum aç olabilir** alanını Evet.

    **Etkinleştirme çoklu oturum açma** alanı otomatik olarak ayarlanacak Evet olarak de.

6. İçinde **tek oturum açma kullanıcı kimliği** alanında, kullanıcının kimliğini yazın. Örneğin, BrittaSimon@contoso.com

    Hesap bilgileri bölümü artık şöyle görünmelidir:  
    ![Tamamlanmış hesap bilgileri bölümü](media/highgear-tutorial/finished-account-info-section.png)

7. Kişinin kaydetmek için tıklatın **Kaydet** bölmesinin alt kısmındaki düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli HighGear kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama HighGear için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

