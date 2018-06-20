---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Achieve3000 | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Achieve3000 arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 83a83d07-ff9c-46c4-b5ba-25fe2b2cd003
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: jeedes
ms.openlocfilehash: b943a805a025610561f0369d6ce4277600391672
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215316"
---
# <a name="tutorial-azure-active-directory-integration-with-achieve3000"></a>Öğretici: Azure Active Directory Tümleştirme Achieve3000 ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Achieve3000 tümleştirmek öğrenin.

Achieve3000 Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Achieve3000 erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Achieve3000 için açan kullanıcılarınıza etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına sahip.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Achieve3000 ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Achieve3000 çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Achieve3000 ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-achieve3000-from-the-gallery"></a>Galeriden Achieve3000 ekleme
Azure AD Achieve3000 tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Achieve3000 eklemeniz gerekir.

**Galeriden Achieve3000 eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Achieve3000**seçin **Achieve3000** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Achieve3000](./media/achieve3000-tutorial/tutorial_achieve3000_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Achieve3000 sınayın.

Tekli çalışmaya oturum için Azure AD Achieve3000 karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Achieve3000 ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Achieve3000 içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Achieve3000 ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Achieve3000 test kullanıcısı oluşturma](#create-an-achieve3000-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Achieve3000 sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Achieve3000 uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Achieve3000 yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Achieve3000** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/achieve3000-tutorial/tutorial_achieve3000_samlbase.png)

3. Üzerinde **Achieve3000 etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Achieve3000 etki alanı ve URL'leri tek oturum açma bilgileri](./media/achieve3000-tutorial/tutorial_achieve3000_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, aşağıdakileri kullanarak URL'sini yazın: desen: `https://saml.achieve3000.com/district/<District Identifier>`

    b. İçinde **tanımlayıcısı** metin değeri yazın: `achieve3000-saml`

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [Achieve3000 istemci destek ekibi](https://www.achieve3000.com/contact-us/) değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/achieve3000-tutorial/tutorial_achieve3000_certificate.png) 

5. Achieve3000 uygulama bekler benzersiz **studentID** ad tanımlayıcısı talep değeri. Müşteri adı tanımlayıcısı talep için doğru değerini eşleyebilirsiniz. Bu durumda, biz eşledikten **user.mail** Tanıtım amaçlı için. Ancak, benzersiz tanımlayıcısına göre doğru değeri için eşlenmesi gerekir.   

    ![Çoklu oturum açma attb yapılandırın](./media/achieve3000-tutorial/tutorial_achieve3000_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | studentID               | User.Mail |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/achieve3000-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/achieve3000-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/achieve3000-tutorial/tutorial_general_400.png)
    
8. Çoklu oturum açma yapılandırmak için **Achieve3000** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Achieve3000 destek ekibi](https://www.achieve3000.com/contact-us/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/achieve3000-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/achieve3000-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/achieve3000-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/achieve3000-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-achieve3000-test-user"></a>Bir Achieve3000 test kullanıcısı oluşturma

Bu bölümde, Achieve3000 içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Achieve3000 destek ekibi](https://www.achieve3000.com/contact-us/) Achieve3000 platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Achieve3000 için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Achieve3000 için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Achieve3000**.

    ![Uygulamalar listesinde Achieve3000 bağlantı](./media/achieve3000-tutorial/tutorial_achieve3000_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Achieve3000 parçasında tıklattığınızda, otomatik olarak Achieve3000 uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/achieve3000-tutorial/tutorial_general_01.png
[2]: ./media/achieve3000-tutorial/tutorial_general_02.png
[3]: ./media/achieve3000-tutorial/tutorial_general_03.png
[4]: ./media/achieve3000-tutorial/tutorial_general_04.png

[100]: ./media/achieve3000-tutorial/tutorial_general_100.png

[200]: ./media/achieve3000-tutorial/tutorial_general_200.png
[201]: ./media/achieve3000-tutorial/tutorial_general_201.png
[202]: ./media/achieve3000-tutorial/tutorial_general_202.png
[203]: ./media/achieve3000-tutorial/tutorial_general_203.png

