---
title: 'Öğretici: Azure Active Directory Tümleştirme ile GaggleAMP | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile GaggleAMP arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: jeedes
ms.openlocfilehash: 797096e1be12124a8e8493ec725913614232d50e
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972027"
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Öğretici: Azure Active Directory Tümleştirme GaggleAMP ile

Bu öğreticide, Azure Active Directory (Azure AD) ile GaggleAMP tümleştirmek öğrenin.

GaggleAMP Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- GaggleAMP erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için GaggleAMP (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme GaggleAMP ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir GaggleAMP çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden GaggleAMP ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-gaggleamp-from-the-gallery"></a>Galeriden GaggleAMP ekleme
Azure AD GaggleAMP tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden GaggleAMP eklemeniz gerekir.

**Galeriden GaggleAMP eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **GaggleAMP**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. Sonuçlar panelinde seçin **GaggleAMP**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı GaggleAMP sınayın.

Tekli çalışmaya oturum için Azure AD GaggleAMP karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının GaggleAMP ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

GaggleAMP içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma GaggleAMP ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[GaggleAMP test kullanıcısı oluşturma](#creating-a-gaggleamp-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı GaggleAMP sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma GaggleAMP uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile GaggleAMP yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **GaggleAMP** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. Üzerinde **GaggleAMP etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://accounts.gaggleamp.com/auth/saml/callback`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_url1.png)

     İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://gaggleamp.com/i/<customerid>`

    > [!NOTE]
    > Oturum açma URL'si değeri gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [GaggleAMP istemci destek ekibi](mailto:sales@gaggleamp.com) bu değeri alınamıyor.
 
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_general_400.png)

7. Üzerinde **GaggleAMP yapılandırma** 'yi tıklatın **yapılandırma GaggleAMP** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

8. Başka bir tarayıcı örneğinde Gaggle tarafınızdan takım desteklemek için oluşturulan SAML SSO sayfasına gidin (örneğin: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

9. Üzerinde **SAML SSO** sayfasında, aşağıdaki adımları gerçekleştirin:  
   
    ![GaggleAMP çoklu oturum açma](./media/gaggleamp-tutorial/tutorial_gaggleamp_06.png)

    a. Seçin **diğer** form **kimlik sağlayıcısı** açılır menüsünde.
    
    b. İçinde **kimlik sağlayıcısı veren** metin değerini yapıştırın **veren URL'si** Azure portalından kopyalanan.
    
    c. İçinde **kimlik sağlayıcısı tek oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
    
    d. İndirilen açmak **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu.
    
    e. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/gaggleamp-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-gaggleamp-test-user"></a>GaggleAMP test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde GaggleAMP adlı bir kullanıcı oluşturmaktır. GaggleAMP yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa GaggleAMP erişme denemesi sırasında oluşturulur. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta GaggleAMP için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**GaggleAMP için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **GaggleAMP**.

    ![Çoklu oturum açmayı yapılandırın](./media/gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli GaggleAMP parçasında tıklattığınızda, otomatik olarak GaggleAMP uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/gaggleamp-tutorial/tutorial_general_203.png
