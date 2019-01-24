---
title: 'Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP Business ByDesign ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 1e3e4440b7d6adf9e5082217fe75b1c2d983a778
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817104"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Öğretici: SAP Business ByDesign ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Business ByDesign tümleştirme konusunda bilgi edinin.

SAP Business ByDesign ile Azure AD tümleştirme ile aşağıdaki avantajları sağlar:

- SAP Business ByDesign erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için SAP Business ByDesign açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Business ByDesign ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir SAP Business ByDesign çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP Business ByDesign galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign galeri ekleme
Azure AD'de SAP Business ByDesign tümleştirmesini yapılandırmak için SAP Business ByDesign Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Business ByDesign Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SAP Business ByDesign**seçin **SAP Business ByDesign** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![SAP Business ByDesign sonuç listesinde](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve "Britta Simon" adlı bir test kullanıcı tabanlı SAP Business ByDesign ile Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne SAP Business ByDesign karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve SAP Business ByDesign ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

SAP Business ByDesign içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP Business ByDesign ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAP Business ByDesign bir test kullanıcısı oluşturma](#create-an-sap-business-bydesign-test-user)**  - kullanıcı Azure AD gösterimini bağlı olan SAP Business ByDesign Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP Business ByDesign uygulamanızda çoklu oturum açmayı yapılandırın.

**SAP Business ByDesign ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SAP Business ByDesign** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

1. Üzerinde **SAP Business ByDesign etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign etki alanı ve URL'ler tek oturum açma bilgileri](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<servername>.sapbydesign.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [SAP Business ByDesign istemci Destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) bu değerleri almak için.

1. Üzerinde **kullanıcı öznitelikleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign özniteliği bölümü](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. İçinde **kullanıcı tanımlayıcısı** listesinden **ExtractMailPrefix()** işlevi.
    
    b. Gelen **posta** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin. Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı kullanmak istediğiniz ve öznitelik değeri içinde ExtensionAttribute2 depoladığınız seçerseniz, user.extensionattribute2 seçin.     

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_general_400.png)

1. Üzerinde **SAP Business ByDesign yapılandırma** bölümünde **yapılandırma SAP Business ByDesign** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SAP Business ByDesign yapılandırma](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

1. Uygulamanız için yapılandırılmış SSO almak için aşağıdaki adımları gerçekleştirin:
   
    a. SAP Business ByDesign portalınız yönetici haklarıyla oturum açın.
   
    b. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.
   
    c. Tıklayın **yeni kimlik sağlayıcısı** ve Azure portalından indirdiğiniz meta veri XML dosyasını seçin. Meta verileri içeri aktararak, sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikasını karşıya yükler.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. Eklenecek **onay belgesi tüketici hizmeti URL'si** SAML isteği seçin **onay belgesi tüketici hizmeti URL'si dahil**.
   
    e. Tıklayın **çoklu oturum açmayı etkinleştirme**.
   
    f. Yaptığınız değişiklikleri kaydedin.
   
    g. Tıklayın **My sistem** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, içine Azure portalından kopyalanan **Azure AD oturum açma URL'si** metin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Kullanıcı kimliği ve parola veya SSO seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçim**.
   
    j. İçinde **SSO URL** bölümünde çalışan oturum açmak için sistem tarafından kullanılması gereken URL'yi belirtin. 
    URL'ye gönderilen çalışan açılan listesi için aşağıdaki seçenekler arasından seçim yapabilirsiniz:
   
    **SSO URL**
   
    Sistem, yalnızca normal sistem URL çalışana gönderir. Çalışan olamaz, SSO kullanarak oturum açın ve parolayı kullanın veya gerekir bunun yerine sertifika.
   
    **SSO URL'Sİ** 
   
    Sistem SSO URL çalışana gönderir. Çalışan SSO kullanarak oturum açabilir. Kimlik doğrulama isteği, IDP yönlendirilir.
   
    **Otomatik Seçim**
   
    SSO etkin değilse, sistemin normal sistem URL çalışana gönderir. SSO etkin olursa, sistem çalışan bir parolaya sahip olup olmadığını denetler. Bir parolası varsa, hem SSO URL hem de olmayan SSO URL çalışana gönderilir. Ancak, çalışan parolası yoksa, yalnızca SSO URL çalışana gönderilir.
   
    k. Yaptığınız değişiklikleri kaydedin.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/sapbusinessbydesign-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/sapbusinessbydesign-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/sapbusinessbydesign-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>SAP Business ByDesign bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon SAP Business ByDesign adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [SAP Business ByDesign istemci Destek ekibine](https://www.sap.com/products/cloud-analytics.support.html) SAP Business ByDesign platform kullanıcıları eklemek için. 

> [!NOTE]
> Nameıd değer SAP Business ByDesign platform kullanıcıadı alanıyla eşleşmelidir emin olun.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SAP Business ByDesign erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için SAP Business ByDesign atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SAP Business ByDesign**.

    ![Uygulamalar listesini SAP Business ByDesign bağlantıdaki](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SAP Business ByDesign kutucuğa tıkladığınızda, otomatik olarak SAP Business ByDesign uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/sapbusinessbydesign-tutorial/tutorial_general_203.png

