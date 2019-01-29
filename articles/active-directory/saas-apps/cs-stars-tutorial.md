---
title: 'Öğretici: CS yıldız ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve CS yıldız arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 6f226cab989a1ff2414c53a72a81d66749dfc4ab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153585"
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a>Öğretici: CS yıldız ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile CS yıldız tümleştirme konusunda bilgi edinin.

CS yıldız Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- CS yıldız erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan CS yıldız (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi CS yıldız ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir yıldız CS çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. CS yıldız galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cs-stars-from-the-gallery"></a>CS yıldız galeri ekleme
Azure AD CS yıldız tümleştirilmesi yapılandırmak için CS yıldız Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden CS yıldız eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **CS yıldız**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/tutorial_csstars_search.png)

1. Sonuçlar panelinde seçin **CS yıldız**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma CS ne "Britta Simon." adlı bir test kullanıcı ile test etme

Tek iş için oturum açma için Azure AD CS yıldız karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı CS yıldız arasında bir bağlantı ilişki kurulması gerekir.

CS yıldız değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma CS yıldız ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[CS yıldız test kullanıcısı oluşturma](#creating-a-cs-stars-test-user)**  - kullanıcı Azure AD gösterimini bağlı CS yıldız Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve CS yıldız uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma CS yıldız ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **CS yıldız** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_samlbase.png)

1. Üzerinde **CS yıldız etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.csstars.com/enterprise/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [CS yıldız istemci Destek ekibine](https://www.marshclearsight.com/support/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_general_400.png) 
<CS>
1. Çoklu oturum açmayı yapılandırma **CS yıldız** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [CS yıldız Destek ekibine](https://www.marshclearsight.com/support/). 
<CE>

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cs-stars-test-user"></a>CS yıldız test kullanıcısı oluşturma

Bu bölümün amacı, CS yıldız Britta Simon adlı bir kullanıcı oluşturmaktır.

İletişime geçmeniz CS yıldız oluşturulan bir kullanıcı almak için [CS yıldız Destek ekibine](https://www.marshclearsight.com/support/).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, CS yıldız erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon CS yıldız atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **CS yıldız**.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  
Erişim panelinde CS yıldız kutucuğa tıkladığınızda, otomatik olarak CS yıldız uygulamanıza açan.
 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/cs-stars-tutorial/tutorial_general_203.png

