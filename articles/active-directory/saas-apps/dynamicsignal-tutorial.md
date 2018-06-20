---
title: 'Öğretici: Azure Active Directory Tümleştirme dinamik sinyali | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve dinamik sinyal arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 863f7340-b065-4f59-b092-daa67da6f703
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: bd99bb452f39bd99f0d92e478b21d7f3786ed785
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36223720"
---
# <a name="tutorial-azure-active-directory-integration-with-dynamic-signal"></a>Öğretici: Azure Active Directory Tümleştirme dinamik sinyali

Bu öğreticide, Azure Active Directory (Azure AD) ile dinamik sinyal tümleştirmek öğrenin.

Dinamik sinyal Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Dinamik sinyal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak dinamik sinyale (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile dinamik sinyal yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir dinamik sinyal çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden dinamik sinyal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-dynamic-signal-from-the-gallery"></a>Galeriden dinamik sinyal ekleme
Azure AD dinamik sinyal tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden dinamik sinyal eklemeniz gerekir.

**Galeriden dinamik sinyal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **dinamik sinyal**seçin **dinamik sinyal** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde dinamik sinyali](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma dinamik "Britta Simon" adlı bir test kullanıcı tabanlı sinyal sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen dinamik sinyal içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının dinamik sinyal ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırmak ve Azure AD çoklu oturum açma dinamik sinyali sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dinamik sinyal test kullanıcısı oluşturma](#create-a-dynamic-signal-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlantılı dinamik sinyal sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma dinamik sinyal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile dinamik sinyal yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **dinamik sinyal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_samlbase.png)

3. Üzerinde **dinamik sinyal etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:
 
    ![Oturum açma bilgileri tek bir dinamik sinyal etki alanı ve URL'leri](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.voicestorm.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.voicestorm.com`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.voicestorm.com/User/SsoResponse`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [dinamik sinyal istemci destek ekibi](mailto:support@dynamicsignal.com) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/dynamicsignal-tutorial/tutorial_general_400.png)
    
6. Üzerinde **dinamik sinyal yapılandırma** 'yi tıklatın **yapılandırma dinamik sinyal** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Dinamik sinyal yapılandırma](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_configure.png) 

7. Çoklu oturum açma yapılandırmak için **dinamik sinyal** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [dinamik Sinyal destek ekibi](mailto:support@dynamicsignal.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dynamicsignal-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/dynamicsignal-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dynamicsignal-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dynamicsignal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dynamic-signal-test-user"></a>Dinamik sinyal test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon dinamik sinyalin adlı bir kullanıcı oluşturmaktır. Dinamik sinyal yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa dinamik sinyal erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [dinamik sinyal destek ekibi](mailto:support@dynamicsignal.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, dinamik sinyale erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Dinamik sinyal Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **dinamik sinyal**.

    ![Uygulamalar listesinde dinamik sinyal bağlantı](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli dinamik sinyal parçasında tıklattığınızda, otomatik olarak dinamik sinyal uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dynamicsignal-tutorial/tutorial_general_01.png
[2]: ./media/dynamicsignal-tutorial/tutorial_general_02.png
[3]: ./media/dynamicsignal-tutorial/tutorial_general_03.png
[4]: ./media/dynamicsignal-tutorial/tutorial_general_04.png

[100]: ./media/dynamicsignal-tutorial/tutorial_general_100.png

[200]: ./media/dynamicsignal-tutorial/tutorial_general_200.png
[201]: ./media/dynamicsignal-tutorial/tutorial_general_201.png
[202]: ./media/dynamicsignal-tutorial/tutorial_general_202.png
[203]: ./media/dynamicsignal-tutorial/tutorial_general_203.png

