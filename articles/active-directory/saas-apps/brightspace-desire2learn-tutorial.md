---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Brightspace Desire2Learn tarafından | Microsoft Docs'
description: Azure Active Directory ve Desire2Learn tarafından Brightspace arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ca90b042532c44d3e8ce7f569f0d7c1dfaafec87
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55161190"
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Öğretici: Azure Active Directory tarafından Desire2Learn Brightspace ile tümleştirmesi

Bu öğreticide, Brightspace Desire2Learn tarafından Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Brightspace Desire2Learn tarafından Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Brightspace Desire2Learn tarafından erişebilir, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Brightspace için tarafından Desire2Learn açma, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Brightspace Desire2Learn tarafından ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Brightspace tarafından Desire2Learn çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Brightspace Desire2Learn tarafından ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-brightspace-by-desire2learn-from-the-gallery"></a>Galeriden Brightspace Desire2Learn tarafından ekleme
Azure AD'de Desire2Learn tarafından Brightspace tümleştirmesini yapılandırmak için Desire2Learn tarafından Brightspace Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Brightspace Desire2Learn tarafından eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Desire2Learn tarafından Brightspace**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

1. Sonuçlar panelinde seçin **Desire2Learn tarafından Brightspace**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Desire2Learn tarafından Brightspace ile test edin.

Tek iş için oturum açma için Azure AD ne Brightspace Desire2Learn tarafından karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı tarafından Desire2Learn Brightspace ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Desire2Learn tarafından Brightspace içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Brightspace Desire2Learn tarafından ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Desire2Learn test kullanıcı tarafından bir Brightspace oluşturma](#creating-a-brightspace-by-desire2learn-test-user)**  - kullanıcı Azure AD gösterimini bağlı Desire2Learn tarafından Brightspace Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Desire2Learn uygulama tarafından Brightspace içinde yapılandırın.

**Azure AD çoklu oturum açma Brightspace Desire2Learn tarafından ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Brightspace Desire2Learn tarafından** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

1. Üzerinde **Desire2Learn etki alanı ve URL'ler tarafından Brightspace** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Desire2Learn destek ekibi tarafından Brightspace](https://www.d2l.com/contact/) bu değerleri almak için.
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/brightspace-desire2learn-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Desire2Learn tarafından Brightspace** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Desire2Learn destek ekibi tarafından Brightspace](https://www.d2l.com/contact/).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a>Bir Brightspace tarafından Desire2Learn test kullanıcısı oluşturma

Brightspace Desire2Learn tarafından oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Brightspace Desire2Learn tarafından sağlanması gerekir.  

Desire2Learn tarafından Brightspace söz konusu olduğunda, kullanıcı hesapları tarafından oluşturulması gerekir, [Desire2Learn destek ekibi tarafından Brightspace](https://www.d2l.com/contact/).

>[!NOTE]
>Diğer bir Brightspace Desire2Learn kullanıcı hesabı oluşturma araçları tarafından kullanabilir veya API'leri tarafından Desire2Learn Brightspace tarafından sağlama için Azure Active Directory kullanıcı hesaplarını sağlanan. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma için Brightspace Desire2Learn tarafından erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Brightspace Desire2Learn tarafından atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Desire2Learn tarafından Brightspace**.

    ![Çoklu oturum açmayı yapılandırın](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Brightspace tarafından erişim panelinde Desire2Learn kutucuğa tıkladığınızda, size otomatik olarak, Brightspace için Desire2Learn uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/brightspace-desire2learn-tutorial/tutorial_general_203.png

