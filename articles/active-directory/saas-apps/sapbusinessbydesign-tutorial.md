---
title: 'Öğretici: SAP Business ByDesign Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SAP Business ByDesign arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 446149e21bb5fff69df26b567469e856e245da16
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215482"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP Business ByDesign tümleştirmek öğrenin.

SAP Business ByDesign Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAP Business ByDesign erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için SAP Business ByDesign açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Business ByDesign ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SAP Business ByDesign çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAP Business ByDesign Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign Galeriden ekleme
Azure AD SAP Business ByDesign tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SAP Business ByDesign eklemeniz gerekir.

**SAP Business ByDesign Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP Business ByDesign**seçin **SAP Business ByDesign** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![SAP Business ByDesign sonuçlar listesinde](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP Business "Britta Simon" adlı bir test kullanıcı tabanlı ByDesign ile test etme.

Tekli çalışmaya oturum için Azure AD SAP Business ByDesign karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SAP Business ByDesign ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SAP Business ByDesign içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP Business ByDesign ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SAP Business ByDesign test kullanıcısı oluşturma](#create-an-sap-business-bydesign-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SAP Business ByDesign sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SAP Business ByDesign uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma SAP Business ByDesign ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAP Business ByDesign** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. Üzerinde **SAP Business ByDesign etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign etki alanı ve URL'leri tek oturum açma bilgileri](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<servername>.sapbydesign.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [SAP Business ByDesign istemci destek ekibi](https://www.sap.com/products/cloud-analytics.support.html) bu değerleri almak için.

4. Üzerinde **kullanıcı öznitelikleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAP Business ByDesign özniteliği bölümü](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. İçinde **kullanıcı tanımlayıcısı** listesinde **ExtractMailPrefix()** işlevi.
    
    b. Gelen **posta** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği seçin. Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı olarak kullanmak istediğiniz ve öznitelik değeri ExtensionAttribute2 depoladığınız, ardından user.extensionattribute2 seçin.     

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. Üzerinde **SAP Business ByDesign yapılandırma** 'yi tıklatın **yapılandırma SAP Business ByDesign** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![SAP Business ByDesign yapılandırma](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. Uygulamanız için yapılandırılmış SSO almak için aşağıdaki adımları gerçekleştirin:
   
    a. SAP Business ByDesign portalınızı yönetici haklarıyla oturum açma.
   
    b. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.
   
    c. Tıklatın **yeni kimlik sağlayıcısı** ve Azure portalından indirdiğiniz meta veri XML dosyası seçin. Meta verileri içe aktararak sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikası karşıya yükleme.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. Eklenecek **onaylama tüketici hizmeti URL'si** SAML isteği seçin **onaylama tüketici hizmeti URL'si dahil**.
   
    e. Tıklatın **çoklu oturum açmayı etkinleştirme**.
   
    f. Yaptığınız değişiklikleri kaydedin.
   
    g. Tıklatın **My sistem** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, içine Azure portalından kopyalanan **Azure AD oturum açma URL'si** metin kutusu.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Kullanıcı kimliği ve parola veya SSO ile seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçimi**.
   
    j. İçinde **SSO URL** bölümünde, çalışan oturum açmak için sistem tarafından kullanılması gereken URL'yi belirtin. 
    URL gönderilen çalışan açılır listesi için aşağıdaki seçenekler arasından seçim yapabilirsiniz:
   
    **SSO olmayan URL'si**
   
    Sistem, yalnızca normal sistem URL çalışana gönderir. Çalışan olamaz SSO kullanarak oturum açın ve parolayı kullanın veya gerekir bunun yerine sertifika.
   
    **SSO URL'Sİ** 
   
    Sistem yalnızca SSO URL çalışana gönderir. Çalışan SSO kullanarak oturum açabilir. Kimlik doğrulama isteği IDP yönlendirilir.
   
    **Otomatik Seçim**
   
    SSO etkin değilse, sistem çalışana normal sistem URL gönderir. SSO etkin değilse, sistem çalışan bir parolaya sahip olup olmadığını denetler. Bir parola kullanılabilir durumdaysa, SSO URL'si ve olmayan SSO URL'si çalışana gönderilir. Ancak, çalışan parolası yoksa, yalnızca SSO URL çalışana gönderilir.
   
    k. Yaptığınız değişiklikleri kaydedin.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>SAP Business ByDesign test kullanıcısı oluşturma

Bu bölümde, SAP Business ByDesign Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak [SAP Business ByDesign istemci destek ekibi](https://www.sap.com/products/cloud-analytics.support.html) SAP Business ByDesign platform kullanıcıları eklemek için. 

> [!NOTE]
> Lütfen NameID değeri SAP Business ByDesign platform kullanıcıadı alanıyla eşleşmelidir emin olun.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP Business ByDesign erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SAP Business ByDesign Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP Business ByDesign**.

    ![Uygulamalar listesinde SAP Business ByDesign bağlantı](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP Business ByDesign parçasında tıklattığınızda, otomatik olarak SAP Business ByDesign uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/sapbusinessbydesign-tutorial/tutorial_general_203.png

