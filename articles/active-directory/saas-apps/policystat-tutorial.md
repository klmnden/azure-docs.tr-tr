---
title: 'Öğretici: Azure Active Directory Tümleştirme ile PolicyStat | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile PolicyStat arasında yapılandırmayı öğrenin.
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
ms.openlocfilehash: 8680f01e8c23ba8e164ec3da3ac116ced37a3c97
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36219141"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Öğretici: Azure Active Directory Tümleştirme PolicyStat ile

Bu öğreticide, Azure Active Directory (Azure AD) ile PolicyStat tümleştirmek öğrenin.

PolicyStat Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- PolicyStat erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için PolicyStat (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme PolicyStat ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir PolicyStat çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden PolicyStat ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-policystat-from-the-gallery"></a>Galeriden PolicyStat ekleme
Azure AD PolicyStat tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden PolicyStat eklemeniz gerekir.

**Galeriden PolicyStat eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **PolicyStat**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/tutorial_policystat_search.png)

5. Sonuçlar panelinde seçin **PolicyStat**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı PolicyStat sınayın.

Tekli çalışmaya oturum için Azure AD PolicyStat karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının PolicyStat ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

PolicyStat içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma PolicyStat ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[PolicyStat test kullanıcısı oluşturma](#creating-a-policystat-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı PolicyStat sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma PolicyStat uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile PolicyStat yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **PolicyStat** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

3. Üzerinde **PolicyStat etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.policystat.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [PolicyStat istemci destek ekibi](http://www.policystat.com/support/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

5. Bu bölümün amacı kullanıcıların PolicyStat için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

    Özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar PolicyStat uygulama bekler, **SAML belirteci öznitelikleri** yapılandırma.  

     Aşağıdaki ekran görüntüsü, bunun bir örneğini gösterir.

     ![Öznitelikleri](./media/policystat-tutorial/tutorial_policystat_attribute.png "öznitelikleri")

6. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı    |   Öznitelik Değeri |
    |------------------- | -------------------- |
    | Kullanıcı Kimliği | ExtractMailPrefix([mail]) |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. İçinde **öznitelik adı** metin kutusuna, türü **uid**.

    c. İçinde **öznitelik değeri** metin kutusuna, select **ExtractMailPrefix()**.    
   
    d. Gelen **posta** listesinde **User.mail**.
    
    e. Tıklatın **Tamam**

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_general_400.png)

8. Farklı web tarayıcısı penceresinde PolicyStat şirket sitenize yönetici olarak oturum açın.

9. Tıklatın **yönetici** sekmesini ve ardından **tek oturum açma yapılandırması** sol gezinti bölmesindeki.
   
    ![Yönetici menü](./media/policystat-tutorial/ic808633.png "yönetici menüsü")

10. İçinde **Kurulum** bölümünde, select **oturum açmayı etkinleştir tek tümleştirme**.
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808634.png "tek oturum açma yapılandırması")

11. Tıklatın **öznitelikleri yapılandırma**ve ardından **öznitelikleri yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808635.png "tek oturum açma yapılandırması")
   
    a. İçinde **Username özniteliği** metin kutusuna, türü **uid**.

    b. İçinde **ad özniteliği** metin kutusuna, türü **firstname** kullanıcının **Britta**.

    c. İçinde **son Name özniteliği** metin kutusuna, türü **lastname** kullanıcının **Simon**.

    d. İçinde **e-posta özniteliği** metin kutusuna, türü **emailaddress** kullanıcının **BrittaSimon@contoso.com**.

    e. Tıklatın **değişiklikleri kaydetmek**.

12. Tıklatın **bilgisayarınızı IDP meta veri**ve ardından **bilgisayarınızı IDP meta veri** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma yapılandırması](./media/policystat-tutorial/ic808636.png "tek oturum açma yapılandırması")
   
    a. İndirilen meta veri dosyanızı açın, içeriği Kopyala ve ardından yapıştırın **bilgisayarınızı kimlik sağlayıcısı meta verileri** metin kutusu.

    b. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/policystat-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-policystat-test-user"></a>PolicyStat test kullanıcısı oluşturma

Azure AD kullanıcıların PolicyStat oturum etkinleştirmek için bunların PolicyStat sağlanmalıdır.  

Yalnızca zaman sağlama kullanıcı PolicyStat destekler. Yani, kullanıcılar için PolicyStat el ile eklemeniz gerekmez. Kullanıcılar kendi ilk oturum açma SSO aracılığıyla üzerinde otomatik olarak eklenir.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için PolicyStat tarafından sağlanan veya herhangi diğer PolicyStat kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta PolicyStat için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**PolicyStat için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **PolicyStat**.

    ![Çoklu oturum açmayı yapılandırın](./media/policystat-tutorial/tutorial_policystat_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli PolicyStat parçasında tıklattığınızda, otomatik olarak PolicyStat uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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

