---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Dovetale | Microsoft Docs'
description: Azure Active Directory ve Dovetale arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81da50c3-df94-458a-8b6a-a30827ee6358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: dd2aa699c22518ebbe7f29ba8dfd296da7385476
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "40162228"
---
# <a name="tutorial-azure-active-directory-integration-with-dovetale"></a>Öğretici: Azure Active Directory Dovetale ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Dovetale tümleştirme konusunda bilgi edinin.

Azure AD ile Dovetale tümleştirme ile aşağıdaki avantajları sağlar:

- Dovetale erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Dovetale (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Dovetale yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Dovetale çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Dovetale ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-dovetale-from-the-gallery"></a>Galeriden Dovetale ekleme

Azure AD'de Dovetale tümleştirmesini yapılandırmak için Dovetale Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Dovetale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Dovetale**seçin **Dovetale** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Dovetale](./media/dovetale-tutorial/tutorial_dovetale_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Dovetale sınayın.

Tek iş için oturum açma için Azure AD ne Dovetale karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Dovetale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Dovetale ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dovetale test kullanıcısı oluşturma](#create-a-dovetale-test-user)**  - kullanıcı Azure AD gösterimini bağlı Dovetale Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Dovetale uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Dovetale yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Dovetale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/dovetale-tutorial/tutorial_dovetale_samlbase.png)

3. Üzerinde **Dovetale etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, kullanıcı uygulamaya zaten Azure ile önceden tümleştirilmiş olduğu gibi tüm adımları gerçekleştirmek sahip değil:

    ![Dovetale etki alanı ve URL'ler tek oturum açma bilgileri](./media/dovetale-tutorial/tutorial_dovetale_url1.png)

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Dovetale etki alanı ve URL'ler tek oturum açma bilgileri](./media/dovetale-tutorial/tutorial_dovetale_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `<COMPANYNAME>.dovetale.com`

    > [!NOTE]
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Dovetale istemci Destek ekibine](mailto:support@dovetale.com) bu değeri alınamıyor.

5. Dovetale uygulama belirli bir biçimde SAML onaylamalarını bekler. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/dovetale-tutorial/tutorial_attribute.png)

6. Tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu **kullanıcı öznitelikleri** bölüm öznitelikleri genişletin. Her birini görüntülenen öznitelikleri - aşağıdaki adımları gerçekleştirin

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |
    | e-posta | User.Mail |
    | first_name | User.givenName |
    | ad | User.userPrincipalName |
    | Soyadı | User.surname |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/dovetale-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/dovetale-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna **öznitelik adı** ilgili satır için gösterilen.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **ad alanı** değeri boş.

    e. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/dovetale-tutorial/tutorial_dovetale_certificate.png) 

8. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/dovetale-tutorial/tutorial_general_400.png)

9. Üzerinde **Dovetale yapılandırma** bölümünde **yapılandırma Dovetale** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Dovetale yapılandırma](./media/dovetale-tutorial/tutorial_dovetale_configure.png)

10. Çoklu oturum açmayı yapılandırma **Dovetale** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta veri URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** için [Dovetale Destek ekibine](mailto:support@dovetale.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dovetale-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/dovetale-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dovetale-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dovetale-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-dovetale-test-user"></a>Dovetale test kullanıcısı oluşturma

Bu bölümün amacı Dovetale Britta Simon adlı bir kullanıcı oluşturmaktır. Dovetale tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Dovetale erişme denemesi sırasında oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Dovetale Destek ekibine](mailto:support@dovetale.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Dovetale erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Dovetale için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Dovetale**.

    ![Uygulamalar listesinde Dovetale bağlantı](./media/dovetale-tutorial/tutorial_dovetale_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Dovetale kutucuğa tıkladığınızda, otomatik olarak Dovetale uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/dovetale-tutorial/tutorial_general_01.png
[2]: ./media/dovetale-tutorial/tutorial_general_02.png
[3]: ./media/dovetale-tutorial/tutorial_general_03.png
[4]: ./media/dovetale-tutorial/tutorial_general_04.png

[100]: ./media/dovetale-tutorial/tutorial_general_100.png

[200]: ./media/dovetale-tutorial/tutorial_general_200.png
[201]: ./media/dovetale-tutorial/tutorial_general_201.png
[202]: ./media/dovetale-tutorial/tutorial_general_202.png
[203]: ./media/dovetale-tutorial/tutorial_general_203.png