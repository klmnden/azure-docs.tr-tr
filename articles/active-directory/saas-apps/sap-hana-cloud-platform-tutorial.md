---
title: 'Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Cloud Platform ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 07b3c32601d90fdeed1c335c0f36a5ccbdbe4f1d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446723"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Öğretici: SAP Cloud Platform ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Cloud Platform tümleştirme konusunda bilgi edinin.

Azure AD ile SAP Cloud Platform tümleştirme ile aşağıdaki avantajları sağlar:

- SAP Cloud Platform erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için SAP Cloud Platform açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Cloud Platform ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- SAP Cloud Platform çoklu oturum açma abonelik etkin.

Bu öğreticiyi tamamladıktan sonra SAP Cloud Platform için atanmış Azure AD kullanıcılarının uygulamayı kullanarak çoklu oturum açma şunları yapabilecek [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Kendi uygulamanızı dağıtmak veya çoklu oturum açmayı test etmek için SAP Cloud Platform hesabınızda bir uygulama abone olmak gerekir. Bu öğreticide, bir uygulama hesabında dağıtılır.
> 

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP Cloud Platform galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>SAP Cloud Platform galeri ekleme
Azure AD'de SAP Cloud Platform tümleştirmesini yapılandırmak için SAP Cloud Platform Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Cloud Platform Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SAP Cloud Platform**seçin **SAP Cloud Platform** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SAP Cloud Platform](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SAP Cloud Platform ile test edin.

Tek iş için oturum açma için Azure AD ne SAP Cloud Platform karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı SAP Cloud Platform arasında bağlantı ilişki kurulması gerekir.

SAP Cloud Platform içinde değeri atamak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP Cloud Platform ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAP Cloud Platform test kullanıcısı oluşturma](#create-a-sap-cloud-platform-test-user)**  - kullanıcı Azure AD gösterimini bağlı SAP Cloud Platform Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP Cloud Platform uygulamanızda çoklu oturum açmayı yapılandırın.

**SAP Cloud Platform ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SAP Cloud Platform** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

1. Üzerinde **SAP Cloud Platform etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Cloud Platform etki alanı ve URL'ler tek oturum açma bilgileri](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

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
     
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde SAP Cloud Platform Kokpit oturum `https://account.<landscape host>.ondemand.com/cockpit`(örneğin: https://account.hanatrial.ondemand.com/cockpit).

1. Tıklayın **güven** sekmesi.
   
    ![Güven](./media/sap-hana-cloud-platform-tutorial/ic790800.png "güven")

1. Güven Yönetimi bölümünde altında **yerel hizmet sağlayıcısı**, aşağıdaki adımları gerçekleştirin:

    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic793931.png "güven Yönetimi")
   
    a. **Düzenle**’ye tıklayın.

    b. Olarak **yapılandırma türü**seçin **özel**.

    c. Olarak **yerel sağlayıcı adı**, varsayılan değeri bırakın. Bu değeri kopyalayın ve yapıştırın **tanımlayıcı** SAP Cloud Platform için Azure AD yapılandırmasında alan.

    d. Oluşturmak için bir **imzalama anahtarı** ve **imzalama sertifikası** anahtar çifti, tıklayın **anahtar çifti oluşturma**.

    e. Olarak **asıl yayma**seçin **devre dışı bırakılmış**.

    f. Olarak **zorla kimlik doğrulaması**seçin **devre dışı bırakılmış**.

    g. **Kaydet**’e tıklayın.

1. Kaydettikten sonra **yerel hizmet sağlayıcısı** ayarlarını yanıt URL'si almak için aşağıdakileri yapın:
   
    ![Meta verilerini al](./media/sap-hana-cloud-platform-tutorial/ic793930.png "meta verilerini al")

    a. SAP Cloud Platform meta veri dosyası tıklayarak indirin **Get Metadata**.

    b. İndirilen SAP Cloud Platform meta veri XML dosyasını açın ve ardından bulun **ns3:AssertionConsumerService** etiketi.
 
    c. Değerini kopyalayın **konumu** özniteliği ve ardından yapıştırın **yanıt URL'si** SAP Cloud Platform için Azure AD yapılandırmasında alan.

1. Tıklayın **güvenilen kimlik sağlayıcı** sekmesine ve ardından **güvenilen kimlik sağlayıcı Ekle**.
   
    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic790802.png "güven Yönetimi")
   
    >[!NOTE]
    >Güvenilen kimlik sağlayıcıları listesini yönetmek için özel yapılandırma türü yerel hizmet sağlayıcısı bölümünde seçtiğiniz gerekir. Varsayılan yapılandırma türü için bir düzenlenemez ve örtük güven SAP kimliği hizmetine sahip. Hiçbiri için herhangi bir güven ayarı yok.
    > 
    > 

1. Tıklayın **genel** sekmesine ve ardından **Gözat** indirilen meta veri dosyası karşıya yüklemek için.
    
    ![Yönetim güven](./media/sap-hana-cloud-platform-tutorial/ic793932.png "güven Yönetimi")
    
    >[!NOTE]
    >Meta veri dosyası, değerleri karşıya yükledikten sonra **çoklu oturum açma URL'si**, **çoklu oturum kapatma URL'si**, ve **imzalama sertifikası** otomatik olarak doldurulur.
    > 
     
1. **Öznitelikler** sekmesine tıklayın.

1. Üzerinde **öznitelikleri** sekmesinde, aşağıdaki adımı uygulayın:
    
    ![Öznitelikleri](./media/sap-hana-cloud-platform-tutorial/ic790804.png "öznitelikleri") 

    a. Tıklayın **Add Assertion-Based özniteliği**ve ardından aşağıdaki onay tabanlı öznitelikler ekleyin:
       
    | Onaylama işlemi özniteliği | Asıl özniteliği |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |firstName |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Soyadı |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-posta |
   
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

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>SAP Cloud Platform test kullanıcısı oluşturma

SAP Cloud Platform için oturum açmak Azure AD kullanıcılarının etkinleştirmek için SAP Cloud Platform rollerinde kendisine atamanız gerekir.

**Bir kullanıcıya rol atamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **SAP Cloud Platform** Kokpit.

1. Aşağıdakileri gerçekleştirin:
   
    ![Yetkilendirmeleri](./media/sap-hana-cloud-platform-tutorial/ic790805.png "yetkilendirmeleri")
   
    a. Tıklayın **yetkilendirme**.

    b. Tıklayın **kullanıcılar** sekmesi.

    c. İçinde **kullanıcı** metin kutusu, kullanıcının e-posta adresini yazın.

    d. Tıklayın **atama** kullanıcıya bir rol ataması.

    e. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP Cloud Platform için erişimi vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için SAP Cloud Platform atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SAP Cloud Platform**.

    ![Uygulamalar listesinde SAP Cloud Platform bağlantısı](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde SAP Cloud Platform kutucuğa tıkladığınızda, otomatik olarak SAP Cloud Platform uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_203.png

