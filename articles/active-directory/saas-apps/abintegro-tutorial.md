---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Abintegro | Microsoft Docs'
description: Azure Active Directory ve Abintegro arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 99287e1f-4189-494a-97c8-e1c03d047fd3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 91e5b1e3920db0c50760f1bede9e76bd14dd67a1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195996"
---
# <a name="tutorial-azure-active-directory-integration-with-abintegro"></a>Öğretici: Abintegro ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Abintegro tümleştirme konusunda bilgi edinin.

Azure AD ile Abintegro tümleştirme ile aşağıdaki avantajları sağlar:

- Abintegro erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Abintegro (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Abintegro yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Abintegro çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Abintegro ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-abintegro-from-the-gallery"></a>Galeriden Abintegro ekleme
Azure AD'de Abintegro tümleştirmesini yapılandırmak için Abintegro Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Abintegro eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Abintegro**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/tutorial_abintegro_search.png)

5. Sonuçlar panelinde seçin **Abintegro**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/tutorial_abintegro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Abintegro ile test etme

Tek iş için oturum açma için Azure AD ne Abintegro karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Abintegro ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Abintegro içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Abintegro ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Abintegro test kullanıcısı oluşturma](#creating-an-abintegro-test-user)**  - kullanıcı Azure AD gösterimini bağlı Abintegro Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Abintegro uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Abintegro yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Abintegro** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/abintegro-tutorial/tutorial_abintegro_samlbase.png)

3. Üzerinde **Abintegro etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/abintegro-tutorial/tutorial_abintegro_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Abintegro istemci Destek ekibine](mailto:support@abintegro.com) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/abintegro-tutorial/tutorial_abintegro_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/abintegro-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **Abintegro** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Abintegro Destek ekibine](mailto:support@abintegro.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/abintegro-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-abintegro-test-user"></a>Bir Abintegro test kullanıcısı oluşturma

Kullanıcı için Abintegro sağlama yapılandırmanız için hiçbir eylem öğesini yoktur. Atanan kullanıcı Abintegro erişim panelini kullanarak açmaya çalıştığında Abintegro kullanıcının var olup olmadığını denetler.
  
Varsa kullanıcı hesabı kullanılabilir henüz Abintegro tarafından otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Abintegro erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Abintegro için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Abintegro**.

    ![Çoklu oturum açmayı yapılandırın](./media/abintegro-tutorial/tutorial_abintegro_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Abintegro kutucuğa tıkladığınızda, oturum açma sayfası Abintegro uygulamanın almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/abintegro-tutorial/tutorial_general_01.png
[2]: ./media/abintegro-tutorial/tutorial_general_02.png
[3]: ./media/abintegro-tutorial/tutorial_general_03.png
[4]: ./media/abintegro-tutorial/tutorial_general_04.png

[100]: ./media/abintegro-tutorial/tutorial_general_100.png

[200]: ./media/abintegro-tutorial/tutorial_general_200.png
[201]: ./media/abintegro-tutorial/tutorial_general_201.png
[202]: ./media/abintegro-tutorial/tutorial_general_202.png
[203]: ./media/abintegro-tutorial/tutorial_general_203.png

