---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Merces tarafından HR2day | Microsoft Docs'
description: Çoklu oturum açma HR2day Merces tarafından Azure Active Directory arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 955fbbe56f2a15531edc863b214b382b52027855
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982864"
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Öğretici: Azure Active Directory Tümleştirme Merces tarafından HR2day ile

Bu öğreticide, Azure Active Directory (Azure AD) ile HR2day Merces tarafından tümleştirmek öğrenin.

HR2day Merces tarafından Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- HR2day Merces tarafından erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak HR2day için Merces tarafından kendi Azure AD hesaplarıyla oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Merces tarafından HR2day yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD abonelik.
- Bir HR2day Merces çoklu oturum açma tarafından abonelik etkin.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Alma bir [bir aylık ücretsiz deneme Azure ad](https://azure.microsoft.com/pricing/free-trial/) , zaten yoksa.  

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Burada özetlenen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden HR2day Merces tarafından ekleniyor.
2. Yapılandırma ve Azure AD sınama çoklu oturum açmayı.

## <a name="add-hr2day-by-merces-from-the-gallery"></a>Galeriden Merces tarafından HR2day Ekle
Azure AD tarafından Merces HR2day tümleştirilmesi yapılandırmak için HR2day Merces tarafından Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

**Galeriden Merces tarafından HR2day eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **HR2day Merces tarafından**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. Sonuçlar panelinde seçin **Merces tarafından HR2day**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Merces tarafından HR2day ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de HR2day Merces tarafından karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, Merces tarafından HR2day içinde bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Merces tarafından HR2day içinde atamak **kullanıcı adı** için Azure AD'de **kullanıcıadı** ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Merces tarafından HR2day ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. [Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on): Bu özelliği kullanmak etkinleştirin.
2. [Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user): Test Azure AD çoklu oturum açma Britta Simon ile.
3. [Merces test kullanıcı tarafından bir HR2day oluşturma](#creating-an-hr2day-by-merces-test-user): Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Merces tarafından HR2day oluşturun.
4. [Azure AD test kullanıcısı atayın](#assigning-the-azure-ad-test-user): Azure AD çoklu oturum açma kullanmak için Britta Simon etkinleştirin.
5. [Test çoklu oturum açma](#testing-single-sign-on): yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, HR2day Merces uygulama tarafından yapılandırın.

**Azure AD çoklu oturum açma Merces tarafından HR2day yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Merces tarafından HR2day** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. İçinde **HR2day Merces etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. İçinde **oturum açma URL'si** kutusuna şu biçimi kullanarak bir URL yazın: `https://<tenantname>.force.com/<instancename>`.

    b. İçinde **tanımlayıcısı** kutusuna şu biçimi kullanarak bir URL yazın: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [HR2day Merces istemci destek ekibi tarafından](mailto:servicedesk@merces.nl) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** bölümünde, select **Certificate(Base64)** ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. Bu bölümde, kullanıcıların kendi hesabıyla Merces tarafından HR2day için Azure AD içinde kimlik doğrulaması sağlamak açıklar. Bunlar SAML protokolünü temel Federasyon kullanarak yapın.

    Merces uygulama tarafından HR2day SAML onaylar, SAML belirteci özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü, bunun bir örneğini gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    SAML onayı yapılandırmadan önce başvurmalıdır [HR2day Merces istemci destek ekibi tarafından](mailto:servicedesk@merces.nl) ve kiracınız için benzersiz tanımlayıcı özniteliği değeri isteyin. Sonraki bölümde yer alan adımları tamamlamak için bu değeri gerekir. 

6. İçinde **çoklu oturum açma** iletişim kutusunda **kullanıcı öznitelikleri** bölümünde, aşağıdaki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın. Ardından aşağıdaki adımları uygulayın.
    
      | Öznitelik adı    |   Öznitelik değeri |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | `join([mail],"102938475Z","@"` |
    
      a. Açmak için **özniteliği eklemek** iletişim kutusunda **Ekle özniteliği**.

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** kutusuna **ATTR_LOGINCLAIM**.

    c. Gelen **değeri** listesinde **Join()**.

    d. Gelen **Dize1** listesinde **user.mail**.

    e. İçin **dize2**, HR2day ekibiniz tarafından sunulan benzersiz bir tanımlayıcı yazın.

    f. İçinde **ayırıcı** kutusuna **\@**.
    
    g. Seçin **Tamam**.

7. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_general_400.png)

8. İçinde **Merces yapılandırma tarafından HR2day** bölümünde, select **yapılandırma HR2day Merces tarafından** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. Uygulamanız için SSO yapılandırmak için ilgili kişi [HR2day Merces istemci destek ekibi tarafından](mailTo:servicedesk@merces.nl). İndirilen attach **Certificate(Base64)** e-postanıza dosya. Ayrıca sağlamak **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** böylece SSO tümleştirmesi için yapılandırılmış.

    > [!NOTE]
    >Bu tümleştirme desenle ayarlanacak varlık kimliği gerektiğini Merces ekibine Bahsediyor **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesi. Katıştırılmış belgeleri aracılığıyla erişim **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. İçinde **Azure portal**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/hr2day-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola**ve ardından parolayı not alın.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Bir HR2day tarafından Merces test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde HR2day tarafından Merces adlı bir kullanıcı oluşturmaktır. HR2day hesap kullanıcılar eklemek için çalışmak [HR2day Merces istemci destek ekibi tarafından](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [HR2day Merces istemci destek ekibi tarafından](mailto:servicedesk@merces.nl).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Merces tarafından HR2day için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**HR2day Merces tarafından Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümü açmak için dizin görünümüne gidin ve ardından Git **kurumsal uygulamalar**. Ardından, **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **HR2day Merces tarafından**.

    ![Çoklu oturum açmayı yapılandırın](./media/hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Seçin **Ekle** düğmesi. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **kullanıcılar** listesinde **Britta Simon**.

6. Tıklatın **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  

Erişim paneli Merces parçasında tarafından HR2day seçtiğinizde, otomatik olarak, HR2day Merces uygulama tarafından oturum.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ilgili öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/hr2day-tutorial/tutorial_general_01.png
[2]: ./media/hr2day-tutorial/tutorial_general_02.png
[3]: ./media/hr2day-tutorial/tutorial_general_03.png
[4]: ./media/hr2day-tutorial/tutorial_general_04.png

[100]: ./media/hr2day-tutorial/tutorial_general_100.png

[200]: ./media/hr2day-tutorial/tutorial_general_200.png
[201]: ./media/hr2day-tutorial/tutorial_general_201.png
[202]: ./media/hr2day-tutorial/tutorial_general_202.png
[203]: ./media/hr2day-tutorial/tutorial_general_203.png

