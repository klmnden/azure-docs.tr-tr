---
title: 'Öğretici: Azure Active Directory Tümleştirme ile EthicsPoint Olay yönetimi (EPIM) | Microsoft Docs'
description: Azure Active Directory ve EthicsPoint Olay yönetimi (EPIM) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c72ed655166dc1fe8045f5b9fdc7221cdf24d567
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52851003"
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Öğretici: Azure Active Directory Tümleştirme ile EthicsPoint Olay yönetimi (EPIM)

Bu öğreticide, Azure Active Directory (Azure AD) ile EthicsPoint Olay yönetimi (EPIM) tümleştirme konusunda bilgi edinin.

EthicsPoint Olay yönetimi (EPIM) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini EthicsPoint Olay yönetimi (EPIM) için Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan EthicsPoint Olay yönetimi (EPIM için) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi EthicsPoint Olay yönetimi (EPIM) yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik EthicsPoint Olay yönetimi (EPIM) çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden EthicsPoint Olay yönetimi (EPIM) ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>Galeriden EthicsPoint Olay yönetimi (EPIM) ekleme
Azure AD'de EthicsPoint Olay yönetimi (EPIM) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden EthicsPoint Olay yönetimi (EPIM) eklemeniz gerekir.

**Galeriden EthicsPoint Olay yönetimi (EPIM) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **EthicsPoint Olay yönetimi (EPIM)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

1. Sonuçlar panelinde seçin **EthicsPoint Olay yönetimi (EPIM)** ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile EthicsPoint Olay yönetimi ("Britta Simon." adlı bir test kullanıcı tabanlı EPIM) test etme

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı EthicsPoint Olay yönetimi (EPIM) bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı EthicsPoint Olay yönetimi (EPIM) arasında bir bağlantı ilişki kurulması gerekir.

EthicsPoint Olay yönetimi (EPIM) değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma EthicsPoint Olay yönetimi (EPIM) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[EthicsPoint Olay yönetimi (EPIM) test kullanıcısı oluşturma](#creating-a-ethicspoint-incident-management-epim-test-user)**  - EthicsPoint Olay yönetimi (EPIM) kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve EthicsPoint Olay yönetimi (EPIM) uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma EthicsPoint Olay yönetimi (EPIM) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **EthicsPoint Olay yönetimi (EPIM)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

1. Üzerinde **EthicsPoint Olay yönetimi (EPIM) etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.navexglobal.com/adfs/services/trust`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<servername>.navexglobal.com/adfs/ls/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si, tanımlayıcıya ve oturum açma URL'si ile güncelleştirin. İlgili kişi [EthicsPoint Olay yönetimi (EPIM) istemci Destek ekibine](https://www.navexglobal.com/company/contact-us) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **EthicsPoint Olay yönetimi (EPIM)** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [EthicsPoint Olay yönetimi (EPIM) destek ekibi ](https://www.navexglobal.com/company/contact-us).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a>EthicsPoint Olay yönetimi (EPIM) test kullanıcısı oluşturma

Bu bölümde, Britta Simon EthicsPoint Olay yönetimi (EPIM) adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [EthicsPoint Olay yönetimi (EPIM) destek ekibi](https://www.navexglobal.com/company/contact-us) EthicsPoint Olay yönetimi (EPIM) platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, EthicsPoint Olay yönetimi (EPIM) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon EthicsPoint Olay yönetimi (EPIM) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **EthicsPoint Olay yönetimi (EPIM)**.

    ![Çoklu oturum açmayı yapılandırın](./media/ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Erişim panelinde EthicsPoint Olay yönetimi (EPIM) kutucuğa tıkladığınızda, otomatik olarak EthicsPoint Olay yönetimi (EPIM) uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/ethicspoint-incident-management-tutorial/tutorial_general_203.png

