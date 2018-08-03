---
title: 'Öğretici: SAP NetWeaver ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SAP NetWeaver ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b773380a21e0c47a1e1519e592aa0ddd5e6388fa
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421959"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Öğretici: SAP NetWeaver ile Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP NetWeaver tümleştirme konusunda bilgi edinin.

SAP NetWeaver'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP NetWeaver erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) SAP NetWeaver için açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).


## <a name="prerequisites"></a>Önkoşullar

SAP NetWeaver ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir SAP NetWeaver çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP NetWeaver galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sap-netweaver-from-the-gallery"></a>SAP NetWeaver galeri ekleme
Azure AD'de SAP NetWeaver tümleştirmesini yapılandırmak için SAP NetWeaver Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP NetWeaver Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **SAP NetWeaver**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

1. Sonuçlar panelinde seçin **SAP NetWeaver**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP NetWeaver tabanlı "Britta Simon." adlı bir test kullanıcı ile test etme

Tek iş için oturum açma için Azure AD ne SAP NetWeaver karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile SAP NetWeaver ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** SAP NetWeaver içinde.

Yapılandırma ve Azure AD çoklu oturum açma SAP NetWeaver ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAP NetWeaver bir test kullanıcısı oluşturma](#creating-an-sap-netweaver-test-user)**  - kullanıcı Azure AD gösterimini bağlı olduğu SAP NetWeaver Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP NetWeaver uygulamanızda çoklu oturum açmayı yapılandırın.

**SAP NetWeaver ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SAP NetWeaver** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

1. Üzerinde **SAP NetWeaver etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<your company instance of SAP NetWeaver>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<your company instance of SAP NetWeaver>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [SAP NetWeaver istemci Destek ekibine](https://www.sap.com/support.html) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_general_400.png)
    
1. Üzerinde **SAP NetWeaver yapılandırma** bölümünde **yapılandırma SAP NetWeaver** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

1. Çoklu oturum açmayı yapılandırma **SAP NetWeaver** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML varlık kimliği** için [SAPNetWeaverdesteği](https://www.sap.com/support.html). 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-netweaver-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **Britta Simon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** Britta simon'un.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-sap-netweaver-test-user"></a>SAP NetWeaver bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon SAP NetWeaver adlı bir kullanıcı oluşturun. Birlikte çalışmak, [SAP NetWeaver Destek](https://www.sap.com/support.html) SAP NetWeaver platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, SAP NetWeaver için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**SAP NetWeaver Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SAP NetWeaver**.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SAP NetWeaver kutucuğa tıkladığınızda, otomatik olarak SAP NetWeaver uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/sap-netweaver-tutorial/tutorial_general_203.png

