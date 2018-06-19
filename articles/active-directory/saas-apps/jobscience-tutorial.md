---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Jobscience | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Jobscience arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 65fc899c73e127750935e3cd8c24a488870ac0ca
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972108"
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Öğretici: Azure Active Directory Tümleştirme Jobscience ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Jobscience tümleştirmek öğrenin.

Jobscience Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Jobscience erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Jobscience (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Jobscience ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Jobscience çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Jobscience ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-jobscience-from-the-gallery"></a>Galeriden Jobscience ekleme
Azure AD Jobscience tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Jobscience eklemeniz gerekir.

**Galeriden Jobscience eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Jobscience**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_search.png)

5. Sonuçlar panelinde seçin **Jobscience**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Jobscience ile test etme

Tekli çalışmaya oturum için Azure AD Jobscience karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Jobscience ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Jobscience içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Jobscience ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jobscience test kullanıcısı oluşturma](#creating-a-jobscience-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Jobscience sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Jobscience uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Jobscience yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Jobscience** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. Üzerinde **Jobscience etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:  `http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Bu değer alma [Jobscience istemci destek ekibi](https://www.jobscience.com/support) veya SSO profilinden öğreticide daha sonra açıklanan oluşturur. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_general_400.png)

6. Üzerinde **Jobscience yapılandırma** 'yi tıklatın **yapılandırma Jobscience** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_configure.png) 

7. Jobscience şirket sitenize yönetici olarak oturum açın.

8. Git **Kurulum**.
   
   ![Kurulum](./media/jobscience-tutorial/IC784358.png "Kurulumu")

9. Sol gezinti bölmesinde, içinde **Yönet** 'yi tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My etki alanı** açmak için **My etki alanı** sayfası. 
   
   ![Etki alanım](./media/jobscience-tutorial/ic767825.png "etki alanım")

10. Etki alanınızın doğru şekilde ayarlanmış olması gerektiğini doğrulamak için içinde olduğundan emin olun "**adım 4 dağıtılan kullanıcılara**" ve gözden geçirin, "**My etki alanı ayarları**".

    ![Etki alanı kullanıcıya dağıtılan](./media/jobscience-tutorial/ic784377.png "kullanıcıya dağıtılan etki alanı")

11. Jobscience şirket sitesinde tıklatın **güvenlik denetimleri**ve ardından **çoklu oturum açma ayarları**.
    
    ![Güvenlik denetimleri](./media/jobscience-tutorial/ic784364.png "güvenlik denetimleri")

12. İçinde **çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma ayarları](./media/jobscience-tutorial/ic781026.png "tek oturum açma ayarları")
    
    a. Seçin **SAML etkin**.

    b. **Yeni**’ye tıklayın.

13. Üzerinde **SAML çoklu oturum açma ayarını Düzenle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma SAML tek ayar](./media/jobscience-tutorial/ic784365.png "oturum açma SAML tek ayar")
    
    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın.

    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    c. İçinde **varlık kimliği** metin kutusuna, türü `https://salesforce-jobscience.com`

    d. Tıklatın **Gözat** Azure AD sertifikanızı karşıya yüklemek için.

    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.

    f. Olarak **SAML kimlik konumu**seçin **kimliktir konu deyimi NameIdentfier öğesinde**.

    g. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    h. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    i. **Kaydet**’e tıklayın.

14. Sol gezinti bölmesinde, içinde **Yönet** 'yi tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My etki alanı** açmak için **My etki alanı** sayfası. 
    
    ![Etki alanım](./media/jobscience-tutorial/ic767825.png "etki alanım")

15. Üzerinde **My etki alanı** sayfasında **oturum açma sayfası markalama** 'yi tıklatın **Düzenle**.
    
    ![Oturum açma sayfası markalama](./media/jobscience-tutorial/ic767826.png "oturum açma sayfası markalama")

16. Üzerinde **oturum açma sayfası markalama** sayfasında **kimlik doğrulama hizmeti** bölümünde, adını, **SAML SSO ayarları** görüntülenir. Seçin ve ardından **kaydetmek**.
    
    ![Oturum açma sayfası markalama](./media/jobscience-tutorial/ic784366.png "oturum açma sayfası markalama")

17. SP almak için üzerinde oturum açma URL'si tıklatıldığında çoklu oturum açma başlatılan **çoklu oturum açma ayarları** içinde **güvenlik denetimleri** menü bölümü.

    ![Güvenlik denetimleri](./media/jobscience-tutorial/ic784368.png "güvenlik denetimleri")
    
    Yukarıdaki adımda oluşturduğunuz SSO profiline tıklayın. Bu sayfa çoklu oturum açma URL'SİNDE şirketiniz için gösterir (örneğin, [ https://companyname.my.salesforce.com?so=companyid ](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jobscience-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-jobscience-test-user"></a>Jobscience test kullanıcısı oluşturma

Azure AD kullanıcıları için Jobscience oturum açmak etkinleştirmek için bunların Jobscience sağlanmalıdır. Jobscience söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>API tarafından Jobscience sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer Jobscience kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>  
        
**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Jobscience** yönetici olarak şirket site.

2. Kurulum için gidin.
   
   ![Kurulum](./media/jobscience-tutorial/ic784358.png "Kurulumu")
3. Git **kullanıcıları yönetme \> kullanıcılar**.
   
   ![Kullanıcıların](./media/jobscience-tutorial/ic784369.png "kullanıcılar")
4. Tıklatın **yeni kullanıcı**.
   
   ![Tüm kullanıcılar](./media/jobscience-tutorial/ic784370.png "tüm kullanıcılar")
5. Üzerinde **kullanıcı düzenleme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı düzenleme](./media/jobscience-tutorial/ic784371.png "kullanıcı düzenleme")
   
   a. İçinde **ad** metin kutusuna, Britta gibi kullanıcının ilk adını yazın.
   
   b. İçinde **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını yazın.
   
   c. İçinde **diğer** metin kutusuna, kullanıcının brittas gibi bir ad yazın.

   d. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

   e. İçinde **kullanıcı adı** metin kutusuna, türü kullanıcının kullanıcı adını ister Brittasimon@contoso.com.

   f. İçinde **takma ad** metin kutusu, kullanıcı Simon gibi takma adını yazın.

   g. **Kaydet**’e tıklayın.

    
> [!NOTE]
> Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Jobscience için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Jobscience için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Jobscience**.

    ![Çoklu oturum açmayı yapılandırın](./media/jobscience-tutorial/tutorial_jobscience_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jobscience parçasında tıklattığınızda, otomatik olarak Jobscience uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jobscience-tutorial/tutorial_general_01.png
[2]: ./media/jobscience-tutorial/tutorial_general_02.png
[3]: ./media/jobscience-tutorial/tutorial_general_03.png
[4]: ./media/jobscience-tutorial/tutorial_general_04.png

[100]: ./media/jobscience-tutorial/tutorial_general_100.png

[200]: ./media/jobscience-tutorial/tutorial_general_200.png
[201]: ./media/jobscience-tutorial/tutorial_general_201.png
[202]: ./media/jobscience-tutorial/tutorial_general_202.png
[203]: ./media/jobscience-tutorial/tutorial_general_203.png

