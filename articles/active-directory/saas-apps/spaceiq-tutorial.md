---
title: 'Öğretici: Azure Active Directory Tümleştirme ile SpaceIQ | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SpaceIQ arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 5b55ae29-491f-401f-9299-d3a6b64a1b99
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: d69a0f801f1bf3fbd44514289dff4b9a95305ba4
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36227930"
---
# <a name="tutorial-azure-active-directory-integration-with-spaceiq"></a>Öğretici: Azure Active Directory Tümleştirme SpaceIQ ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SpaceIQ tümleştirmek öğrenin.

SpaceIQ Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SpaceIQ erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için SpaceIQ (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SpaceIQ ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SpaceIQ çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SpaceIQ ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-spaceiq-from-the-gallery"></a>Galeriden SpaceIQ ekleme
Azure AD SpaceIQ tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SpaceIQ eklemeniz gerekir.

**Galeriden SpaceIQ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SpaceIQ**seçin **SpaceIQ** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde SpaceIQ](./media/spaceiq-tutorial/tutorial_spaceiq_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı SpaceIQ ile test etme

Tekli çalışmaya oturum için Azure AD SpaceIQ karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SpaceIQ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SpaceIQ içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SpaceIQ ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SpaceIQ test kullanıcısı oluşturma](#create-a-spaceiq-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SpaceIQ sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SpaceIQ uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SpaceIQ yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SpaceIQ** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/spaceiq-tutorial/tutorial_spaceiq_samlbase.png)

3. Üzerinde **SpaceIQ etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SpaceIQ etki alanı ve URL'leri tek oturum açma bilgileri](./media/spaceiq-tutorial/tutorial_spaceiq_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://api.spaceiq.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api.spaceiq.com/saml/<instanceid>/callback`

    > [!NOTE] 
    > Fiili yanıt URL'si ve daha sonra öğreticide açıklandığı tanımlayıcısı ile bu değerleri güncelleştirin.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **(sertifika Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/spaceiq-tutorial/tutorial_spaceiq_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/spaceiq-tutorial/tutorial_general_400.png)

6. Üzerinde **SpaceIQ yapılandırma** 'yi tıklatın **yapılandırma SpaceIQ** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![SpaceIQ yapılandırma](./media/spaceiq-tutorial/tutorial_spaceiq_configure.png) 

7.  Yeni bir tarayıcı penceresi açın ve SpaceIQ ortamınıza yönetici olarak oturum açın.

8. Oturum açtığında sağ üst köşedeki Bulmacanın oturum tıklayın ve ardından üzerinde **"Tümleştirmeler"**

    ![Hesap ayarları](./media/spaceiq-tutorial/setting1.png) 

9. Altında **tüm sağlama & SSO**, tıklayın **Azure** döşeme IDP Azure örneği eklemek için.

    ![SAML simgesi](./media/spaceiq-tutorial/setting2.png)

10. İçinde **SSO** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulama ayarları](./media/spaceiq-tutorial/setting3.png)

    a. İçinde **SAML veren URL'si** kutusunda, yapıştırma **SAML varlık kimliği** değeri Azure AD uygulama yapılandırma penceresinden kopyalar.
    
    b. Kopya **SAML geri çağırma uç nokta URL'si (salt okunur)** değer ve değeri yapıştırın **yanıt URL'si** SpaceIQ altında Azure portalında kutusunda **etki alanı ve URL'leri** bölümü.
    
    c. Kopya **SAML İzleyici URI'si (salt okunur)** değer ve değeri yapıştırın **tanımlayıcısı** SpaceIQ altında Azure portalında kutusunda **etki alanı ve URL'leri** bölümü.

    d. İndirilen sertifika dosyasını Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **X.509 sertifikası** kutusu.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/spaceiq-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/spaceiq-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/spaceiq-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/spaceiq-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-spaceiq-test-user"></a>SpaceIQ test kullanıcısı oluşturma

Bu bölümde, SpaceIQ içinde Britta Simon adlı bir kullanıcı oluşturun. İş [SpaceIQ destek ekibi](mailto:eng@spaceiq.com) SpaceIQ platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta SpaceIQ için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SpaceIQ için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SpaceIQ**.

    ![Uygulamalar listesinde SpaceIQ bağlantı](./media/spaceiq-tutorial/tutorial_spaceiq_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SpaceIQ parçasında tıklattığınızda, otomatik olarak SpaceIQ uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spaceiq-tutorial/tutorial_general_01.png
[2]: ./media/spaceiq-tutorial/tutorial_general_02.png
[3]: ./media/spaceiq-tutorial/tutorial_general_03.png
[4]: ./media/spaceiq-tutorial/tutorial_general_04.png

[100]: ./media/spaceiq-tutorial/tutorial_general_100.png

[200]: ./media/spaceiq-tutorial/tutorial_general_200.png
[201]: ./media/spaceiq-tutorial/tutorial_general_201.png
[202]: ./media/spaceiq-tutorial/tutorial_general_202.png
[203]: ./media/spaceiq-tutorial/tutorial_general_203.png

