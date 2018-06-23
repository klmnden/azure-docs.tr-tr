---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ekran kaydı-O-Matic | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve ekran kaydı-O-Matic arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: 7212e0b07b525286f0b194a53c6780269630ad9c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320733"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Öğretici: Ekran kaydı-O-Matic Azure Active Directory Tümleştirme

Bu öğreticide, ekran kaydı-O-Matic Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Ekran kaydı-O-Matic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Ekran kaydı-O-Matic erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak ekran kaydı-O-Matic için (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ekran kaydı-O-Matic ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ekran kaydı-O-Matic çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Ekran kaydı-O-Matic Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Ekran kaydı-O-Matic Galeriden ekleme
Azure AD, ekran kaydı-O-Matic tümleştirilmesi yapılandırmak için ekran kaydı-O-Matic Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Ekran kaydı-O-Matic Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ekran kaydı-O-Matic**seçin **ekran kaydı-O-Matic** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Ekran kaydı-O-Matic sonuçlar listesinde](./media/screencast-tutorial/tutorial_screencast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ekran kaydı-O-"Britta Simon" adlı bir test kullanıcı tabanlı Matic ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen ekran kaydı-O-Matic içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı ekran kaydı-O-Matic arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ekran kaydı-O-Matic ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ekran kaydı-O-Matic test kullanıcısı oluşturma](#create-a-screencast-o-matic-test-user)**  - ekran kaydı-O-kullanıcı Azure AD gösterimini bağlantılı Matic Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ekran kaydı-O-Matic uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ekran kaydı-O-Matic ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ekran kaydı-O-Matic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/screencast-tutorial/tutorial_screencast_samlbase.png)

3. Üzerinde **ekran kaydı-O-Matic etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Ekran kaydı-O-Matic etki alanı ve URL'leri tek oturum açma bilgileri](./media/screencast-tutorial/tutorial_screencast_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [ekran kaydı-O-Matic istemci destek ekibi](mailto:support@screencast-o-matic.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/screencast-tutorial/tutorial_screencast_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/screencast-tutorial/tutorial_general_400.png)

6. Bir farklı web tarayıcısı penceresinde, ekran kaydı-O-Matic yönetici olarak oturum.

7. Tıklayın **abonelik**.

    ![Abonelik](./media/screencast-tutorial/tutorial_screencast_sub.png)

8. Altında **erişim sayfası** bölümünde, tıklatın **Kurulum**.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_setup.png)

9. Üzerinde **Kurulum erişim sayfası**, aşağıdaki adımları gerçekleştirin:

    * Altında **erişim URL'si** bölümünde, belirtilen metin kutusuna, InstanceName yazın.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_access.png)

    * Seçin **gerektiren etki alanı kullanıcısı** altında **SAML kullanıcı kısıtlaması (isteğe bağlı)** bölümü.

    * Altında **IDP meta veri XML dosyasını karşıya yükle**, tıklatın **Dosya Seç** Azure Portalı'ndan indirilen meta veriler karşıya yüklemek için.

    * **Tamam**’a tıklayın. 

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/screencast-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/screencast-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/screencast-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/screencast-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-screencast-o-matic-test-user"></a>Ekran kaydı-O-Matic test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon ekran kaydı-O-Matic içinde adlı bir kullanıcı oluşturmaktır. Ekran kaydı-O-Matic yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa ekran kaydı-O-Matic erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [ekran kaydı-O-Matic istemci destek ekibi](mailto:support@screencast-o-matic.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, ekran kaydı-O-Matic için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ekran kaydı-O-Matic için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ekran kaydı-O-Matic**.

    ![Uygulamalar listesinde ekran kaydı-O-Matic bağlantı](./media/screencast-tutorial/tutorial_screencast_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ekran kaydı-O-Matic parçasında tıklattığınızda, otomatik olarak ekran kaydı-O-Matic uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/screencast-tutorial/tutorial_general_01.png
[2]: ./media/screencast-tutorial/tutorial_general_02.png
[3]: ./media/screencast-tutorial/tutorial_general_03.png
[4]: ./media/screencast-tutorial/tutorial_general_04.png

[100]: ./media/screencast-tutorial/tutorial_general_100.png

[200]: ./media/screencast-tutorial/tutorial_general_200.png
[201]: ./media/screencast-tutorial/tutorial_general_201.png
[202]: ./media/screencast-tutorial/tutorial_general_202.png
[203]: ./media/screencast-tutorial/tutorial_general_203.png

