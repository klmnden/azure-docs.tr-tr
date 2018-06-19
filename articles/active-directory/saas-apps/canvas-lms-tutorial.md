---
title: 'Öğretici: Azure Active Directory Tümleştirme tuvale Lms ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve tuvale LMS arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: cc078890ec6c93bc2e410fac86a14623de8f8ce4
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980480"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Öğretici: Tuvale LMS Azure Active Directory Tümleştirme

Bu öğreticide, tuvale Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Tuvale Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tuvale erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak tuvale (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme tuvali ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir tuval çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden tuvale ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-canvas-from-the-gallery"></a>Galeriden tuvale ekleme
Azure AD tuvale tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden tuvale eklemeniz gerekir.

**Galeriden tuvale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **tuvale**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. Sonuçlar panelinde seçin **tuvale**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı tuvali ile test etme

Tekli çalışmaya oturum için Azure AD tuvale karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının tuvale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Tuvalde, değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma tuvali sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tuvale test kullanıcısı oluşturma](#creating-a-canvas-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı tuvale sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma tuvale uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma tuvali ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **tuvale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. Üzerinde **tuvale etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenant-name>.instructure.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, şu biçimi kullanarak değeri yazın: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [tuvale istemci destek ekibi](https://community.canvaslms.com/community/help) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_general_400.png)

6. Üzerinde **tuvale yapılandırma** 'yi tıklatın **yapılandırma tuvale** açmak için **yapılandırma oturum açma** penceresi. Kopya **değişiklik parola URL'si, Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. Farklı web tarayıcısı penceresinde tuvale şirket sitenize yönetici olarak oturum açın.

8. Git **kurslar \> yönetilen hesapları \> Microsoft**.
   
    ![Tuvale](./media/canvas-lms-tutorial/IC775990.png "tuvali")

9. Sol gezinti bölmesinde seçin **kimlik doğrulaması**ve ardından **ekleme yeni SAML Config**.
   
    ![Kimlik doğrulama](./media/canvas-lms-tutorial/IC775991.png "kimlik doğrulaması")

10. Geçerli tümleştirme sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Geçerli tümleştirme](./media/canvas-lms-tutorial/IC775992.png "geçerli tümleştirme")

    a. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    b. İçinde **günlük üzerinde URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. İçinde **oturum kapatma URL'sini** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.

    d. İçinde **değişiklik parola bağlantısı** metin değerini yapıştırın **değişiklik parola URL'si** Azure portalından kopyalanan. 

    e. İçinde **sertifika parmak izi** metin kutusuna, Yapıştır **parmak izi** Azure portalından kopyaladığınız sertifika değeri.      
        
    f. Gelen **oturum açma özniteliği** listesinde **NameID**.

    g. Gelen **tanımlayıcı biçimi** listesinde **emailAddress**.

    h. Tıklatın **kimlik doğrulama ayarlarını Kaydet**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/canvas-lms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-canvas-test-user"></a>Tuvale test kullanıcısı oluşturma

Azure AD kullanıcılarının tuvale oturum açmayı etkinleştirmek için bunlar tuvale sağlanmalıdır.

Tuvale durumunda, kullanıcı sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **tuvale** Kiracı.

2. Git **kurslar \> yönetilen hesapları \> Microsoft**.
   
   ![Tuvale](./media/canvas-lms-tutorial/IC775990.png "tuvali")

3. **Kullanıcılar**’a tıklayın.
   
   ![Kullanıcıların](./media/canvas-lms-tutorial/IC775995.png "kullanıcılar")

4. Tıklatın **yeni kullanıcı Ekle**.
   
   ![Kullanıcıların](./media/canvas-lms-tutorial/IC775996.png "kullanıcılar")

5. Yeni kullanıcı iletişim Sayfa Ekle üzerinde aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı ekleme](./media/canvas-lms-tutorial/IC775997.png "kullanıcı ekleme")
   
   a. İçinde **tam adı** metin gibi kullanıcı adını girin **BrittaSimon**.

   b. İçinde **e-posta** metin kutusuna, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

   c. İçinde **oturum açma** metin kutusuna, kullanıcının Azure AD e-posta adresi gibi girin **brittasimon@contoso.com**.

   d. Seçin **bu hesap oluşturma hakkında kullanıcıya e-posta**.

   e. Tıklatın **kullanıcı ekleme**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına tuvale tarafından sağlanan veya herhangi diğer tuvale kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta tuvale erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Tuvale Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **tuvale**.

    ![Çoklu oturum açmayı yapılandırın](./media/canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli tuvale parçasında tıklattığınızda, otomatik olarak tuvale uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/canvas-lms-tutorial/tutorial_general_203.png

