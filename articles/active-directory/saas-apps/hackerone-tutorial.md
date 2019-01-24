---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Hackerone | Microsoft Docs'
description: Azure Active Directory ve Hackerone arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: fe070505970516efcd4e2ae46dedff2792f95b08
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817206"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Öğretici: HackerOne ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile HackerOne tümleştirme konusunda bilgi edinin.

Azure AD ile HackerOne tümleştirme ile aşağıdaki avantajları sağlar:

- HackerOne erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için HackerOne (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile HackerOne yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik HackerOne çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden HackerOne ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-hackerone-from-the-gallery"></a>Galeriden HackerOne ekleme
Azure AD'de HackerOne tümleştirmesini yapılandırmak için HackerOne Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden HackerOne eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **HackerOne**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/tutorial_hackerone_search.png)

1. Sonuçlar panelinde seçin **HackerOne**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı HackerOne ile test etme

Tek iş için oturum açma için Azure AD ne HackerOne karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının HackerOne ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

HackerOne içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma HackerOne ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[HackerOne test kullanıcısı oluşturma](#creating-a-hackerone-test-user)**  - kullanıcı Azure AD gösterimini bağlı HackerOne Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve HackerOne uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile HackerOne yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **HackerOne** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_samlbase.png)

1. Üzerinde **HackerOne tek oturum açma URL ve tanımlayıcıdır** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://hackerone.com/<company name>/authentication`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL:  `https://hackerone.com/users/saml/metadata`
    
    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [HackerOne Destek ekibine](mailto:support@hackerone.com) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_general_400.png)

1. Üzerinde **HackerOne yapılandırma** bölümünde **yapılandırma HackerOne** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_configure.png) 

1. Oturum açma HackerOne kiracınıza yönetici olarak.

1. Üstteki menüden "**ayarları**."
   
    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_001.png) 

1. Gidin "**kimlik doğrulaması**"ve tıklayın"**SAML ayarlarını ekleyin**."
   
    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_003.png) 

1. Üzerinde **SAML ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_004.png) 

    a. İçinde **e-posta etki alanı** metin kaydedilmiş bir etki alanı yazın.

    b. İçinde  **üzerinde çoklu oturum URL'si** metin kutuları, değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Açık, **sertifika dosyası** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X509 sertifika**  metin.
    
    d. **Kaydet**’e tıklayın.

1. Kimlik doğrulama ayarları iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_005.png) 

    a. Tıklayın **test çalıştırması**.

    b. Varsa değerini **durumu** alan eşittir **son test durumu: oluşturulan**, kişi, [HackerOne Destek ekibine](mailto:support@hackerone.com) yapılandırmanızın bir inceleme istemek için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hackerone-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-hackerone-test-user"></a>HackerOne test kullanıcısı oluşturma

Sonra Britta Simon HackerOne içinde adlı bir kullanıcı oluşturun. HackerOne tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. HackerOne eriştiğinizde, henüz mevcut değilse yeni bir kullanıcı oluşturuldu.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, Certify Destek ekibine başvurun gerekir. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için HackerOne erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon HackerOne için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **HackerOne**.

    ![Çoklu oturum açmayı yapılandırın](./media/hackerone-tutorial/tutorial_hackerone_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Son olarak, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.  

Erişim panelinde HackerOne kutucuğa tıkladığınızda, otomatik olarak HackerOne uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/hackerone-tutorial/tutorial_general_01.png
[2]: ./media/hackerone-tutorial/tutorial_general_02.png
[3]: ./media/hackerone-tutorial/tutorial_general_03.png
[4]: ./media/hackerone-tutorial/tutorial_general_04.png

[100]: ./media/hackerone-tutorial/tutorial_general_100.png

[200]: ./media/hackerone-tutorial/tutorial_general_200.png
[201]: ./media/hackerone-tutorial/tutorial_general_201.png
[202]: ./media/hackerone-tutorial/tutorial_general_202.png
[203]: ./media/hackerone-tutorial/tutorial_general_203.png

