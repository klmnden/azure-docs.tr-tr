---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Weekdone | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Weekdone arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: jeedes
ms.openlocfilehash: a64daea76eb8b20cd8a8bb202705c995915806a4
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982059"
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a>Öğretici: Azure Active Directory Tümleştirme Weekdone ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Weekdone tümleştirmek öğrenin.

Weekdone Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Weekdone erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Weekdone (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Weekdone ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Weekdone çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Weekdone ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-weekdone-from-the-gallery"></a>Galeriden Weekdone ekleme
Azure AD Weekdone tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Weekdone eklemeniz gerekir.

**Galeriden Weekdone eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Weekdone**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/tutorial_weekdone_search.png)

5. Sonuçlar panelinde seçin **Weekdone**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Weekdone sınayın.

Tekli çalışmaya oturum için Azure AD Weekdone karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Weekdone ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Weekdone ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Weekdone test kullanıcısı oluşturma](#creating-a-weekdone-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Weekdone sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Weekdone uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Weekdone yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Weekdone** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. Üzerinde **Weekdone etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_url1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://weekdone.com/a/<tenant>/metadata`

    > [!NOTE]
    > Aynı URL'yi kullanarak ile weekdone meta veri dosyasından alınabilir.

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://weekdone.com/a/<tenantname>`

4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://weekdone.com/a/<tenantname>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Weekdone istemci destek ekibi](mailto:hello@weekdone.com) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_general_400.png)
    
7. Üzerinde **Weekdone yapılandırma** 'yi tıklatın **yapılandırma Weekdone** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_configure.png) 

8. Çoklu oturum açma yapılandırmak için **Weekdone** yan, indirilen göndermek için ihtiyacınız **meta veri XML'i, Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [Weekdone destek ekibi ](mailto:hello@weekdone.com).

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/weekdone-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-weekdone-test-user"></a>Weekdone test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Weekdone adlı bir kullanıcı oluşturmaktır. Weekdone yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Weekdone erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [Weekdone istemci destek ekibi](mailto:hello@weekdone.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Weekdone için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Weekdone için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Weekdone**.

    ![Çoklu oturum açmayı yapılandırın](./media/weekdone-tutorial/tutorial_weekdone_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim paneli Weekdone parçasında tıklattığınızda, otomatik olarak Weekdone uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/weekdone-tutorial/tutorial_general_01.png
[2]: ./media/weekdone-tutorial/tutorial_general_02.png
[3]: ./media/weekdone-tutorial/tutorial_general_03.png
[4]: ./media/weekdone-tutorial/tutorial_general_04.png

[100]: ./media/weekdone-tutorial/tutorial_general_100.png

[200]: ./media/weekdone-tutorial/tutorial_general_200.png
[201]: ./media/weekdone-tutorial/tutorial_general_201.png
[202]: ./media/weekdone-tutorial/tutorial_general_202.png
[203]: ./media/weekdone-tutorial/tutorial_general_203.png
