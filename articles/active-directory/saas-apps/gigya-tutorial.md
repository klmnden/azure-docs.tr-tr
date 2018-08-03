---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Gigya | Microsoft Docs'
description: Azure Active Directory ve Gigya arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 3759cfdb30620622912c5866e43f81699ac37eb7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424695"
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Öğretici: Azure Active Directory Gigya ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Gigya tümleştirme konusunda bilgi edinin.

Azure AD ile Gigya tümleştirme ile aşağıdaki avantajları sağlar:

- Gigya erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Gigya (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Gigya yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Gigya çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Gigya ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-gigya-from-the-gallery"></a>Galeriden Gigya ekleme
Azure AD'de Gigya tümleştirmesini yapılandırmak için Gigya Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Gigya eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Gigya**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/tutorial_gigya_search.png)

1. Sonuçlar panelinde seçin **Gigya**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Gigya sınayın.

Tek iş için oturum açma için Azure AD ne Gigya karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Gigya ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Gigya içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Gigya ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Gigya test kullanıcısı oluşturma](#creating-a-gigya-test-user)**  - kullanıcı Azure AD gösterimini bağlı Gigya Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Gigya uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Gigya yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Gigya** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_gigya_samlbase.png)

1. Üzerinde **Gigya etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_gigya_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `http://<companyname>.gigya.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://fidm.gigya.com/saml/v2.0/<companyname>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Gigya istemci Destek ekibine](https://www.gigya.com/support-policy/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_gigya_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_general_400.png)

1. Üzerinde **Gigya yapılandırma** bölümünde **yapılandırma Gigya** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_gigya_configure.png) 

1. Farklı bir web tarayıcı penceresinde Gigya şirket sitenize yönetici olarak oturum.

1. Git **ayarları \> SAML oturum açma**ve ardından **Ekle** düğmesi.
   
    ![SAML oturum açma](./media/gigya-tutorial/ic789532.png "SAML oturum açma")

1. İçinde **SAML oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML yapılandırma](./media/gigya-tutorial/ic789533.png "SAML yapılandırma")
   
    a. İçinde **adı** metin yapılandırmanız için bir ad yazın.
   
    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız. 
   
    c. İçinde **çoklu oturum açma hizmeti URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    d. İçinde **ad kimliği biçimi** metin değerini yapıştırın **ad tanımlayıcı biçimi** , Azure Portalı'ndan kopyaladığınız.
   
    e. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.
   
    f. Tıklayın **ayarlarını kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gigya-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-gigya-test-user"></a>Gigya test kullanıcısı oluşturma

Gigya açarken Azure AD kullanıcılarının etkinleştirmek için bunların Gigya sağlanması gerekir.  
Gigya söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Gigya** yönetici olarak şirketin site.

1. Git **yönetici \> Kullanıcıları Yönet**ve ardından **kullanıcıları davet**.
   
    ![Kullanıcıları Yönet](./media/gigya-tutorial/ic789535.png "kullanıcıları yönetme")

1. Kullanıcıları davet iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcıları davet](./media/gigya-tutorial/ic789536.png "kullanıcılar davet edin")
   
    a. İçinde **e-posta** metin kutusuna geçerli bir Azure Active Directory hesabı sağlamak istediğiniz e-posta diğer adı yazın.
    
    b. Tıklayın **kullanıcı davet**.
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Gigya erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Gigya için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Gigya**.

    ![Çoklu oturum açmayı yapılandırın](./media/gigya-tutorial/tutorial_gigya_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde Gigya kutucuğa tıkladığınızda, otomatik olarak Gigya uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/gigya-tutorial/tutorial_general_01.png
[2]: ./media/gigya-tutorial/tutorial_general_02.png
[3]: ./media/gigya-tutorial/tutorial_general_03.png
[4]: ./media/gigya-tutorial/tutorial_general_04.png

[100]: ./media/gigya-tutorial/tutorial_general_100.png

[200]: ./media/gigya-tutorial/tutorial_general_200.png
[201]: ./media/gigya-tutorial/tutorial_general_201.png
[202]: ./media/gigya-tutorial/tutorial_general_202.png
[203]: ./media/gigya-tutorial/tutorial_general_203.png

