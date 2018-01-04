---
title: "Öğretici: Azure Active Directory Tümleştirme Böl perakende – bilgi yönetimi ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory Böl perakende – bilgi yönetimi arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: f31f493e06dc8b097a1ba24da19404a7584046c5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a>Öğretici: Azure Active Directory Tümleştirme ile Böl perakende – bilgi yönetimi

Bu öğreticide, Böl perakende – bilgi yönetimi Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Böl perakende – bilgi yönetimi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Böl perakende – bilgi yönetim erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Böl perakende – bilgi yönetimi (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Böl perakende – bilgi yönetimi yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Böl perakende – bilgi yönetimi çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Bilgi Yönetimi galerisinden Böl perakende – ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-infor-retail--information-management-from-the-gallery"></a>Bilgi Yönetimi galerisinden Böl perakende – ekleme
Böl perakende – Azure AD'ye bilgi yönetimi tümleştirmesini yapılandırmak için Böl perakende – galerisinden bilgi yönetimi yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Bilgi Yönetimi Galerisi'nden Böl perakende – eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **etmek perakende – bilgi yönetimi**seçin **etmek perakende – bilgi yönetimi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Böl perakende – sonuçlar listesinde bilgi yönetimi](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Böl perakende – bilgi "Britta Simon" adlı bir test kullanıcı tabanlı yönetim ile test etme.

Tekli çalışmaya oturum için Azure AD Böl perakende – bilgi yönetimi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Böl perakende – bilgi yönetimi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Böl perakende – bilgi yönetimi değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Böl perakende – bilgi yönetimi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Böl perakende – bilgi yönetimi test kullanıcısı oluşturma](#create-an-infor-retail--information-management-test-user)**  - Britta Simon, karşılık gelen Böl perakende – kullanıcı Azure AD gösterimini bağlı bilgi yönetimi sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Böl perakende – bilgi yönetimi uygulamasını yapılandırın.

**Azure AD çoklu oturum açma Böl perakende – bilgi yönetimi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **etmek perakende – bilgi yönetimi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. Üzerinde **etmek perakende – bilgi yönetim etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Böl perakende – bilgi yönetim etki alanı ve URL'ler tek oturum açma bilgileri IDP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın: 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.mingle.infor.com/sp/ACS.saml2`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Böl perakende – bilgi yönetim etki alanı ve URL'leri tek oturum açma bilgileri SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<company name>.mingle.infor.com/<company code>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [etmek perakende – bilgi yönetimi istemci destek ekibi](mailto:innovate@infor.com) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **etmek perakende – bilgi yönetimi** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [etmek perakende – bilgi yönetimi destek ekibi](mailto:innovate@infor.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-an-infor-retail--information-management-test-user"></a>Böl perakende – bilgi yönetimi test kullanıcısı oluşturma

Bu bölümde, Britta Simon Böl perakende – bilgi yönetimi adlı bir kullanıcı oluşturun. Lütfen çalışmak [etmek perakende – bilgi yönetimi destek ekibi](mailto:innovate@infor.com) etmek perakende – bilgi yönetimi platformu kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta etmek perakende – bilgi yönetimi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Böl perakende – bilgi yönetimi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **etmek perakende – bilgi yönetimi**.

    ![Böl perakende – uygulamalar listesinde bilgi yönetimi bağlantı](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Böl perakende – bilgi yönetimi kutucuğu erişim Paneli'nde tıklattığınızda, otomatik olarak, Böl perakende – bilgi yönetimi uygulamasını açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

