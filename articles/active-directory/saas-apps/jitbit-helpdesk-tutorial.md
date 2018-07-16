---
title: 'Öğretici: Azure Active Directory Jitbit Yardım Masası ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Jitbit Yardım Masası arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: bbfee75497681df0d51e164b3035b9b88c67ccaa
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052144"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Öğretici: Azure Active Directory Jitbit Yardım Masası ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jitbit Yardım Masası tümleştirme konusunda bilgi edinin.

Azure AD ile Jitbit Yardım Masası tümleştirme ile aşağıdaki avantajları sağlar:

- Jitbit Yardım Masası erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Yardım Masası Jitbit açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Jitbit Yardım Masası ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Jitbit Yardım Masası çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Jitbit Yardım Masası ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Galeriden Jitbit Yardım Masası ekleme
Azure AD'de Jitbit Yardım Masası tümleştirmesini yapılandırmak için Jitbit Yardım Masası Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Jitbit Yardım Masası eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Jitbit Yardım Masası**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. Sonuçlar panelinde seçin **Jitbit Yardım Masası**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Jitbit Yardım Masası ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Jitbit Yardım Masası karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Jitbit Yardım Masası ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Jitbit Yardım Masası, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Jitbit Yardım Masası ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jitbit Yardım Masası test kullanıcısı oluşturma](#creating-a-jitbit-helpdesk-test-user)**  - Jitbit kullanıcı Azure AD gösterimini bağlı olan Yardım Masası Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Jitbit Yardım Masası uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Jitbit Yardım Masası ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Jitbit Yardım Masası** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. Üzerinde **Jitbit Yardım Masası etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Jitbit Yardım Masası istemci Destek ekibine](https://www.jitbit.com/support/) bu değeri alınamıyor. 
    
    b.  İçinde **tanımlayıcı** metin kutusuna bir URL şu şekilde: `https://www.jitbit.com/web-helpdesk/`

    
 


4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. Üzerinde **Jitbit Yardım Masası yapılandırma** bölümünde **yapılandırma Jitbit Yardım Masası** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. Farklı bir web tarayıcı penceresinde Jitbit Yardım Masası şirket sitenize yönetici olarak oturum.

8. Üst araç çubuğunda tıklatın **Yönetim**.
   
    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

9. Tıklayın **genel ayarlar**.
   
    ![Kullanıcıları, şirketler ve izinleri](./media/jitbit-helpdesk-tutorial/ic777680.png "kullanıcıları, şirketler ve izinleri")

10. İçinde **kimlik doğrulama ayarları** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama ayarları](./media/jitbit-helpdesk-tutorial/ic777683.png "kimlik doğrulama ayarları")
    
    a. Seçin **SAML 2.0 tek etkinleştirme oturum**, ile çoklu oturum açma (SSO), kullanarak oturum açmanız **OneLogin**.

    b. İçinde **uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Açık, **base-64** kodlanmış sertifika Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin kutusu

    d. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin türü adı olarak **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Jitbit Yardım Masası test kullanıcısı oluşturma

Azure AD kullanıcılarının Jitbit Yardım Masası oturum etkinleştirmek için bunlar Jitbit Yardım Masası sağlanması gerekir.  Jitbit Yardım Masası söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Jitbit Yardım Masası** Kiracı.

2. Üstteki menüden **Yönetim**.
   
    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

3. Tıklayın **kullanıcıları, şirketler ve izinleri**.
   
    ![Kullanıcıları, şirketler ve izinleri](./media/jitbit-helpdesk-tutorial/ic777682.png "kullanıcıları, şirketler ve izinleri")

4. Tıklayın **Kullanıcı Ekle**.
   
    ![Kullanıcı Ekle](./media/jitbit-helpdesk-tutorial/ic777685.png "Kullanıcı Ekle")
   
5. Oluşturma bölümünde şu şekilde sağlamak için Azure AD hesap verilerini yazın:

    ![Oluşturma](./media/jitbit-helpdesk-tutorial/ic777686.png "oluşturma")
   
   a. İçinde **kullanıcıadı** metin kutusuna **BrittaSimon**, Azure portalında olduğu gibi kullanıcı adı.

   b. İçinde **e-posta** metin kutusu, kullanıcının e-posta ister **BrittaSimon@contoso.com**.

   c. İçinde **ad** metin adı gibi kullanıcı türü **Britta**.

   d. İçinde **Soyadı** metin türü Soyadı gibi kullanıcının **Simon**.
   
   e. **Oluştur**’a tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Jitbit Yardım Masası kullanıcı hesabı oluşturma araçları veya Jitbit Yardım Masası tarafından sağlanan API'leri kullanabilirsiniz.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Jitbit Yardım Masası için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Jitbit yardım masasına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Jitbit Yardım Masası**.

    ![Çoklu oturum açmayı yapılandırın](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jitbit Yardım Masası kutucuğa tıkladığınızda, oturum açma sayfası Jitbit Yardım Masası uygulaması almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/jitbit-helpdesk-tutorial/tutorial_general_203.png

