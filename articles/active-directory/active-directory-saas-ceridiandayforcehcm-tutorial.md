---
title: "Öğretici: Azure Active Directory Tümleştirme ile Ceridian Dayforce HCM | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Ceridian Dayforce HCM arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 9b87fe59f2761c26319ce9e13168dc6c4bf95f8b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Öğretici: Azure Active Directory Tümleştirme Ceridian Dayforce HCM ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Ceridian Dayforce HCM tümleştirmek öğrenin.

Ceridian Dayforce HCM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Ceridian Dayforce HCM erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Ceridian Dayforce HCM açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Ceridian Dayforce HCM ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Ceridian Dayforce HCM çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Ceridian Dayforce HCM ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Galeriden Ceridian Dayforce HCM ekleme
Azure AD Ceridian Dayforce HCM tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Ceridian Dayforce HCM eklemeniz gerekir.

**Galeriden Ceridian Dayforce HCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Ceridian Dayforce HCM**seçin **Ceridian Dayforce HCM** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Ceridian Dayforce HCM](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Ceridian Dayforce "Britta Simon" adlı bir test kullanıcı tabanlı HCM ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Ceridian Dayforce HCM, bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Ceridian Dayforce HCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Ceridian Dayforce HCM Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Ceridian Dayforce HCM ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ceridian Dayforce HCM test kullanıcısı oluşturma](#create-a-ceridian-dayforce-hcm-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Ceridian Dayforce HCM sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Ceridian Dayforce HCM uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Ceridian Dayforce HCM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Ceridian Dayforce HCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. Üzerinde **Ceridian Dayforce HCM etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL kullanıcılarınıza oturum açma Ceridian Dayforce HCM uygulamanıza tarafından kullanılıyor.
    
    | Ortam | URL |
    | :-- | :-- |
    | Üretim amaçlı | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Test için | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    
    | Ortam | URL |
    | :-- | :-- |
    | Üretim amaçlı | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Test için | `https://fs-test.dayforcehcm.com/sp` |
    
    c. İçinde **yanıt URL'si** metin kutusuna, türü URL kullanılan Azure AD tarafından yanıt gönderme.
    
    | Ortam | URL |
    | :-- | :-- |
    | Üretim amaçlı | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Test için | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Ceridian Dayforce HCM istemci destek ekibi](https://www.ceridian.com/contact-us/index.html) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. Ceridian Dayforce HCM uygulamanızı belirli bir biçimde SAML onaylar bekliyor. Çalışmak [Ceridian Dayforce HCM destek ekibi](https://www.ceridian.com/contact-us/index.html) doğru kullanıcı tanımlayıcısı belirlemek için öncelikle. Microsoft önerir kullanarak **"name"** kullanıcı tanımlayıcısı olarak özniteliği. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.  

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri |
    | --------------- | -------------------- |    
    | ad  | User.extensionattribute2 |    

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. İçinde **değeri** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği seçin.
    Örneğin, EmployeeID kullanmak istiyorsanız, benzersiz bir kullanıcı tanımlayıcısı ve öznitelik değeri ExtensionAttribute2 depoladığınız, ardından **user.extensionattribute2**.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. Üzerinde **Ceridian Dayforce HCM yapılandırma** 'yi tıklatın **yapılandırma Ceridian Dayforce HCM** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Ceridian Dayforce HCM yapılandırma](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. Çoklu oturum açma yapılandırmak için **Ceridian Dayforce HCM** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [Ceridian Dayforce HCM destek ekibi](https://www.ceridian.com/contact-us/index.html).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce HCM test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Ceridian Dayforce HCM adlı bir kullanıcı oluşturmaktır. Çalışmak [Ceridian Dayforce HCM destek ekibi](https://www.ceridian.com/contact-us/index.html) Ceridian Dayforce HCM uygulamada eklenen kullanıcıları alınamadı. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Ceridian Dayforce HCM erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Ceridian Dayforce HCM Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Ceridian Dayforce HCM**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Ceridian Dayforce HCM erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Ceridian Dayforce HCM Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Ceridian Dayforce HCM**.

    ![Uygulamalar listesinde Ceridian Dayforce HCM bağlantı](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Erişim paneli Ceridian Dayforce HCM parçasında tıklattığınızda, otomatik olarak Ceridian Dayforce HCM uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

