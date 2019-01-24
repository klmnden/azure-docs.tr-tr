---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle & Açıkçası | Microsoft Docs'
description: Azure Active Directory arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin ve & Açıkçası.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1d702060-1b89-4e9d-9f01-ede4f1171c73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 73acaeff6cbffc16aac1b30b9d63974c930c1537
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54818073"
---
# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Öğretici: Azure Active Directory tümleştirmesiyle & Açıkçası

Bu öğretici sayesinde nasıl tümleştireceğinizi öğrenin ve Azure Active Directory (Azure AD) ile Açıkçası.

Tümleştirme ve Açıkçası Azure AD ile aşağıdaki faydaları sağlar:

- & Açıkçası erişimi olan Azure AD'de denetleyebilirsiniz.
- Oturum açma & Açıkçası otomatik olarak almak, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD yapılandırmanız tümleştirmesiyle & Açıkçası, aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- A & Açıkçası çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Ekleme ve Açıkçası galerisinden
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-frankly-from-the-gallery"></a>Ekleme ve Açıkçası galerisinden
Eklemenize gerek & Açıkçası, Azure AD ile tümleştirme yapılandırmak için & Açıkçası listenize yönetilen SaaS uygulamaları Galerisi'nden.

**Ekleme ve Açıkçası galerideki, aşağıdaki adımları gerçekleştirmek için:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **& Açıkçası**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/tutorial_andfrankly_search.png)

5. Sonuçlar panelinde seçin **& Açıkçası**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/tutorial_andfrankly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile test & Açıkçası "Britta Simon." adlı bir test kullanıcı tabanlı

Tek çalışmak için oturum açma, Azure AD içinde karşılık gelen kullanıcının bilmesi gerekir & bir kullanıcının Azure AD'de Açıkçası olduğu. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki & Açıkçası kurulması gerekir.

& Açıkçası, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırmanıza ve sınamanıza Azure AD çoklu oturum açma ile & Açıkçası, size aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Oluşturma bir & Açıkçası test kullanıcısı](#creating-a-frankly-test-user)**  - bir karşılığı Britta simon'un sağlamak için kullanıcı Azure AD gösterimini & Açıkçası diğer bir deyişle bağlı.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki etkinleştirme ve çoklu oturum açmayı içindeki yapılandırma bilgilerinizi & Açıkçası uygulama.

**Azure'u yapılandırmak için AD ile oturum & tek Açıkçası, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **& Açıkçası** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_andfrankly_samlbase.png)

3. Üzerinde **& Açıkçası etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_andfrankly_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

4. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_andfrankly_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirme oturum açma ve yanıt URL'si. İlgili kişi [andfrankly Destek ekibine](mailto:help@andfrankly.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_andfrankly_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_general_400.png)

7. Çoklu oturum açmayı yapılandırma **& Açıkçası** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [andfrankly Destek ekibine](mailto:help@andfrankly.com). 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/andfrankly-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-frankly-test-user"></a>Oluşturma bir & Açıkçası test kullanıcısı

Bu bölümde, Britta Simon & Açıkçası adlı bir kullanıcı oluşturun. Çalışmak [andfrankly Destek ekibine](mailto:help@andfrankly.com) & Açıkçası kullanıcıları eklemek için platform.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma & Açıkçası erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon & Açıkçası atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **& Açıkçası**.

    ![Çoklu oturum açmayı yapılandırın](./media/andfrankly-tutorial/tutorial_andfrankly_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

' A tıkladığınızda & Açıkçası, erişim Paneli'nde kutucuğunda, otomatik olarak açan almalısınız & Açıkçası uygulama

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/andfrankly-tutorial/tutorial_general_04.png

[100]: ./media/andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/andfrankly-tutorial/tutorial_general_201.png
[202]: ./media/andfrankly-tutorial/tutorial_general_202.png
[203]: ./media/andfrankly-tutorial/tutorial_general_203.png

