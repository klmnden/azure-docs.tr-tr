---
title: 'Öğretici: Azure Active Directory Tümleştirme ile birleştirme çevrimiçi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile birleştirme çevrimiçi arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c5e8c6f-e4fb-43fe-8841-e371f568ebed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: e5cabbd26c978bc8bcdabeadfca31ae99c951558
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-montage-online"></a>Öğretici: Azure Active Directory Tümleştirme ile birleştirme çevrimiçi

Bu öğreticide, Azure Active Directory (Azure AD) ile birleştirme çevrimiçi tümleştirmek öğrenin.

Birleştirme çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Birleştirme çevrimiçi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak birleştirme Online'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme birleştirme Online ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir birleştirme çevrimiçi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Birleştirme çevrimiçi galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-montage-online-from-the-gallery"></a>Birleştirme çevrimiçi galeriden ekleme
Azure AD birleştirme Online tümleştirilmesi yapılandırmak için birleştirme çevrimiçi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Birleştirme çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **birleştirme çevrimiçi**seçin **birleştirme çevrimiçi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Çevrimiçi sonuçlar listesinde birleştirme](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma birleştirme "Britta Simon" adlı bir test kullanıcı tabanlı Online ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen birleştirme çevrimiçi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı birleştirme çevrimiçi arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma birleştirme Online ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Birleştirme çevrimiçi test kullanıcısı oluşturma](#create-a-montage-online-test-user)**  - birleştirme kullanıcı Azure AD gösterimini bağlantılı çevrimiçi Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma birleştirme çevrimiçi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma birleştirme Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **birleştirme çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_samlbase.png)

3. Üzerinde **birleştirme çevrimiçi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Birleştirme çevrimiçi etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    Üretim ortamı için: `https://<subdomain>.montageonline.co.nz/`

    Test ortamı için: `https://build-<subdomain>.montageonline.co.nz/`

    b. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın:

    Üretim ortamı için: `MOL_Azure`

    Test ortamı için: `MOL_Azure_Build`

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [birleştirme çevrimiçi istemci destek ekibi](https://www.montage.co.nz/contact-us/) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-montageonline-tutorial/tutorial_general_400.png)

6. Üzerinde **birleştirme çevrimiçi yapılandırma** 'yi tıklatın **birleştirme çevrimiçi yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Birleştirme çevrimiçi yapılandırma](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_configure.png) 

7. Çoklu oturum açma yapılandırmak için **birleştirme çevrimiçi** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [birleştirme Çevrimiçi destek ekibi](https://www.montage.co.nz/contact-us/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-montageonline-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-montageonline-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-montageonline-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-montageonline-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-montage-online-test-user"></a>Birleştirme çevrimiçi test kullanıcısı oluşturma

Bu bölümde, Britta Simon birleştirme çevrimiçi olarak adlandırılan bir kullanıcı oluşturun. Çalışmak [birleştirme çevrimiçi destek ekibi](https://www.montage.co.nz/contact-us/) birleştirme çevrimiçi platform kullanıcıları eklemek için. Kullanıcıların gerekir ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta birleştirme Online'a erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon birleştirme Online'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **birleştirme çevrimiçi**.

    ![Uygulamalar listesinde birleştirme çevrimiçi bağlantı](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde birleştirme çevrimiçi kutucuğa tıkladığınızda, otomatik olarak birleştirme çevrimiçi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_203.png

