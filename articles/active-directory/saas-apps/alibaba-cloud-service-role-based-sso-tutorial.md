---
title: 'Öğretici: Alibaba bulut hizmeti (rol tabanlı SSO) ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Alibaba bulut hizmeti (rol tabanlı SSO) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3667841e-acfc-4490-acf5-80d9ca3e71e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdfd19d9a0e928e26ad6f01ba4b9c3f493aacb0c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107159"
---
# <a name="tutorial-azure-active-directory-integration-with-alibaba-cloud-service-role-based-sso"></a>Öğretici: Alibaba bulut hizmeti (rol tabanlı SSO) ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Alibaba bulut hizmeti (rol tabanlı SSO) tümleştirme konusunda bilgi edinin.
Alibaba bulut hizmeti (rol tabanlı SSO) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Alibaba bulut hizmeti (rol tabanlı SSO) erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Alibaba bulut hizmetine (rol tabanlı SSO) oturum açmış, kullanıcıların etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Alibaba bulut hizmeti (rol tabanlı SSO) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Alibaba bulut hizmeti (rol tabanlı SSO) çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Alibaba bulut hizmeti (rol tabanlı SSO) destekler **IDP** tarafından başlatılan

## <a name="adding-alibaba-cloud-service-role-based-sso-from-the-gallery"></a>Galeriden Alibaba bulut hizmeti (rol tabanlı SSO) ekleme

Azure AD'de Alibaba bulut hizmeti (rol tabanlı SSO) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Alibaba bulut hizmeti (rol tabanlı SSO) eklemeniz gerekir.

**Galeriden Alibaba bulut hizmeti (rol tabanlı SSO) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Alibaba bulut hizmeti (rol tabanlı SSO)** seçin **Alibaba bulut hizmeti (rol tabanlı SSO)** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde Alibaba bulut hizmeti (rol tabanlı SSO)](common/search-new-app.png)

5. Üzerinde **Alibaba bulut hizmeti (rol tabanlı SSO)** sayfasında **özellikleri** sol taraftaki gezinti bölmesinde ve kopya **nesne kimliği** ve bilgisayarınızda kaydedin sonraki kullanın.

    ![Özellikleri yapılandırma](./media/alibaba-cloud-service-role-based-sso-tutorial/Properties.png)
    
## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcı Alibaba bulut hizmeti (rol tabanlı SSO) ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Alibaba bulut hizmetinde (rol tabanlı SSO) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Alibaba bulut hizmeti (rol tabanlı SSO) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Alibaba bulut hizmetinde rol tabanlı çoklu oturum açmayı yapılandırma](#configure-role-based-single-sign-on-in-alibaba-cloud-service)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Alibaba bulut hizmeti (rol tabanlı SSO) çoklu oturum açmayı yapılandırma](#configure-alibaba-cloud-service-role-based-sso-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Alibaba bulut hizmeti (rol tabanlı SSO) test kullanıcısı oluşturma](#create-alibaba-cloud-service-role-based-sso-test-user)**  - kullanıcı Azure AD gösterimini bağlı Alibaba bulut hizmetinde (rol tabanlı SSO) Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma (rol tabanlı SSO) ile Alibaba bulut hizmeti yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Alibaba bulut hizmeti (rol tabanlı SSO)** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Bu hizmet sağlayıcısı meta verileri alırsınız [URL'si](https://signin.alibabacloud.com/saml-role/sp-metadata.xml)

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![image](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik Alibaba bulut hizmeti (rol tabanlı SSO) bölümünde metin kutusunda doldurulur:

    ![image](common/idp-intiated.png)

    > [!Note]
    > Varsa **tanımlayıcı** ve **yanıt URL'si** değerlerin değil otomatik doldurulan alın ve ardından değerleri ihtiyacınıza göre el ile girin.

5. Alibaba bulut hizmeti (rol tabanlı SSO) uygulaması, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. Yukarıdaki için ayrıca Alibaba bulut hizmeti (rol tabanlı SSO) uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad | Ad Alanı | Kaynak özniteliği|
    | ---------------| ------------| --------------- |
    | Rol | https:\//www.aliyun.com/SAML-Role/Attribute | User.assignedroles |
    | RoleSessionName | https:\//www.aliyun.com/SAML-Role/Attribute | User.userPrincipalName |

    > [!NOTE]
    > Lütfen tıklayın [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) nasıl yapılandırılacağını öğrenmek için **rol** Azure AD'de

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **Alibaba bulut hizmeti'kurmak (rol tabanlı SSO)** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-role-based-single-sign-on-in-alibaba-cloud-service"></a>Alibaba bulut hizmetinde rol tabanlı çoklu oturum açmayı yapılandırın

1. Alibaba buluta oturum [RAM konsol](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fram.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.8.7d904e167h6Yg9) Firma1 kullanarak.

2. Sol taraftaki gezinti bölmesinde seçin **SSO**.

3. Üzerinde **rol tabanlı SSO** sekmesinde **IDP oluşturma**.

4. Görüntülenen sayfasında girin `AAD` IDP ad alanında bir açıklama girin **Not** alan, tıklayın **karşıya** önce indirdiğiniz Federasyon meta veri dosyasını karşıya yükleyin ve tıklayın **Tamam**.

5. IDP başarıyla oluşturulduktan sonra tıklayın **RAM Rol Oluştur**.

6. İçinde **RAM rol adı** alana `AADrole`seçin `AAD` gelen **IDP seçmek** aşağı açılan listesinde ve Tamam'a tıklayın.

    >[!NOTE]
    >Gerektiğinde rolüne izin verebilirsiniz. Idp'nin ve karşılık gelen rolü oluşturduktan sonra ARNs IDP kullanmak için rol ve kaydetmenizi öneririz. IDP bilgileri sayfası ve rol bilgileri sayfası ARNs elde edebilirsiniz.

7. Alibaba bulut RAM rol (AADrole), Azure AD kullanıcısı (u2) ile ilişkilendirin: Azure AD kullanıcısının bulunduğu RAM rolü ilişkilendirmek için aşağıdaki adımları izleyerek Azure AD'de bir rolü oluşturmalıdır:

    a. Oturum [Azure AD Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer?spm=a2c63.p38356.879954.9.7d904e167h6Yg9).

    b. Tıklayın **değiştirme izinleri** bir rol oluşturmak için gerekli izinleri almak için.

    ![Graph yapılandırma](./media/alibaba-cloud-service-role-based-sso-tutorial/graph01.png)

    c. Aşağıdaki izinleri listeden seçip tıklayın **değiştirme izinlerini**aşağıdaki şekilde gösterildiği gibi.

    ![Graph yapılandırma](./media/alibaba-cloud-service-role-based-sso-tutorial/graph02.png)

    >[!NOTE]
    >İzinler verilene sonra Graph Explorer'a tekrar oturum açın.

    d. Graph Gezgini sayfasında **alma** ilk açılan listeden ve **beta** ikinci aşağı açılan listeden. Enter `https://graph.microsoft.com/beta/servicePrincipals` yanında açılan listeler ve tıklatın alanında **Sorgu Çalıştır**.

    ![Graph yapılandırma](./media/alibaba-cloud-service-role-based-sso-tutorial/graph03.png)

    >[!NOTE]
    >Birden çok dizini kullanıyorsanız, girdiğiniz `https://graph.microsoft.com/beta/contoso.com/servicePrincipals` sorgu alanında.

    e. İçinde **yanıt Önizleme** bölümü, gelen ' hizmet sorumlusu ' kullanmak için appRoles özelliği ayıklayın.

    ![Graph yapılandırma](./media/alibaba-cloud-service-role-based-sso-tutorial/graph05.png)

    >[!NOTE]
    >Girerek appRoles özelliği bulabilirsiniz `https://graph.microsoft.com/beta/servicePrincipals/<objectID>` sorgu alanında. Unutmayın `objectID` Azure AD'den kopyalanıp nesne kimliği **özellikleri** sayfası.

    f. Graph Explorer'a geri dönün, elde edilen yöntemi Değiştir **Al** için **düzeltme eki**, aşağıdaki içeriği yapıştırın **istek gövdesi** bölümünde ve tıklayın **Sorgu Çalıştır** :
    ```
    { 
    "appRoles": [
        { 
        "allowedMemberTypes":[
            "User"
        ],
        "description": "msiam_access",
        "displayName": "msiam_access",
        "id": "41be2db8-48d9-4277-8e86-f6d22d35****",
        "isEnabled": true,
        "origin": "Application",
        "value": null
        },
        { "allowedMemberTypes": [
            "User"
        ],
        "description": "Admin,AzureADProd",
        "displayName": "Admin,AzureADProd",
        "id": "68adae10-8b6b-47e6-9142-6476078cdbce",
        "isEnabled": true,
        "origin": "ServicePrincipal",
        "value": "acs:ram::187125022722****:role/aadrole,acs:ram::187125022722****:saml-provider/AAD"
        }
    ]
    }
    ```
    > [!NOTE]
    > `value` ARNs IDP RAM konsolunda oluşturulan rolü ve olduğu. Burada, gerektiğinde birden çok rol ekleyebilirsiniz. Azure AD, bu rolleri değerini SAML yanıtını talep değeri olarak gönderir. Ancak, yalnızca sonra yeni rolleri ekleyebilirsiniz `msiam_access` düzeltme eki işlemi için bölüm. Oluşturma işlemini düzgün ilerlemesi için gerçek zamanlı olarak kimlikleri oluşturmak için GUID Oluşturucu gibi bir kimliği Oluşturucu kullanmanızı öneririz.

    g. ' Hizmet sorumlusu ' ile gerekli rol düzeltme eki sonra rol (u2) Azure AD kullanıcısının bulunduğu adımları izleyerek ekleme **Azure AD'ye test kullanıcı atama** öğreticinin bölümünde.

### <a name="configure-alibaba-cloud-service-role-based-sso-single-sign-on"></a>Alibaba bulut hizmeti (rol tabanlı SSO) çoklu oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **Alibaba bulut hizmeti (rol tabanlı SSO)** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun URL'ler için Azure portalından [ Alibaba bulut hizmeti (rol tabanlı SSO) Destek ekibine](https://www.aliyun.com/service/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Alibaba bulut hizmetine (rol tabanlı SSO) erişimi vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Alibaba bulut hizmeti (rol tabanlı SSO)** .

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Alibaba bulut hizmeti (rol tabanlı SSO)** .

    ![Alibaba bulut hizmeti (rol tabanlı SSO), uygulamalar listesinde bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Üzerinde **kullanıcılar ve gruplar** sekmesinde u2 kullanıcı listesinden seçin ve tıklayın **seçin**. ' A tıklayarak **atama**.

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test01.png)

6. Atanan role görüntülemek ve test Alibaba bulut hizmeti (rol tabanlı SSO).

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test02.png)

    >[!NOTE]
    >Kullanıcı (u2) atadıktan sonra oluşturulan rol kullanıcıya otomatik olarak eklenir. Birden çok rol oluşturduysanız, gerektiği gibi kullanıcı için uygun rolü eklemek gerekir. Rol tabanlı birden fazla Alibaba bulut hesapları için Azure ad SSO uygulamak istiyorsanız, önceki adımları yineleyin.

### <a name="create-alibaba-cloud-service-role-based-sso-test-user"></a>Alibaba bulut hizmeti (rol tabanlı SSO) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Alibaba bulut hizmeti (rol tabanlı SSO) adlı bir kullanıcı oluşturun. Çalışmak [Alibaba bulut hizmeti (rol tabanlı SSO) Destek ekibine](https://www.aliyun.com/service/) Alibaba bulut hizmeti (rol tabanlı SSO) platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Yukarıdaki yapılandırma tamamlandıktan sonra aşağıdaki adımları izleyerek Alibaba bulut hizmeti (rol tabanlı SSO) test edin:

1. Azure portalında Git **Alibaba bulut hizmeti (rol tabanlı SSO)** sayfasında **çoklu oturum açma**, tıklatıp **Test**.

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test03.png)

2. **Geçerli kullanıcı olarak oturum aç**'a tıklayın.

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test04.png)

3. Hesap seçimi sayfasında u2 seçin.

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test05.png)

4. Rol tabanlı SSO başarılı olduğunu gösteren aşağıdaki sayfa görüntülenir.

    ![Test yapılandırması](./media/alibaba-cloud-service-role-based-sso-tutorial/test06.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

