---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle PolicyStat | Microsoft Docs'
description: Azure Active Directory ve PolicyStat arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 97154f0ee8f07e0fa4fe8d70fef997144251c27d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042000"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Öğretici: Azure Active Directory PolicyStat ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile PolicyStat tümleştirme konusunda bilgi edinin.

Azure AD ile PolicyStat tümleştirme ile aşağıdaki avantajları sağlar:

- PolicyStat erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için PolicyStat (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile PolicyStat yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik PolicyStat çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden PolicyStat ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-policystat-from-the-gallery"></a>Galeriden PolicyStat ekleme
Azure AD'de PolicyStat tümleştirmesini yapılandırmak için PolicyStat Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden PolicyStat eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **PolicyStat**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/tutorial_policystat_search.png)

5. Sonuçlar panelinde seçin **PolicyStat**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı PolicyStat sınayın.

Tek iş için oturum açma için Azure AD ne PolicyStat karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının PolicyStat ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

PolicyStat içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma PolicyStat ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[PolicyStat test kullanıcısı oluşturma](#creating-a-policystat-test-user)**  - kullanıcı Azure AD gösterimini bağlı PolicyStat Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve PolicyStat uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile PolicyStat yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **PolicyStat** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

3. Üzerinde **PolicyStat etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.policystat.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [PolicyStat istemci Destek ekibine](http://www.policystat.com/support/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

5. Bu bölümün amacı, kullanıcıların Azure AD'de SAML protokolü temel alarak Federasyon kullanarak PolicyStat için kendi hesabıyla kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

    Özel öznitelik eşlemeleri eklemek gerektiren belirli bir biçimde SAML onaylamalarını PolicyStat uygulama bekliyor, **SAML belirteci öznitelikleri** yapılandırma.  

     Bunun bir örneğini aşağıdaki ekran gösterilir.

     ![Öznitelikleri](./media/policystat-tutorial/tutorial_policystat_attribute.png "öznitelikleri")

6. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı    |   Öznitelik Değeri |
    |------------------- | -------------------- |
    | Kullanıcı Kimliği | ExtractMailPrefix([mail]) |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. İçinde **öznitelik adı** metin kutusuna **uid**.

    c. İçinde **öznitelik değeri** metin seçme **ExtractMailPrefix()**.    
   
    d. Gelen **posta** listesinden **User.mail**.
    
    e. Tıklayın **Tamam**

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_general_400.png)

8. Farklı bir web tarayıcı penceresinde PolicyStat şirket sitenize yönetici olarak oturum.

9. Tıklayın **yönetici** sekmesine ve ardından **çoklu oturum açma yapılandırması** sol gezinti bölmesinde.
   
    ![Yönetici menü](./media/policystat-tutorial/ic808633.png "yönetici menüsü")

10. İçinde **Kurulum** bölümünden **etkinleştirme tek oturum açma tümleştirmesi**.
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808634.png "çoklu oturum açma yapılandırması")

11. Tıklayın **öznitelikleri yapılandırma**ve ardından **öznitelikleri yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808635.png "çoklu oturum açma yapılandırması")
   
    a. İçinde **kullanıcı adı özniteliği** metin kutusuna **uid**.

    b. İçinde **ad özniteliği** metin kutusuna **firstname** kullanıcının **Britta**.

    c. İçinde **son Name özniteliği** metin kutusuna **lastname** kullanıcının **Simon**.

    d. İçinde **e-posta özniteliği** metin kutusuna **emailaddress** kullanıcının **BrittaSimon@contoso.com**.

    e. Tıklayın **değişiklikleri kaydetmek**.

12. Tıklayın **bilgisayarınızı IDP meta verileri**ve ardından **bilgisayarınızı IDP meta verileri** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808636.png "çoklu oturum açma yapılandırması")
   
    a. İndirilen meta verileri dosyanızı açın, içeriği kopyalayın ve ardından yapıştırın **uygulamanızın kimlik sağlayıcısı meta verileri** metin.

    b. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-policystat-test-user"></a>PolicyStat test kullanıcısı oluşturma

PolicyStat açarken Azure AD kullanıcılarının etkinleştirmek için bunların PolicyStat sağlanması gerekir.  

Yalnızca zaman sağlama kullanıcı PolicyStat destekler. Diğer bir deyişle, kullanıcılar için PolicyStat el ile eklemeniz gerekmez. Kullanıcılar, kendi ilk oturum açma aracılığıyla SSO üzerinde otomatik olarak eklenir.

>[!NOTE]
>Herhangi diğer PolicyStat kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için PolicyStat tarafından sağlanan.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için PolicyStat erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon PolicyStat için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **PolicyStat**.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde PolicyStat kutucuğa tıkladığınızda, otomatik olarak PolicyStat uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/policystat-tutorial/tutorial_general_01.png
[2]: ./media/policystat-tutorial/tutorial_general_02.png
[3]: ./media/policystat-tutorial/tutorial_general_03.png
[4]: ./media/policystat-tutorial/tutorial_general_04.png

[100]: ./media/policystat-tutorial/tutorial_general_100.png

[200]: ./media/policystat-tutorial/tutorial_general_200.png
[201]: ./media/policystat-tutorial/tutorial_general_201.png
[202]: ./media/policystat-tutorial/tutorial_general_202.png
[203]: ./media/policystat-tutorial/tutorial_general_203.png

