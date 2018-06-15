---
title: 'Öğretici: Azure Active Directory Tümleştirme ile moconavi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile moconavi arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: efdd4ba6ca8fbe7db81c7ce6061cf17314933955
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728061"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Öğretici: Azure Active Directory Tümleştirme moconavi ile

Bu öğreticide, Azure Active Directory (Azure AD) ile moconavi tümleştirmek öğrenin.

Moconavi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Moconavi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Azure AD hesaplarına sahip (çoklu oturum açma) moconavi için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme moconavi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir moconavi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden moconavi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-moconavi-from-the-gallery"></a>Galeriden moconavi ekleme
Azure AD moconavi tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden moconavi eklemeniz gerekir.

**Galeriden moconavi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **moconavi**seçin **moconavi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde moconavi](./media/active-directory-saas-moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı moconavi sınayın.

Tekli çalışmaya oturum için Azure AD moconavi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının moconavi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma moconavi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Moconavi test kullanıcısı oluşturma](#create-a-moconavi-test-user)**  - bir Britta Simon karşılık gelen kullanıcı Azure AD gösterimini bağlı moconavi içinde olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma moconavi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile moconavi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **moconavi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. Üzerinde **moconavi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![moconavi etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-moconavi-tutorial/tutorial_moconavi_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<yourserverurl>/moconavi-saml2`

    C. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. Kişi [moconavi istemci destek ekibi](mailto:support@recomot.co.jp) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-moconavi-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **moconavi** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [moconavi destek ekibi](mailto:support@recomot.co.jp). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-moconavi-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-moconavi-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-moconavi-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-moconavi-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-moconavi-test-user"></a>Moconavi test kullanıcısı oluşturma

Bu bölümde, moconavi içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [moconavi destek ekibi](mailto:support@recomot.co.jp) moconavi platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta moconavi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Moconavi için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **moconavi**.

    ![Uygulamalar listesinde moconavi bağlantı](./media/active-directory-saas-moconavi-tutorial/tutorial_moconavi_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

1. Moconavi Microsoft Mağazası'ndan yükleyin.

2. Moconavi başlatın.

3. Tıklatın **bağlanma ayarını** düğmesi.

    ![Çoklu oturum açmayı test etme](./media/active-directory-saas-moconavi-tutorial/testing1.png)

4. Girin `https://mcs-admin.moconavi.biz/gateway` içine **URL Bağlan** metin kutusuna ve ardından **Bitti** düğmesi.

    ![Çoklu oturum açmayı test etme](./media/active-directory-saas-moconavi-tutorial/testing2.png)

5. Aşağıdaki ekran görüntüsü üzerinde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı test etme](./media/active-directory-saas-moconavi-tutorial/testing3.png)

    a. Girin **giriş kimlik doğrulama anahtarı**:`azureAD` içine **giriş kimlik doğrulama anahtarı** metin kutusu.

    b. Girin **giriş kullanıcı kimliği**: `your ad account` içine **giriş kullanıcı kimliği** metin kutusu.

    c. Tıklatın **oturum açma**.

6. Azure AD parolanızı giriş **parola** metin kutusuna ve ardından **oturum açma** düğmesi.

    ![Çoklu oturum açmayı test etme](./media/active-directory-saas-moconavi-tutorial/testing4.png)

7. Azure AD kimlik doğrulama başarılı olur menüsü görüntülenir.

    ![Çoklu oturum açmayı test etme](./media/active-directory-saas-moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moconavi-tutorial/tutorial_general_203.png

