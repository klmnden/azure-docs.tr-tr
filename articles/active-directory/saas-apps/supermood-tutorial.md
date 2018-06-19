---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Supermood | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Supermood arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 586518b6e82e1ae11f738bc57d69c67f04a9b279
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972240"
---
# <a name="tutorial-azure-active-directory-integration-with-supermood"></a>Öğretici: Azure Active Directory Tümleştirme Supermood ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Supermood tümleştirmek öğrenin.

Supermood Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Supermood erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Supermood (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Supermood ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Supermood çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Supermood ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-supermood-from-the-gallery"></a>Galeriden Supermood ekleme
Azure AD'ye Supermood tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Supermood eklemeniz gerekir.

**Galeriden Supermood eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Supermood**seçin **Supermood** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde supermood](./media/supermood-tutorial/tutorial_supermood_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Supermood sınayın.

Tekli çalışmaya oturum için Azure AD Supermood karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Supermood ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Supermood ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Supermood test kullanıcısı oluşturma](#create-a-supermood-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Supermood sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Supermood uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Supermood yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Supermood** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/supermood-tutorial/tutorial_supermood_samlbase.png)

3. Üzerinde **Supermood etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Supermood etki alanı ve URL'leri tek oturum açma bilgileri](./media/supermood-tutorial/tutorial_supermood_url.png)

    a. Denetleme **Göster Gelişmiş URL ayarları**.

    b. Uygulamada yapılandırmak istiyorsanız **IDP** modunda başlatılan **geçiş durumunu** metin kutusuna, bir URL yazın: `https://supermood.co/auth/sso/saml20`

    c. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan **oturum açma URL'si** metin kutusuna, bir URL yazın: `https://supermood.co/app/#!/loginv2`

4. Supermood uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/supermood-tutorial/tutorial_supermood_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | FirstName | User.givenName |
    | Soyadı | User.surname |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/supermood-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/supermood-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklatın **Tamam**

6. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/supermood-tutorial/tutorial_supermood_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/supermood-tutorial/tutorial_general_400.png)

8. Güvenlik Yöneticisi olarak Supermood.co yönetici Masası'na gidin.

9. Tıklayın **Hesabımı** (sol alt) ve **çoklu oturum açma (SSO)**.

    ![Tek sertifika](./media/supermood-tutorial/tutorial_supermood_single.png)
10. Üzerinde **bilgisayarınızı SAML 2.0 yapılandırmaları**, tıklatın **bir e-posta etki alanı için bir SAML 2.0 yapılandırması eklemek**.

    ![Sertifika Ekle](./media/supermood-tutorial/tutorial_supermood_add.png)

11. Üzerinde **bir e-posta etki alanı için bir SAML 2.0 yapılandırması eklemek**. bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Sertifika saml](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. İçinde **bu kimlik sağlayıcısı için e-posta etki alanı** metin kutusuna, etki alanınızın yazın.

    b. İçinde **bir meta veri URL'sini kullanmak** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portalından kopyalanan.

    c. **Ekle**'ye tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/supermood-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/supermood-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/supermood-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/supermood-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-supermood-test-user"></a>Supermood test kullanıcısı oluşturma

Bu bölümde, Supermood içinde Britta Simon adlı bir kullanıcı oluşturun. Supermood yalnızca zaman sağlama, varsayılan olarak, e-postaları Supermood sonunda yapılandırma sırasında eklenen etki alanlarına ait kullanıcılar için etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Supermood erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Supermood destek ekibi](mailto:hello@supermood.fr).


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Supermood için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Supermood için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Supermood**.

    ![Uygulamalar listesinde Supermood bağlantı](./media/supermood-tutorial/tutorial_supermood_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Supermood parçasında tıklattığınızda, otomatik olarak Supermood uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/supermood-tutorial/tutorial_general_01.png
[2]: ./media/supermood-tutorial/tutorial_general_02.png
[3]: ./media/supermood-tutorial/tutorial_general_03.png
[4]: ./media/supermood-tutorial/tutorial_general_04.png

[100]: ./media/supermood-tutorial/tutorial_general_100.png

[200]: ./media/supermood-tutorial/tutorial_general_200.png
[201]: ./media/supermood-tutorial/tutorial_general_201.png
[202]: ./media/supermood-tutorial/tutorial_general_202.png
[203]: ./media/supermood-tutorial/tutorial_general_203.png

