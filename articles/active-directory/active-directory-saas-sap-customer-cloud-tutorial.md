---
title: 'Öğretici: Müşteri için SAP bulut Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SAP bulut arasında müşteri için yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 18172fb634c369fb4add5fe7c55e5fec08df7670
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Öğretici: Müşteri için SAP bulut Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile müşteri için SAP bulut tümleştirme öğrenin.

Azure AD ile müşteri için SAP bulut tümleştirme ile aşağıdaki avantajları sağlar:

- SAP buluta müşteri için erişimi olan Azure AD'de kontrol edebilirsiniz
- Otomatik olarak SAP buluta müşteri için (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için müşteri SAP bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- SAP Bulutu müşteri çoklu oturum açma için abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAP bulut müşteri için Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP bulut müşteri için Galeriden ekleme
SAP bulut tümleştirme Azure AD'ye müşteri için yapılandırmak için SAP bulut müşteri için Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**SAP bulut müşteri için Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **müşteri için SAP bulut**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Sonuçlar panelinde seçin **müşteri için SAP bulut**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAP bulut ile "Britta Simon" adlı bir test kullanıcıya bağlı müşteri için test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen SAP bulutta müşteri için bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve SAP bulutta müşteri için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri müşteri SAP buluta atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma SAP Bulutu ile müşterinin sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Müşteri test kullanıcısı için bir SAP bulut oluşturma](#creating-a-sap-cloud-for-customer-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı müşteri için SAP buluta sahip.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SAP bulut müşteri uygulaması için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma için müşteri SAP bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SAP bulut müşteri için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Üzerinde **müşteri etki alanı ve URL'ler için SAP bulut** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server name>.crm.ondemand.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [müşteri istemci destek ekibi için SAP bulut](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) bu değerleri almak için. 

4. Üzerinde **kullanıcı öznitelikleri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. İçinde **kullanıcı tanımlayıcısı** listesinde **ExtractMailPrefix()** işlevi.

    b. Gelen **posta** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği seçin.
    Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı olarak kullanmak istediğiniz ve öznitelik değeri ExtensionAttribute2 depoladığınız, ardından user.extensionattribute2 seçin.  

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Üzerinde **müşteri yapılandırması için SAP bulut** 'yi tıklatın **müşteri için SAP bulut yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. Yapılandırılmış SSO almak için aşağıdaki adımları gerçekleştirin:
   
    a. Müşteri portalı yönetici haklarıyla oturum açma SAP buluta.
   
    b. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.
   
    c. Tıklatın **yeni kimlik sağlayıcısı** ve Azure portalından indirdiğiniz meta veri XML dosyasını seçin. Meta verileri içe aktararak sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikası karşıya yükleme.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory gerekir böylece select SAML isteği onaylama tüketici hizmeti URL'si öğesinde **onaylama tüketici hizmeti URL'si dahil** onay kutusu.
   
    e. Tıklatın **çoklu oturum açmayı etkinleştirme**.
   
    f. Yaptığınız değişiklikleri kaydedin.
   
    g. Tıklatın **My sistem** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. İçinde **Azure AD oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Kullanıcı kimliği ve parola veya SSO ile seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçimi**.
   
    j. İçinde **SSO URL** bölümünde, sisteme oturum açmaya çalışanlarınız tarafından kullanılan URL'yi belirtin. 
    İçinde **URL gönderilen çalışana** listesinde, aşağıdaki seçenekler arasından seçim yapabilirsiniz:
   
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

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Müşteri test kullanıcısı için bir SAP bulut oluşturma

Bu bölümde, Britta Simon SAP bulutta müşteri için adlı bir kullanıcı oluşturun. Lütfen çalışmak [müşteri destek ekibi için SAP bulut](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) müşteri platform için SAP bulutta kullanıcıları eklemek için. 

> [!NOTE]
> Lütfen NameID değeri SAP bulut müşteri platform için kullanıcı adı alanıyla eşleşmelidir emin olun.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, müşteri için SAP buluta erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon müşteri için SAP buluta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **müşteri için SAP bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Müşteri kutucuğu erişim Paneli'nde SAP bulut tıklattığınızda, otomatik olarak SAP Bulutunuzu müşteri uygulaması için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

