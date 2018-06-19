---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Proxyclick | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Proxyclick arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.openlocfilehash: 3fb6c034df38486dfefd0ffccc6e05db32d9087d
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972331"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Öğretici: Azure Active Directory Tümleştirme Proxyclick ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Proxyclick tümleştirmek öğrenin.

Proxyclick Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Proxyclick erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Proxyclick (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Proxyclick ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Proxyclick çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Proxyclick ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-proxyclick-from-the-gallery"></a>Galeriden Proxyclick ekleme
Azure AD Proxyclick tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Proxyclick eklemeniz gerekir.

**Galeriden Proxyclick eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Proxyclick**seçin **Proxyclick** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Proxyclick](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Proxyclick sınayın.

Tekli çalışmaya oturum için Azure AD Proxyclick karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Proxyclick ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Proxyclick ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Proxyclick test kullanıcısı oluşturma](#create-a-proxyclick-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Proxyclick sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Proxyclick uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Proxyclick yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Proxyclick** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

3. Üzerinde **Proxyclick etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Proxyclick etki alanı ve URL'leri tek oturum açma bilgileri](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://saml.proxyclick.com/init/<companyId>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://saml.proxyclick.com/consume/<companyId>`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Proxyclick etki alanı ve URL'leri tek oturum açma bilgileri](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticide daha sonra açıklanan URL ile güncelleştirir.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/proxyclick-tutorial/tutorial_general_400.png)

7. Üzerinde **Proxyclick yapılandırma** 'yi tıklatın **yapılandırma Proxyclick** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

8. Farklı web tarayıcısı penceresinde Proxyclick şirket sitenize yönetici olarak oturum açın.

9. Seçin **hesap & ayarları**.

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure1.png)

10. Ekranı aşağı kaydırarak **TÜMLEŞTİRMELER** seçip **SAML**.

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure2.png)

11. İçinde **SAML** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure3.png)

    a. Kopya **SAML tüketici URL** değer ve yapıştırın **yanıt URL'si** metin kutusuna **Proxyclick etki alanı ve URL'leri** Azure Portal'daki bölümü.

    b. Kopyalama **SAML SSO tekrar yönlendirme URL'sini** değer ve yapıştırın **URL üzerinde oturum** ve **tanımlayıcısı** metin kutuları içinde **Proxyclick etki alanı ve URL'leri** bölümü Azure Portal'da.

    c. Seçin **SAML istek yöntemi** olarak **HTTP yeniden yönlendirme**.

    d. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyaladığınız değeri.

    e. İçinde **SAML 2.0 uç nokta URL'si** metin kutusuna, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanır.

    f. Not Defteri'nde Azure Portalı'ndan indirilen sertifika dosyanızı açın ve ardından yapıştırın **sertifika** metin kutusu.

    g. Tıklatın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/proxyclick-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/proxyclick-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/proxyclick-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-proxyclick-test-user"></a>Proxyclick test kullanıcısı oluşturma

Azure AD kullanıcıları için Proxyclick oturum açmak etkinleştirmek için bunların Proxyclick sağlanmalıdır. Proxyclick söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Proxyclick şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **iş arkadaşlarınızı** üst gezinti çubuğunda.

    ![Çalışanı ekleyin](./media/proxyclick-tutorial/user1.png)

3. Tıklatın **iş arkadaşı ekleyin**

    ![Çalışanı ekleyin](./media/proxyclick-tutorial/user2.png)

4. İçinde **bir iş arkadaşı eklemek** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/proxyclick-tutorial/user3.png)

    a. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister **brittasimon@contoso.com**.

    b. İçinde **ad** metin kutusu, kullanıcı Britta gibi ilk türünün adı.

    c. İçinde **Soyadı** metin kutusuna, son kullanıcı Simon gibi yazın.

    d. Tıklatın **kullanıcı ekleme**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Proxyclick için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Proxyclick için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Proxyclick**.

    ![Uygulamalar listesinde Proxyclick bağlantı](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Proxyclick parçasında tıklattığınızda, otomatik olarak Proxyclick uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/proxyclick-tutorial/tutorial_general_01.png
[2]: ./media/proxyclick-tutorial/tutorial_general_02.png
[3]: ./media/proxyclick-tutorial/tutorial_general_03.png
[4]: ./media/proxyclick-tutorial/tutorial_general_04.png

[100]: ./media/proxyclick-tutorial/tutorial_general_100.png

[200]: ./media/proxyclick-tutorial/tutorial_general_200.png
[201]: ./media/proxyclick-tutorial/tutorial_general_201.png
[202]: ./media/proxyclick-tutorial/tutorial_general_202.png
[203]: ./media/proxyclick-tutorial/tutorial_general_203.png

