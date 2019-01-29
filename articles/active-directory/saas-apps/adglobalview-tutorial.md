---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ADP Globalview | Microsoft Docs'
description: Azure Active Directory ve ADP Globalview arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: f5d11dd1ed8e23414eed5c9d7cf5c248b58aaa8a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163730"
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Öğretici: ADP Globalview ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ADP Globalview tümleştirme konusunda bilgi edinin.

Azure AD ile ADP Globalview tümleştirme ile aşağıdaki avantajları sağlar:

- ADP Globalview erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için ADP Globalview açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ADP Globalview yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir ADP Globalview çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ADP Globalview ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-adp-globalview-from-the-gallery"></a>Galeriden ADP Globalview ekleme
Azure AD'de ADP Globalview tümleştirmesini yapılandırmak için ADP Globalview Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ADP Globalview eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **ADP Globalview**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. Sonuçlar panelinde seçin **ADP Globalview**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve ADP Globalview ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne ADP Globalview karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ADP Globalview ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** ADP Globalview içinde.

Yapılandırma ve Azure AD çoklu oturum açma ADP Globalview ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir ADP Globalview test kullanıcısı oluşturma](#creating-an-adp-globalview-test-user)**  - kullanıcı Azure AD gösterimini bağlı ADP Globalview Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ADP Globalview uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ADP Globalview yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ADP Globalview** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. Üzerinde **ADP Globalview etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_url.png)

     İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.globalview.adp.com/federate` veya `https://<subdomain>.globalview.adp.com/federate2`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [ADP Globalview Destek](https://www.adp.com/contact-us/overview.aspx) değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. ADP GlobalView uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. 

6. Aşağıdaki ekran için bir örnek gösterilmektedir. Her zaman talep adları olması **"PersonImmutableID"** ve hangisinin size eşlenen kullanıcının EmployeeID içeren ExtensionAttribute2 için değer. ADP GlobalView Azure AD'den kullanıcı eşlemeyi üzerinde EmployeeID burada yapılır, ancak Ayrıca uygulama ayarlarınızı temel alan farklı bir değer eşleyebilirsiniz. İlk olarak, bir kullanıcının doğru tanımlayıcı kullanın ve bu değerle eşleştirmek için ADP GlobalView ekibiyle birlikte çalışabilir **"PersonImmutableID"** talep. Ayrıca, resimde gösterildiği gibi e-posta ve UserID talep eşleyebilirsiniz.

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | personalimmutableid | User.extensionattribute2 |
    | e-posta               | User.Mail |
    | Kullanıcı Kimliği              | User.userPrincipalName|
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

    > [!NOTE] 
    > SAML onaylaması yapılandırmadan önce iletişime geçmeniz, [ADP Globalview Destek](https://www.adp.com/contact-us/overview.aspx) ve kiracınız için benzersiz tanımlayıcı özniteliği değeri isteyin. Uygulamanız için özel talep yapılandırmak için bu değere ihtiyacınız. 

8. Üzerinde **ADP Globalview yapılandırma** bölümünde **yapılandırma ADP Globalview** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_general_400.png)

10. Çoklu oturum açmayı yapılandırma **ADP Globalview** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** için [ADP Globalview Destek](https://www.adp.com/contact-us/overview.aspx).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/create_aaduser_01.png) 

2.  Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/adglobalview-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-adp-globalview-test-user"></a>Bir ADP Globalview test kullanıcısı oluşturma

Bu bölümün amacı ADP GlobalView Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışmak [ADP Globalview Destek](https://www.adp.com/contact-us/overview.aspx) ADP GlobalView hesabında kullanıcıları eklemek için. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için ADP Globalview erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon ADP Globalview için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **ADP Globalview**.

    ![Çoklu oturum açmayı yapılandırın](./media/adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.  

Erişim panelinde ADP GlobalView kutucuğa tıkladığınızda, otomatik olarak ADP GlobalView uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/adglobalview-tutorial/tutorial_general_203.png

