---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle 123ContactForm | Microsoft Docs'
description: Azure Active Directory ve 123ContactForm arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ecbe627697fc4f8b5fbfecf96c3cb65d9ffe4607
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054361"
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Öğretici: Azure Active Directory 123ContactForm ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 123ContactForm tümleştirme konusunda bilgi edinin.

Azure AD ile 123ContactForm tümleştirme ile aşağıdaki avantajları sağlar:

- 123ContactForm erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan 123ContactForm için açma, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile 123ContactForm yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik 123ContactForm çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden 123ContactForm ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-123contactform-from-the-gallery"></a>Galeriden 123ContactForm ekleme
Azure AD'de 123ContactForm tümleştirmesini yapılandırmak için 123ContactForm Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden 123ContactForm eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **123ContactForm**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/tutorial_123contactform_search.png)

5. Sonuçlar panelinde seçin **123ContactForm**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı 123ContactForm ile test etme

Tek iş için oturum açma için Azure AD ne 123ContactForm karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının 123ContactForm ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

123ContactForm içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma 123ContactForm ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[123ContactForm test kullanıcısı oluşturma](#creating-a-123contactform-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı 123ContactForm içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve 123ContactForm uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile 123ContactForm yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **123ContactForm** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. Üzerinde **123ContactForm etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Uygulamada yapılandırmak istiyorsanız **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/url2.png)

    a. Tıklayın **Gelişmiş URL ayarlarını göster** seçeneği

    b. İçinde **işareti bulunan URL'si** metin kutusuna bir URL: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerini gerçek URL'leri ve öğreticinin sonraki bölümlerinde açıklanan tanımlayıcısı güncelleştirmeniz gerekir.
    
5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/tutorial_general_400.png)

7. Çoklu oturum açmayı yapılandırma **123ContactForm** tarafı, Git [ https://www.123contactform.com/form-2709121/ ](https://www.123contactform.com/form-2709121/) ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/submit.png) 

    a. İçinde **e-posta** metin kullanıcı yani e-postası girin **BrittaSimon@Contoso.com**.

    b. Tıklayın **karşıya** ve Azure portalından indirdiğiniz meta veri XML dosyasını bulun.

    c. Tıklayın **gönderme FORM**.

8. Üzerinde **Microsoft Azure AD çoklu oturum açma - uygulama ayarlarını yapılandırma** aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/url3.png)

    a. Uygulamada yapılandırmak istiyorsanız **IDP tarafından başlatılan modu**, kopyalama **TANIMLAYICI** yapıştırın ve değer Örneğiniz için **tanımlayıcı** metin kutusunda  **123ContactForm etki alanı ve URL'ler** bölümü Azure portalı.
    
    b. Uygulamada yapılandırmak istiyorsanız **IDP tarafından başlatılan modu**, kopyalama **yanıt URL'si** yapıştırın ve değer Örneğiniz için **yanıt URL'si** metin kutusunda  **123ContactForm etki alanı ve URL'ler** bölümü Azure portalı.

    c. Uygulamada yapılandırmak istiyorsanız **SP tarafından başlatılan modu**, kopyalama **oturum açık URL'si** yapıştırın ve değer Örneğiniz için **işareti bulunan URL'si** metin kutusunda  **123ContactForm etki alanı ve URL'ler** bölümü Azure portalı.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/123contactform-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-123contactform-test-user"></a>123ContactForm test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma 123ContactForm erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon 123ContactForm için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **123ContactForm**.

    ![Çoklu oturum açmayı yapılandırın](./media/123contactform-tutorial/tutorial_123contactform_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde 123ContactForm kutucuğa tıkladığınızda, otomatik olarak 123ContactForm uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/123contactform-tutorial/tutorial_general_01.png
[2]: ./media/123contactform-tutorial/tutorial_general_02.png
[3]: ./media/123contactform-tutorial/tutorial_general_03.png
[4]: ./media/123contactform-tutorial/tutorial_general_04.png

[100]: ./media/123contactform-tutorial/tutorial_general_100.png

[200]: ./media/123contactform-tutorial/tutorial_general_200.png
[201]: ./media/123contactform-tutorial/tutorial_general_201.png
[202]: ./media/123contactform-tutorial/tutorial_general_202.png
[203]: ./media/123contactform-tutorial/tutorial_general_203.png

