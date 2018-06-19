---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Dealpath | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Dealpath arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51ace608-5a4f-48c0-9446-d9f86ad2e890
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: jeedes
ms.openlocfilehash: 9f088a4eb5a31de08b18eb56a5944a52e404c547
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971884"
---
# <a name="tutorial-azure-active-directory-integration-with-dealpath"></a>Öğretici: Azure Active Directory Tümleştirme Dealpath ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Dealpath tümleştirmek öğrenin.

Dealpath Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Dealpath erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Dealpath (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Dealpath ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Dealpath çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Dealpath ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-dealpath-from-the-gallery"></a>Galeriden Dealpath ekleme
Azure AD Dealpath tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Dealpath eklemeniz gerekir.

**Galeriden Dealpath eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Dealpath**seçin **Dealpath** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Dealpath](./media/dealpath-tutorial/tutorial_dealpath_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Dealpath sınayın.

Tekli çalışmaya oturum için Azure AD Dealpath karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Dealpath ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Dealpath içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Dealpath ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dealpath test kullanıcısı oluşturma](#create-a-dealpath-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Dealpath sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Dealpath uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Dealpath yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Dealpath** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dealpath-tutorial/tutorial_dealpath_samlbase.png)

3. Üzerinde **Dealpath etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Dealpath etki alanı ve URL'leri tek oturum açma bilgileri](./media/dealpath-tutorial/tutorial_dealpath_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://app.dealpath.com/account/login`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api.dealpath.com/saml/metadata/<ID>`

    > [!NOTE] 
    > Tanımlayıcı gerçek değil. Değerin gerçek tanımlayıcısı ile güncelleştirin. Kişi [Dealpath istemci destek ekibi](mailto:kenter@dealpath.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/dealpath-tutorial/tutorial_dealpath_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/dealpath-tutorial/tutorial_general_400.png)

6. Üzerinde **Dealpath yapılandırma** 'yi tıklatın **yapılandırma Dealpath** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Dealpath yapılandırma](./media/dealpath-tutorial/tutorial_dealpath_configure.png) 

7. Bir farklı web tarayıcısı penceresinde, Dealpath yönetici olarak oturum açın.

8. Sağ üst tıklatın **Yönetim Araçları** gidin **tümleştirmeler**, ardından **SAML 2.0 kimlik doğrulaması** bölümünde **güncelleştirme ayarlarını**:

    ![Dealpath yapılandırma](./media/dealpath-tutorial/tutorial_dealpath_admin.png)

9. İçinde **SAML 2.0 kimlik doğrulamasını kurma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Dealpath yapılandırma](./media/dealpath-tutorial/tutorial_dealpath_saml.png) 

    a. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    b. İçinde **kimlik sağlayıcısı veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    c. İndirilen içeriği Kopyala **certificate(Base64)** dosyasını Not Defteri'nde ve ardından yapıştırın **ortak sertifika** metin kutusu.

    d. Tıklatın **ayarlarını güncelleştirme**.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dealpath-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/dealpath-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dealpath-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dealpath-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dealpath-test-user"></a>Dealpath test kullanıcısı oluşturma

Bu bölümde, Dealpath içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Dealpath istemci destek ekibi](mailto:kenter@dealpath.com) Dealpath platform kullanıcıları eklemek için. Kullanıcıların gerekir ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Dealpath için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Dealpath için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Dealpath**.

    ![Uygulamalar listesinde Dealpath bağlantı](./media/dealpath-tutorial/tutorial_dealpath_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Dealpath parçasında tıklattığınızda, otomatik olarak Dealpath uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dealpath-tutorial/tutorial_general_01.png
[2]: ./media/dealpath-tutorial/tutorial_general_02.png
[3]: ./media/dealpath-tutorial/tutorial_general_03.png
[4]: ./media/dealpath-tutorial/tutorial_general_04.png

[100]: ./media/dealpath-tutorial/tutorial_general_100.png

[200]: ./media/dealpath-tutorial/tutorial_general_200.png
[201]: ./media/dealpath-tutorial/tutorial_general_201.png
[202]: ./media/dealpath-tutorial/tutorial_general_202.png
[203]: ./media/dealpath-tutorial/tutorial_general_203.png

