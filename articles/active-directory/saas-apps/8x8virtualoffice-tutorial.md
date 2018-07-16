---
title: 'Öğretici: Azure Active Directory 8x8lik sanal Office ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve 8x8lik sanal Office arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a33f9ba0ca744709e21e9e55acc22b657c2adc2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048428"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Öğretici: Azure Active Directory 8x8lik sanal Office ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 8x8lik sanal Office tümleştirme konusunda bilgi edinin.

8x8lik tümleştirmek Azure AD ile sanal Office ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini 8x8lik sanal Office için Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) 8x8lik sanal Office açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi 8x8lik sanal Office ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik 8x8lik sanal Office çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden 8x8lik sanal Office ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Galeriden 8x8lik sanal Office ekleme
Azure AD'de 8x8lik sanal Office tümleştirmesini yapılandırmak için 8x8lik sanal Office Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden 8x8lik sanal Office eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **8x8lik sanal Office**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. Sonuçlar panelinde seçin **8x8lik sanal Office**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcısı üzerinde sanal tabanlı 8x8lik ile test etme

Tek iş için oturum açma için Azure AD 8 x 8 sanal Office, Azure AD'de bir kullanıcı için olan karşılığı kullanıcının bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının 8x8lik ilgili kullanıcı arasında bir bağlantı ilişki sanal Office kurulması gerekir.

Değerini 8x8lik sanal Office'te atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma 8x8lik sanal Office ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir 8 x 8 sanal Office test kullanıcısı oluşturma](#creating-a-8x8-virtual-office-test-user)**  - kullanıcı Azure AD gösterimini bağlı 8x8lik sanal Office Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve 8x8lik sanal Office uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma 8x8lik sanal Office ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **8x8lik sanal Office** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Üzerinde **8x8lik sanal Office etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [8x8lik sanal Office Destek ekibine](https://www.8x8.com/about-us/contact-us) bu değerleri almak için.
 


4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Üzerinde **8x8lik sanal Office yapılandırma** bölümünde **yapılandırma 8x8lik sanal Office** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. 8x8lik sanal Office kiracınıza yönetici olarak oturum.

8. Seçin **sanal Office hesabı Mgr** uygulama panosunda.
   
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Seçin **iş** yönetmek ve hesap **oturum** düğmesi.
   
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Tıklayın **hesapları** menü listesi sekmesindedir.
   
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Tıklayın **çoklu oturum açma** hesapları listesinde.
   
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Seçin **çoklu oturum açma** kimlik doğrulama yöntemi ve tıklatın altında **SAML**.
    
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Kopyalama **SAML SSO URL**, **çoklu oturum kapatma hizmeti URL'si** ve **veren URL'si** için Azure AD'den **URL'de oturum**, **oturum kapatma URL'si**  ve **veren URL'si** 8x8lik sanal Office. 
    
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Tıklayın **tarayıcı** Azure AD'den yüklediğiniz sertifikayı karşıya yüklemek için düğmesini **Kaydet** düğmesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>Bir 8 x 8 sanal Office test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon 8x8lik sanal Office adlı bir kullanıcı oluşturmaktır. 8x8lik sanal Office tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa 8x8lik sanal Office erişme denemesi sırasında oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [8x8lik sanal Office Destek ekibine](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, 8x8lik sanal Office için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon 8x8lik sanal Office atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **8x8lik sanal Office**.

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli 8x8lik sanal Office kutucuğa tıkladığınızda, size otomatik olarak 8x8lik sanal Office uygulamanızın açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/8x8virtualoffice-tutorial/tutorial_general_203.png

