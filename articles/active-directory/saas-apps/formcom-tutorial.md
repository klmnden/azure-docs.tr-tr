---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Form.com | Microsoft Docs'
description: Azure Active Directory ve Form.com arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f1bc0112-315c-4e6f-8c69-7c6873007bcf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: d5748cad11e8e0e4bdfd1000b312ab44fc304059
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55161074"
---
# <a name="tutorial-azure-active-directory-integration-with-formcom"></a>Öğretici: Form.com ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Form.com tümleştirme konusunda bilgi edinin.

Azure AD ile form.com tümleştirme ile aşağıdaki avantajları sağlar:

- Form.com erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Form.com (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Form.com yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Form.com çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden form.com ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-formcom-from-the-gallery"></a>Galeriden form.com ekleme
Azure AD'de Form.com tümleştirmesini yapılandırmak için Form.com Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden form.com eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Form.com**seçin **Form.com** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde form.com](./media/formcom-tutorial/tutorial_form.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Form.com sınayın.

Tek iş için oturum açma için Azure AD ne Form.com karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Form.com ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Form.com içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Form.com ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Form.com test kullanıcısı oluşturma](#create-a-formcom-test-user)**  - kullanıcı Azure AD gösterimini bağlı Form.com Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Form.com uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Form.com yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Form.com** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/formcom-tutorial/tutorial_form.com_samlbase.png)

1. Üzerinde **Form.com etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Form.com etki alanı ve URL'ler tek oturum açma bilgileri](./media/formcom-tutorial/tutorial_form.com_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.wa-form.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.form.com`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<subdomain>.wa-form.com/Member/UserAccount/SAML2.action` |
    | `https://<subdomain>.form.com/Member/UserAccount/SAML2.action` |
    
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si yanıt URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Form.com istemci Destek ekibine](https://form.com/about/company/contact-us/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/formcom-tutorial/tutorial_metadataurl.png)

    a. Kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    b. Tıklayın **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.
     
1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/formcom-tutorial/tutorial_general_400.png)

1. Üzerinde **Form.com yapılandırma** bölümünde **yapılandırma Form.com** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Form.com yapılandırma](./media/formcom-tutorial/tutorial_form.com_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Form.com** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **uygulama Federasyon meta verileri URL'sini**, ve **SAML Çoklu oturum açma hizmeti URL'si** için [Form.com Destek ekibine](https://form.com/about/company/contact-us/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/formcom-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/formcom-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/formcom-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/formcom-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-formcom-test-user"></a>Form.com test kullanıcısı oluşturma

Bu bölümün amacı Form.com Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışmak [Form.com Destek ekibine](https://form.com/about/company/contact-us/) Form.com hesabında kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Form.com erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Form.com için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Form.com**.

    ![Uygulamalar listesinde Form.com bağlantı](./media/formcom-tutorial/tutorial_form.com_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Form.com kutucuğa tıkladığınızda, otomatik olarak Form.com uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/formcom-tutorial/tutorial_general_01.png
[2]: ./media/formcom-tutorial/tutorial_general_02.png
[3]: ./media/formcom-tutorial/tutorial_general_03.png
[4]: ./media/formcom-tutorial/tutorial_general_04.png

[100]: ./media/formcom-tutorial/tutorial_general_100.png

[200]: ./media/formcom-tutorial/tutorial_general_200.png
[201]: ./media/formcom-tutorial/tutorial_general_201.png
[202]: ./media/formcom-tutorial/tutorial_general_202.png
[203]: ./media/formcom-tutorial/tutorial_general_203.png

