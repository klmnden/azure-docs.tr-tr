---
title: 'Öğretici: Predictix sıralama ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Predictix sıralama arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: f9b290af13b45f58af4deb5be9a8ce1a85609970
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177574"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a>Öğretici: Predictix sıralama ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Predictix sıralama öğrenin.

Azure AD ile tümleştirme Predictix sıralama ile aşağıdaki avantajları sağlar:

- Predictix sıralama erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Predictix sıralama için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Predictix sıralama ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Predictix sıralama çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Predictix sıralama ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-predictix-ordering-from-the-gallery"></a>Galeriden Predictix sıralama ekleme
Azure AD'de Predictix sipariş tümleştirmesini yapılandırmak için Predictix sıralama Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Predictix sıralama eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Predictix sıralama**seçin **Predictix sıralama** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Predictix sıralama](./media/predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Predictix sıralama ile test edin.

Tek iş için oturum açma için Azure AD ne Predictix sıralama karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili Predictix sıralama kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değeri atamak Predictix sıralamada **kullanıcı adı** değerini Azure AD'de **kullanıcı adı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Predictix sıralama ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Predictix sıralama test kullanıcısı oluşturma](#create-a-predictix-ordering-test-user)**  - kullanıcı Azure AD gösterimini bağlı Predictix sıralama Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Predictix sıralama uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Predictix sıralama ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Predictix sıralama** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

1. Üzerinde **Predictix sıralama etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Predictix sıralama etki alanı ve URL'ler tek oturum açma bilgileri](./media/predictixordering-tutorial/tutorial_predictixordering_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname-pricing>.ordering.predictix.com/sso/request`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Predictix sıralama istemci Destek ekibine](https://www.predix.io/support/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/predictixordering-tutorial/tutorial_general_400.png)

1. Üzerinde **Predictix sıralama yapılandırma** bölümünde **yapılandırma Predictix sıralama** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Predictix sıralama yapılandırma](./media/predictixordering-tutorial/tutorial_predictixordering_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Predictix sıralama** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** için [Predictix sıralama Destek ekibine](https://www.predix.io/support/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/predictixordering-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/predictixordering-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/predictixordering-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/predictixordering-tutorial/create_aaduser_04.png)

   a. İçinde **adı** kutusuna **BrittaSimon**.

   b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

   c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

   d. **Oluştur**’a tıklayın.
 
### <a name="create-a-predictix-ordering-test-user"></a>Predictix sıralama test kullanıcısı oluşturma

Bu bölümde, Britta Simon Predictix sıralamada adlı bir kullanıcı oluşturun. Çalışmak [Predictix sıralama Destek ekibine](https://www.predix.io/support/) Predictix sıralama platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Predictix sıralama için erişim izni verme kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Predictix sıralama için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Predictix sıralama**.

    ![Uygulamalar listesinde Predictix sıralama bağlantı](./media/predictixordering-tutorial/tutorial_predictixordering_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Predictix sıralama kutucuğa tıkladığınızda, otomatik olarak Predictix sıralama uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/predictixordering-tutorial/tutorial_general_203.png

