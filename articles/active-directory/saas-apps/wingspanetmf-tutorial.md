---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Wingspan eTMF | Microsoft Docs'
description: Azure Active Directory ve Wingspan eTMF arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 1573b7259134037a8e573c6a01588ad782505ea3
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821167"
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a>Öğretici: Wingspan eTMF ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Wingspan eTMF tümleştirme konusunda bilgi edinin.

Azure AD ile Wingspan eTMF tümleştirme ile aşağıdaki avantajları sağlar:

- Wingspan eTMF erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Wingspan eTMF (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Wingspan eTMF yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Wingspan eTMF çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Wingspan eTMF ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-wingspan-etmf-from-the-gallery"></a>Galeriden Wingspan eTMF ekleme
Azure AD'de Wingspan eTMF tümleştirmesini yapılandırmak için Wingspan eTMF Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Wingspan eTMF eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Wingspan eTMF**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

1. Sonuçlar panelinde seçin **Wingspan eTMF**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Wingspan eTMF ile test etme

Tek iş için oturum açma için Azure AD ne Wingspan eTMF karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Wingspan eTMF ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Wingspan eTMF içinde.

Yapılandırma ve Azure AD çoklu oturum açma Wingspan eTMF ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Wingspan eTMF test kullanıcısı oluşturma](#creating-a-wingspan-etmf-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı Wingspan eTMF içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Wingspan eTMF uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Wingspan eTMF yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Wingspan eTMF** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

1. Üzerinde **Wingspan eTMF etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer name>.<instance name>.mywingspan.com/saml`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `http://saml.<instance name>.wingspan.com/shibboleth`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer name>.<instance name>.mywingspan.com/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve gerçek müşteri adı ve örnek adı dahil olmak üzere yanıt URL'si ile güncelleştirin. İlgili kişi [Wingspan eTMF istemci Destek ekibine](http://www.wingspan.com/contact-us/) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/wingspanetmf-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Wingspan eTMF** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Wingspan eTMF Destek](http://www.wingspan.com/contact-us/). Bunlar bu iki tarafı da düzgün ayarlandığından SAML SSO bağlantısı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wingspanetmf-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-wingspan-etmf-test-user"></a>Wingspan eTMF test kullanıcısı oluşturma

Bu bölümde, Britta Simon Wingspan eTMF içinde adlı bir kullanıcı oluşturun. Çalışmak [Wingspan eTMF Destek](http://www.wingspan.com/contact-us/) Wingspan eTMF uygulamada kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Wingspan eTMF erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Wingspan eTMF için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Wingspan eTMF**.

    ![Çoklu oturum açmayı yapılandırın](./media/wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin. 

Erişim panelinde Wingspan eTMF kutucuğa tıklayarak, kuruluşunuzun oturum açma sayfası için yönlendirilirsiniz. Başarıyla oturum açtıktan sonra Wingspan eTMF uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/wingspanetmf-tutorial/tutorial_general_203.png

