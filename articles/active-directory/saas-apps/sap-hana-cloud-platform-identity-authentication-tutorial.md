---
title: 'Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: ea8b6506857a17a2095bfdee4041fd62b7a4b232
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187751"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Öğretici: SAP Cloud Platform kimlik doğrulaması ile Azure Active Directory Tümleştirme

Bu öğreticide, SAP Cloud Platform kimlik doğrulaması Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin. SAP Cloud Platform kimlik doğrulaması, bir proxy olarak IDP ana Idp'si olarak Azure AD'yi kullanan SAP uygulamalara erişmek için kullanılır.

SAP Cloud Platform kimlik doğrulaması, Azure AD ile tümleştirdiğinizde, aşağıdaki avantajlardan yararlanabilirsiniz:

- SAP uygulama erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak SAP uygulamaları Azure AD'ye hesaplarıyla oturum açmak, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Cloud Platform kimlik doğrulaması ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz.
- Tek bir oturum üzerinde etkin olmayan abonelik için SAP Cloud Platform kimlik doğrulaması.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD deneme ortamı yoksa [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Galeriden SAP Cloud Platform kimlik doğrulaması ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

Teknik ayrıntılara girmeden önce bakmak için gideceğinizi kavramları anlamak için önemlidir. SAP Cloud Platform kimlik doğrulaması ve Active Directory Federasyon Hizmetleri, uygulamaları veya Hizmetleri (bir IDP) olarak Azure AD ile SAP uygulamaları tarafından korunan ve SAP bulut tarafından korunan hizmetleri arasında SSO uygulamanızı etkinleştirme Platform kimlik doğrulaması.

Şu anda SAP Cloud Platform kimlik doğrulaması, SAP uygulamaları için bir ara sunucu kimlik sağlayıcısı olarak görev yapar. Azure Active Directory, modunu bu kurulumunda önde gelen kimlik sağlayıcısı olarak görev yapar. 

Aşağıdaki diyagramda, bu ilişkiyi göstermektedir:

![Bir Azure AD test kullanıcısı oluşturma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Bu kurulum ile SAP Cloud Platform kimlik doğrulaması kiracınızın, Azure Active Directory'de güvenilen bir uygulama olarak yapılandırılır. 

Daha sonra tüm SAP uygulamalarını ve bu şekilde korumak istediğiniz hizmetleri SAP Cloud Platform kimlik doğrulaması Yönetimi konsolunda yapılandırılır.

Bu nedenle, SAP uygulama ve hizmetlere erişim izni verme için yetkilendirme SAP Cloud Platform kimlik doğrulaması (aksine, Azure Active Directory) içinde gerçekleşmesi gerekir.

SAP Cloud Platform kimlik doğrulaması Azure Active Directory Marketi üzerinden bir uygulama olarak yapılandırarak, tek bir talep veya SAML onaylamalarını yapılandırmanız gerekmez.

>[!NOTE] 
>Şu anda yalnızca Web SSO her iki taraf tarafından test edilmiştir. Uygulama API veya API iletişim için gerekli olan akışların çalışması gerekir, ancak henüz test edilmemiştir. Sonraki etkinliklerin sırasında sınanır.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>SAP Cloud Platform kimlik doğrulaması Galeriden Ekle
Azure AD'de SAP Cloud Platform kimlik doğrulaması tümleştirmesini yapılandırmak için SAP Cloud Platform kimlik doğrulaması Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SAP Cloud Platform kimlik doğrulaması eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti panelinde seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SAP Cloud Platform kimlik doğrulaması**. 

1. Seçin **SAP Cloud Platform kimlik doğrulaması** sonuçlar paneli ve ardından **Ekle** düğmesi.

    ![Sonuç listesinde SAP Cloud Platform kimlik doğrulaması](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP Cloud Platform kimlik doğrulaması ile test edin. Yapılandırma ve "Britta Simon." adlı bir test kullanıcı ile test

Tek iş için oturum açma için Azure AD, SAP Cloud Platform kimlik doğrulaması karşılığı kullanıcısının kim olduğunu bilmek ister. Diğer bir deyişle, SAP Cloud Platform kimlik doğrulaması bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

SAP Cloud Platform kimlik doğrulaması değeri vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcı arasındaki bağlantı kurduğunuz.

Yapılandırma ve Azure AD çoklu oturum açma SAP Cloud Platform kimlik doğrulaması ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. [SAP Cloud Platform kimlik doğrulaması bir test kullanıcısı oluşturma](#create-an-sap-cloud-platform-identity-authentication-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı SAP Cloud Platform kimlik doğrulaması sağlamak için.
1. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP Cloud Platform kimlik doğrulaması uygulamanızda çoklu oturum açmayı yapılandırın.

**SAP Cloud Platform kimlik doğrulaması ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **SAP Cloud Platform kimlik doğrulaması** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. İçinde **çoklu oturum açma** iletişim kutusunun **SAML tabanlı oturum açma**seçin **modu** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_samlbase.png)

1. Uygulamada yapılandırmak istiyorsanız **IDP** modunda başlatılan **SAP Cloud Platform kimlik kimlik doğrulaması etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:  

    ![SAP Cloud Platform kimlik kimlik doğrulaması etki alanı ve URL'ler tek oturum açma bilgileri](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url.png)

    a. İçinde **tanımlayıcı** kutusuna aşağıdaki desene sahip bir URL yazın: `<IAS-tenant-id>.accounts.ondemand.com`

    b. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SAP Cloud Platform kimlik kimlik doğrulama istemcisi Destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) bu değerleri almak için. Tanımlayıcı değeri anlamıyorsanız, SAP Cloud Platform kimlik doğrulaması belgeleri hakkında okuyun [Kiracı SAML 2.0 Yapılandırması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

1. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, select **Gelişmiş URL ayarlarını göster**.

    ![SAP Cloud Platform kimlik kimlik doğrulaması etki alanı ve URL'ler tek oturum açma bilgileri](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url1.png)

    İçinde **işareti bulunan URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `{YOUR BUSINESS APPLICATION URL}`.

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. Lütfen oturum açma, belirli iş uygulama URL'sini kullanın. İlgili kişi [SAP Cloud Platform kimlik kimlik doğrulama istemcisi Destek ekibine](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) herhangi bir konuda şüpheleriniz varsa.

1. İçinde **SAML imzalama sertifikası** bölümünden **meta veri XML**. Bilgisayarınızda meta verileri dosyayı kaydedin.

    ![Sertifika indirme bağlantısı](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_certificate.png)

1. SAP Cloud Platform kimlik doğrulaması uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu öznitelikleri değerlerini yönetmek **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsünde biçimi örneği gösterilmektedir. 

    ![Çoklu oturum açmayı yapılandırma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/attribute.png)

1. SAP uygulama gibi bir öznitelik bekliyorsa **firstName**, ekleme **firstName** özniteliğini **kullanıcı öznitelikleri** bölümü. Bu seçenek kullanılabilir **çoklu oturum açma** iletişim kutusunun **SAML belirteci öznitelikleri** iletişim kutusu...

    a. Açmak için **öznitelik Ekle** iletişim kutusunda **eklemek agentconfigutil**. 
    
    ![Çoklu oturum açmayı yapılandırma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırma](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** öznitelik adı yazın **firstName**.
    
    c. Gelen **değer** listesinde, öznitelik değeri seçin **user.givenname**.
    
    d. **Tamam**’ı seçin.

1. **Kaydet** düğmesini seçin.

    ![Kaydet düğmesi çoklu oturum açmayı yapılandırın](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_400.png)

1. İçinde **SAP Cloud Platform kimlik kimlik doğrulaması Yapılandırması** bölümünden **SAP Cloud Platform kimlik doğrulaması yapılandırma** açmak için **oturum açmaYapılandırma**penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SAP Cloud Platform kimlik kimlik doğrulaması yapılandırması](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_configure.png) 

1. Uygulamanız için yapılandırılmış SSO edinmek için SAP Cloud Platform kimlik doğrulaması yönetim konsoluna gidin. URL şu desene sahip: `https://<tenant-id>.accounts.ondemand.com/admin`. Ardından SAP Cloud Platform kimlik doğrulaması hakkında belgeleri okuyun [Microsoft Azure AD ile tümleştirme](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

1. Azure portalında **Kaydet** düğmesi.

1. Yalnızca ekleme ve başka bir SAP uygulama için SSO'yu etkinleştirmek isterseniz, aşağıdaki ile devam edin. Bölümünde adımları yineleyin **galerisinden SAP Cloud Platform kimlik doğrulaması ekleme**.

1. Azure portalında, üzerinde **SAP Cloud Platform kimlik doğrulaması** uygulama tümleştirme sayfasında **bağlantılı oturum açma**.

    ![Bağlantılı oturum açmayı yapılandırın](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

1. Yapılandırmayı kaydedin.

>[!NOTE] 
>Yeni uygulama, çoklu oturum açma yapılandırması önceki SAP uygulamasının yararlanır. SAP Cloud Platform kimlik doğrulaması yönetim konsolunda aynı Kurumsal kimlik sağlayıcıları kullandığınızdan emin olun.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekme ve katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>SAP Cloud Platform kimlik doğrulaması bir test kullanıcısı oluşturma

SAP Cloud Platform kimlik doğrulaması bir kullanıcı oluşturmanız gerekmez. Azure AD kullanıcı deposunda olan kullanıcılar, SSO işlevselliğini kullanabilirsiniz.

SAP Cloud Platform kimlik doğrulaması Kimlik Federasyonu seçeneğini destekler. Bu seçenek, Kurumsal kimlik sağlayıcısı tarafından doğrulanır kullanıcılar kullanıcı deposu, SAP Cloud Platform kimlik doğrulaması içinde olup olmadığını denetlemek için uygulamanın sağlar. 

Kimlik Federasyonu seçeneği varsayılan olarak devre dışıdır. Kimlik Federasyonu etkinleştirilirse, SAP Cloud Platform kimlik doğrulaması içeri aktarılan kullanıcılar uygulamaya erişebilir. 

Etkinleştirmek veya SAP Cloud Platform kimlik doğrulaması ile Kimlik Federasyonu devre dışı bırakma hakkında daha fazla bilgi için "Etkinleştirme kimlik Federasyon ile SAP Cloud Platform kimlik doğrulaması" bölümüne bakın. [ile Kimlik Federasyonu yapılandırma Kullanıcı Store, SAP Cloud Platform kimlik doğrulaması](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP Cloud Platform kimlik doğrulaması için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP Cloud Platform kimlik doğrulaması için Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümü açın ve ardından dizin görünümüne gidin. Ardından, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SAP Cloud Platform kimlik doğrulaması**.

    ![Uygulamalar listesinde SAP Cloud Platform kimlik doğrulaması bağlantı](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesine **kullanıcılar ve gruplar** iletişim kutusu.

1. Seçin **atama** düğmesine **atama Ekle** iletişim kutusu.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde SAP Cloud Platform kimlik doğrulaması kutucuğu seçtiğinizde, SAP Cloud Platform kimlik doğrulaması uygulamanıza oturumunuz otomatik olarak.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/sapcloudauth-tutorial/tutorial_general_203.png
