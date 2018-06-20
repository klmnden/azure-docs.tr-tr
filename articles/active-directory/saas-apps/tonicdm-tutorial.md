---
title: 'Öğretici: Azure Active Directory Tümleştirme ile TonicDM | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TonicDM arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 19ea9a07-9ecf-43dc-91ba-593ce3c00b01
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: jeedes
ms.openlocfilehash: 83402821d49c85849bce417615b04b0393cd061a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229409"
---
# <a name="tutorial-azure-active-directory-integration-with-tonicdm"></a>Öğretici: Azure Active Directory Tümleştirme TonicDM ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TonicDM tümleştirmek öğrenin.

TonicDM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TonicDM erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için TonicDM (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme TonicDM ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TonicDM çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TonicDM ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-tonicdm-from-the-gallery"></a>Galeriden TonicDM ekleme
Azure AD TonicDM tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TonicDM eklemeniz gerekir.

**Galeriden TonicDM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TonicDM**seçin **TonicDM** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde TonicDM](./media/tonicdm-tutorial/tutorial_tonicdm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TonicDM sınayın.

Tekli çalışmaya oturum için Azure AD TonicDM karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TonicDM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TonicDM ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TonicDM test kullanıcısı oluşturma](#create-a-tonicdm-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TonicDM sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TonicDM uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TonicDM yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TonicDM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/tonicdm-tutorial/tutorial_tonicdm_samlbase.png)

3. Üzerinde **TonicDM etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TonicDM etki alanı ve URL'leri tek oturum açma bilgileri](./media/tonicdm-tutorial/tutorial_tonicdm_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın: `https://tonicdm.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://tonicdm.com/saml/metadata`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/tonicdm-tutorial/tutorial_tonicdm_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/tonicdm-tutorial/tutorial_general_400.png)

6. Üzerinde **TonicDM yapılandırma** 'yi tıklatın **yapılandırma TonicDM** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TonicDM yapılandırma](./media/tonicdm-tutorial/tutorial_tonicdm_configure.png) 

7. Çoklu oturum açma yapılandırmak için **TonicDM** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [TonicDM desteği Takım](mailto:support@tonicdm.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/tonicdm-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/tonicdm-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/tonicdm-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/tonicdm-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-tonicdm-test-user"></a>TonicDM test kullanıcısı oluşturma

Bu bölümde, TonicDM içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [TonicDM destek ekibi](mailto:support@tonicdm.com) TonicDM platform kullanıcıları eklemek için. Kullanıcıların gerekir ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TonicDM için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**TonicDM için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TonicDM**.

    ![Uygulamalar listesinde TonicDM bağlantı](./media/tonicdm-tutorial/tutorial_tonicdm_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TonicDM parçasında tıklattığınızda, otomatik olarak TonicDM uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tonicdm-tutorial/tutorial_general_01.png
[2]: ./media/tonicdm-tutorial/tutorial_general_02.png
[3]: ./media/tonicdm-tutorial/tutorial_general_03.png
[4]: ./media/tonicdm-tutorial/tutorial_general_04.png

[100]: ./media/tonicdm-tutorial/tutorial_general_100.png

[200]: ./media/tonicdm-tutorial/tutorial_general_200.png
[201]: ./media/tonicdm-tutorial/tutorial_general_201.png
[202]: ./media/tonicdm-tutorial/tutorial_general_202.png
[203]: ./media/tonicdm-tutorial/tutorial_general_203.png

