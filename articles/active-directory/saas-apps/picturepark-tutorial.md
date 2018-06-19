---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Picturepark | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Picturepark arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: cd2caca48992d5fa3bec64004047ecd0fc2b2bfa
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980644"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Öğretici: Azure Active Directory Tümleştirme Picturepark ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Picturepark tümleştirmek öğrenin.

Picturepark Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Picturepark erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Picturepark (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Picturepark ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Picturepark çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Picturepark ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-picturepark-from-the-gallery"></a>Galeriden Picturepark ekleme
Azure AD Picturepark tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Picturepark eklemeniz gerekir.

**Galeriden Picturepark eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Picturepark**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/tutorial_picturepark_search.png)

5. Sonuçlar panelinde seçin **Picturepark**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Picturepark sınayın.

Tekli çalışmaya oturum için Azure AD Picturepark karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Picturepark ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Picturepark içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Picturepark ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Picturepark test kullanıcısı oluşturma](#creating-a-picturepark-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Picturepark sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Picturepark uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Picturepark yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Picturepark** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. Üzerinde **Picturepark etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.picturepark.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Picturepark istemci destek ekibi](https://picturepark.com/about/contact/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_general_400.png)

6. Üzerinde **Picturepark yapılandırma** 'yi tıklatın **yapılandırma Picturepark** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_configure.png) 

7. Farklı web tarayıcısı penceresinde Picturepark şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **Yönetim Konsolu**.
   
    ![Yönetim Konsolu](./media/picturepark-tutorial/ic795062.png "Yönetim Konsolu")

9. Tıklatın **kimlik doğrulaması**ve ardından **kimlik sağlayıcıları**.
   
    ![Kimlik doğrulama](./media/picturepark-tutorial/ic795063.png "kimlik doğrulaması")

10. İçinde **kimlik sağlayıcı Yapılandırması** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı Yapılandırması](./media/picturepark-tutorial/ic795064.png "kimlik sağlayıcı yapılandırması")
   
    a. **Ekle**'ye tıklayın.
  
    b. Yapılandırmanız için bir ad yazın.
   
    c. Seçin **varsayılan olarak ayarla**.
   
    d. İçinde **veren URI** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    e. İçinde **güvenilir verenin parmak iziyle** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü. 

11. Tıklatın **JoinDefaultUsersGroup**.

12. Ayarlamak için **Emailaddress** özniteliğini **talep** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` tıklatıp **kaydetmek**.

      ![Yapılandırma](./media/picturepark-tutorial/ic795065.png "yapılandırma")

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/picturepark-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-picturepark-test-user"></a>Picturepark test kullanıcısı oluşturma

Azure AD kullanıcıların Picturepark oturum etkinleştirmek için bunların Picturepark sağlanmalıdır. Picturepark söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Picturepark** Kiracı.

2. Üstteki araç çubuğunda tıklatın **Yönetimsel Araçlar**ve ardından **kullanıcılar**.
   
    ![Kullanıcıların](./media/picturepark-tutorial/ic795067.png "kullanıcılar")

3. İçinde **kullanıcılara genel bakış** sekmesini tıklatın, **yeni**.
   
    ![Kullanıcı Yönetimi](./media/picturepark-tutorial/ic795068.png "kullanıcı yönetimi")

4. Üzerinde **kullanıcı oluştur** iletişim kutusunda, geçerli bir Azure Active Directory istediğiniz kullanıcı sağlamak için aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/picturepark-tutorial/ic795069.png "kullanıcı oluştur")
   
    a. İçinde **e-posta adresi** metin kutusuna, türü **e-posta adresi** kullanıcının **BrittaSimon@contoso.com**.  
   
    b. İçinde **parola** ve **parolayı onayla** metin kutuları, türü **parola** BrittaSimon biri. 
   
    c. İçinde **ad** metin kutusuna, türü **ad** kullanıcının **Britta**. 
   
    d. İçinde **Soyadı** metin kutusuna, türü **Soyadı** kullanıcının **Simon**.
   
    e. İçinde **şirket** metin kutusuna, türü **şirket adı** kullanıcının. 
   
    f. İçinde **Ülke** metin kutusuna, select **Ülke** kullanıcının.
  
    g. İçinde **ZIP** metin kutusuna, türü **posta kodu** şehrin.
   
    h. İçinde **Şehir** metin kutusuna, türü **şehir adı** kullanıcının.

    i. Seçin bir **dil**.
   
    j. **Oluştur**’a tıklayın.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için Picturepark tarafından sağlanan veya herhangi diğer Picturepark kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Picturepark için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Picturepark için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Picturepark**.

    ![Çoklu oturum açmayı yapılandırın](./media/picturepark-tutorial/tutorial_picturepark_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Picturepark parçasında tıklattığınızda, otomatik olarak Picturepark uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/picturepark-tutorial/tutorial_general_01.png
[2]: ./media/picturepark-tutorial/tutorial_general_02.png
[3]: ./media/picturepark-tutorial/tutorial_general_03.png
[4]: ./media/picturepark-tutorial/tutorial_general_04.png

[100]: ./media/picturepark-tutorial/tutorial_general_100.png

[200]: ./media/picturepark-tutorial/tutorial_general_200.png
[201]: ./media/picturepark-tutorial/tutorial_general_201.png
[202]: ./media/picturepark-tutorial/tutorial_general_202.png
[203]: ./media/picturepark-tutorial/tutorial_general_203.png

