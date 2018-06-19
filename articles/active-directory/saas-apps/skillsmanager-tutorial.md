---
title: 'Öğretici: Azure Active Directory Tümleştirme becerileri yöneticisiyle | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve yetenekleri Yöneticisi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dab8debd-3b7b-4656-9bf0-1963ad8fce05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: jeedes
ms.openlocfilehash: c9e54481557010d2064fa44a6dc5e9811e2dce2c
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982171"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-manager"></a>Öğretici: Azure Active Directory Tümleştirme becerileri yöneticisiyle

Bu öğreticide, becerileri Yöneticisi Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Yetenekler Yöneticisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yetenekler Yöneticisi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak becerileri Manager'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme becerileri Yöneticisi ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir yetenek Yöneticisi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden becerileri Yöneticisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-skills-manager-from-the-gallery"></a>Galeriden becerileri Yöneticisi ekleme
Azure AD becerileri Yöneticisi tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden becerileri Yöneticisi eklemeniz gerekir.

**Galeriden becerileri Yöneticisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **becerileri Yöneticisi**seçin **becerileri Yöneticisi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde becerileri Yöneticisi](./media/skillsmanager-tutorial/tutorial_skillsmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yetenekleri "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen becerileri Yöneticisi'nde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı becerileri Yöneticisi'nde arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini becerileri Yöneticisi'nde, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma yetenekleri Yöneticisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yetenekler Yöneticisi test kullanıcısı oluşturma](#create-a-skills-manager-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı becerileri Yöneticisi sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma yetenekleri Yöneticisi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma yetenekleri Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **becerileri Yöneticisi** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/skillsmanager-tutorial/tutorial_skillsmanager_samlbase.png)

3. Üzerinde **becerileri yöneticisi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yetenekler yöneticisi etki alanı ve URL'leri tek oturum açma bilgileri](./media/skillsmanager-tutorial/tutorial_skillsmanager_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://subdomain.skills-manager.com/kennametal`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://subdomain.skills-manager.com/public/SamlLogin2.aspx`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [becerileri Manager destek ekip](https://www.ibm.com/support/uk/?lnk=msu_uk) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/skillsmanager-tutorial/tutorial_skillsmanager_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/skillsmanager-tutorial/tutorial_general_400.png)

6. Üzerinde **becerileri Yöneticisi yapılandırma** 'yi tıklatın **becerileri Yapılandırma Yöneticisi'ni** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Yetenekler Yöneticisi yapılandırması](./media/skillsmanager-tutorial/tutorial_skillsmanager_configure.png) 

7. Çoklu oturum açma yapılandırmak için **becerileri Yöneticisi** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64)**, **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [becerileri Manager destek ekip](https://www.ibm.com/support/uk/?lnk=msu_uk). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/skillsmanager-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/skillsmanager-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/skillsmanager-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/skillsmanager-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-skills-manager-test-user"></a>Yetenekler Yöneticisi test kullanıcısı oluşturma

Bu bölümde, Britta Simon becerileri Yöneticisi'nde adlı bir kullanıcı oluşturun. Çalışmak [becerileri Manager destek ekip](https://www.ibm.com/support/uk/?lnk=msu_uk) becerileri Yöneticisi platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta becerileri Yöneticisi için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon becerileri yöneticiye atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **becerileri Yöneticisi**.

    ![Uygulamalar listesinde becerileri Yöneticisi bağlantı](./media/skillsmanager-tutorial/tutorial_skillsmanager_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli becerileri Yöneticisi parçasında tıklattığınızda, otomatik olarak becerileri Yöneticisi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsmanager-tutorial/tutorial_general_01.png
[2]: ./media/skillsmanager-tutorial/tutorial_general_02.png
[3]: ./media/skillsmanager-tutorial/tutorial_general_03.png
[4]: ./media/skillsmanager-tutorial/tutorial_general_04.png

[100]: ./media/skillsmanager-tutorial/tutorial_general_100.png

[200]: ./media/skillsmanager-tutorial/tutorial_general_200.png
[201]: ./media/skillsmanager-tutorial/tutorial_general_201.png
[202]: ./media/skillsmanager-tutorial/tutorial_general_202.png
[203]: ./media/skillsmanager-tutorial/tutorial_general_203.png

