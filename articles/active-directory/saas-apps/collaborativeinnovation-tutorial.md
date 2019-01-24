---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile işbirliğine dayalı yenilik | Microsoft Docs'
description: Azure Active Directory ve işbirliğine dayalı yenilik arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 402b8be8ea3a569679baa833ef52fef9769bac30
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819569"
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a>Öğretici: Azure Active Directory Tümleştirmesi ile işbirliğine dayalı yenilik

Bu öğreticide, Azure Active Directory (Azure AD) ile işbirliğine dayalı yenilik tümleştirme konusunda bilgi edinin.

Azure AD ile işbirliğine dayalı yenilik tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi işbirliğine dayalı bir yenilik olan Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için işbirliğine dayalı yenilik açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile işbirliğine dayalı yenilik yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir işbirliğine dayalı yenilik çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden işbirliğine dayalı yenilik ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-collaborative-innovation-from-the-gallery"></a>Galeriden işbirliğine dayalı yenilik ekleme
Azure AD'de işbirliğine dayalı yenilik tümleştirmesini yapılandırmak için işbirliğine dayalı yenilik Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden işbirliğine dayalı yenilik eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **işbirliğine dayalı yenilik**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

1. Sonuçlar panelinde seçin **işbirliğine dayalı yenilik**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile işbirliğine dayalı "Britta Simon." adlı bir test kullanıcı tabanlı yenilik test etme

Tek iş için oturum açma için Azure AD ne işbirliğine dayalı yenilik karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile işbirliğine dayalı yenilik ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

İşbirliğine dayalı yeniliğe değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile işbirliğine dayalı yenilik sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[İşbirliğine dayalı yenilik test kullanıcısı oluşturma](#creating-a-collaborative-innovation-test-user)**  - kullanıcı Azure AD gösterimini bağlı işbirliğine dayalı yenilik Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve işbirliğine dayalı yenilik uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile işbirliğine dayalı yenilik yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **işbirliğine dayalı yenilik** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

1. Üzerinde **işbirliğine dayalı yenilik etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instancename>.foundry.<companyname>.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<instancename>.foundry.<companyname>.com`
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [işbirliğine dayalı yenilik istemci Destek ekibine](https://www.unilever.com/contact/) bu değerleri almak için.  

1. İşbirliğine dayalı yenilik uygulama belirli bir biçimde SAML onaylamalarını bekler. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/attribute.png)
    
1. Tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu **kullanıcı öznitelikleri** bölüm öznitelikleri genişletin. Her birini görüntülenen öznitelikleri - aşağıdaki adımları gerçekleştirin

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | givenName | User.givenName |
    | Soyadı | User.surname |
    | emailaddress | User.userPrincipalName |
    | ad | User.userPrincipalName |

    a. Öznitelik açmak için tıklayın **özniteliğini Düzenle** penceresi.

    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/url_update.png)

    b. URL'si değerini Sil **Namespace**.
    
    c. Tıklayın **Tamam** ayarı kaydedilemiyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **işbirliğine dayalı yenilik** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [işbirliğine dayalı yenilik Destek ekibine](https://www.unilever.com/contact/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/collaborativeinnovation-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-collaborative-innovation-test-user"></a>İşbirliğine dayalı yenilik test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açmak için işbirliğine dayalı yenilik sağlamak için bunların işbirliğine dayalı İnovasyon sağlanması gerekir.  

Yalnızca zaman sağlama kullanıcı uygulamanın desteklediği gibi olması durumunda bu uygulamayı otomatik olarak yapılır ve sağlama. Bu nedenle burada herhangi bir adımı gerçekleştirmek için gerek yoktur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma erişimi işbirliğine dayalı yenilik vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon işbirliğine dayalı yenilik atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **işbirliğine dayalı yenilik**.

    ![Çoklu oturum açmayı yapılandırın](./media/collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli işbirliğine dayalı yenilik kutucuğa tıkladığınızda, işbirliğine dayalı yenilik uygulamanın oturum açma sayfası almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/collaborativeinnovation-tutorial/tutorial_general_203.png

