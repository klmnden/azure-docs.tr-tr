---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Tangoe komut Premium mobil | Microsoft Docs'
description: Azure Active Directory ve Tangoe komut Premium Mobile arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c9f410fa890df7aac3c3bf4d89468b92e69ba38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60869393"
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Öğretici: Tangoe komut Premium Mobile ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Tangoe komut Premium mobil tümleştirme konusunda bilgi edinin.

Azure AD ile Tangoe komut Premium mobil tümleştirme ile aşağıdaki avantajları sağlar:

- Tangoe komut Premium mobil erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Itanium tabanlı sistemler için Tangoe komut Premium Mobile için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Tangoe komut Premium Mobile ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Tangoe komut Premium mobil çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Tangoe komut Premium mobil Ekle
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a>Galeriden Tangoe komut Premium mobil Ekle
Azure AD'de Tangoe komut Premium Mobile tümleştirmesini yapılandırmak için Tangoe komut Premium mobil Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Tangoe komut Premium mobil eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Tangoe komut Premium mobil**seçin **Tangoe komut Premium mobil** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Galeriden Tangoe komut Premium mobil Ekle](./media/tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tangoe komut Premium "Britta Simon" adlı bir test kullanıcı tabanlı mobil test.

Tek iş için oturum açma için Azure AD ne Tangoe komut Premium mobil karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Tangoe komut Premium Mobile'da arasında bir bağlantı ilişki kurulması gerekir.

Tangoe komut Premium Mobile'da değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tangoe komut Premium Mobile ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Tangoe komut Premium mobil test kullanıcısı oluşturma](#create-a-tangoe-command-premium-mobile-test-user)**  - kullanıcı Azure AD gösterimini bağlı Tangoe komut Premium mobil Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Tangoe komut Premium mobil uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Tangoe komut Premium Mobile ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Tangoe komut Premium mobil** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML Tabanlı Oturum Açma](./media/tangoe-tutorial/tutorial_tangoe_samlbase.png)

1. Üzerinde **Tangoe komut Premium mobil etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Tangoe komut Premium mobil etki alanı ve URL'ler](./media/tangoe-tutorial/tutorial_tangoe_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Tangoe komut Premium mobil istemci Destek ekibine](https://www.tangoe.com/contact-us/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/tangoe-tutorial/tutorial_tangoe_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Kaydet düğmesi](./media/tangoe-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Tangoe komut Premium mobil yapılandırma** bölümünde **yapılandırma Tangoe komut Premium mobil** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Tangoe komut Premium mobil yapılandırma bölümü](./media/tangoe-tutorial/tutorial_tangoe_configure.png) 

1. Uygulamanız için yapılandırılmış SSO almak için iletişime geçin, [Tangoe komut Premium mobil istemci Destek ekibine](https://www.tangoe.com/contact-us/) ve şu bilgileri sağlayın:

   - İndirilen meta veri dosyası
   - **SAML varlık kimliği**
   - **SAML çoklu oturum açma hizmeti URL'si**
   - **Oturum kapatma URL'si**

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tangoe-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Tüm kullanıcılar, kullanıcılar ve Gruplar ->](./media/tangoe-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Kullanıcı ekle](./media/tangoe-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu sayfası](./media/tangoe-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Tangoe komut Premium mobil test kullanıcısı oluşturma

Bu bölümde, Britta Simon Tangoe komut Premium Mobile'da adlı bir kullanıcı oluşturun. 

Tangoe komut Premium mobil uygulama tüm kullanıcıları çoklu oturum açma yapmadan önce uygulamanın sağlanması gerekir. Bu nedenle iş Lütfen ile [Tangoe komut Premium mobil istemci Destek ekibine](https://www.tangoe.com/contact-us/) bu kullanıcılar uygulamaya sağlamak için. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Tangoe komut Premium Mobile için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Tangoe komut Premium mobil atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Tangoe komut Premium mobil**.

    ![Uygulama listesinde Tangoe komut Premium mobil](./media/tangoe-tutorial/tutorial_tangoe_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD SSO yapılandırmanızı erişim panelini kullanarak test edin.

Erişim panelinde Tangoe komut Premium mobil kutucuğa tıkladığınızda, otomatik olarak Tangoe komut Premium mobil uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tangoe-tutorial/tutorial_general_01.png
[2]: ./media/tangoe-tutorial/tutorial_general_02.png
[3]: ./media/tangoe-tutorial/tutorial_general_03.png
[4]: ./media/tangoe-tutorial/tutorial_general_04.png

[100]: ./media/tangoe-tutorial/tutorial_general_100.png

[200]: ./media/tangoe-tutorial/tutorial_general_200.png
[201]: ./media/tangoe-tutorial/tutorial_general_201.png
[202]: ./media/tangoe-tutorial/tutorial_general_202.png
[203]: ./media/tangoe-tutorial/tutorial_general_203.png

