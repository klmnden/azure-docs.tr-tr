---
title: 'Öğretici: Azure Active Directory temel beceriler ile tümleştirmesi | Microsoft Docs'
description: Azure Active Directory temel beceriler arasındaki çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 237d90c4-8243-4f80-a305-b5ad9204159e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bc0353453cf5fe689eec398f6a7d73fb356b178
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190849"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-base"></a>Öğretici: Azure Active Directory temel beceriler ile tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile temel becerileri tümleştirme konusunda bilgi edinin.

Azure AD ile temel becerileri tümleştirme ile aşağıdaki avantajları sağlar:

- Beceriler temel erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) temel becerileri açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile temel becerileri yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir temel becerileri çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden temel yetenekleri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-skills-base-from-the-gallery"></a>Galeriden temel yetenekleri ekleme
Azure AD temel becerileri tümleştirilmesi yapılandırmak için temel becerileri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden becerileri temel eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **becerileri temel**seçin **becerileri temel** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde temel becerileri](./media/skillsbase-tutorial/tutorial_skillsbase_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve becerileri "Britta Simon" adlı bir test kullanıcıya dayanarak temel Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD temel becerileri karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı becerileri Base arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile becerilerinizi temel test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Temel becerileri test kullanıcısı oluşturma](#create-a-skills-base-test-user)**  - kullanıcı Azure AD gösterimini bağlı becerileri Base Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve becerileri temel uygulamanızda çoklu oturum açma yapılandırın.

**Azure AD çoklu oturum açma ile temel becerileri yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **becerileri temel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/skillsbase-tutorial/tutorial_skillsbase_samlbase.png)

3. Üzerinde **becerileri temel etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Beceriler temel etki alanı ve URL'ler tek oturum açma bilgileri](./media/skillsbase-tutorial/tutorial_skillsbase_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://app.skills-base.com/o/<customer-unique-key>`

    > [!NOTE] 
    > Temel becerileri uygulamasından oturum açma URL'sini alabilirsiniz. Lütfen yönetici gidin ve yönetici olarak oturum açma -> Ayarlar -> örneği Ayrıntılar -> kısayol bağlantısı. Oturum açma URL'sini kopyalayın ve metin kutusuna yapıştırın.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/skillsbase-tutorial/tutorial_skillsbase_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/skillsbase-tutorial/tutorial_general_400.png)

6. Bir başka web tarayıcı penceresinde becerileri temel bir güvenlik yöneticisi olarak oturum açın.

7. Menü, sol tarafındaki altında **yönetici** tıklayın **kimlik doğrulaması**.

    ![Yönetici](./media/skillsbase-tutorial/tutorial_skillsbase_auth.png)

8. Üzerinde **kimlik doğrulaması** seçin sayfasında, çoklu oturum açma olarak **SAML 2**.

    ![Tek](./media/skillsbase-tutorial/tutorial_skillsbase_single.png)

9. Üzerinde **kimlik doğrulaması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Tek](./media/skillsbase-tutorial/tutorial_skillsbase_save.png)

    a. Tıklayarak **güncelleştirme IDP meta verileri** düğmesinin yanındaki **durumu** seçenek ve belirtilen metin kutusuna Azure portalından indirdiğiniz bir meta veri XML içeriğini kopyalayabilir.

    > [!Note]
    > IDP meta veriler üzerinden de doğrulayabilirsiniz **meta verileri Doğrulayıcı** aracı olarak yukarıdaki vurgulanmış ekran görüntüsü.

    b. **Kaydet**’e tıklayın.
    
### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/skillsbase-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/skillsbase-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/skillsbase-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/skillsbase-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-skills-base-test-user"></a>Temel becerileri test kullanıcısı oluşturma

Bu bölümün amacı temel becerileri Britta Simon adlı bir kullanıcı oluşturmaktır. Temel becerileri tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse, temel becerileri erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, yönergeleri [burada](http://wiki.skills-base.net/index.php?title=Adding_people_and_enabling_them_to_log_in).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, becerileri Bankası'na erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon becerileri tabanına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **becerileri temel**.

    ![Uygulamalar listesinde becerileri temel bağlantı](./media/skillsbase-tutorial/tutorial_skillsbase_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli becerileri temel kutucuğa tıkladığınızda, size otomatik olarak becerileri temel uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsbase-tutorial/tutorial_general_01.png
[2]: ./media/skillsbase-tutorial/tutorial_general_02.png
[3]: ./media/skillsbase-tutorial/tutorial_general_03.png
[4]: ./media/skillsbase-tutorial/tutorial_general_04.png

[100]: ./media/skillsbase-tutorial/tutorial_general_100.png

[200]: ./media/skillsbase-tutorial/tutorial_general_200.png
[201]: ./media/skillsbase-tutorial/tutorial_general_201.png
[202]: ./media/skillsbase-tutorial/tutorial_general_202.png
[203]: ./media/skillsbase-tutorial/tutorial_general_203.png

