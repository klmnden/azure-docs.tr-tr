---
title: "Öğretici: Azure Active Directory Tümleştirme ile ServiceNow | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve ServiceNow ve ServiceNow Express arasında yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a91fab90a94b655b93c8ae9064ea4836b80d7678
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Öğretici: ServiceNow Azure Active Directory Tümleştirme
Bu öğreticide, Azure Active Directory (Azure AD) ile ServiceNow ve ServiceNow Express tümleştirmek öğrenin.

ServiceNow ve ServiceNow Express Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* ServiceNow ve ServiceNow Express erişimi, Azure AD'de kontrol edebilirsiniz
* Otomatik olarak ServiceNow ve ServiceNow Express (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
* Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar
Azure AD tümleştirme ServiceNow ve ServiceNow Express ile yapılandırmak için aşağıdaki öğeleri gerekir:

* Bir Azure AD aboneliği
* ServiceNow, örneği veya ServiceNow, Calgary sürüm Kiracı için veya üzeri
* ServiceNow Express, ServiceNow Express, Helsinki sürüm örneği için veya üzeri
* ServiceNow kiracısına sahip olmanız gerekir [birden çok sağlayıcı tek oturum üzerinde eklentisi](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin. Bu yapılabilir [hizmet isteği göndererek](https://hi.service-now.com). 

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.
> 
> 

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

* Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
* Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ServiceNow ekleme
2. Çoklu oturum açmayı ServiceNow veya ServiceNow Express için yapılandırma ve Azure AD sınama

## <a name="adding-servicenow-from-the-gallery"></a>Galeriden ServiceNow ekleme
Azure AD ServiceNow veya ServiceNow Express tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ServiceNow eklemeniz gerekir. 

**ServiceNow Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**. 
   
    ![Active Directory][1]
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamalar][2]
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulamalar][3]
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Uygulamalar][4]
6. Arama kutusuna **ServiceNow**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. Sonuçlar bölmesinde seçin **ServiceNow**ve ardından **tam** uygulama eklemek için.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve ServiceNow veya ServiceNow Express ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ServiceNow karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ServiceNow ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.
Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** ServiceNow içinde. Yapılandırma ve Azure AD çoklu oturum açma ServiceNow ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma için ServiceNow yapılandırma](#configuring-azure-ad-single-sign-on-for-servicenow)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Azure AD çoklu oturum açma için ServiceNow Express yapılandırma](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[ServiceNow test kullanıcısı oluşturma](#creating-a-servicenow-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı ServiceNow sağlamak için.
5. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

> [!NOTE]
> ServiceNow yapılandırmak istiyorsanız, 2. adım atlayın. Benzer şekilde, ServiceNow Express yapılandırmak istiyorsanız, 1. adım atlayın.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure AD çoklu oturum açma ServiceNow için yapılandırma
1. Azure AD Klasik portalında üzerinde **ServiceNow** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırma** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749323.png "çoklu oturum açmayı yapılandırın")

2. Üzerinde **ServiceNow oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749324.png "çoklu oturum açmayı yapılandırın")

3. Üzerinde **uygulama ayarlarını yapılandır** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/IC769497.png "uygulama URL'sini yapılandırın")
   
    a. içinde **üzerinde ServiceNow oturum URL'si** metin kutusuna, türü URL'nizi kullanıcılarınıza oturum açma desen aşağıdaki ServiceNow uygulamanız tarafından kullanılan: `https://<instance-name>.service-now.com`.
   
    b. İçinde **tanımlayıcısı** metin kutusuna, türü URL'nizi kullanıcılarınız tarafından desen aşağıdaki ServiceNow uygulamanıza oturum açma için kullanılan: `https://<instance-name>.service-now.com`.
   
    c. **İleri**’ye tıklayın

4. Azure AD ServiceNow SAML tabanlı kimlik doğrulaması için otomatik olarak yapılandırmak için ServiceNow örneği adı, yönetici kullanıcı adı ve yönetici parolası girin **otomatik yapılandır çoklu oturum açma** form ve tıklayın *yapılandırma*. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanan rolü. Aksi takdirde, Azure AD SAML kimlik sağlayıcısı kullanmak için ServiceNow el ile yapılandırmak için tıklatın **el ile çoklu oturum açma için uygulamayı yapılandırma**, ardından **sonraki** ve aşağıdaki adımları tamamlayın.
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "uygulama URL'sini yapılandırın")

5. Üzerinde **Servicenow'da çoklu oturum açma yapılandırma** sayfasında, **indirme sertifika**, yerel olarak bilgisayarınızda sertifika dosyasını kaydedin.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749325.png "çoklu oturum açmayı yapılandırın")

6. Servicenow'ı uygulamanıza bir yönetici olarak oturum.

7. Etkinleştirme *tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini* sonraki adımları izleyerek Eklentisi:
   
    a. Sol taraftaki gezinti bölmesinde, Git **sistem tanımı** bölümünde ve ardından **eklentileri**.
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "eklentisini etkinleştirme")
   
    b. Arama *tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini*.
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "eklentisini etkinleştirme")
   
    c. Eklentiyi seçin. Rigth tıklayın ve **etkinleştir/yükseltme**.
   
    d. Tıklatın **etkinleştirme** düğmesi.

8. Sol taraftaki gezinti bölmesinde tıklayın **özellikleri**.  
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "uygulama URL'sini yapılandırın")

9. Üzerinde **birden çok sağlayıcı SSO özelliklerini** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "uygulama URL'sini yapılandırın")
   
    a. Olarak **birden çok sağlayıcı SSO etkinleştirme**seçin **Evet**.
   
    b. Olarak **hata ayıklama günlüğünü var birden çok sağlayıcı SSO tümleştirme etkinleştir**seçin **Evet**.
   
    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna, türü **user_name**.
   
    d. **Kaydet** düğmesine tıklayın.

10. Sol taraftaki gezinti bölmesinde tıklayın **x509 sertifikaları**.
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "çoklu oturum açmayı yapılandırın")

11. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, tıklatın **yeni**.
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "çoklu oturum açmayı yapılandırın")

12. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "çoklu oturum açmayı yapılandırın")
    
     a. **Yeni**’ye tıklayın.
    
     b. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).
    
     c. Seçin **etkin**.
    
     d. Olarak **biçimi**seçin **PEM**.
    
     e. Olarak **türü**seçin **deposu sertifika güven**.
    
     f. Base64 ile kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **PEM sertifika** metin kutusu.
    
     g. Tıklatın **güncelleştirme**.

13. Sol taraftaki gezinti bölmesinde tıklayın **kimlik sağlayıcıları**.
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

14. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklatın **yeni**:
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "çoklu oturum açmayı yapılandırın")

15. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklatın **SAML2 Update1?**:
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "çoklu oturum açmayı yapılandırın")

16. SAML2 Update1 Özellikler iletişim kutusunda aşağıdaki adımları gerçekleştirin:
    
     ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "çoklu oturum açmayı yapılandırın")

    a. içinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. İçinde **kullanıcı alanı** metin kutusuna, türü **e-posta** veya **user_name**bağlı olarak hangi alan kullanıcılar ServiceNow dağıtımınızdaki benzersiz şekilde tanımlamak için kullanılır. 

    > [!NOTE] 
    > Azure AD kullanıcı kimliği (kullanıcı asıl adı) ya da e-posta adresini SAML belirtecinde benzersiz tanımlayıcısı olarak giderek yaymak üzere Azure AD configue yapabilecekleriniz **ServiceNow > öznitelikler > çoklu oturum açma** Klasik Azure portalı ve istenen alan eşleme bölümünü **NameIdentifier** özniteliği. Seçili öznitelik için Azure AD (örneğin kullanıcı asıl adı) depolanan değer alanına girilen için (örneğin user_name) ServiceNow içinde depolanan değerle eşleşmelidir

    c. Azure AD Klasik portalında kopyalama **kimlik sağlayıcı kimliği** değer ve ardından yapıştırın **kimlik sağlayıcısı URL'si** metin kutusu.

    d. Azure AD Klasik portalında kopyalama **kimlik doğrulaması istek URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısının AuthnRequest** metin kutusu.

    e. Azure AD Klasik portalında kopyalama **tek Sign-Out hizmeti URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısının SingleLogoutRequest** metin kutusu.

    f. İçinde **ServiceNow giriş sayfası** metin kutusuna, ServiceNow örneği giriş sayfanız URL'sini yazın.

    > [!NOTE] 
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL** ve **/navpage.do** (örn:`https://fabrikam.service-now.com/navpage.do`).

    g. İçinde **varlık kimliği / veren** metin kutusuna, ServiceNow kiracınız URL'sini yazın.

    h. İçinde **İzleyici URL** metin kutusuna, ServiceNow kiracınız URL'sini yazın. 

    ı. İçinde **Protokolü bağlama IDP'ın SingleLogoutRequest için** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.

    j. NameID İlkesi metin kutusuna yazın **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.

    k. Seçimini **bir AuthnContextClass oluşturma**.

    l. İçinde **AuthnContextClassRef yöntemi**, türü `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Bu, yalnızca bulut yalnızca kuruluş ise gereklidir. Kullanıyorsanız, ADFS veya MFA kimlik doğrulaması için bu değeri yapılandırmamalısınız sonra şirket içi. 

    m. İçinde **saat eğriltme** metin kutusuna, türü **60**.

    n. Olarak **üzerinde tek oturum betiği**seçin **MultiSSO_SAML2_Update1**.

    o. Olarak **x509 sertifika**, önceki adımda oluşturduğunuz sertifikayı seçin.

    p. Tıklatın **gönderme**. 

1. Azure AD Klasik portalında tek oturum açma yapılandırması onay seçin ve ardından **sonraki**. 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "çoklu oturum açmayı yapılandırın")

2. Üzerinde **tek oturum açma onay** sayfasında, **tam**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "çoklu oturum açmayı yapılandırın")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD çoklu oturum açma ServiceNow Express için yapılandırma
1. Azure AD Klasik portalında üzerinde **ServiceNow** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırma** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749323.png "çoklu oturum açmayı yapılandırın")

2. Üzerinde **ServiceNow oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749324.png "çoklu oturum açmayı yapılandırın")

3. Üzerinde **uygulama ayarlarını yapılandır** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/IC769497.png "uygulama URL'sini yapılandırın")
   
    a. içinde **üzerinde ServiceNow oturum URL'si** metin kutusuna, türü URL'nizi kullanıcılarınıza oturum açma desen aşağıdaki ServiceNow uygulamanız tarafından kullanılan: `https://<instance-name>.service-now.com`.
   
    b. İçinde **veren URL'si** metin kutusuna, türü URL'nizi kullanıcılarınız tarafından desen aşağıdaki ServiceNow uygulamanıza oturum açma için kullanılan `https://<instance-name>.service-now.com`.
   
    c. **İleri**’ye tıklayın

4. Tıklatın **el ile çoklu oturum açma için uygulamayı yapılandırma**, ardından **sonraki** ve aşağıdaki adımları tamamlayın.
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "uygulama URL'sini yapılandırın")

5. Üzerinde **Servicenow'da çoklu oturum açma yapılandırma** sayfasında, **indirme sertifika**, yerel olarak bilgisayarınızda sertifika dosyasını kaydedin ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC749325.png "çoklu oturum açmayı yapılandırın")

6. ServiceNow Express uygulamanıza bir yönetici olarak oturum.

7. Sol taraftaki gezinti bölmesinde tıklayın **çoklu oturum açma**.  
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "uygulama URL'sini yapılandırın")

8. Üzerinde **çoklu oturum açma** iletişim kutusunda, sağ üst yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "uygulama URL'sini yapılandırın")
   
    a. İki durumlu **birden çok sağlayıcı SSO etkinleştirme** sağındaki.
   
    b. İki durumlu **etkinleştir hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü** sağındaki.
   
    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna, türü **user_name**.
9. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklatın **yeni sertifika Ekle**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "çoklu oturum açmayı yapılandırın")
10. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "çoklu oturum açmayı yapılandırın")
    
    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).
    
    b. Seçin **etkin**.
    
    c. Olarak **biçimi**seçin **PEM**.
    
    d. Olarak **türü**seçin **deposu sertifika güven**.
    
    e. Bir Base64 ile kodlanmış dosyası indirilen sertifikanızı oluşturun.
    
    > [!NOTE]
    > Daha fazla ayrıntı için bkz: [ikili bir sertifika bir metin dosyasına dönüştürmek nasıl](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. Base64 ile kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **PEM sertifika** metin kutusu.
    
    g. Tıklatın **güncelleştirme**.
11. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklatın **ekleme yeni IDP**.
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "çoklu oturum açmayı yapılandırın")
12. Üzerinde **yeni kimlik sağlayıcı Ekle** iletişim altında **yapılandırma kimlik sağlayıcısı**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "çoklu oturum açmayı yapılandırın")

    a. içinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. Azure AD Klasik portalında kopyalama **kimlik sağlayıcı kimliği** değer ve ardından yapıştırın **kimlik sağlayıcısı URL'si** metin kutusu.

    c. Azure AD Klasik portalında kopyalama **kimlik doğrulaması istek URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısının AuthnRequest** metin kutusu.

    d. Azure AD Klasik portalında kopyalama **tek Sign-Out hizmeti URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısının SingleLogoutRequest** metin kutusu.

    e. Olarak **kimlik sağlayıcısı sertifikası**, önceki adımda oluşturduğunuz sertifikayı seçin.


1. Tıklatın **Gelişmiş ayarları**ve altında **ek kimlik sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "çoklu oturum açmayı yapılandırın")
   
    a. İçinde **Protokolü bağlama IDP'ın SingleLogoutRequest için** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.
   
    b. İçinde **NameID İlkesi** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.    
   
    c. İçinde **AuthnContextClassRef yöntemi**, türü **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Seçimini **bir AuthnContextClass oluşturma**.

2. Altında **ek hizmet sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "çoklu oturum açmayı yapılandırın")
   
    a. İçinde **ServiceNow giriş sayfası** metin kutusuna, ServiceNow örneği giriş sayfanız URL'sini yazın.
   
    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL** ve **/navpage.do** (örn: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. İçinde **varlık kimliği / veren** metin kutusuna, ServiceNow kiracınız URL'sini yazın.
   
    c. İçinde **İzleyici URI'si** metin kutusuna, ServiceNow kiracınız URL'sini yazın. 
   
    d. İçinde **saat eğriltme** metin kutusuna, türü **60**.
   
    e. İçinde **kullanıcı alanı** metin kutusuna, türü **e-posta** veya **user_name**bağlı olarak hangi alan kullanıcılar ServiceNow dağıtımınızdaki benzersiz şekilde tanımlamak için kullanılır.
   
    > [!NOTE]
    > Azure AD kullanıcı kimliği (kullanıcı asıl adı) ya da e-posta adresini SAML belirtecinde benzersiz tanımlayıcısı olarak giderek yaymak üzere Azure AD configue yapabilecekleriniz **ServiceNow > öznitelikler > çoklu oturum açma** Klasik Azure portalı ve istenen alan eşleme bölümünü **NameIdentifier** özniteliği. Seçili öznitelik için Azure AD (örneğin kullanıcı asıl adı) depolanan değer alanına girilen için (örneğin user_name) ServiceNow içinde depolanan değerle eşleşmelidir
    > 
    > 
   
    f. **Kaydet** düğmesine tıklayın. 

3. Azure AD Klasik portalında tek oturum açma yapılandırması onay seçin ve ardından **sonraki**. 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "çoklu oturum açmayı yapılandırın")

4. Üzerinde **tek oturum açma onay** sayfasında, **tam**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "çoklu oturum açmayı yapılandırın")

## <a name="configuring-user-provisioning"></a>Kullanıcı hazırlama işleminin yapılandırılması
Bu bölümün amacı, Active Directory kullanıcı hesaplarının ServiceNow kullanıcı sağlamayı etkinleştirme anahat sağlamaktır.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Azure yönetim Klasik Portalı'nda üzerinde **ServiceNow** uygulama tümleştirmesi sayfasında, tıklatın **kullanıcı sağlamayı Yapılandır**. 
   
    ![Kullanıcı sağlamayı](./media/active-directory-saas-servicenow-tutorial/IC769498.png "kullanıcı hazırlama")

2. Üzerinde **otomatik kullanıcı sağlamayı etkinleştirmek için ServiceNow kimlik bilgilerinizi girin** sayfasında, aşağıdaki yapılandırma ayarlarını sağlayın:
   
     a. İçinde **ServiceNow örneği adı** metin kutusuna, ServiceNow örneğinin adını yazın.
   
     b. İçinde **ServiceNow yönetici kullanıcı adı** metin kutusuna, ServiceNow yönetici hesabının adını yazın.
   
     c. İçinde **ServiceNow yönetici parolası** metin kutusuna, bu hesabın parolasını yazın.
   
     d. Tıklatın **doğrulamak** yapılandırmanızı doğrulayın.
   
     e. Tıklatın **sonraki** açmak için düğmeye **sonraki adımlar** sayfası.
   
     f. Bu uygulama için tüm kullanıcılar sağlamak istiyorsanız, seçin "**otomatik olarak bu uygulama dizinindeki tüm kullanıcı hesapları sağlama**". 
   
    ![Sonraki adımlar](./media/active-directory-saas-servicenow-tutorial/IC698804.png "sonraki adımlar")
   
     g. Üzerinde **sonraki adımlar** sayfasında, **tam** yapılandırmanızı kaydetmek için.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, bir test kullanıcı Britta Simon adlı Klasik portalda oluşturun.

![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.
   
    b. Kullanıcı adı **textbox**, türü **BrittaSimon**.
   
    c. **İleri**’ye tıklayın.

6. Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
   ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. İçinde **ad** metin kutusuna, türü **Britta**.  
   
   b. İçinde **Soyadı** metin kutusuna, türü, **Simon**.
   
   c. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
   
   d. İçinde **rol** listesinde **kullanıcı**.
   
   e. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Değerini yazmak **yeni parola**.
   
    b. **Tamamla**’ya tıklayın.   

### <a name="creating-a-servicenow-test-user"></a>ServiceNow test kullanıcısı oluşturma
Bu bölümde, ServiceNow içinde Britta Simon adlı bir kullanıcı oluşturun. Bu bölümde, ServiceNow içinde Britta Simon adlı bir kullanıcı oluşturun. ServiceNow veya ServiceNow Express hesabınızda bir kullanıcı eklemek nasıl bilmiyorsanız, ServiceNow Destek ekibine başvurun.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama
Bu bölümde, Britta ServiceNow kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**ServiceNow Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.
   
    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ServiceNow**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. Üstteki menüde tıklatın **kullanıcılar**.
   
    ![Kullanıcı atama][203] 

4. Tüm kullanıcılar listesinden seçin **Britta Simon**.

5. Araç çubuğunda alt tıklatın **atamak**.
   
    ![Kullanıcı atama][205]

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme
Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli ServiceNow parçasında tıklattığınızda, otomatik olarak ServiceNow uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
