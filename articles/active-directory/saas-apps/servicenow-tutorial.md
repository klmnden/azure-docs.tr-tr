---
title: 'Öğretici: Azure Active Directory ServiceNow ile tümleştirme | Microsoft Docs'
description: Azure Active Directory hem de Servicenow'a arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 8aebe6bef536840722d9b07c846687eaf6d195db
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051078"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Öğretici: Azure Active Directory ServiceNow ile tümleştirme

Bu öğreticide, ServiceNow, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

ServiceNow'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ServiceNow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan ServiceNow'ın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi ServiceNow ile yapılandırma için aşağıdakiler gerekir:

- Azure AD aboneliğiniz
- Servicenow'ı, bir örnek veya Kiracı, ServiceNow, Calgary sürümü veya üzeri
- Servicenow'ı hızlı, ServiceNow Express, Helsinki sürümü örneğini veya üzeri
- ServiceNow kiracısına sahip olmanız gerekir [birden çok sağlayıcı tek oturum üzerinde eklentisi](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin. Bunu aşağıdaki yöntemlerle yapabilirsiniz [hizmet isteği göndererek](https://hi.service-now.com).
- Otomatik yapılandırma için için ServiceNow Çoklu Sağlayıcı eklentisini etkinleştirin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. ServiceNow galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow galeri ekleme
Servicenow'ı Azure AD'ye tümleştirmesini yapılandırmak için ServiceNow Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**ServiceNow Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ServiceNow**seçin **ServiceNow** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ServiceNow](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve "Britta Simon" adlı bir test kullanıcı tabanlı servicenow'ı Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne ServiceNow karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili ServiceNow kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

ServiceNow'ın değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ServiceNow ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma için servicenow'ı yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Azure AD çoklu oturum açma için ServiceNow Express yapılandırma](#configure-azure-ad-single-sign-on-for-servicenow-express)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Servicenow'ı test kullanıcısı oluşturma](#create-a-servicenow-test-user)**  - kullanıcı Azure AD gösterimini bağlı ServiceNow Britta simon'un bir karşılığı vardır.
5. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Azure AD çoklu oturum açma için servicenow'ı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ServiceNow uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ServiceNow ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Üzerinde **ServiceNow etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ServiceNow etki alanı ve URL'ler tek oturum açma bilgileri](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve öğreticinin sonraki bölümlerinde açıklanan tanımlayıcısı güncelleştirmeniz gerekir.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin: 

    ![Sertifika indirme bağlantısı](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** bu uygulama Federasyon meta veri URL'si, öğreticinin ilerleyen bölümlerinde kullanılacak şekilde kopyalayıp Not Defteri'ne yapıştırın.

    b. Tıklayın **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/servicenow-tutorial/tutorial_general_400.png)

6. ServiceNow uygulamanızı yönetici olarak oturum açın.

7. Etkinleştirme **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini** sonraki adımları izleyerek Eklentisi:

    a. Sol taraftaki gezinti bölmesinde, arama **sistem tanımı** arama bölümüne ve ardından **eklentileri**.

    ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_03.png "eklentisini etkinleştirme")

     b. Arama **tümleştirmesi - birden çok sağlayıcı tek oturum açma yükleyicisini**.

     ![Eklenti etkinleştirme](./media/servicenow-tutorial/tutorial_servicenow_04.png "eklentisini etkinleştirme")

    c. Eklentiyi seçin. Sağ tıklatın ve seçin **etkinleştir/yükseltme**.

    d. Tıklayın **etkinleştirme** düğmesi.

8. Burada iki yolla **ServiceNow** yapılandırılmış otomatik ve el ile olabilir.

9. Yapılandırma **ServiceNow** izleyin otomatik olarak aşağıdaki adımları

    a. Geri dönüp **ServiceNow** Signle-oturum açma sayfası Azure portalında.

    b. Tek bir tıklamayla hizmeti yapılandırmak için ServiceNow diğer bir deyişle, Azure için AD otomatik olarak yapılandırma ServiceNow SAML tabanlı kimlik doğrulaması için sağlanır. Bu hizmet etkinleştirmek için Git **servicenow'ı yapılandırma** bölümünde **yapılandırma ServiceNow** yapılandırma oturum açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    c. ServiceNow örnek adı, yönetici kullanıcı adı ve yönetici parola girin **yapılandırma oturum açma** tıklayın ve form **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanmış rol. Aksi takdirde, SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde servicenow'ı el ile yapılandırmak için tıklayın **çoklu oturum açmayı el ile yapılandırmanız** ve kopyalama **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** hızlı başvuru bölümünden.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

    d. ServiceNow uygulamanızı yönetici olarak oturum açın.

    e. Gerekli tüm ayarları yapılandırılır otomatik yapılandırmayı **ServiceNow** yan ancak **X.509 sertifikası** varsayılan olarak etkin değildir. El ile kimlik sağlayıcınızda ServiceNow için eşlemeniz gerekir. izleyin aynı için aşağıdaki adımları:
    
    * Sol taraftaki gezinti bölmesinde **kimlik sağlayıcıları** altında **Çoklu Sağlayıcı SSO**.

      ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

    * Otomatik olarak oluşturulan kimlik sağlayıcısına tıklayın

      ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_08.png "çoklu oturum açmayı yapılandırın")

    * Ekranı aşağı kaydırarak **X.509 sertifikası** bölümü. **Düzenle**’yi seçin.

      ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_09.png "çoklu oturum açmayı yapılandırın")
    
    * Sertifikayı seçin ve sertifika eklemek için sağ ok simgesine tıklayın

      ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_11.png "çoklu oturum açmayı yapılandırın")

    * **Kaydet**’e tıklayın.

    * Tıklayarak **etkinleştirme** sayfanın üst sağ köşesindeki.

10. Yapılandırma **ServiceNow** el ile izleyerek aşağıdaki adımları

11. ServiceNow uygulamanızı yönetici olarak oturum açın.

12. Sol taraftaki gezinti bölmesinde, arama **Çoklu Sağlayıcı SSO** arama bölümüne ve ardından **özellikleri**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/tutorial_servicenow_06.png "uygulama URL'sini yapılandırın")

13. Üzerinde **Çoklu Sağlayıcı SSO özelliklerini** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981.png "uygulama URL'sini yapılandırın")

    a. Olarak **Çoklu Sağlayıcı SSO etkinleştirme**seçin **Evet**.

    b. Olarak **kullanıcıların ve kullanıcı tabloya tüm kimlik sağlayıcılardan gelen otomatik içeri aktarma**seçin **Evet**.

    c. Olarak **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü**seçin **Evet**.

    d. İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.

    e. **Kaydet**’e tıklayın.

14. Sol taraftaki gezinti bölmesinde, arama **Çoklu Sağlayıcı SSO** arama bölümüne ve ardından **x509 sertifikaları**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_05.png "çoklu oturum açmayı yapılandırın")

15. Üzerinde **X.509 sertifikaları** iletişim kutusunda, tıklayın **yeni**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694974.png "çoklu oturum açmayı yapılandırın")

16. Üzerinde **X.509 sertifikaları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **Store sertifika güven**.

    e. Not Defteri'nde Azure'dan indirilen, Base64 kodlu sertifika açın, içeriğini, panoya kopyalayın ve ardından yapıştırın **PEM sertifikası** metin.

     f. Tıklayın **gönderme**.

17. Sol taraftaki gezinti bölmesinde **kimlik sağlayıcıları**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/tutorial_servicenow_07.png "çoklu oturum açmayı yapılandırın")

18. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklayın **yeni**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694977.png "çoklu oturum açmayı yapılandırın")

19. Üzerinde **kimlik sağlayıcıları** iletişim kutusunda, tıklayın **SAML2 güncelleştirme 1?**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694978.png "çoklu oturum açmayı yapılandırın")

20. SAML2 güncelleştirme 1 Özellikler iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/idp.png "çoklu oturum açmayı yapılandırın")

    a. Seçin **URL** seçeneğini **kimlik sağlayıcısı meta verileri içeri aktarma** iletişim kutusu.

    b. Girin **uygulama Federasyon meta verileri URL'sini** , Azure Portalı'ndan kopyaladığınız.

    c. **İçeri Aktar**’a tıklayın.

21. IDP meta veri URL'sini okur ve tüm alanları bilgileri doldurur.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin, **SAML 2.0**).
    
    b. Kopyalama **ServiceNow giriş sayfası** değeri, yapıştırın **oturum açma URL'si** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL'si** ve **/navpage.do** (örneğin:`https://fabrikam.service-now.com/navpage.do`).

    c. Kopyalama **varlık kimliği / veren** değeri, yapıştırın **tanımlayıcı** metin kutusunda **ServiceNow etki alanı ve URL'ler** bölümü Azure portalı.

    d. Tıklayın **Gelişmiş**. İçinde **kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**bağlı olarak, hangi alanın ServiceNow Dağıtımınızdaki kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılır.

    > [!NOTE]
    > Azure AD kullanıcı kimlik (kullanıcı asıl adı) veya e-posta adresi SAML belirtecindeki benzersiz tanımlayıcı olarak giderek yaymak için Azure AD'yi yapılandırabilirsiniz **ServiceNow > öznitelikleri > çoklu oturum açma** Azure portal'ın bölümü ve istenen alan için eşleme **NameIdentifier** özniteliği. Seçili öznitelik için (örneğin, kullanıcı asıl adı) Azure AD'de depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değeri eşleşmelidir

    e. Altında **x509 sertifika**, önceki adımda oluşturduğunuz sertifika listeler.

    > [!NOTE]
    > ServiceNow izin vermiyor ıdp'nin etkinleştirme ve test bağlantısı düğmesine tıklayarak olmadan, aynı geçersiz kılmak için lütfen izleyin aşağıdaki adımları.

22. Yapılandırmanın bir parçası olarak oluşturulan, yeni bir kimlik sağlayıcısından gelen ve liste seçin menü simgesine tıklayın **sys_id kopyalayın**

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694992.png "çoklu oturum açmayı yapılandırın")

23. Sol üst arama kutusuna arama **sys_properties.list** ve enter tuşuna basın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694993.png "çoklu oturum açmayı yapılandırın")

24. **Yeni**’ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694994.png "çoklu oturum açmayı yapılandırın")

25. İçinde **sistem özelliği** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694995.png "çoklu oturum açmayı yapılandırın")

    a. Girin `glide.authenticate.sso.redirect.idp` adı metin kutusuna bir değer.

    b. İçinde **değer** metin Kopyala sys_id değerini, önceki adımlarda kopyaladığınız yapıştırın.

    c. Seçin **özel**.

    d. Tıklayın **gönderme**.

26. **Yeni**’ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694994.png "çoklu oturum açmayı yapılandırın")

27. İçinde **sistem özelliği** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694996.png "çoklu oturum açmayı yapılandırın")

    a. Girin `glide.authenticate.multisso.test.connection.mandatory` adı metin kutusuna bir değer.

    b. İçinde **değer** metin girin **false**.

    c. Tıklayın **gönderme**.

28. Bunun yapılması Yukarıdaki adımı sonra artık yeni kimlik sağlayıcınız etkinleştirmeniz mümkün olacaktır ve, SSO çalışması gerekir

> [!NOTE]
> Ayrıca, yeni bir gizli pencerede yeni IDP yapılandırmanızı test etme olduğunu, unutmayın

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD çoklu oturum açma için ServiceNow Express yapılandırın.

1. Azure portalında, üzerinde **ServiceNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

3. Üzerinde **ServiceNow etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<instance-name>.service-now.com/navpage.do`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [ServiceNow istemci Destek ekibine](https://www.servicenow.com/support/contact-support.html) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_general_400.png)

6. Tek bir tıklamayla hizmeti yapılandırmak için ServiceNow diğer bir deyişle, Azure için AD otomatik olarak yapılandırma ServiceNow SAML tabanlı kimlik doğrulaması için sağlanır. Bu hizmet etkinleştirmek için Git **servicenow'ı yapılandırma** bölümünde **yapılandırma ServiceNow** yapılandırma oturum açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. ServiceNow örnek adı, yönetici kullanıcı adı ve yönetici parola girin **yapılandırma oturum açma** tıklayın ve form **Şimdi Yapılandır**. Belirtilen yönetici kullanıcı adı olmalıdır Not **security_admin** çalışması için bu ServiceNow atanmış rol. Aksi takdirde, SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde servicenow'ı el ile yapılandırmak için tıklayın **çoklu oturum açmayı el ile yapılandırmanız** ve kopyalama **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** hızlı başvuru bölümünden.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/configure.png "uygulama URL'sini yapılandırın")

8. ServiceNow Express uygulamanıza yönetici olarak oturum açın.

9. Sol taraftaki gezinti bölmesinde **çoklu oturum açma**.

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694980ex.png "uygulama URL'sini yapılandırın")

10. Üzerinde **çoklu oturum açma** iletişim kutusunda sağ üst köşedeki yapılandırma simgesine tıklayın ve aşağıdaki özellikleri ayarlayın:

    ![Uygulama URL'sini Yapılandır](./media/servicenow-tutorial/ic7694981ex.png "uygulama URL'sini yapılandırın")

    a. İki durumlu **Çoklu Sağlayıcı SSO etkinleştirme** sağ.
    
    b. İki durumlu **etkin hata ayıklama için birden çok sağlayıcı SSO tümleştirme günlüğü** sağ.
    
    c. İçinde **kullanıcı alanı tablo...**  metin kutusuna **user_name**.

11. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **yeni sertifika Ekle**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694973ex.png "çoklu oturum açmayı yapılandırın")

12. Üzerinde **X.509 sertifikaları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694975.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **TestSAML2.0**).

    b. Seçin **etkin**.

    c. Olarak **biçimi**seçin **PEM**.

    d. Olarak **türü**seçin **Store sertifika güven**.

    e. Not Defteri'nde Azure portalından indirilen, Base64 olarak kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın için **PEM sertifikası** metin.

    f. Tıklayın **güncelleştirme**

13. Üzerinde **çoklu oturum açma** iletişim kutusunda, tıklayın **ekleme yeni IDP**.

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694976ex.png "çoklu oturum açmayı yapılandırın")

14. Üzerinde **yeni kimlik sağlayıcısı Ekle** iletişim altında **kimlik sağlayıcısı yapılandırmak**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694982ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: **SAML 2.0**).

    b. İçinde **kimlik sağlayıcısı URL'si** alan, değerini yapıştırın **kimlik sağlayıcı kimliği**, hangi Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **kimlik sağlayıcısının AuthnRequest** alan, değerini yapıştırın **kimlik doğrulaması istek URL**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **kimlik sağlayıcısının SingleLogoutRequest** alan, değerini yapıştırın **çoklu oturum kapatma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız

    e. Olarak **kimlik sağlayıcısı sertifikası**, önceki adımda oluşturduğunuz sertifikayı seçin.

15. Tıklayın **Gelişmiş ayarlar**, altında **ek bir kimlik sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694983ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **Protokolü bağlama IDP'nin SingleLogoutRequest için** metin kutusuna **urn: OASIS: adları: tc: SAML:2.0:bindings:HTTP-yeniden yönlendirme**.

    b. İçinde **Nameıd ilke** metin kutusuna **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: belirtilmeyen**.

    c. İçinde **AuthnContextClassRef yöntemi**, türü `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Seçimini **bir AuthnContextClass oluşturma**.

16. Altında **ek hizmet sağlayıcısı özellikleri**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/servicenow-tutorial/ic7694984ex.png "çoklu oturum açmayı yapılandırın")

    a. İçinde **ServiceNow giriş sayfası** metin, ServiceNow örneği giriş sayfası URL'sini yazın.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, bir yapıdır, **ServieNow Kiracı URL'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do`).

    b. İçinde **varlık kimliği / veren** metin ServiceNow kiracınızın URL'sini yazın.

    c. İçinde **hedef kitle URİ'si** metin ServiceNow kiracınızın URL'sini yazın.

    d. İçinde **saat eğriltme** metin kutusuna **60**.

    e. İçinde **kullanıcı alanı** metin kutusuna **e-posta** veya **user_name**bağlı olarak, hangi alanın ServiceNow Dağıtımınızdaki kullanıcıları benzersiz olarak tanımlanabilmesi için kullanılır.

    > [!NOTE]
    > Azure AD kullanıcı kimlik (kullanıcı asıl adı) veya e-posta adresi SAML belirtecindeki benzersiz tanımlayıcı olarak giderek yaymak için Azure AD'yi yapılandırabilirsiniz **ServiceNow > öznitelikleri > çoklu oturum açma** Azure portal'ın bölümü ve istenen alan için eşleme **NameIdentifier** özniteliği. Seçili öznitelik için (örneğin, kullanıcı asıl adı) Azure AD'de depolanan değer alanına girilen için (örneğin, user_name) ServiceNow içinde depolanan değeri eşleşmelidir

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/servicenow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/servicenow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/servicenow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/servicenow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-servicenow-test-user"></a>Servicenow'ı test kullanıcısı oluşturma

Bu bölümün amacı, ServiceNow Britta Simon adlı bir kullanıcı oluşturmaktır. ServiceNow otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](servicenow-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [ServiceNow istemci destek ekibi](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için ServiceNow erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ServiceNow atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **ServiceNow**.

    ![Uygulamalar listesinde ServiceNow bağlantısı](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ServiceNow kutucuğa tıkladığınızda, otomatik olarak ServiceNow uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](servicenow-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
