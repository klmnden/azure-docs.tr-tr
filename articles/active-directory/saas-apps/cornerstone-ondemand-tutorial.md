---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle dönüm OnDemand | Microsoft Docs'
description: Azure Active Directory ve temel taşıdır OnDemand arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: 4927421afeddc337856c027b3ed32539f4f8c1fc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441706"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Öğretici: Azure Active Directory temel taşıdır OnDemand ile tümleştirme

Bu öğreticide, temel taşıdır OnDemand Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile temel taşıdır OnDemand tümleştirme ile aşağıdaki avantajları sağlar:

- Temel taşıdır OnDemand erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için dönüm OnDemand açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile dönüm OnDemand yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir dönüm OnDemand çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden dönüm OnDemand ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Galeriden dönüm OnDemand ekleme
Azure AD'de dönüm OnDemand tümleştirmesini yapılandırmak için dönüm OnDemand Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden dönüm OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **dönüm OnDemand**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

1. Sonuçlar panelinde seçin **dönüm OnDemand**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma dönüm OnDemand tabanlı "Britta Simon." adlı bir test kullanıcı ile test etme

Tek iş için oturum açma için Azure AD temel taşıdır OnDemand karşılığı kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının temel taşıdır OnDemand ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Temel taşıdır OnDemand içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma dönüm OnDemand ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir dönüm OnDemand test kullanıcısı oluşturma](#creating-a-cornerstone-ondemand-test-user)**  - kullanıcı Azure AD gösterimini bağlı dönüm OnDemand Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve dönüm OnDemand uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile dönüm OnDemand yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **dönüm OnDemand** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

1. Üzerinde **dönüm OnDemand etki alanı ve URL'ler** bölümünde, aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company>.csod.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company>.csod.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [dönüm OnDemand istemci Destek ekibine](mailTo:moreinfo@csod.com) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_general_400.png)

1. Üzerinde **dönüm OnDemand yapılandırma** bölümünde **yapılandırma temel taşıdır OnDemand** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

1. Çoklu oturum açmayı yapılandırma **dönüm OnDemand** tarafı, indirilen göndermek için ihtiyacınız **sertifika**, **oturum kapatma URL'si** ve **SAML çoklu oturum açma Hizmet URL'si** için [dönüm OnDemand Destek ekibine](mailTo:moreinfo@csod.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_03.png)

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-cornerstone-ondemand-test-user"></a>Bir dönüm OnDemand test kullanıcısı oluşturma

Bu bölümün amacı temel taşıdır OnDemand Britta Simon adlı bir kullanıcı oluşturmaktır. Temel taşıdır OnDemand otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](cornerstone-ondemand-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

Kullanıcı sağlamayı yapılandırma için bilgi gönder (örn: adı, e-posta) Azure AD kullanıcı hakkında sağlamak istediğiniz [dönüm OnDemand Destek ekibine](mailTo:moreinfo@csod.com).

>[!NOTE]
>Herhangi diğer dönüm OnDemand kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak dönüm OnDemand tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için dönüm OnDemand erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için dönüm OnDemand atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **dönüm OnDemand**.

    ![Çoklu oturum açmayı yapılandırın](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde dönüm OnDemand kutucuğa tıkladığınızda, otomatik olarak dönüm OnDemand uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](cornerstone-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/cornerstone-ondemand-tutorial/tutorial_general_203.png
