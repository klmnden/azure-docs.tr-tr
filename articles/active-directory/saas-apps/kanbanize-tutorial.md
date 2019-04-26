---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Kanbanize | Microsoft Docs'
description: Azure Active Directory ve Kanbanize arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22c136225e5a8526afd482e5ef8400198947422f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60263637"
---
# <a name="tutorial-azure-active-directory-integration-with-kanbanize"></a>Öğretici: Kanbanize ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Kanbanize tümleştirme konusunda bilgi edinin.

Azure AD ile Kanbanize tümleştirme ile aşağıdaki avantajları sağlar:

- Kanbanize erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Kanbanize (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kanbanize yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Kanbanize çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Kanbanize ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-kanbanize-from-the-gallery"></a>Galeriden Kanbanize ekleme
Azure AD'de Kanbanize tümleştirmesini yapılandırmak için Kanbanize Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kanbanize eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Kanbanize**seçin **Kanbanize** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Kanbanize sınayın.

Tek iş için oturum açma için Azure AD ne Kanbanize karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Kanbanize ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Kanbanize ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kanbanize test kullanıcısı oluşturma](#create-a-kanbanize-test-user)**  - kullanıcı Azure AD gösterimini bağlı Kanbanize Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Kanbanize uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Kanbanize yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kanbanize** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/kanbanize-tutorial/tutorial_kanbanize_samlbase.png)

3. Üzerinde **Kanbanize etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Kanbanize etki alanı ve URL'ler tek oturum açma bilgileri](./media/kanbanize-tutorial/tutorial_kanbanize_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.kanbanize.com/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.kanbanize.com/saml/acs`

    c. Denetleme **Gelişmiş URL ayarlarını göster**.

    d.  İçinde **geçiş durumu** metin kutusuna bir URL: `/ctrl_login/saml_login`

    e. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.kanbanize.com`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Kanbanize istemci Destek ekibine](mailto:support@ms.kanbanize.com) bu değerleri almak için. 

5. Kanbanize uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak Kanbanize bu kullanıcının e-posta adresi ile eşlenmesini bekliyor. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın
    
    ![Çoklu oturum açmayı yapılandırın](./media/kanbanize-tutorial/tutorial_Kanbanize_attribute.png)

6. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/kanbanize-tutorial/tutorial_kanbanize_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/kanbanize-tutorial/tutorial_general_400.png)
    
8. Üzerinde **Kanbanize yapılandırma** bölümünde **yapılandırma Kanbanize** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Kanbanize yapılandırma](./media/kanbanize-tutorial/tutorial_kanbanize_configure.png)

9. Bir başka web tarayıcı penceresinde Kanbanize bir güvenlik yöneticisi olarak oturum açın. 

10. Sayfanın sağ üst, tıklayarak Git **ayarları** logosu.

    ![Kanbanize ayarları](./media/kanbanize-tutorial/tutorial_kanbanize_set.png)

11. Menü sol tarafındaki yönetim paneli sayfasına tıklayın **tümleştirmeler** ve etkinleştirin **çoklu oturum açma**. 

    ![Kanbanize tümleştirmeleri](./media/kanbanize-tutorial/tutorial_kanbanize_admin.png)

12. Tümleştirmeler bölümü altında tıklayarak **yapılandırma** açmak için **tek oturum açma tümleştirmesi** sayfası.

    ![Kanbanize yapılandırma](./media/kanbanize-tutorial/tutorial_kanbanize_config.png)

13. Üzerinde **tek oturum açma tümleştirmesi** altındaki **yapılandırmaları**, aşağıdaki adımları gerçekleştirin:

    ![Kanbanize tümleştirmeleri](./media/kanbanize-tutorial/tutorial_kanbanize_save.png)

    a. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız.

    b. İçinde **Idp'nin oturum açma uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    c. İçinde **IDP oturum kapatma uç noktasına** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    d. İçinde **e-posta için öznitelik adı** metin kutusuna şu değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. İçinde **ad özniteliği adı** metin kutusuna şu değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    f. İçinde **öznitelik adı soyadı** metin kutusuna şu değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` 
    > [!Note]
    > Azure portalındaki kullanıcı öznitelikleri bölümünden ilgili öznitelik ad alanı ve ad değerlerini bir araya getirerek, bu değerleri alabilirsiniz.

    g. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini (olmadan, başlangıç ve bitiş işaretleri) kopyalayın ve ardından yapıştırın **IDP X.509 sertifikası** kutusu.

    h. Denetleme **etkinleştir oturum açma SSO hem Kanbanize**.
    
    i. Tıklayın **ayarlarını kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/kanbanize-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/kanbanize-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/kanbanize-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/kanbanize-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-kanbanize-test-user"></a>Kanbanize test kullanıcısı oluşturma

Bu bölümün amacı Kanbanize Britta Simon adlı bir kullanıcı oluşturmaktır. Kanbanize tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Kanbanize erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Kanbanize istemci Destek ekibine](mailto:support@ms.kanbanize.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Kanbanize erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Kanbanize için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Kanbanize**.

    ![Uygulamalar listesinde Kanbanize bağlantı](./media/kanbanize-tutorial/tutorial_kanbanize_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Kanbanize kutucuğa tıkladığınızda, otomatik olarak Kanbanize uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kanbanize-tutorial/tutorial_general_01.png
[2]: ./media/kanbanize-tutorial/tutorial_general_02.png
[3]: ./media/kanbanize-tutorial/tutorial_general_03.png
[4]: ./media/kanbanize-tutorial/tutorial_general_04.png

[100]: ./media/kanbanize-tutorial/tutorial_general_100.png

[200]: ./media/kanbanize-tutorial/tutorial_general_200.png
[201]: ./media/kanbanize-tutorial/tutorial_general_201.png
[202]: ./media/kanbanize-tutorial/tutorial_general_202.png
[203]: ./media/kanbanize-tutorial/tutorial_general_203.png

