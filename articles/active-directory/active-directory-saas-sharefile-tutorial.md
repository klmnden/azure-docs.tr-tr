---
title: "Öğretici: Azure Active Directory Tümleştirme ile Citrix ShareFile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Citrix ShareFile arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: 8473c262f98e77708f01d17419e935979a533307
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Öğretici: Citrix ShareFile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Citrix ShareFile tümleştirmek öğrenin.

Citrix ShareFile Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Citrix ShareFile erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Citrix ShareFile açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Citrix ShareFile ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Citrix ShareFile çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Citrix ShareFile Galerisi'nden ekleme
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-citrix-sharefile-from-the-gallery"></a>Citrix ShareFile Galerisi'nden ekleme
Azure AD Citrix ShareFile tümleştirilmesi yapılandırmak için Citrix ShareFile Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Citrix ShareFile eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Citrix ShareFile**seçin **Citrix ShareFile** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Citrix ShareFile](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Citrix ShareFile ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD Citrix ShareFile karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Citrix ShareFile ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Citrix ShareFile içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Citrix ShareFile ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Citrix ShareFile test kullanıcısı oluşturma](#create-a-citrix-sharefile-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Citrix ShareFile sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Citrix ShareFile uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Citrix ShareFile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Citrix ShareFile** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. Üzerinde **Citrix ShareFile etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Citrix ShareFile etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenant-name>.sharefile.com/saml/login`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Citrix ShareFile istemci destek ekibi](https://www.citrix.co.in/products/sharefile/support.html) bu değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. Üzerinde **Citrix ShareFile yapılandırma** 'yi tıklatın **yapılandırma Citrix ShareFile** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Citrix ShareFile yapılandırma](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **Citrix ShareFile** yönetici olarak şirket site.

8. Üstteki araç çubuğunda tıklatın **yönetici**.

9. Sol gezinti bölmesinde seçin **yapılandırma çoklu oturum açma**.
   
    ![Hesap Yönetimi](./media/active-directory-saas-sharefile-tutorial/ic773627.png "hesap yönetimi")

10. Üzerinde **çoklu oturum açma / SAML 2.0 yapılandırma** iletişim sayfasında altında **temel ayarları**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/active-directory-saas-sharefile-tutorial/ic773628.png "çoklu oturum açma")
   
    a. Tıklatın **etkinleştirmek SAML**.
    
    b. İçinde **bilgisayarınızı IDP veren / varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    c. Tıklatın **değişiklik** yanına **X.509 sertifikası** alan ve Azure portalından indirdiğiniz sertifika yükleyin.
    
    d. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
    
    e. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.

11. Tıklatın **kaydetmek** Citrix ShareFile Yönetim Portalı.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-citrix-sharefile-test-user"></a>Citrix ShareFile test kullanıcısı oluşturma

Azure AD kullanıcıların Citrix ShareFile oturum etkinleştirmek için bunların Citrix ShareFile sağlanmalıdır. Citrix ShareFile söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Citrix ShareFile** Kiracı.

2. Tıklatın **kullanıcıları yönetme \> kullanıcıların giriş yönetmek \> + çalışan oluşturma**.
   
   ![Çalışan oluşturma](./media/active-directory-saas-sharefile-tutorial/IC781050.png "çalışan oluşturma")

3. Üzerinde **temel bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Temel bilgileri](./media/active-directory-saas-sharefile-tutorial/IC799951.png "temel bilgileri")
   
   a. İçinde **e-posta adresi** metin kutusuna, Britta Simon e-posta adresini yazın  **brittasimon@contoso.com** .
   
   b. İçinde **ad** metin kutusuna, türü **ad** kullanıcının **Britta**.
   
   c. İçinde **Soyadı** metin kutusuna, türü **Soyadı** kullanıcının **Simon**.

4. Tıklatın **kullanıcı ekleme**.
  
   >[!NOTE]
   >Azure AD hesap sahibi bir e-posta almak ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izleyin. Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Citrix ShareFile kullanıcı hesabı oluşturma araçlarını veya Citrix ShareFile tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Citrix ShareFile erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Citrix ShareFile Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Citrix ShareFile**.

    ![Uygulamalar listesinde Citrix ShareFile bağlantı](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Citrix ShareFile parçasında tıklattığınızda, otomatik olarak Citrix ShareFile uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

