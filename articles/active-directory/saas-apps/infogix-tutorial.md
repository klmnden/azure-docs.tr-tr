---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Infogix Data3Sixty yöneten | Microsoft Docs'
description: Azure Active Directory ve Infogix Data3Sixty yöneten arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: 5e9b805786346abd6dfe830c9ce6ae4cc341c9e7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194279"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Öğretici: Infogix Data3Sixty yöneten ile Azure Active Directory Tümleştirme

Bu öğreticide, Infogix Data3Sixty yöneten Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile tümleştirme Infogix Data3Sixty yöneten ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Infogix Data3Sixty yönetmek için Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Infogix Data3Sixty yönetmek için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Infogix Data3Sixty yöneten ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Infogix Data3Sixty yöneten çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Infogix Data3Sixty yöneten ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Galeriden Infogix Data3Sixty yöneten ekleme
Infogix Data3Sixty yöneten tümleştirmesini Azure AD'de yapılandırmak için Infogix Data3Sixty yöneten Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Infogix Data3Sixty yöneten eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Infogix Data3Sixty yöneten**seçin **Infogix Data3Sixty yöneten** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Infogix Data3Sixty yöneten sonuç listesinde](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Data3Sixty Infogix yöneten ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Infogix Data3Sixty yöneten bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili Infogix Data3Sixty belirleyen kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Infogix Data3Sixty yöneten ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Infogix Data3Sixty yöneten bir test kullanıcısı oluşturma](#create-an-infogix-data3sixty-govern-test-user)**  - Infogix Data3Sixty bağlanan kullanıcı Azure AD gösterimini yöneten bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Infogix Data3Sixty yöneten uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Infogix Data3Sixty yöneten ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Infogix Data3Sixty yöneten** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

1. Üzerinde **Infogix Data3Sixty yöneten etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Infogix Data3Sixty yöneten etki alanı ve URL'ler tek oturum açma bilgileri](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://data3sixty.com/ui`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.data3sixty.com/sso/acs`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Infogix Data3Sixty yöneten etki alanı ve URL'ler tek oturum açma bilgileri](./media/infogix-tutorial/tutorial_infogix_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Infogix Data3Sixty yöneten istemci Destek ekibine](mailto:data3sixtysupport@infogix.com) bu değerleri almak için.

1. Uygulama Infogix Data3Sixty yöneten belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açma attb yapılandırın](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | firstName           | User.givenName |
    | Soyadı        | User.surname |
    | kullanıcı adı       | User.Mail    |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/infogix-tutorial/tutorial_infogix_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/infogix-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Infogix Data3Sixty yöneten yapılandırma** bölümünde **Infogix Data3Sixty yöneten yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Infogix Data3Sixty yöneten yapılandırma](./media/infogix-tutorial/tutorial_infogix_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Infogix Data3Sixty yöneten** tarafı, indirilen göndermek için ihtiyacınız **sertifika (ham), oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** için [ Destek ekibine Infogix Data3Sixty yöneten](mailto:data3sixtysupport@infogix.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/infogix-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/infogix-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/infogix-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/infogix-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Infogix Data3Sixty yöneten bir test kullanıcısı oluşturma


Bu bölümün amacı, Britta Simon Infogix Data3Sixty yöneten içinde adlı bir kullanıcı oluşturmaktır. Infogix Data3Sixty yöneten tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Infogix Data3Sixty yöneten erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Infogix Data3Sixty yönetir, Destek ekibine](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, erişim izni verme Infogix Data3Sixty yönetmek için Azure çoklu oturum açmayı kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Infogix Data3Sixty yönetmek için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Infogix Data3Sixty yöneten**.

    ![Uygulamalar listesinde Infogix Data3Sixty yöneten bağlantı](./media/infogix-tutorial/tutorial_infogix_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Infogix Data3Sixty yöneten kutucuğa tıkladığınızda, otomatik olarak Infogix Data3Sixty yöneten uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

