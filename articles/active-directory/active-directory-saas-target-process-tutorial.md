---
title: "Öğretici: Azure Active Directory Tümleştirme ile TargetProcess | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile TargetProcess arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d15931a5d430252bbd9ae342e1f8fde1a539355b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Öğretici: Azure Active Directory Tümleştirme TargetProcess ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TargetProcess tümleştirmek öğrenin.

TargetProcess Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TargetProcess erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için TargetProcess (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme TargetProcess ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TargetProcess çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TargetProcess Ekle
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-targetprocess-from-the-gallery"></a>Galeriden TargetProcess Ekle
Azure AD TargetProcess tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TargetProcess eklemeniz gerekir.

**Galeriden TargetProcess eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TargetProcess**seçin **TargetProcess** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Galeriden Ekle TargetProcess](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TargetProcess sınayın.

Tekli çalışmaya oturum için Azure AD TargetProcess karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TargetProcess ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TargetProcess içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TargetProcess ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TargetProcess test kullanıcısı oluşturma](#create-a-targetprocess-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TargetProcess sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TargetProcess uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TargetProcess yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TargetProcess** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML tabanlı oturum açma](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. Üzerinde **TargetProcess etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TargetProcess etki alanı ve URL'ler bölümü](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.tpondemand.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.tpondemand.com/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [TargetProcess istemci destek ekibi](mailto:support@targetprocess.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet düğmesi](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. Üzerinde **TargetProcess yapılandırma** 'yi tıklatın **yapılandırma TargetProcess** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TargetProcess yapılandırma bölümü](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. Yönetici olarak TargetProcess uygulamanıza oturum.

8. Üstteki menüde tıklatın **Kurulum**.
   
    ![Kurulum](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. Tıklatın **ayarları**.
   
    ![Ayarlar](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. Tıklatın **çoklu oturum açma**.
   
    ![Çoklu oturum açma'ı tıklatın](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. Çoklu oturum açma ayarları iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    a. Tıklatın **çoklu oturum açmayı etkinleştir**.
    
    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. İndirilen sertifikanızı Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **sertifika** metin kutusu.
    
    d. tıklatın **JIT etkinleştirme sağlama**.

    e. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların listesini görüntülemek için](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı bölümü](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-targetprocess-test-user"></a>TargetProcess test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde TargetProcess adlı bir kullanıcı oluşturmaktır.

Yalnızca zaman sağlama TargetProcess destekler. İçinde zaten etkinleştirdiğiniz [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

Bu bölümde, eylem öğe yok.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TargetProcess için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**TargetProcess için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TargetProcess**.

    ![Uygulama listesinde TargetProcess](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli TargetProcess parçasında tıklattığınızda, otomatik olarak TargetProcess uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

