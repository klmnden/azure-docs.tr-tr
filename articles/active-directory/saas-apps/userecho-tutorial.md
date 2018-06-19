---
title: 'Öğretici: Azure Active Directory Tümleştirme ile UserEcho | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile UserEcho arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f05189f70428eb3a3199870992956000dc55b954
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982193"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Öğretici: Azure Active Directory Tümleştirme UserEcho ile

Bu öğreticide, Azure Active Directory (Azure AD) ile UserEcho tümleştirmek öğrenin.

UserEcho Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- UserEcho erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için UserEcho (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme UserEcho ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir UserEcho çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden UserEcho ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-userecho-from-the-gallery"></a>Galeriden UserEcho ekleme
Azure AD UserEcho tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden UserEcho eklemeniz gerekir.

**Galeriden UserEcho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **UserEcho**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/tutorial_userecho_search.png)

5. Sonuçlar panelinde seçin **UserEcho**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UserEcho sınayın.

Tekli çalışmaya oturum için Azure AD UserEcho karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının UserEcho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

UserEcho içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma UserEcho ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[UserEcho test kullanıcısı oluşturma](#creating-a-userecho-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı UserEcho sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma UserEcho uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile UserEcho yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **UserEcho** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_samlbase.png)

3. Üzerinde **UserEcho etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.userecho.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [UserEcho istemci destek ekibi](https://feedback.userecho.com/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_general_400.png)

6. Üzerinde **UserEcho yapılandırma** 'yi tıklatın **yapılandırma UserEcho** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_configure.png) 

7. Başka bir tarayıcı penceresinde UserEcho şirket sitenize yönetici olarak oturum açma.

8. Üstteki araç çubuğunda menüsünü genişletin kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png) 

9. Tıklatın **tümleştirmeler**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_07.png) 

10. Tıklatın **Web sitesi**ve ardından **çoklu oturum açma (SAML2)**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_08.png) 

11. Üzerinde **çoklu oturum açma (SAML)** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Olarak **SAML etkin**seçin **Evet**.
    
    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **SAML SSO URL** metin kutusu.
    
    c. Yapıştır **Sign-Out URL**, Azure portalından kopyalanan **uzak logoout URL** metin kutusu.
    
    d. İndirilen sertifikanızı Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **X.509 sertifikası** metin kutusu.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/userecho-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-userecho-test-user"></a>UserEcho test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde UserEcho adlı bir kullanıcı oluşturmaktır.

**İçinde UserEcho Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. UserEcho şirket sitenize yönetici olarak oturum.

2. Üstteki araç çubuğunda menüsünü genişletin kullanıcı adınıza tıklayın ve ardından **Kurulum**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_06.png)

3. Tıklatın **kullanıcılar**, genişletmek için **kullanıcılar** bölümü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_10.png)

4. **Kullanıcılar**’a tıklayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_11.png)

5. Tıklatın **yeni bir kullanıcı davet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_12.png)

6. Üzerinde **yeni bir kullanıcı davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. İçinde **adı** metin kutusu, kullanıcı Britta Simon gibi türünün adı.
    
    b.  İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.
    
    c. Tıklatın **davet**.

Her UserEcho kullanmaya başlamak sağlayan Britta için bir davet gönderilir. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta UserEcho için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**UserEcho için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **UserEcho**.

    ![Çoklu oturum açmayı yapılandırın](./media/userecho-tutorial/tutorial_userecho_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.  

Erişim paneli UserEcho parçasında tıklattığınızda, otomatik olarak UserEcho uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/userecho-tutorial/tutorial_general_01.png
[2]: ./media/userecho-tutorial/tutorial_general_02.png
[3]: ./media/userecho-tutorial/tutorial_general_03.png
[4]: ./media/userecho-tutorial/tutorial_general_04.png

[100]: ./media/userecho-tutorial/tutorial_general_100.png

[200]: ./media/userecho-tutorial/tutorial_general_200.png
[201]: ./media/userecho-tutorial/tutorial_general_201.png
[202]: ./media/userecho-tutorial/tutorial_general_202.png
[203]: ./media/userecho-tutorial/tutorial_general_203.png

