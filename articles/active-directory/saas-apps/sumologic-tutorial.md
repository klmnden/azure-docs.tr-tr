---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile SumoLogic | Microsoft Docs'
description: Azure Active Directory ve SumoLogic arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fee91b857d9fd127839baaf7a70199c25cfab33
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191648"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Öğretici: SumoLogic ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SumoLogic tümleştirme konusunda bilgi edinin.

Azure AD ile SumoLogic tümleştirme ile aşağıdaki avantajları sağlar:

- SumoLogic erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için SumoLogic (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SumoLogic yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik SumoLogic çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SumoLogic ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sumologic-from-the-gallery"></a>Galeriden SumoLogic ekleme
Azure AD'de SumoLogic tümleştirmesini yapılandırmak için SumoLogic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SumoLogic eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **SumoLogic**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/tutorial_sumologic_search.png)

1. Sonuçlar panelinde seçin **SumoLogic**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SumoLogic sınayın.

Tek iş için oturum açma için Azure AD ne SumoLogic karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SumoLogic ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SumoLogic içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SumoLogic ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SumoLogic test kullanıcısı oluşturma](#creating-a-sumologic-test-user)**  - kullanıcı Azure AD gösterimini bağlı SumoLogic Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SumoLogic uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SumoLogic yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SumoLogic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_sumologic_samlbase.png)

1. Üzerinde **SumoLogic etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_sumologic_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenantname>.SumoLogic.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [SumoLogic istemci Destek ekibine](https://www.sumologic.com/contact-us/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_sumologic_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_general_400.png)

1. Üzerinde **SumoLogic yapılandırma** bölümünde **yapılandırma SumoLogic** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_sumologic_configure.png) 

1. Farklı bir web tarayıcı penceresinde SumoLogic şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **yönetme \> güvenlik**.
   
    ![Yönetme](./media/sumologic-tutorial/ic778556.png "yönetme")

1. Tıklayın **SAML**.
   
    ![Genel güvenlik ayarları](./media/sumologic-tutorial/ic778557.png "genel güvenlik ayarları")

1. Gelen **bir yapılandırma seçin veya yeni bir tane oluşturun** listesinde, seçin **Azure AD'ye**ve ardından **yapılandırma**.
   
    ![SAML 2.0 yapılandırma](./media/sumologic-tutorial/ic778558.png "SAML 2.0 yapılandırma")

1. Üzerinde **SAML 2.0 yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![SAML 2.0 yapılandırma](./media/sumologic-tutorial/ic778559.png "SAML 2.0 yapılandırma")
   
    a. İçinde **yapılandırma adı** metin kutusuna **Azure AD'ye**. 

    b. Seçin **hata ayıklama modu**.

    c. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız. 

    d. İçinde **Authn istek URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Tüm sertifika içine yapıştırın, base-64 kodlanmış sertifika Not Defteri'nde açın ve içeriğini, panoya kopyalanmak **X.509 sertifikası** metin.

    f. Olarak **e-posta özniteliği**seçin **kullanım SAML konu**.  

    g. Seçin **SP tarafından başlatılan oturum açma yapılandırması**.

    h. İçinde **oturum açma yoluna** metin kutusuna **Azure** tıklatıp **Kaydet**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sumologic-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sumologic-test-user"></a>SumoLogic test kullanıcısı oluşturma

Azure AD kullanıcıları için SumoLogic oturum açmak etkinleştirmek için bunlar için SumoLogic sağlanması gerekir.  

* SumoLogic söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **SumoLogic** Kiracı.

1. Git **yönetme \> kullanıcılar**.
   
    ![Kullanıcılar](./media/sumologic-tutorial/ic778561.png "kullanıcılar")

1. **Ekle**'ye tıklayın.
   
    ![Kullanıcılar](./media/sumologic-tutorial/ic778562.png "kullanıcılar")

1. Üzerinde **yeni kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı](./media/sumologic-tutorial/ic778563.png "yeni kullanıcı") 
 
    a. Zbilgisayarlar istediğiniz Azure AD hesabının ilgili bilgileri yazın **ad**, **Soyadı**, ve **e-posta** metin kutuları.
  
    b. Bir rol seçin.
  
    c. Olarak **durumu**seçin **etkin**.
  
    d. **Kaydet**’e tıklayın.

>[!NOTE]
>Herhangi diğer SumoLogic kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak SumoLogic tarafından sağlanan. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için SumoLogic erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SumoLogic için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SumoLogic**.

    ![Çoklu oturum açmayı yapılandırın](./media/sumologic-tutorial/tutorial_sumologic_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde SumoLogic kutucuğa tıkladığınızda, otomatik olarak SumoLogic uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sumologic-tutorial/tutorial_general_01.png
[2]: ./media/sumologic-tutorial/tutorial_general_02.png
[3]: ./media/sumologic-tutorial/tutorial_general_03.png
[4]: ./media/sumologic-tutorial/tutorial_general_04.png

[100]: ./media/sumologic-tutorial/tutorial_general_100.png

[200]: ./media/sumologic-tutorial/tutorial_general_200.png
[201]: ./media/sumologic-tutorial/tutorial_general_201.png
[202]: ./media/sumologic-tutorial/tutorial_general_202.png
[203]: ./media/sumologic-tutorial/tutorial_general_203.png

