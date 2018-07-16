---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Intralinks | Microsoft Docs'
description: Azure Active Directory ve Intralinks arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 2dbf52d0e157379687b144feba5c7933a7c5a3e3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046847"
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Öğretici: Azure Active Directory Intralinks ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Intralinks tümleştirme konusunda bilgi edinin.

Azure AD ile Intralinks tümleştirme ile aşağıdaki avantajları sağlar:

- Intralinks erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Intralinks (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Intralinks yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Intralinks çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Intralinks ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-intralinks-from-the-gallery"></a>Galeriden Intralinks ekleme
Azure AD'de Intralinks tümleştirmesini yapılandırmak için Intralinks Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Intralinks eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Intralinks**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/tutorial_intralinks_search.png)

5. Sonuçlar panelinde seçin **Intralinks**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Intralinks sınayın.

Tek iş için oturum açma için Azure AD ne Intralinks karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Intralinks ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Intralinks içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Intralinks ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Intralinks test kullanıcısı oluşturma](#creating-an-intralinks-test-user)**  - kullanıcı Azure AD gösterimini bağlı Intralinks Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Intralinks uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Intralinks yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Intralinks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. Üzerinde **Intralinks etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Intralinks istemci Destek ekibine](https://www.intralinks.com/contact-1) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **Intralinks** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** [Intralinks Destek ekibine](https://www.intralinks.com/contact-1). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-intralinks-test-user"></a>Bir Intralinks test kullanıcısı oluşturma

Bu bölümde, Britta Simon Intralinks içinde adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Intralinks Destek ekibine](https://www.intralinks.com/contact-1) Intralinks platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Intralinks erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Intralinks için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Intralinks**.

    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="add-intralinks-via-or-elite-application"></a>Intralinks aracılığıyla veya Elite uygulama ekleme

Intralinks anlaşma Nexus uygulama hariç tüm diğer Intralinks uygulamalar için aynı SSO kimlik platformu kullanır. Başka bir Intralinks uygulaması'nı kullanmayı planlıyorsanız, bu nedenle sonra ilk, SSO yukarıda açıklanan yordamı kullanarak, bir birincil Intralinks uygulaması için yapılandırmanız gerekir.

Bundan sonra izleyebilirsiniz aşağıda bu birincil uygulama SSO için yararlanabilir, kiracınızdaki başka bir Intralinks uygulamaya eklenecek yordam. 

>[!NOTE]
>Bu özellik yalnızca Azure AD Premium SKU müşterileri için kullanılabilir ve ücretsiz veya temel SKU müşteriler için kullanılabilir.

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]


2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Intralinks**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intralinks-tutorial/tutorial_intralinks_search.png)

5. Üzerinde **uygulama Intralinks Ekle** aşağıdaki adımları gerçekleştirin:

    ![Intralinks aracılığıyla veya Elite uygulama ekleme](./media/intralinks-tutorial/tutorial_intralinks_addapp.png)

    a. İçinde **adı** metin, örneğin uygulamanın uygun bir ad girin **Intralinks Elite**.

    b. Tıklayın **Ekle** düğmesi.

6.  Azure portalında, üzerinde **Intralinks** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

7. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **bağlantılı oturum açma**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. SP tarafından başlatılan SSO'yu URL'den alma [Intralinks takım](https://www.intralinks.com/contact-1) diğer Intralinks uygulamanın girin **yapılandırma oturum açma URL'si** aşağıda gösterildiği gibi. 
    
     ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     Üzerinde oturum URL'si metin kutusuna, kullanıcılarınız için oturum açma şu biçimi kullanarak Intralinks uygulamanıza tarafından kullanılan URL'yi yazın:
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/intralinks-tutorial/tutorial_general_400.png)

10. Bölümde gösterildiği kullanıcı veya grupları uygulamaya atama  **[Azure AD'ye test kullanıcı atama](#assigning-the-azure-ad-test-user)**.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Intralinks kutucuğa tıkladığınızda, otomatik olarak Intralinks uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/intralinks-tutorial/tutorial_general_01.png
[2]: ./media/intralinks-tutorial/tutorial_general_02.png
[3]: ./media/intralinks-tutorial/tutorial_general_03.png
[4]: ./media/intralinks-tutorial/tutorial_general_04.png

[100]: ./media/intralinks-tutorial/tutorial_general_100.png

[200]: ./media/intralinks-tutorial/tutorial_general_200.png
[201]: ./media/intralinks-tutorial/tutorial_general_201.png
[202]: ./media/intralinks-tutorial/tutorial_general_202.png
[203]: ./media/intralinks-tutorial/tutorial_general_203.png

