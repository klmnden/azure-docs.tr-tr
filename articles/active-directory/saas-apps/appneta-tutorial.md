---
title: 'Öğretici: Azure Active Directory Tümleştirme AppNeta Performans İzleyicisi ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory AppNeta Performans İzleyicisi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: 4899c6d27c034ce3f92efc3b0ddfb8b3446a6115
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229682"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Öğretici: Azure Active Directory Tümleştirme AppNeta Performans İzleyicisi ile

Bu öğreticide, Azure Active Directory (Azure AD) ile AppNeta Performans İzleyicisi'ni tümleştirmek öğrenin.

AppNeta Performans İzleyicisi'ni Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- AppNeta Performans İzleyicisi'ni erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) AppNeta Performans İzleyicisi açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme AppNeta Performans İzleyicisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir AppNeta Performans İzleyicisi'ni çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden AppNeta Performans İzleyicisi'ni ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Galeriden AppNeta Performans İzleyicisi'ni ekleme
Azure AD AppNeta Performans İzleyicisi'ni tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden AppNeta Performans İzleyicisi'ni eklemeniz gerekir.

**Galeriden AppNeta Performans İzleyicisi'ni eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **AppNeta Performans İzleyicisi'ni**seçin **AppNeta Performans İzleyicisi'ni** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde AppNeta Performans İzleyicisi](./media/appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı AppNeta Performans İzleyicisi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen AppNeta Performans İzleyicisi'nde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının AppNeta Performans İzleyicisi'nde ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AppNeta Performans İzleyicisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma](#create-an-appneta-performance-monitor-test-user)**  - kullanıcı Azure AD gösterimini bağlı AppNeta Performans İzleyicisi'nde Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AppNeta Performans İzleyicisi'ni uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma AppNeta Performans İzleyicisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **AppNeta Performans İzleyicisi'ni** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/appneta-tutorial/tutorial_appneta_samlbase.png)

3. Üzerinde **AppNeta Performans İzleyicisi'ni etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AppNeta Performans İzleyicisi'ni etki alanı ve URL'leri tek oturum açma bilgileri](./media/appneta-tutorial/tutorial_appneta_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.pm.appneta.com`

    b. İçinde **tanımlayıcısı** metin değeri yazın: `PingConnect`

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [AppNeta Performans İzleyicisi'ni istemci destek ekibi](mailto:support@appneta.com) bu değeri alınamıyor. 

5. AppNeta Performans İzleyicisi'ni uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm.

    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
           
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | FirstName| User.givenName|
    | Soyadı| User.surname|
    | e-posta| User.userPrincipalName|
    | ad| User.userPrincipalName|
    | gruplar   | User.assignedroles |
    | telefon| User.telephoneNumber |
    | başlık| user.jobtitle|

    > [!NOTE]
    > 'Gruplar', 'Role' Azure AD'de eşlenen Appneta güvenlik grubunda ifade eder. Lütfen [bu](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) belge, Azure AD'de özel roller oluşturma açıklanmaktadır.
        
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı boş bırakın.
    
    e. **Tamam**’a tıklayın.  

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/appneta-tutorial/tutorial_appneta_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/appneta-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **AppNeta Performans İzleyicisi'ni** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [AppNeta Performans İzleyicisi'ni destek ekibi](mailto:support@appneta.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/appneta-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/appneta-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/appneta-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/appneta-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Bir AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon AppNeta Performans İzleyicisi'nde adlı bir kullanıcı oluşturmaktır. AppNeta Performans İzleyicisi'ni yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa AppNeta Performans İzleyicisi'ni erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [AppNeta Performans İzleyicisi'ni destek ekibi](mailto:support@appneta.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta AppNeta Performans İzleyicisi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon AppNeta Performans İzleyicisi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **AppNeta Performans İzleyicisi'ni**.

    ![Uygulamalar listesinde AppNeta Performans İzleyicisi'ni bağlantı](./media/appneta-tutorial/tutorial_appneta_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AppNeta Performans İzleyicisi'ni parçasında tıklattığınızda, otomatik olarak AppNeta Performans İzleyicisi'ni uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appneta-tutorial/tutorial_general_01.png
[2]: ./media/appneta-tutorial/tutorial_general_02.png
[3]: ./media/appneta-tutorial/tutorial_general_03.png
[4]: ./media/appneta-tutorial/tutorial_general_04.png

[100]: ./media/appneta-tutorial/tutorial_general_100.png

[200]: ./media/appneta-tutorial/tutorial_general_200.png
[201]: ./media/appneta-tutorial/tutorial_general_201.png
[202]: ./media/appneta-tutorial/tutorial_general_202.png
[203]: ./media/appneta-tutorial/tutorial_general_203.png

