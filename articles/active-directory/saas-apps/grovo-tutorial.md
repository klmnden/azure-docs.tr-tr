---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Grovo | Microsoft Docs'
description: Azure Active Directory ve Grovo arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 399cecc3-aa62-4914-8b6c-5a35289820c1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: be49cbba53441124bd538a5d82e8c0e1d20d9e45
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433071"
---
# <a name="tutorial-azure-active-directory-integration-with-grovo"></a>Öğretici: Azure Active Directory Grovo ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Grovo tümleştirme konusunda bilgi edinin.

Azure AD ile Grovo tümleştirme ile aşağıdaki avantajları sağlar:

- Grovo erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Grovo (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Grovo yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Grovo çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Grovo ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-grovo-from-the-gallery"></a>Galeriden Grovo ekleme
Azure AD'de Grovo tümleştirmesini yapılandırmak için Grovo Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Grovo eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Grovo**seçin **Grovo** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Grovo](./media/grovo-tutorial/tutorial_grovo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Grovo sınayın.

Tek iş için oturum açma için Azure AD ne Grovo karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Grovo ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Grovo içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Grovo ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Grovo test kullanıcısı oluşturma](#create-a-grovo-test-user)**  - kullanıcı Azure AD gösterimini bağlı Grovo Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Grovo uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Grovo yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Grovo** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/grovo-tutorial/tutorial_grovo_samlbase.png)

1. Üzerinde **Grovo etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Grovo etki alanı ve URL'ler tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.grovo.com/sso/saml2/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

1. Denetleme **Gelişmiş URL ayarlarını göster**, aşağıdaki adımı uygulayın:

    ![Grovo etki alanı ve URL'ler tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url1.png)

    a. İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak:`https://<subdomain>.grovo.com`

    b. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Grovo etki alanı ve URL'ler tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url2.png)
    
    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si, oturum açma URL'si ve geçiş durumunu güncelleştirin. İlgili kişi [Grovo Destek ekibine](https://www.grovo.com/contact-us) bu değerleri almak için.
 
1. Grovo uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Lütfen eşleme **kullanıcı tanımlayıcısı** ile **user.mail** ve aşağıdaki ekran görüntüsünde gösterildiği gibi diğer öznitelikleri yapılandırabilirsiniz.
    
    ![Çoklu oturum açma attb yapılandırın](./media/grovo-tutorial/tutorial_grovo_attribute.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | Ad          | User.givenName |
    | Soyadı           | User.surname |
    | E-posta adresi       | User.Mail    |
    | EmployeeID          | User.employeeid |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/grovo-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/grovo-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.


1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/grovo-tutorial/tutorial_grovo_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/grovo-tutorial/tutorial_general_400.png)

1. Üzerinde **Grovo yapılandırma** bölümünde **yapılandırma Grovo** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_configure.png) 

1. Bir başka web tarayıcı penceresinde Grovo yönetici olarak oturum açın.

1. Git **yönetici** > **tümleştirmeler**.
 
    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_admin.png) 

1. Tıklayın **Ayarla** altında **SP tarafından başlatılan SAML 2.0** bölümü.

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_setup.png)

1. İçinde **SP tarafından başlatılan SAML 2.0** açılan penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_saml.png)

    a. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **çoklu oturum açma hizmet uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Seçin **çoklu oturum açma hizmet uç noktası bağlaması** olarak `urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`.
    
    d. İndirilen açın **Base64 olarak kodlanmış sertifika** Defteri'nde Azure portalından yapıştırın **ortak anahtar** metin.

    e. **İleri**’ye tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/grovo-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/grovo-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/grovo-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/grovo-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-grovo-test-user"></a>Grovo test kullanıcısı oluşturma

Bu bölümün amacı Grovo Britta Simon adlı bir kullanıcı oluşturmaktır. Grovo tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Grovo erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [Grovo Destek ekibine](https://www.grovo.com/contact-us).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Grovo erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Grovo için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Grovo**.

    ![Uygulamalar listesinde Grovo bağlantı](./media/grovo-tutorial/tutorial_grovo_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Grovo kutucuğa tıkladığınızda, otomatik olarak Grovo uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/grovo-tutorial/tutorial_general_01.png
[2]: ./media/grovo-tutorial/tutorial_general_02.png
[3]: ./media/grovo-tutorial/tutorial_general_03.png
[4]: ./media/grovo-tutorial/tutorial_general_04.png

[100]: ./media/grovo-tutorial/tutorial_general_100.png

[200]: ./media/grovo-tutorial/tutorial_general_200.png
[201]: ./media/grovo-tutorial/tutorial_general_201.png
[202]: ./media/grovo-tutorial/tutorial_general_202.png
[203]: ./media/grovo-tutorial/tutorial_general_203.png

