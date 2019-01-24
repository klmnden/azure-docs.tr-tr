---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile operasyon müdür Yardımcısı | Microsoft Docs'
description: Azure Active Directory ve operasyon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 7e444e37685d862839569f676375fca64fcd4793
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826420"
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Öğretici: Azure Active Directory Tümleştirmesi ile operasyon müdür Yardımcısı

Bu öğreticide, operasyon müdür yardımcısı, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Operasyon müdür yardımcısı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Operasyon erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanmış için operasyon (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile operasyon yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir operasyon çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Operasyon galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-deputy-from-the-gallery"></a>Operasyon galeri ekleme
Azure AD'ye operasyon tümleştirmesini yapılandırmak için operasyon müdür yardımcısı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden operasyon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **operasyon**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/tutorial_deputy_search.png)

1. Sonuçlar panelinde seçin **operasyon**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı operasyon birlikte test etme

Tek çalışmak için oturum açma için Azure AD ne operasyon karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve operasyon ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değeri, operasyon müdür yardımcısı, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma operasyon birlikte test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Operasyon test kullanıcısı oluşturma](#creating-a-deputy-test-user)**  - kullanıcı Azure AD gösterimini bağlı baş Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve operasyon uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile operasyon müdür Yardımcısı'nı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **operasyon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_samlbase.png)

1. Üzerinde **operasyon etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<your-subdomain>.<region>.deputy.com`
    
    >[!NOTE]
    > Operasyon bölge soneki isteğe bağlı ya da bunlardan birini kullanmanız gerekir: au | na | AB | olarak | la | af | bir | ent au | ent na | ent AB | ent-olarak | üst la | üst-af | bir üst

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [operasyon Destek ekibine](https://www.deputy.com/call-centers-customer-support-scheduling-software) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_general_400.png)
    
1. Üzerinde **operasyon yapılandırma** bölümünde **yapılandırma operasyon** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_configure.png) 

1. Aşağıdaki URL'ye gidin:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config). Git **güvenlik ayarları** tıklatıp **Düzenle**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_004.png)

1. Bu **güvenlik ayarları** sayfasında, aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_005.png)
    
    a. Etkinleştirme **sosyal oturum açma**.
   
    b. Not Defteri'nde Azure portalından indirilen, Base64 olarak kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın için **OpenSSL sertifikası** metin.
   
    c. SAML SSO URL metin kutusuna yazın. `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. SAML SSO URL metin kutusuna değiştirin `<your subdomain>` , alt etki alanı ile.
   
    e. SAML SSO URL metin kutusuna değiştirin `<saml sso url>` ile **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız.
   
    f. Tıklayın **ayarlarını kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/deputy-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-deputy-test-user"></a>Operasyon test kullanıcısı oluşturma

Operasyon için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların operasyon sağlanması gerekir. Operasyon durumunda sağlama bir el ile bir görevdir.

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:
1. Operasyon şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üst gezinti bölmesinde tıklatın **kişiler**.
   
   ![Kişiler](./media/deputy-tutorial/tutorial_deputy_001.png "kişiler")

1. Tıklayın **ekleme kişiler** düğmesini tıklatın ve tıklatın **tek bir kişi ekleyin**.
   
   ![Kişi Ekle](./media/deputy-tutorial/tutorial_deputy_002.png "Kişi Ekle")

1. Aşağıdaki adımları uygulayın ve tıklayın **Kaydet ve davet**.
   
   ![Yeni kullanıcı](./media/deputy-tutorial/tutorial_deputy_003.png "yeni kullanıcı")

   a. İçinde **adı** metin kutusuna bir kullanıcı gibi tür adını **BrittaSimon**.
   
   b. İçinde **e-posta** metin sağlamak istediğiniz bir Azure AD hesabı e-posta adresini yazın.
   
   c. İçinde **sırasında iş** metin iş adı yazın.
   
   d. Tıklayın **Kaydet ve davet** düğmesi.

1. AAD hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler. Herhangi diğer operasyon kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak operasyon müdür Yardımcısı tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, operasyon için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon operasyon için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **operasyon**.

    ![Çoklu oturum açmayı yapılandırın](./media/deputy-tutorial/tutorial_deputy_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde operasyon kutucuğa tıkladığınızda, otomatik olarak operasyon uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/deputy-tutorial/tutorial_general_01.png
[2]: ./media/deputy-tutorial/tutorial_general_02.png
[3]: ./media/deputy-tutorial/tutorial_general_03.png
[4]: ./media/deputy-tutorial/tutorial_general_04.png

[100]: ./media/deputy-tutorial/tutorial_general_100.png

[200]: ./media/deputy-tutorial/tutorial_general_200.png
[201]: ./media/deputy-tutorial/tutorial_general_201.png
[202]: ./media/deputy-tutorial/tutorial_general_202.png
[203]: ./media/deputy-tutorial/tutorial_general_203.png

