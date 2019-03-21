---
title: 'Öğretici: SCC yaşam döngüsü ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: SCC yaşam döngüsü ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6376a479d2e6969505310de0e8e73e74738a04c0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57881342"
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Öğretici: SCC yaşam döngüsü ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SCC yaşam döngüsü tümleştirme konusunda bilgi edinin.

SCC yaşam döngüsü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SCC yaşam döngüsü erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için yaşam döngüsü SCC açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SCC yaşam döngüsü ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir SCC yaşam döngüsünün çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [Deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SCC yaşam döngüsü galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-scc-lifecycle-from-the-gallery"></a>SCC yaşam döngüsü galeri ekleme
Azure AD'de SCC yaşam döngüsünün tümleştirmesini yapılandırmak için SCC yaşam döngüsü Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SCC yaşam döngüsü eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **SCC yaşam döngüsü**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/tutorial_scclifecycle_search.png)

1. Sonuçlar panelinde seçin **SCC yaşam döngüsü**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/tutorial_scclifecycle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırmanız ve SCC yaşam döngüsü ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne SCC yaşam döngüsünde karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı SCC yaşam döngüsünde arasında bir bağlantı ilişki kurulması gerekir.

SCC yaşam döngüsünde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SCC yaşam döngüsü ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SCC yaşam döngüsü bir test kullanıcısı oluşturma](#creating-an-scc-lifecycle-test-user)**  - kullanıcı Azure AD gösterimini bağlı SCC ömrü Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SCC yaşam döngüsü uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma SCC yaşam döngüsü ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SCC yaşam döngüsü** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/scclifecycle-tutorial/tutorial_scclifecycle_samlbase.png)

1. Üzerinde **SCC yaşam döngüsü etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/scclifecycle-tutorial/tutorial_scclifecycle_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<sub-domain>.scc.com/ic7/welcome/customer/PICTtest.aspx`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    
    | |
    |--|--|
    | `https://bs1.scc.com/<entity>`|
    | `https://lifecycle.scc.com/<entity>`|
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [SCC yaşam döngüsü istemci Destek ekibine](mailto:lifecycle.support@scc.com) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/scclifecycle-tutorial/tutorial_scclifecycle_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/scclifecycle-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **SCC yaşam döngüsü** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [SCC yaşam döngüsü Destek ekibine](mailto:lifecycle.support@scc.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

   >[!NOTE]
   >Çoklu oturum açma SCC yaşam döngüsü destek ekibi tarafından etkinleştirilmesi gerekir.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/scclifecycle-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-scc-lifecycle-test-user"></a>SCC yaşam döngüsü bir test kullanıcısı oluşturma

SCC yaşam döngüsü açarken Azure AD kullanıcılarının etkinleştirmek için bunlar SCC yaşam döngüsü içinde sağlanması gerekir. SCC yaşam döngüsü için kullanıcı sağlamayı yapılandırmanız için hiçbir eylem öğesini yoktur.

Atanan bir kullanıcı, SCC yaşam döngüsü içinde oturum açmaya çalıştığında, SCC yaşam döngüsü hesabınız yoksa otomatik olarak oluşturulur.

> [!NOTE]
> Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, SCC yaşam döngüsü için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SCC yaşam döngüsü için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamalar.**

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SCC yaşam döngüsü**.

    ![Çoklu oturum açmayı yapılandırın](./media/scclifecycle-tutorial/tutorial_scclifecycle_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SCC yaşam döngüsü kutucuğa tıkladığınızda, otomatik olarak SCC yaşam döngüsü uygulamaya açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/scclifecycle-tutorial/tutorial_general_01.png
[2]: ./media/scclifecycle-tutorial/tutorial_general_02.png
[3]: ./media/scclifecycle-tutorial/tutorial_general_03.png
[4]: ./media/scclifecycle-tutorial/tutorial_general_04.png

[100]: ./media/scclifecycle-tutorial/tutorial_general_100.png

[200]: ./media/scclifecycle-tutorial/tutorial_general_200.png
[201]: ./media/scclifecycle-tutorial/tutorial_general_201.png
[202]: ./media/scclifecycle-tutorial/tutorial_general_202.png
[203]: ./media/scclifecycle-tutorial/tutorial_general_203.png

