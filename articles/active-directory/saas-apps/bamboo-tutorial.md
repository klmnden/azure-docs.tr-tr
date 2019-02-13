---
title: 'Öğretici: GmbH çözünürlüğün Bamboo için SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Bamboo için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5353cc921d4fc07770737bb70d02361fa0e5f438
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198448"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Bamboo için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Bamboo için SAML SSO tümleştirme konusunda bilgi edinin.

SAML SSO için Bamboo çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO için Bamboo GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanmış için Bamboo için SAML SSO (çoklu oturum açma) GmbH çözünürlüğün Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Bamboo için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- SAML SSO etkin abonelikte GmbH çoklu oturum çözünürlüğün Bamboo için

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Bamboo için SAML SSO GmbH çözünürlüğüyle galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Bamboo için SAML SSO GmbH çözünürlüğüyle galeri ekleme
Bamboo için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için SAML SSO için Bamboo GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Bamboo GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **GmbH çözünürlüğün Bamboo için SAML SSO**seçin **GmbH çözünürlüğün Bamboo için SAML SSO** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

    ![SAML SSO için sonuç listesinde GmbH çözünürlüğün Bamboo](./media/bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bamboo ile GmbH "Britta Simon" adlı bir test kullanıcı tabanlı bir çözüm olarak test edin.

Tek çalışmak için oturum açma için Azure AD hangi karşılık gelen kullanıcı SAML SSO için Bamboo çözünürlüğün GmbH, Azure AD'de bir kullanıcı için olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve SAML SSO Bamboo için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

SAML SSO GmbH çözünürlüğün Bamboo, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Bamboo ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Bamboo oluşturma](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Bamboo için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO için Bamboo GmbH uygulama çözünürlüğüne göre yapılandırın.

**Azure AD çoklu oturum açma SAML SSO için Bamboo ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **GmbH çözünürlüğün Bamboo için SAML SSO** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bamboo-tutorial/tutorial_bamboo_samlbase.png)

1. Üzerinde **Bamboo çözünürlüğün GmbH etki alanı ve URL'ler için SAML SSO** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir SAML SSO için Bamboo çözünürlüğün GmbH etki alanı ve URL'ler](./media/bamboo-tutorial/tutorial_bamboo_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Bamboo çözünürlüğün GmbH etki alanı ve URL'ler](./media/bamboo-tutorial/tutorial_bamboo_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Bamboo çözünürlüğün GmbH istemci için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/bamboo-tutorial/tutorial_bamboo_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/bamboo-tutorial/tutorial_general_400.png)

1. SAML SSO için çözüm GmbH şirket site tarafından Bamboo için yönetici olarak oturum.

1. Ana araç çubuğunun sağ tarafında tıklayın **ayarları** > **eklentileri**.

    ![Ayarları](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. Güvenlik bölümüne gidin, tıklayarak **SAML SingleSignOn** menü çubuğu üzerinde.

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. Üzerinde **SAML SIngleSignOn eklentisi yapılandırma sayfası**, tıklayın **IDP ekleme**. 

    ![IDP Ekle](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısı](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Seçin **IDP türü** olarak **AZURE AD'ye**.

    b. İçinde **adı** metin kutusuna bir ad yazın.

    c. İçinde **açıklama** metin kutusuna bir açıklama yazın.

    d. **İleri**’ye tıklayın.

1. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki**.

    ![Kimlik yapılandırma](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1.  Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, **yük dosyası** karşıya yüklemek için **meta veri XML** Azure portalından indirdiğiniz dosyası.

    ![İdpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. **İleri**’ye tıklayın.

1. **Ayarları kaydet**’e tıklayın.

    ![Kaydetme](./media/bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/bamboo-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/bamboo-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/bamboo-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/bamboo-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Bamboo oluşturma

Bu bölümün amacı, Britta Simon SAML SSO için Bamboo GmbH çözünürlüğüyle adlı bir kullanıcı oluşturmaktır. SAML SSO GmbH çözünürlüğün Bamboo, just-ın-time sağlamayı destekler ve ayrıca kullanıcılar el ile oluşturulabilir başvurun [SAML SSO Bamboo çözünürlüğün GmbH istemci için Destek ekibine](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) ihtiyacınıza göre.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Bamboo için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Bamboo için SAML SSO GmbH çözünürlüğüyle atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **GmbH çözünürlüğün Bamboo için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Bamboo için](./media/bamboo-tutorial/tutorial_bamboo_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözümleme GmbH kutucuk erişim Paneli'nde tarafından Bamboo için SAML SSO'ye tıkladığınızda, otomatik olarak Bamboo için SAML SSO için çözümleme GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-tutorial/tutorial_general_203.png

