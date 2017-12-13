---
title: "Öğretici: Azure Active Directory Tümleştirme Merkezi Masaüstü ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Merkezi Masaüstü arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 62f1a1e2193d9693519bdea86196cf296d4b9780
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Azure Active Directory Tümleştirme ile Merkezi Masaüstü

Bu öğreticide, Azure Active Directory (Azure AD) ile Merkezi Masaüstü tümleştirmek öğrenin.

Merkezi Masaüstü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Merkezi Masaüstü erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) merkezi masaüstüne açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Merkezi Masaüstü ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir merkezi Masaüstü çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Merkezi Masaüstü ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-central-desktop-from-the-gallery"></a>Galeriden Merkezi Masaüstü ekleme
Azure AD Merkezi Masaüstü tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Merkezi Masaüstü eklemeniz gerekir.

**Merkezi Masaüstü Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Merkezi Masaüstü**seçin **Merkezi Masaüstü** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Merkezi Masaüstü](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Merkezi "Britta Simon" adlı bir test kullanıcı tabanlı masaüstü ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Merkezi Masaüstü bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Merkezi Masaüstü ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Merkezi Masaüstü değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Merkezi Masaüstü ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Merkezi Masaüstü test kullanıcısı oluşturma](#create-a-central-desktop-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı olduğu Merkezi Masaüstü sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Merkezi Masaüstü uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Merkezi Masaüstü ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Merkezi Masaüstü** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. Üzerinde **Merkezi Masaüstü etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir merkezi Masaüstü etki alanı ve URL'leri](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.centraldesktop.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Merkezi Masaüstü istemci destek ekibi](https://imeetcentral.com/contact-us) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. Üzerinde **merkezi masaüstü yapılandırması** 'yi tıklatın **yapılandırma Merkezi Masaüstü** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Merkezi masaüstü yapılandırması](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Oturum, **Merkezi Masaüstü** Kiracı.

8. Git **ayarları**, tıklatın **Gelişmiş**ve ardından **çoklu oturum açma**.

    ![Kurulum - Gelişmiş](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "Kurulum - Gelişmiş")

9. Üzerinde **tek oturum açma ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma ayarları](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "tek oturum açma ayarları")
    
    a. Seçin **etkinleştir SAML v2 çoklu oturum açma**.
    
    b. İçinde **SSO URL** metin kutusuna, Yapıştır **SAML varlık kimliği** Azure portalından kopyaladığınız değeri.
    
    c. İçinde **SSO oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.
    
    d. İçinde **SSO oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri.

10. İçinde **ileti imzası doğrulama yöntemi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![İleti imzası doğrulama yöntemi](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "ileti imzası doğrulama yöntemi") bir. Seçin **sertifika**.
    
    b. Gelen **SSO sertifika** listesinde **RSH SHA256**.
    
    c. İndirilen sertifikanızı Not Defteri'nde açın, sertifika içeriğini kopyalayın ve ardından yapıştırın **SSO sertifika** alan.
        
    d. Seçin **SAMLv2 oturum açma sayfanız bir bağlantı görüntüler**.
    
    e. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-central-desktop-test-user"></a>Merkezi Masaüstü test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açabilmesi için bunların Merkezi Masaüstü uygulamaya sağlanmalıdır. Bu bölümde Azure AD kullanıcı hesaplarının Merkezi Masaüstü nasıl oluşturulacağı açıklanmaktadır.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için herhangi bir merkezi Masaüstü kullanıcı hesabı oluşturma araçlarını veya Merkezi Masaüstü tarafından sağlanan API'leri kullanabilirsiniz

**Merkezi Masaüstü kullanıcı hesaplarına sağlamak için:**

1. Merkezi Masaüstü kiracınız için oturum açın.

2. Git **kişiler \> iç üyeleri**.

3. Tıklatın **iç üye eklemek**.

    ![Kişiler](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "kişiler")
    
4. İçinde **yeni üyeler e-posta adresi** metin kutusuna, sağlamak istediğiniz ve ardından Azure AD hesap türü **sonraki**.

    ![E-posta adreslerini yeni üyeler](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "e-posta adreslerini yeni üyeler")

5. Tıklatın **Ekle iç üyeleri**.

    ![İç üye ekleme](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "iç üye ekleme")
   
   >[!NOTE]
   >Eklemiş olduğunuz kullanıcı hesabını etkinleştirmek için tıklatın için gereksinim duydukları onay bağlantısı içeren bir e-posta alırsınız.
   
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Merkezi Masaüstü erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Merkezi Masaüstü Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Merkezi Masaüstü**.

    ![Uygulamalar listesinde Merkezi Masaüstü Bağlantısı](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Merkezi Masaüstü parçasında tıklattığınızda, otomatik olarak Merkezi Masaüstü uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_203.png

