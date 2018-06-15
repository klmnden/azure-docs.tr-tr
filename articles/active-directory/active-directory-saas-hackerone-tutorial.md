---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Hackerone | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Hackerone arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 2d82b4cae516b88a01c4a31c8a72c82203e91630
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344023"
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Öğretici: Azure Active Directory Tümleştirme HackerOne ile

Bu öğreticide, Azure Active Directory (Azure AD) ile HackerOne tümleştirmek öğrenin.

HackerOne Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- HackerOne erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için HackerOne (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme HackerOne ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir HackerOne çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden HackerOne ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-hackerone-from-the-gallery"></a>Galeriden HackerOne ekleme
Azure AD HackerOne tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden HackerOne eklemeniz gerekir.

**Galeriden HackerOne eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **HackerOne**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. Sonuçlar panelinde seçin **HackerOne**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı HackerOne ile test etme

Tekli çalışmaya oturum için Azure AD HackerOne karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının HackerOne ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

HackerOne içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma HackerOne ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[HackerOne test kullanıcısı oluşturma](#creating-a-hackerone-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı HackerOne sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma HackerOne uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile HackerOne yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **HackerOne** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. Üzerinde **HackerOne tek oturum açma URL'si ve tanımlayıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://hackerone.com/<company name>/authentication`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:  `https://hackerone.com/users/saml/metadata`
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [HackerOne destek ekibi](mailto:support@hackerone.com) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. Üzerinde **HackerOne yapılandırma** 'yi tıklatın **yapılandırma HackerOne** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. Oturum açmayı HackerOne Kiracı yönetici olarak.

8. Üstteki menüde tıklayın "**ayarları**."
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. Gidin "**kimlik doğrulaması**"tıklatıp"**SAML ayarları ekleme**."
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. Üzerinde **SAML ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    a. İçinde **e-posta etki alanı** metin kutusuna, kayıtlı bir etki alanı yazın.

    b. İçinde **üzerinde tek oturum URL'si** metin kutuları, yapıştırma değerini **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. Açık, **sertifika dosyası** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X509 sertifika** metin kutusu.
    
    d. **Kaydet**’e tıklayın.

11. Kimlik doğrulama ayarları iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    a. Tıklatın **testi**.

    b. Varsa değerini **durum** alan eşittir **durumu'son test: oluşturulan**, kişi, [HackerOne destek ekibi](mailto:support@hackerone.com) yapılandırmanızı gözden istemek için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-hackerone-test-user"></a>HackerOne test kullanıcısı oluşturma

Ardından, HackerOne içinde Britta Simon adlı bir kullanıcı oluşturun. HackerOne yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. HackerOne eriştiğinizde, henüz yoksa yeni bir kullanıcı oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, Certify Destek ekibine başvurun gerekir. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta HackerOne için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**HackerOne için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **HackerOne**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Son olarak, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.  

Erişim paneli HackerOne parçasında tıklattığınızda, otomatik olarak HackerOne uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

