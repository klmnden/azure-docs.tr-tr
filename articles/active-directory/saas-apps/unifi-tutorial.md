---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle UNIFI | Microsoft Docs'
description: Azure Active Directory ve UNIFI arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a93e4863a8466ad6599b11e6fe6e53d8d4d971a4
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049924"
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Öğretici: Azure Active Directory UNIFI ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile UNIFI tümleştirme konusunda bilgi edinin.

Azure AD ile UNIFI tümleştirme ile aşağıdaki avantajları sağlar:

- UNIFI erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için UNIFI (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile UNIFI yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik UNIFI çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden UNIFI ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-unifi-from-the-gallery"></a>Galeriden UNIFI ekleme
Azure AD'de UNIFI tümleştirmesini yapılandırmak için UNIFI Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden UNIFI eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **UNIFI**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/tutorial_unifi_search.png)

5. Sonuçlar panelinde seçin **UNIFI**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UNIFI sınayın.

Tek iş için oturum açma için Azure AD ne UNIFI karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının UNIFI ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

UNIFI içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma UNIFI ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir UNIFI oluşturma, kullanıcı test](#creating-a-unifi-test-user)**  - kullanıcı Azure AD gösterimini bağlı UNIFI Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve UNIFI uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile UNIFI yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **UNIFI** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_samlbase.png)

3. Üzerinde **UNIFI etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_url1.png)

    İçinde **tanımlayıcı** metin değeri yazın: `INVIEWlabs` 

4. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://app.discoverunifi.com/login`

5. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_general_400.png)
    
7. Üzerinde **UNIFI yapılandırma** bölümünde **yapılandırma UNIFI** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_configure.png)

8. Farklı bir web tarayıcı penceresinde oturum açın, **UNIFI** şirketinizin sitesi yöneticisi olarak.

9. Tıklayarak **kullanıcılar**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app1.png) 

10. Tıklayarak **yeni kimlik sağlayıcısı Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app2.png)

11. İçinde **kimlik sağlayıcısı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:   

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/app3.png) 

    a. İçinde **sağlayıcı adı** metin kimlik sağlayıcısı adını yazın...

    b. İçinde **sağlayıcısı URL'si** textbox Yapıştır **SAML çoklu oturum açma hizmeti URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikayı kaldırmak **---BEGIN CERTIFICATE---** ve **---END CERTIFICATE---** etiketleyin ve kalan içeriği içinde yapıştırın**Sertifika** metin.

    d. Seçin **varsayılan sağlayıcıdır** onay kutusu.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/unifi-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-unifi-test-user"></a>UNIFI test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı oluşturun. **UNIFI** hiçbir el ile yapılacak adımlar gerekli olacak şekilde otomatik kullanıcı sağlamayı destekler. Kullanıcılar, Azure AD'den başarılı kimlik doğrulamadan sonra otomatik olarak oluşturulur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için UNIFI erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon UNIFI için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **UNIFI**.

    ![Çoklu oturum açmayı yapılandırın](./media/unifi-tutorial/tutorial_unifi_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde UNIFI kutucuğa tıkladığınızda, otomatik olarak UNIFI uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/unifi-tutorial/tutorial_general_01.png
[2]: ./media/unifi-tutorial/tutorial_general_02.png
[3]: ./media/unifi-tutorial/tutorial_general_03.png
[4]: ./media/unifi-tutorial/tutorial_general_04.png

[100]: ./media/unifi-tutorial/tutorial_general_100.png

[200]: ./media/unifi-tutorial/tutorial_general_200.png
[201]: ./media/unifi-tutorial/tutorial_general_201.png
[202]: ./media/unifi-tutorial/tutorial_general_202.png
[203]: ./media/unifi-tutorial/tutorial_general_203.png

