---
title: 'Öğretici: Azure Active Directory Tümleştirme Jitbit Yardım Masası ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Jitbit Yardım Masası arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: c8e387bb98ad2e23c667ba058ff8ab5dbfedffbf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34343938"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Öğretici: Azure Active Directory Tümleştirme Jitbit Yardım Masası ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Jitbit Yardım Masası tümleştirmek öğrenin.

Jitbit Yardım Masası Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Jitbit Yardım Masası erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Yardım Masası Jitbit açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Jitbit Yardım Masası ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Jitbit Yardım Masası çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Jitbit Yardım Masası ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Galeriden Jitbit Yardım Masası ekleme
Azure AD Jitbit Yardım Masası tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Jitbit Yardım Masası eklemeniz gerekir.

**Galeriden Jitbit Yardım Masası eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Jitbit Yardım Masası**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. Sonuçlar panelinde seçin **Jitbit Yardım Masası**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Jitbit Yardım Masası ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD Jitbit Yardım Masası karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Jitbit Yardım Masası ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Jitbit Yardım Masası Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Jitbit Yardım Masası ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jitbit Yardım Masası test kullanıcısı oluşturma](#creating-a-jitbit-helpdesk-test-user)**  - Britta Simon, karşılık gelen Jitbit kullanıcı Azure AD gösterimini bağlı olan Yardım Masası sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Jitbit Yardım Masası uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Jitbit Yardım Masası ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Jitbit Yardım Masası** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. Üzerinde **Jitbit Yardım Masası etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Jitbit Yardım Masası istemci destek ekibi](https://www.jitbit.com/support/) bu değeri alınamıyor. 
    
    b.  İçinde **tanımlayıcısı** metin kutusuna, şu URL'yi yazın: `https://www.jitbit.com/web-helpdesk/`

    
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. Üzerinde **Jitbit Yardım Masası yapılandırma** 'yi tıklatın **yapılandırma Jitbit Yardım Masası** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. Farklı web tarayıcısı penceresinde Jitbit Yardım Masası şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **Yönetim**.
   
    ![Yönetim](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

9. Tıklatın **genel ayarları**.
   
    ![Kullanıcılar, şirketler ve izinleri](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "kullanıcıları, şirketler ve izinleri")

10. İçinde **kimlik doğrulama ayarlarını** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama ayarlarını](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "kimlik doğrulama ayarları")
    
    a. Seçin **SAML 2.0 tek etkinleştirmek oturum**, çoklu oturum açma (SSO), kullanarak oturum **OneLogin**.

    b. İçinde **uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. Açık, **base-64** kodlanmış sertifika Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu

    d. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, tür adı olarak **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Jitbit Yardım Masası test kullanıcısı oluşturma

Azure AD kullanıcıların Jitbit Yardım Masası oturum etkinleştirmek için bunların Jitbit Yardım Masası sağlanmalıdır.  Jitbit Yardım Masası söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Jitbit Yardım Masası** Kiracı.

2. Üstteki menüde tıklatın **Yönetim**.
   
    ![Yönetim](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

3. Tıklatın **kullanıcıları, şirketler ve izinleri**.
   
    ![Kullanıcılar, şirketler ve izinleri](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "kullanıcıları, şirketler ve izinleri")

4. Tıklatın **Kullanıcı Ekle**.
   
    ![Kullanıcı Ekle](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Kullanıcı Ekle")
   
5. Create bölümünde aşağıdaki gibi sağlamak istediğiniz Azure AD hesabının veri türü:

    ![Oluşturma](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "oluşturma")
   
   a. İçinde **kullanıcıadı** metin kutusuna, türü **BrittaSimon**, Azure portalında olduğu gibi kullanıcı adı.

   b. İçinde **e-posta** metin kutusuna, kullanıcının e-posta ister **BrittaSimon@contoso.com**.

   c. İçinde **ad** metin kutusuna, tür ilk gibi kullanıcı adını **Britta**.

   d. İçinde **Soyadı** metin kutusuna, türü kullanıcının soyadını gibi **Simon**.
   
   e. **Oluştur**’a tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Jitbit Yardım Masası kullanıcı hesabı oluşturma araçlarını veya Jitbit Yardım Masası tarafından sağlanan API'leri kullanabilirsiniz.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Jitbit Yardım Masası için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Jitbit Yardım Masası için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Jitbit Yardım Masası**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jitbit Yardım Masası parçasında tıkladığınızda, oturum açma sayfasına Jitbit Yardım Masası uygulamasının almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

