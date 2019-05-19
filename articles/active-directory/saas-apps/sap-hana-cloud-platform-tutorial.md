---
title: 'Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Cloud Platform ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b345656b30a9bb182c097a4c9e18d71a293bf420
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65868031"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Cloud Platform tümleştirme konusunda bilgi edinin.
Azure AD ile SAP Cloud Platform tümleştirme ile aşağıdaki avantajları sağlar:

* SAP Cloud Platform erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için SAP Cloud Platform oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP Cloud Platform ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAP Cloud Platform çoklu oturum açmayı abonelik etkin.

Bu öğreticiyi tamamladıktan sonra SAP Cloud Platform için atanmış Azure AD kullanıcılarının uygulamayı kullanarak çoklu oturum açma şunları yapabilecek [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Kendi uygulamanızı dağıtmak veya çoklu oturum açmayı test etmek için SAP Cloud Platform hesabınızda bir uygulama abone olmak gerekir. Bu öğreticide, bir uygulama hesabında dağıtılır.
> 

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP Cloud Platform destekler **SP** tarafından başlatılan

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>SAP Cloud Platform galeri ekleme

Azure AD'de SAP Cloud Platform tümleştirmesini yapılandırmak için SAP Cloud Platform Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Cloud Platform Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP Cloud Platform**seçin **SAP Cloud Platform** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde SAP Cloud Platform](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı SAP Cloud Platform ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı SAP Cloud Platform arasında bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP Cloud Platform ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP Cloud Platform çoklu oturum açmayı yapılandırma](#configure-sap-cloud-platform-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP Cloud Platform test kullanıcısı oluşturma](#create-sap-cloud-platform-test-user)**  - kullanıcı Azure AD gösterimini bağlı SAP Cloud Platform Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

SAP Cloud Platform ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP Cloud Platform** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Cloud Platform etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **işareti bulunan URL'si** metin kutusu, türü URL kullanılan, kullanıcılarınızın oturum açmak için **SAP Cloud Platform** uygulama. Bu işlev, SAP Cloud Platform uygulamanızdaki korumalı bir kaynağın hesaba özel URL'dir. URL aşağıdaki desenini esas: `https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Bu kullanıcının kimlik doğrulamasını gerektiren SAP Cloud Platform uygulamanızda URL'dir.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. İçinde **tanımlayıcı** textbox sağlayacağınızı SAP bulut platformun aşağıdaki desenlerden birini kullanarak bir URL yazın: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcıya ve yanıt URL'si ile güncelleştirin. İlgili kişi [SAP Cloud Platform istemci Destek ekibine](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) oturum açma URL'si ve tanımlayıcısı alınamıyor. Yanıt URL'si, öğreticinin ilerleyen bölümlerinde açıklanan güven yönetim bölümünden elde edebilirsiniz.
    > 
4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-sap-cloud-platform-single-sign-on"></a>SAP Cloud Platform çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde SAP Cloud Platform Kokpit oturum `https://account.<landscape host>.ondemand.com/cockpit`(örneğin: https://account.hanatrial.ondemand.com/cockpit).

2. Tıklayın **güven** sekmesi.
   
    ![Güven](./media/sap-hana-cloud-platform-tutorial/ic790800.png "güven")

3. Güven Yönetimi bölümünde altında **yerel hizmet sağlayıcısı**, aşağıdaki adımları gerçekleştirin:

    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic793931.png "güven Yönetimi")
   
    a. **Düzenle**‘ye tıklayın.

    b. Olarak **yapılandırma türü**seçin **özel**.

    c. Olarak **yerel sağlayıcı adı**, varsayılan değeri bırakın. Bu değeri kopyalayın ve yapıştırın **tanımlayıcı** SAP Cloud Platform için Azure AD yapılandırmasında alan.

    d. Oluşturmak için bir **imzalama anahtarı** ve **imzalama sertifikası** anahtar çifti, tıklayın **anahtar çifti oluşturma**.

    e. Olarak **asıl yayma**seçin **devre dışı bırakılmış**.

    f. Olarak **zorla kimlik doğrulaması**seçin **devre dışı bırakılmış**.

    g. **Kaydet**’e tıklayın.

4. Kaydettikten sonra **yerel hizmet sağlayıcısı** ayarlarını yanıt URL'si almak için aşağıdakileri yapın:
   
    ![Meta verilerini al](./media/sap-hana-cloud-platform-tutorial/ic793930.png "meta verilerini al")

    a. SAP Cloud Platform meta veri dosyası tıklayarak indirin **Get Metadata**.

    b. İndirilen SAP Cloud Platform meta veri XML dosyasını açın ve ardından bulun **ns3:AssertionConsumerService** etiketi.
 
    c. Değerini kopyalayın **konumu** özniteliği ve ardından yapıştırın **yanıt URL'si** SAP Cloud Platform için Azure AD yapılandırmasında alan.

5. Tıklayın **güvenilen kimlik sağlayıcı** sekmesine ve ardından **güvenilen kimlik sağlayıcı Ekle**.
   
    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic790802.png "güven Yönetimi")
   
    >[!NOTE]
    >Güvenilen kimlik sağlayıcıları listesini yönetmek için özel yapılandırma türü yerel hizmet sağlayıcısı bölümünde seçtiğiniz gerekir. Varsayılan yapılandırma türü için bir düzenlenemez ve örtük güven SAP kimliği hizmetine sahip. Hiçbiri için herhangi bir güven ayarı yok.
    > 
    > 

6. Tıklayın **genel** sekmesine ve ardından **Gözat** indirilen meta veri dosyası karşıya yüklemek için.
    
    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic793932.png "güven Yönetimi")
    
    >[!NOTE]
    >Meta veri dosyası, değerleri karşıya yükledikten sonra **çoklu oturum açma URL'si**, **çoklu oturum kapatma URL'si**, ve **imzalama sertifikası** otomatik olarak doldurulur.
    > 
     
7. **Öznitelikler** sekmesine tıklayın.

8. Üzerinde **öznitelikleri** sekmesinde, aşağıdaki adımı uygulayın:
    
    ![Öznitelikleri](./media/sap-hana-cloud-platform-tutorial/ic790804.png "öznitelikleri") 

    a. Tıklayın **Add Assertion-Based özniteliği**ve ardından aşağıdaki onay tabanlı öznitelikler ekleyin:
       
    | Onaylama işlemi özniteliği | Asıl özniteliği |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |firstName |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Soyadı |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |email |
   
     >[!NOTE]
     >Nasıl SCP şirket uygulamaları, diğer bir deyişle, SAML yanıtta bekledikleri ve hangi adı (asıl özniteliği) altında kod bu öznitelikte erişim hangi öznitelikleri geliştirilen üzerinde öznitelikleri yapılandırmasına bağlıdır.
     > 
    
    b. **Varsayılan öznitelik** ekran görüntüsünde yalnızca gösterim amaçlıdır. İş senaryosu yapmak için gerekli değildir.  
 
    c. Adlarını ve değerlerini **sorumlusu özniteliği** ekran görüntüsünde gösterilen nasıl uygulamanın geliştirilme yöntemi bağlıdır. Bu, uygulamanızın farklı eşlemeleri gerektirdiği mümkündür.

### <a name="assertion-based-groups"></a>Onaylama işlemi tabanlı grupları

İsteğe bağlı bir adım, onaylama işlemi tabanlı grupları Azure Active Directory kimlik sağlayıcınız için yapılandırabilirsiniz.

SAP Cloud Platform üzerinde grupları kullanarak dinamik olarak bir veya daha fazla kullanıcı, SAP Cloud Platform uygulamalarınızda SAML 2.0 onaylama özniteliklerin değerleri tarafından belirlenen bir veya daha fazla rol atamak sağlar. 

Örneğin, öznitelik onaylama içeriyorsa, "*sözleşme geçici =*", gruba eklenecek etkilenen tüm kullanıcıların isteyebilirsiniz"*geçici*". Grup "*geçici*" SAP Cloud Platform hesabınızdaki dağıtılan bir veya daha fazla uygulamalardan bir veya daha fazla rol içerebilir.
 
Onaylama tabanlı grupları, uygulamaların, SAP Cloud Platform hesabınızdaki bir veya daha fazla rolleri aynı anda çok sayıda kullanıcı atamak istediğinizde kullanın. Yalnızca tek ya da küçük sayıda kullanıcıları belirli rollere atamak istiyorsanız, bunları doğrudan atama öneririz "**yetkilendirmeleri**" SAP Cloud Platform Kokpit sekmesi.

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

Bu bölümde, SAP Cloud Platform için erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP Cloud Platform**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **SAP Cloud Platform**.

    ![Uygulamalar listesinde SAP Cloud Platform bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-cloud-platform-test-user"></a>SAP Cloud Platform test kullanıcısı oluşturma

SAP Cloud Platform için oturum açmak Azure AD kullanıcılarının etkinleştirmek için SAP Cloud Platform rollerinde kendisine atamanız gerekir.

**Bir kullanıcıya rol atamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **SAP Cloud Platform** Kokpit.

2. Aşağıdakileri gerçekleştirin:
   
    ![Yetkilendirmeleri](./media/sap-hana-cloud-platform-tutorial/ic790805.png "yetkilendirmeleri")
   
    a. Tıklayın **yetkilendirme**.

    b. Tıklayın **kullanıcılar** sekmesi.

    c. İçinde **kullanıcı** metin kutusu, kullanıcının e-posta adresini yazın.

    d. Tıklayın **atama** kullanıcıya bir rol ataması.

    e. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP Cloud Platform kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama SAP Cloud Platform için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

