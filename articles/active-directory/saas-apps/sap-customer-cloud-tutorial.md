---
title: 'Öğretici: Müşteri için SAP bulut Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAP bulut ile Azure Active Directory arasında müşteri için yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 8855a82c1490c916e040f61c07e1116d9125e7e6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045871"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Öğretici: Müşteri için SAP bulut Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile müşteri için SAP bulut tümleştirme konusunda bilgi edinin.

Azure AD ile müşteri için SAP bulut tümleştirme ile aşağıdaki avantajları sağlar:

- Müşteri için SAP Cloud erişebilir, Azure AD'de denetleyebilirsiniz
- Otomatik olarak SAP Cloud (çoklu oturum açma) müşteriler için Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi, müşteri için SAP bulut ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir müşteri çoklu oturum açma için SAP Cloud abonelik etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP Cloud müşteri için Galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP Cloud müşteri için Galeri ekleme
Azure AD'de müşteri için SAP Cloud tümleştirmesini yapılandırmak için SAP Cloud müşteri için Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Cloud müşteri için Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **müşteri için SAP Cloud**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Sonuçlar panelinde seçin **müşteri için SAP Cloud**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP bulut ile "Britta Simon" adlı bir test kullanıcısı üzerinde temel müşteri için test edin.

Tek iş için oturum açma için Azure AD ne müşteri için SAP Cloud karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve müşteri için SAP Cloud ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Müşteri için SAP Cloud, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAP bulut ile müşteri için test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir test kullanıcısı müşteri için SAP Cloud oluşturma](#creating-a-sap-cloud-for-customer-test-user)**  - kullanıcı Azure AD gösterimini bağlı müşteri için SAP bulutta Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP bulut müşteri uygulaması için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma müşteri için SAP bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **müşteri için SAP Cloud** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Üzerinde **müşteri etki alanı ve URL'ler için SAP Cloud** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server name>.crm.ondemand.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [müşteri istemci destek ekibi için SAP Cloud](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) bu değerleri almak için. 

4. Üzerinde **kullanıcı öznitelikleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. İçinde **kullanıcı tanımlayıcısı** listesinden **ExtractMailPrefix()** işlevi.

    b. Gelen **posta** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin.
    Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı kullanmak istediğiniz ve öznitelik değeri içinde ExtensionAttribute2 depoladığınız seçerseniz, user.extensionattribute2 seçin.  

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Üzerinde **müşteri yapılandırması için SAP Cloud** bölümünde **müşteri için SAP Cloud yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. Yapılandırılmış SSO almak için aşağıdaki adımları gerçekleştirin:
   
    a. Yönetici haklarıyla müşteri portalı için SAP Cloud oturum açın.
   
    b. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.
   
    c. Tıklayın **yeni kimlik sağlayıcısı** ve Azure portalından indirilen meta veri XML dosyasını seçin. Meta verileri içeri aktararak, sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikasını karşıya yükler.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory gerektiren ' % s'öğesi onay belgesi tüketici hizmeti URL'si seçin SAML isteğindeki **onay belgesi tüketici hizmeti URL'si dahil** onay kutusu.
   
    e. Tıklayın **çoklu oturum açmayı etkinleştirme**.
   
    f. Yaptığınız değişiklikleri kaydedin.
   
    g. Tıklayın **My sistem** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. İçinde **Azure AD oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Kullanıcı kimliği ve parola veya SSO seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçim**.
   
    j. İçinde **SSO URL** bölümünde, çalışanlarınız tarafından sisteme oturum açmak için kullanılması gereken URL'yi belirtin. 
    İçinde **URL'ye gönderilen çalışana** listesinde, aşağıdaki seçenekler arasından seçim yapabilirsiniz:
   
    **SSO URL**
   
    Sistem, yalnızca normal sistem URL çalışana gönderir. Çalışan olamaz, SSO kullanarak oturum açın ve parolayı kullanın veya gerekir bunun yerine sertifika.
   
    **SSO URL'Sİ** 
   
    Sistem SSO URL çalışana gönderir. Çalışan SSO kullanarak oturum açabilir. Kimlik doğrulama isteği, IDP yönlendirilir.
   
    **Otomatik Seçim**
   
    SSO etkin değilse, sistemin normal sistem URL çalışana gönderir. SSO etkin olursa, sistem çalışan bir parolaya sahip olup olmadığını denetler. Bir parolası varsa, hem SSO URL hem de olmayan SSO URL çalışana gönderilir. Ancak, çalışan parolası yoksa, yalnızca SSO URL çalışana gönderilir.
   
    k. Yaptığınız değişiklikleri kaydedin.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Test kullanıcısı müşteri için SAP bulut oluşturma

Bu bölümde, Britta Simon müşteri için SAP bulutta adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [müşteri destek ekibi için SAP Cloud](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) müşteri platform için SAP bulut kullanıcıları eklemek için. 

> [!NOTE]
> Nameıd değer SAP Cloud platform müşteri için kullanıcı adı alanıyla eşleşmelidir emin olun.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, müşteri için SAP Cloud erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon müşteri için SAP Cloud atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **müşteri için SAP Cloud**.

    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde kutucuk müşteri için SAP Cloud tıkladığınızda, otomatik olarak SAP Bulutunuza müşteri uygulaması için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/sap-customer-cloud-tutorial/tutorial_general_203.png

