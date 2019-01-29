---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Pasifik zaman çizelgesi | Microsoft Docs'
description: Azure Active Directory ve Pasifik zaman çizelgesi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 8e2a5c0091c4dee5ee548b8572e02724c8ce0643
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203632"
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Pasifik zaman çizelgesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Pasifik zaman çizelgesi tümleştirme konusunda bilgi edinin.

Pasifik zaman çizelgesi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pasifik zaman çizelgesi erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için zaman çizelgesi Pasifik açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Pasifik zaman çizelgesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Pasifik zaman çizelgesi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Pasifik zaman çizelgesi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pacific-timesheet-from-the-gallery"></a>Galeriden Pasifik zaman çizelgesi ekleme
Azure AD'de Pasifik zaman çizelgesi tümleştirmesini yapılandırmak için Pasifik zaman çizelgesi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pasifik zaman çizelgesini eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Pasifik zaman çizelgesi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

1. Sonuçlar panelinde seçin **Pasifik zaman çizelgesi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Pasifik "Britta Simon" adlı bir test kullanıcıya dayanarak zaman çizelgesi sınayın.

Tek iş için oturum açma için Azure AD ne Pasifik zaman çizelgesi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Pasifik çizelgesinde arasında bir bağlantı ilişki kurulması gerekir.

Pasifik zaman çizelgesinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pasifik zaman çizelgesi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Pasifik zaman çizelgesi test kullanıcısı oluşturma](#creating-a-pacific-timesheet-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pasifik çizelgesinde Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Pasifik zaman çizelgesi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Pasifik zaman çizelgesini yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Pasifik zaman çizelgesi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

1. Üzerinde **Pasifik zaman çizelgesi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Pasifik zaman çizelgesi Destek ekibine](https://www.pacifictimesheet.com/support) bu değerleri almak için.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_general_400.png)

1. Üzerinde **Pasifik zaman çizelgesi yapılandırma** bölümünde **yapılandırma Pasifik zaman çizelgesi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Pasifik zaman çizelgesi** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **SAML çoklu oturum açma hizmeti URL'si**ve **SAML varlık kimliği** için [Pasifik zaman çizelgesi Destek ekibine](https://www.pacifictimesheet.com/support). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pacific-timesheet-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-pacific-timesheet-test-user"></a>Pasifik zaman çizelgesi test kullanıcısı oluşturma

Bu bölümde, Pasifik zaman çizelgesinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Pasifik zaman çizelgesi Destek ekibine](https://www.pacifictimesheet.com/support) uygulamada bir kullanıcı oluşturun.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Pasifik zaman çizelgesi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Pasifik zaman çizelgesine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Pasifik zaman çizelgesi**.

    ![Çoklu oturum açmayı yapılandırın](./media/pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Pasifik zaman çizelgesi kutucuğa tıkladığınızda, otomatik olarak Pasifik zaman çizelgesi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/pacific-timesheet-tutorial/tutorial_general_203.png

