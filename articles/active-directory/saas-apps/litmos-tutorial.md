---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Litmos | Microsoft Docs'
description: Azure Active Directory ve Litmos arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a0c70ee6419280b0975d77fb213f9406286708cc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428011"
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Öğretici: Azure Active Directory Litmos ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Litmos tümleştirme konusunda bilgi edinin.

Azure AD ile Litmos tümleştirme ile aşağıdaki avantajları sağlar:

- Litmos erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Litmos (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Litmos yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Litmos çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Litmos ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-litmos-from-the-gallery"></a>Galeriden Litmos ekleme
Azure AD'de Litmos tümleştirmesini yapılandırmak için Litmos Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Litmos eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Litmos**seçin **Litmos** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Litmos](./media/litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Litmos sınayın.

Tek iş için oturum açma için Azure AD ne Litmos karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Litmos ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Litmos içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Litmos ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Litmos test kullanıcısı oluşturma](#create-a-litmos-test-user)**  - kullanıcı Azure AD gösterimini bağlı Litmos Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Litmos uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Litmos yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Litmos** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/litmos-tutorial/tutorial_litmos_samlbase.png)

1. Üzerinde **Litmos etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Litmos etki alanı ve URL'ler tek oturum açma bilgileri](./media/litmos-tutorial/tutorial_litmos_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.litmos.com/account/Login`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.litmos.com/integration/samllogin`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcı ve yanıt URL'si, daha sonra öğreticide veya kişi açıklanacak güncelleştirme [Litmos Destek ekibine](https://www.litmos.com/contact-us/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/litmos-tutorial/tutorial_litmos_certificate.png)

1. Özelleştirmeniz gerekiyorsa yapılandırmasının bir parçası olarak **SAML belirteci öznitelikleri** Litmos uygulamanız için.

    ![Öznitelik bölümü](./media/litmos-tutorial/tutorial_attribute.png)
           
    | Öznitelik Adı   | Öznitelik Değeri |   
    | ---------------  | ----------------|
    | FirstName |User.givenName |
    | LastName  |User.surname |
    | Email |User.Mail |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Öznitelik ekleyin](./media/litmos-tutorial/tutorial_attribute_04.png)

    ![Öznitelik Dailog Ekle](./media/litmos-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.     

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/litmos-tutorial/tutorial_general_400.png)

1. Bir farklı bir tarayıcı penceresinde Litmos şirketinizin sitesi için yönetici olarak oturum.

1. Sol taraftaki gezinti çubuğunda **hesapları**.
   
    ![Uygulama tarafında hesaplar bölümü][22] 

1. Tıklayın **tümleştirmeler** sekmesi.
   
    ![Tümleştirme sekmesi][23] 

1. Üzerinde **tümleştirmeler** sekmesinde, aşağı kaydırarak **3. taraf entegrasyonlara**ve ardından **SAML 2.0** sekmesi.
   
    ![SAML 2.0 bölümü][24] 

1. Değerin altında kopyalama **litmos için SAML uç noktası:** yapıştırın **yanıt URL'si** metin kutusunda **Litmos etki alanı ve URL'ler** bölümü Azure Portalı'nda. 
   
    ![SAML uç noktası][26] 

1. İçinde **Litmos** uygulama, aşağıdaki adımları gerçekleştirin:
    
     ![Litmos uygulama][25] 
     
     a. Tıklayın **etkinleştirme SAML**.
    
     b. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **SAML X.509 sertifikası** metin.
     
     c. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/litmos-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/litmos-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/litmos-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/litmos-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-litmos-test-user"></a>Litmos test kullanıcısı oluşturma

Bu bölümün amacı Litmos Britta Simon adlı bir kullanıcı oluşturmaktır.  
Just-ın-Time Litmos uygulamanın desteklediği sağlama. Yani, bir kullanıcı hesabı otomatik olarak gerekirse erişim panelini kullanarak uygulamaya erişim denemesi sırasında oluşturulur.

**Britta Simon Litmos içinde adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Bir farklı bir tarayıcı penceresinde Litmos şirketinizin sitesi için yönetici olarak oturum.

1. Sol taraftaki gezinti çubuğunda **hesapları**.
   
    ![Uygulama tarafında hesaplar bölümü][22] 

1. Tıklayın **tümleştirmeler** sekmesi.
   
    ![Tümleştirmeler sekmesi][23] 

1. Üzerinde **tümleştirmeler** sekmesinde, aşağı kaydırarak **3. taraf entegrasyonlara**ve ardından **SAML 2.0** sekmesi.
   
    ![SAML 2.0][24] 
    
1. Seçin **kullanıcılar otomatik olarak oluştur**
   
    ![Kullanıcıları otomatik olarak oluştur][27] 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Litmos erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Litmos için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Litmos**.

    ![Uygulamalar listesinde Litmos bağlantı](./media/litmos-tutorial/tutorial_litmos_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  

Erişim panelinde Litmos kutucuğa tıkladığınızda, otomatik olarak Litmos uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/litmos-tutorial/tutorial_general_01.png
[2]: ./media/litmos-tutorial/tutorial_general_02.png
[3]: ./media/litmos-tutorial/tutorial_general_03.png
[4]: ./media/litmos-tutorial/tutorial_general_04.png
[21]: ./media/litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/litmos-tutorial/tutorial_general_100.png

[200]: ./media/litmos-tutorial/tutorial_general_200.png
[201]: ./media/litmos-tutorial/tutorial_general_201.png
[202]: ./media/litmos-tutorial/tutorial_general_202.png
[203]: ./media/litmos-tutorial/tutorial_general_203.png

