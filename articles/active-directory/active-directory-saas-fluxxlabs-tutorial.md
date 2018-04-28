---
title: 'Öğretici: Azure Active Directory Tümleştirme Fluxx Labs ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Fluxx Labs arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: jeedes
ms.openlocfilehash: 0bba820c14c5eddc6db99923e3fb1de58c110f4c
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Öğretici: Azure Active Directory Tümleştirme ile Fluxx Laboratuvarları

Bu öğreticide, Azure Active Directory (Azure AD) ile Fluxx Labs tümleştirmek öğrenin.

Fluxx Labs Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Fluxx Labs erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Fluxx Labs açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Fluxx Labs ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Fluxx Labs çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Fluxx Labs ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Fluxx Labs ekleme
Azure AD Fluxx Labs tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Fluxx Labs eklemeniz gerekir.

**Galeriden Fluxx Labs eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Fluxx Labs**seçin **Fluxx Labs** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Fluxx Laboratuvarları](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma Fluxx "Britta Simon" adlı bir test kullanıcı tabanlı Labs sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Fluxx ortamlarındaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Fluxx ortamlarındaki arasında bir bağlantı ilişkisi kurulması gerekir.

Fluxx Labs'de değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Fluxx Labs ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Fluxx Labs test kullanıcısı oluşturma](#create-a-fluxx-labs-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Fluxx Labs sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Fluxx Labs uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Fluxx Labs ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Fluxx Labs** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

3. Üzerinde **Fluxx Labs etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Fluxx Labs etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    | Ortam | URL deseni|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|
        
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Fluxx Labs destek ekibi](mailto:travis@fluxxlabs.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_400.png)

6. Üzerinde **Fluxx Labs yapılandırma** 'yi tıklatın **yapılandırma Fluxx Labs** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

7. Farklı web tarayıcısı penceresinde Fluxx Labs şirket sitenize yönetici olarak oturum açma.

8. Seçin **yönetici** aşağıda **ayarları** bölümü.

    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config1.png)

9. Yönetici panelinde seçin **eklentileri** > **tümleştirmeler** ve ardından **SAML SSO-(Disabled)**

    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config2.png)
    
10. Öznitelik bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config3.png)

    a. Seçin **SAML SSO** onay kutusu.

    b. İçinde **istek yolu** metin kutusuna, türü **/auth/saml**.

    c. İçinde **geri çağırma yolu** metin kutusuna, türü **/auth/saml/callback**.

    d. İçinde **onaylama tüketici hizmet Url(Single Sign-On URL)** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    e. İçinde **İzleyici (SP varlık kimliği)** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    f. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası** metin kutusu.

    g. İçinde **ad tanımlayıcısı biçimi** metin değeri girin `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    h. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Kaydedilmiş içeriği alanın bir kez güvenlik için boş görünür, ancak değeri yapılandırmada kaydedildi.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-fluxxlabs-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-fluxxlabs-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-fluxxlabs-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-fluxxlabs-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-fluxx-labs-test-user"></a>Fluxx Labs test kullanıcısı oluşturma

Fluxx Labs oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların Fluxx Labs sağlanmalıdır. Fluxx Labs söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Fluxx Labs şirket sitenize yönetici olarak oturum açın.

2. Tıklayın aşağıda gösterilen **simgesi**.

    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config6.png)

3. Panoda tıklayın açmak için görüntülenen simgesinin altında **yeni kişiler** kart.

    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config4.png)

4. Üzerinde **yeni kişiler** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Fluxx Labs yapılandırma](./media/active-directory-saas-fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs e-posta SSO oturumları için benzersiz tanımlayıcı olarak kullanın. Doldurmak **SSO UID** alan SSO ile oturum açma olarak kullanarak e-posta adresiyle eşleşen kullanıcının e-posta adresine sahip.

    b. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Fluxx Labs erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Fluxx Labs atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Fluxx Labs**.

    ![Uygulamalar listesinde Fluxx Labs bağlantı](./media/active-directory-saas-fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Fluxx Labs parçasında tıklattığınızda, otomatik olarak Fluxx Labs uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fluxxlabs-tutorial/tutorial_general_203.png
