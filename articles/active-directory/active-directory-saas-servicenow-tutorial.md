---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ServiceNow | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve ServiceNow arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: bc5b41da83f183aaf62723212f7197fb50dc536d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Öğretici: ServiceNow Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ServiceNow tümleştirmek öğrenin.

ServiceNow Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ServiceNow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak ServiceNow (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ServiceNow yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- ServiceNow, örneği veya ServiceNow, Calgary sürüm Kiracı için veya üzeri
- ServiceNow Express, ServiceNow Express, Helsinki sürüm örneği için veya üzeri
- ServiceNow kiracısına sahip olmanız gerekir [birden çok sağlayıcı tek oturum üzerinde eklentisi](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin. Bu yapılabilir [hizmet isteği göndererek](https://hi.service-now.com).
- Otomatik yapılandırma, birden çok sağlayıcı eklentisi için ServiceNow etkinleştirin.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ServiceNow ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-servicenow-from-the-gallery"></a>Galeriden ServiceNow ekleme
Azure AD ServiceNow tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ServiceNow eklemeniz gerekir.

**ServiceNow Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ServiceNow**seçin **ServiceNow** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ServiceNow](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ServiceNow sınayın.

Tekli çalışmaya oturum için Azure AD ServiceNow karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ServiceNow ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Servicenow'ı içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ServiceNow ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma için ServiceNow yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Azure AD çoklu oturum açma için ServiceNow Express yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow-express)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[ServiceNow test kullanıcısı oluşturma](#create-a-servicenow-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ServiceNow sağlamak için.
5. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
6. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD çoklu oturum açma ServiceNow için yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ServiceNow uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ServiceNow yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Üzerinde **ServiceNow etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ServiceNow etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra öğreticide açıklandığı tanımlayıcısı güncelleştirmeniz gerekir.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin: 

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve bu uygulama Federasyon meta verileri URL'yi öğreticide daha sonra kullanılacak şekilde Not Defteri'ne yapıştırın.

    b. Tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_general_400.png)

6. ServiceNow uygulamanızı yönetici olarak oturum açma.

7. Etkinleştirme **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini** sonraki adımları izleyerek Eklentisi:

    a. Sol taraftaki gezinti bölmesinde, arama **sistem tanımı** bölümünde arama çubuğunda ve ardından **eklentileri**.

    ![Eklentisini etkinleştirmek](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "eklentisini etkinleştirme")

     b. Arama **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini**.

     ![Eklentisini etkinleştirmek](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "eklentisini etkinleştirme")

    c. Eklentiyi seçin. Sağ tıklayın ve **etkinleştir/yükseltme**.

    d. Tıklatın **etkinleştirme** düğmesi.

8. İki yolla **ServiceNow** yapılandırılmış otomatik ve el ile olabilir.

9. Yapılandırma **ServiceNow** otomatik olarak izleyin aşağıdaki adımları

    a. Geri dönüp **ServiceNow** Signle-oturum açma sayfası Azure portalında.

    b. Tek bir tıklatmayla hizmeti yapılandırmak için SAML tabanlı kimlik doğrulaması ServiceNow ServiceNow için diğer bir deyişle, Azure için AD otomatik olarak yapılandırın sağlanır. Bu hizmeti etkinleştirmek için Git **ServiceNow yapılandırma** 'yi tıklatın **yapılandırma ServiceNow** yapılandırma oturum açma penceresini açın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_configure.png)

    c. ServiceNow örneği adı, yönetici kullanıcı adı ve yönetici parolası girin **yapılandırma oturum açma** form ve tıklayın **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanan rolü. Aksi takdirde, Azure AD SAML kimlik sağlayıcısı kullanmak için ServiceNow el ile yapılandırmak için tıklatın **çoklu oturum açma el ile yapılandırmanız** ve kopyalama **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** hızlı başvuru bölümünden.

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

    d. ServiceNow uygulamanızı yönetici olarak oturum açma.

    e. Gerekli tüm ayarları yapılandırılmış otomatik yapılandırmasında **ServiceNow** yan ancak **X.509 sertifikası** varsayılan olarak etkin değildir. El ile kimlik sağlayıcınız ServiceNow için eşleme gerekir. izleyin aşağıdaki adımları aynı için:
    
    * Sol taraftaki gezinti bölmesinde tıklayın **kimlik sağlayıcıları** altında **çok sağlayıcı SSO**.

      ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

    * Üzerinde otomatik olarak oluşturulan kimlik sağlayıcısı'na tıklayın

      ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_08.png "çoklu oturum açmayı yapılandırın")

    * Ekranı aşağı kaydırarak **X.509 sertifikası** bölümü. **Düzenle**’yi seçin.

      ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_09.png "çoklu oturum açmayı yapılandırın")
    
    * Sertifikayı seçin ve sertifika eklemek için sağ ok simgesine tıklayın

      ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_11.png "çoklu oturum açmayı yapılandırın")

    * **Kaydet**’e tıklayın.

    * Tıklayın **etkinleştirme** sayfanın sağ üst köşesinde adresindeki.

10. Yapılandırma **ServiceNow** el ile izleyin aşağıdaki adımları

11. ServiceNow uygulamanızı yönetici olarak oturum açma.

12. Sol taraftaki gezinti bölmesinde, arama **çok sağlayıcı SSO** bölümünde arama çubuğunda ve ardından **özellikleri**.

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "uygulama URL'sini yapılandırın")

13. Üzerinde **birden çok sağlayıcı SSO özelliklerini** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/ic7694981.png "uygulama URL'sini yapılandırın")

    a. Olarak **birden çok sağlayıcı SSO etkinleştirme**seçin **Evet**.

    b. Olarak **kullanıcı tabloya tüm kimlik sağlayıcılardan gelen kullanıcıların otomatik içeri aktarma**seçin **Evet**.

    c. Olarak **etkinleştir hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü**seçin **Evet**.

    d. İçinde **kullanıcı alanı tablo...**  metin kutusuna, türü **user_name**.

    e. **Kaydet**’e tıklayın.

14. Sol taraftaki gezinti bölmesinde, arama **çok sağlayıcı SSO** bölümünde arama çubuğunda ve ardından **x509 sertifikaları**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "çoklu oturum açmayı yapılandırın")

15. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, tıklatın **yeni**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694974.png "çoklu oturum açmayı yapılandırın")

16. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **deposu sertifika güven**.

    e. Not Defteri'nde Azure'dan indirilen Base64 ile kodlanmış sertifikanızı açın, içeriğini, panoya kopyalayın ve ardından yapıştırın **PEM sertifika** metin kutusu.

     f. Tıklatın **gönderme**.

17. Sol taraftaki gezinti bölmesinde tıklayın **kimlik sağlayıcıları**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

18. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklatın **yeni**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694977.png "çoklu oturum açmayı yapılandırın")

19. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklatın **SAML2 Update1?**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694978.png "çoklu oturum açmayı yapılandırın")

20. SAML2 Update1 Özellikler iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/idp.png "çoklu oturum açmayı yapılandırın")

    a. Seçin **URL** seçeneğini **kimlik sağlayıcısı meta verileri içeri aktarma** iletişim kutusu.

    b. Girin **uygulama Federasyon meta veri URL'sini** Azure portalından kopyalanan.

    c. **İçeri Aktar**’a tıklayın.

21. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694982.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin, **SAML 2.0**).
    
    b. Kopya **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusuna **ServiceNow etki alanı ve URL'leri** Azure Portal'daki bölümü.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

    c. Kopya **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcısı** metin kutusuna **ServiceNow etki alanı ve URL'leri** Azure Portal'daki bölüm.

    d. Tıklatın **Gelişmiş**. İçinde **kullanıcı alanı** metin kutusuna, türü **e-posta** veya **user_name**bağlı olarak hangi alan kullanıcılar ServiceNow dağıtımınızdaki benzersiz şekilde tanımlamak için kullanılır.

    > [!NOTE]
    > Azure AD kullanıcı kimliği (kullanıcı asıl adı) ya da e-posta adresini SAML belirtecinde benzersiz tanımlayıcısı olarak giderek yaymak üzere Azure AD yapılandırabilirsiniz **ServiceNow > öznitelikler > çoklu oturum açma** Azure portalının bölümü ve istenen alan eşleme **NameIdentifier** özniteliği. Seçili öznitelik için Azure AD (örneğin, kullanıcı asıl adı) depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değerle eşleşmelidir

     e. Altında **x509 sertifika**, önceki adımda oluşturduğunuz sertifika listeler.

     > [!NOTE]
     > ServiceNow izin vermiyor IDP etkinleştirme test bağlantısı düğmesini tıklatarak olmadan, aynı geçersiz kılmak için lütfen izleyin aşağıdaki adımları.

22. Listesinde öğesini seçin ve yapılandırmasının bir parçası oluşturulan yeni kimlik sağlayıcınızı önbellekten menüsü simgesini tıklatın **sys_id kopyalama**

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694992.png "çoklu oturum açmayı yapılandırın")

23. Üst sol arama kutusuna arama **sys_properties.list** ve ENTER tuşuna basın.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694993.png "çoklu oturum açmayı yapılandırın")

24. **Yeni**’ye tıklayın.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694994.png "çoklu oturum açmayı yapılandırın")

25. İçinde **sistem özelliği** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694995.png "çoklu oturum açmayı yapılandırın")

    a. Girin `glide.authenticate.sso.redirect.idp` adı metin kutusuna değeri.

    b. İçinde **değeri** metin kutusuna, kopya sys_id değerini, önceki adımda kopyaladığınız Yapıştır.

    c. Seçin **özel**.

    d. Tıklatın **gönderme**.

26. **Yeni**’ye tıklayın.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694994.png "çoklu oturum açmayı yapılandırın")

27. İçinde **sistem özelliği** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694996.png "çoklu oturum açmayı yapılandırın")

    a. Girin `glide.authenticate.multisso.test.connection.mandatory` adı metin kutusuna değeri.

    b. İçinde **değeri** metin girin **false**.

    c. Tıklatın **gönderme**.

28. Bunu yaptığınızda Yukarıdaki adımı sonra artık yeni kimlik sağlayıcınızı etkinleştirmeniz mümkün olacaktır ve, SSO çalışması gerekir

> [!NOTE]
> Ayrıca Lütfen yeni bir incognito penceresi yeni IDP yapılandırmanızı test, sahip unutmayın

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD çoklu oturum açma ServiceNow Express için yapılandırma

1. Azure portalında üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Üzerinde **ServiceNow etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [ServiceNow istemci destek ekibi](https://www.servicenow.com/support/contact-support.html) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_certificates.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_general_400.png)

6. Tek bir tıklatmayla hizmeti yapılandırmak için SAML tabanlı kimlik doğrulaması ServiceNow ServiceNow için diğer bir deyişle, Azure için AD otomatik olarak yapılandırın sağlanır. Bu hizmeti etkinleştirmek için Git **ServiceNow yapılandırma** 'yi tıklatın **yapılandırma ServiceNow** yapılandırma oturum açma penceresini açın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_configure.png)

7. ServiceNow örneği adı, yönetici kullanıcı adı ve yönetici parolası girin **yapılandırma oturum açma** form ve tıklayın **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanan rolü. Aksi takdirde, Azure AD SAML kimlik sağlayıcısı kullanmak için ServiceNow el ile yapılandırmak için tıklatın **çoklu oturum açma el ile yapılandırmanız** ve kopyalama **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** hızlı başvuru bölümünden.

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

8. ServiceNow Express uygulamanızı yönetici olarak oturum açma.

9. Sol taraftaki gezinti bölmesinde tıklayın **çoklu oturum açma**.

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "uygulama URL'sini yapılandırın")

10. Üzerinde **çoklu oturum açma** iletişim kutusunda, sağ üst yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:

    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "uygulama URL'sini yapılandırın")

    a. İki durumlu **birden çok sağlayıcı SSO etkinleştirme** sağındaki.
    
    b. İki durumlu **etkinleştir hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü** sağındaki.
    
    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna, türü **user_name**.

11. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklatın **yeni sertifika Ekle**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "çoklu oturum açmayı yapılandırın")

12. Üzerinde **X.509 sertifikalarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **deposu sertifika güven**.

    e. Not Defteri'nde Azure portalından indirdiğiniz Base64 ile kodlanmış sertifikanızı açın, içeriğini, panoya kopyalayın ve ardından yapıştırın **PEM sertifika** metin kutusu.

    f. Tıklatın **güncelleştirme**

13. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklatın **ekleme yeni IDP**.

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "çoklu oturum açmayı yapılandırın")

14. Üzerinde **yeni kimlik sağlayıcı Ekle** iletişim altında **yapılandırma kimlik sağlayıcısı**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. İçinde **kimlik sağlayıcısı URL'si** alan, değerini yapıştırın **kimlik sağlayıcı kimliği**, Azure portalından kopyalanan.
    
    c. İçinde **kimlik sağlayıcısının AuthnRequest** alan, değerini yapıştırın **kimlik doğrulaması istek URL'si**, hangi Azure portalından kopyalanır.

    d. İçinde **kimlik sağlayıcısının SingleLogoutRequest** alan, değerini yapıştırın **tek Sign-Out hizmeti URL'si**, Azure portalından kopyalanan

    e. Olarak **kimlik sağlayıcısı sertifikası**, önceki adımda oluşturduğunuz sertifikayı seçin.

15. Tıklatın **Gelişmiş ayarları**ve altında **ek kimlik sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **Protokolü bağlama IDP'ın SingleLogoutRequest için** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.

    b. İçinde **NameID İlkesi** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.

    c. İçinde **AuthnContextClassRef yöntemi**, türü `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Seçimini **bir AuthnContextClass oluşturma**.

16. Altında **ek hizmet sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **ServiceNow giriş sayfası** metin kutusuna, ServiceNow örneği giriş sayfanız URL'sini yazın.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do`).

    b. İçinde **varlık kimliği / veren** metin kutusuna, ServiceNow kiracınız URL'sini yazın.

    c. İçinde **İzleyici URI'si** metin kutusuna, ServiceNow kiracınız URL'sini yazın.

    d. İçinde **saat eğriltme** metin kutusuna, türü **60**.

    e. İçinde **kullanıcı alanı** metin kutusuna, türü **e-posta** veya **user_name**bağlı olarak hangi alan kullanıcılar ServiceNow dağıtımınızdaki benzersiz şekilde tanımlamak için kullanılır.

    > [!NOTE]
    > Azure AD kullanıcı kimliği (kullanıcı asıl adı) ya da e-posta adresini SAML belirtecinde benzersiz tanımlayıcısı olarak giderek yaymak üzere Azure AD yapılandırabilirsiniz **ServiceNow > öznitelikler > çoklu oturum açma** Azure portalının bölümü ve istenen alan eşleme **NameIdentifier** özniteliği. Seçili öznitelik için Azure AD (örneğin, kullanıcı asıl adı) depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değerle eşleşmelidir

    f. **Kaydet**’e tıklayın.
 
### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-servicenow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-servicenow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-servicenow-test-user"></a>ServiceNow test kullanıcısı oluşturma

Bu bölümde, ServiceNow içinde Britta Simon adlı bir kullanıcı oluşturun. ServiceNow veya ServiceNow Express hesabınızda bir kullanıcı eklemek nasıl bilmiyorsanız başvurun [ServiceNow istemci destek ekibi](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ServiceNow erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**ServiceNow Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ServiceNow**.

    ![Uygulamalar listesinde ServiceNow bağlantı](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ServiceNow parçasında tıklattığınızda, otomatik olarak ServiceNow uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png

