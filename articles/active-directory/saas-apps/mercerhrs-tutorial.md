---
title: 'Öğretici: Azure Active Directory Tümleştirme Mercer BenefitsCentral (MBC) ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Mercer BenefitsCentral (MBC) arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: 8d63bd83f35f6659ce0da6db2468e8e1676133d8
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226179"
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Öğretici: Azure Active Directory Tümleştirme ile Mercer BenefitsCentral (MBC)

Bu öğreticide, Azure Active Directory (Azure AD) ile Mercer BenefitsCentral (MBC) tümleştirme öğrenin.

Mercer BenefitsCentral (MBC) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mercer BenefitsCentral (MBC) erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Mercer BenefitsCentral (MBC için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Mercer BenefitsCentral (MBC ile) yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Mercer BenefitsCentral (MBC) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Mercer BenefitsCentral (MBC) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Galeriden Mercer BenefitsCentral (MBC) ekleme
Azure AD tümleştirmeye Mercer BenefitsCentral (MBC) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Mercer BenefitsCentral (MBC) eklemeniz gerekir.

**Galeriden Mercer BenefitsCentral (MBC) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Mercer BenefitsCentral (MBC)** seçin **Mercer BenefitsCentral (MBC)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde mercer BenefitsCentral (MBC)](./media/mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Mercer BenefitsCentral ("Britta Simon" adlı bir test kullanıcı tabanlı MBC) test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Mercer BenefitsCentral (MBC) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Mercer BenefitsCentral (MBC) arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Mercer BenefitsCentral (MBC içinde), Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mercer BenefitsCentral (MBC) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mercer BenefitsCentral (MBC) test kullanıcısı oluşturma](#create-a-mercer-benefitscentral-mbc-test-user)**  - Britta Simon, karşılık gelen içinde Mercer BenefitsCentral (kullanıcı Azure AD gösterimini bağlantılı MBC) sahip.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mercer BenefitsCentral (MBC) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Mercer BenefitsCentral (MBC ile) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mercer BenefitsCentral (MBC)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

3. Üzerinde **Mercer BenefitsCentral (MBC) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mercer BenefitsCentral (MBC) etki alanı ve URL'leri tek oturum açma bilgileri](./media/mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `stg.mercerhrs.com/saml2.0`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > Yanıt URL'si değeri gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [Mercer BenefitsCentral (MBC) destek ekibi](https://www.mercer.com/contact-us.html) bu değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/mercerhrs-tutorial/tutorial_general_400.png)

6. Üzerinde **Mercer BenefitsCentral (MBC) yapılandırma** 'yi tıklatın **yapılandırma Mercer BenefitsCentral (MBC)** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Mercer BenefitsCentral (MBC) yapılandırma](./media/mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Mercer BenefitsCentral (MBC)** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML çoklu oturum açma hizmet URL'si** için[Mercer BenefitsCentral (MBC) destek ekibi](https://www.mercer.com/contact-us.html). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/mercerhrs-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/mercerhrs-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/mercerhrs-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/mercerhrs-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Mercer BenefitsCentral (MBC) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Mercer SA olarak adlandırılan bir kullanıcı oluşturun. Çalışmak [Mercer BenefitsCentral (MBC) destek ekibi](https://www.mercer.com/contact-us.html) Mercer SA platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Mercer BenefitsCentral (MBC) erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Mercer BenefitsCentral (MBC) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Mercer BenefitsCentral (MBC)**.

    ![Uygulamalar listesinde Mercer BenefitsCentral (MBC) bağlantısı](./media/mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mercer BenefitsCentral (MBC) parçasında tıklattığınızda, otomatik olarak Mercer BenefitsCentral (MBC) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/mercerhrs-tutorial/tutorial_general_203.png

