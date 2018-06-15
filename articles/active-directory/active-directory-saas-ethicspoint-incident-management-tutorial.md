---
title: 'Öğretici: Azure Active Directory Tümleştirme EthicsPoint Olay yönetimi (EPIM) | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve EthicsPoint Olay yönetimi (EPIM) arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: be400e9b51e2956557297b3a57313f08a434f96d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34342557"
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Öğretici: Azure Active Directory Tümleştirme EthicsPoint Olay yönetimi (EPIM)

Bu öğreticide, Azure Active Directory (Azure AD) ile EthicsPoint Olay yönetimi (EPIM) tümleştirme öğrenin.

EthicsPoint Olay yönetimi (EPIM) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi EthicsPoint Olay yönetimi (EPIM için) Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak EthicsPoint Olay yönetimi (EPIM için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme EthicsPoint Olay yönetimi (EPIM) yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir EthicsPoint Olay yönetimi (EPIM) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden EthicsPoint Olay yönetimi (EPIM) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>Galeriden EthicsPoint Olay yönetimi (EPIM) ekleme
Azure AD tümleştirmeye EthicsPoint Olay yönetimi (EPIM) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden EthicsPoint Olay yönetimi (EPIM) eklemeniz gerekir.

**Galeriden EthicsPoint Olay yönetimi (EPIM) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **EthicsPoint Olay yönetimi (EPIM)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. Sonuçlar panelinde seçin **EthicsPoint Olay yönetimi (EPIM)** ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile EthicsPoint Olay yönetimi ("Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı EPIM) test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen EthicsPoint Olay yönetimi (EPIM) bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı EthicsPoint Olay yönetimi (EPIM) arasında bir bağlantı ilişkisi kurulması gerekir.

EthicsPoint Olay yönetimi (EPIM) değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma EthicsPoint Olay yönetimi (EPIM) ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[EthicsPoint Olay yönetimi (EPIM) test kullanıcısı oluşturma](#creating-a-ethicspoint-incident-management-epim-test-user)**  - Britta Simon, karşılık gelen EthicsPoint Olay yönetimi (EPIM) kullanıcı Azure AD gösterimini bağlı olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma EthicsPoint Olay yönetimi (EPIM) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma EthicsPoint Olay yönetimi (EPIM) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **EthicsPoint Olay yönetimi (EPIM)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. Üzerinde **EthicsPoint Olay yönetimi (EPIM) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.navexglobal.com/adfs/services/trust`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<servername>.navexglobal.com/adfs/ls/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek yanıt URL'si tanımlayıcısı ve oturum açma URL'si ile güncelleştirin. Kişi [EthicsPoint Olay yönetimi (EPIM) istemci destek ekibi](http://www.navexglobal.com/company/contact-us) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. Çoklu oturum açma yapılandırmak için **EthicsPoint Olay yönetimi (EPIM)** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [EthicsPoint Olay yönetimi (EPIM) destek ekibi ](http://www.navexglobal.com/company/contact-us).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a>EthicsPoint Olay yönetimi (EPIM) test kullanıcısı oluşturma

Bu bölümde, Britta Simon EthicsPoint Olay yönetimi (EPIM) adlı bir kullanıcı oluşturun. Lütfen çalışmak [EthicsPoint Olay yönetimi (EPIM) destek ekibi](http://www.navexglobal.com/company/contact-us) EthicsPoint Olay yönetimi (EPIM) platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta EthicsPoint Olay yönetimi (EPIM) erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon EthicsPoint Olay yönetimi (EPIM) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **EthicsPoint Olay yönetimi (EPIM)**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Erişim paneli EthicsPoint Olay yönetimi (EPIM) parçasında tıklattığınızda, otomatik olarak EthicsPoint Olay yönetimi (EPIM) uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

