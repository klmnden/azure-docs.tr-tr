---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ClickTime | Microsoft Docs'
description: Azure Active Directory ve ClickTime arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: f1c7c3cf850ed48412c8a232e364f927248ed9bf
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811137"
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Öğretici: ClickTime ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ClickTime tümleştirme konusunda bilgi edinin.

Azure AD ile ClickTime tümleştirme ile aşağıdaki avantajları sağlar:

- ClickTime erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için ClickTime (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ClickTime yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik ClickTime çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ClickTime ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-clicktime-from-the-gallery"></a>Galeriden ClickTime ekleme
Azure AD'de ClickTime tümleştirmesini yapılandırmak için ClickTime Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ClickTime eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **ClickTime**seçin **ClickTime** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ClickTime](./media/clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ClickTime sınayın.

Tek iş için oturum açma için Azure AD ne ClickTime karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ClickTime ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ClickTime içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ClickTime ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[ClickTime test kullanıcısı oluşturma](#create-a-clicktime-test-user)**  - kullanıcı Azure AD gösterimini bağlı ClickTime Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ClickTime uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ClickTime yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ClickTime** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/clicktime-tutorial/tutorial_clicktime_samlbase.png)

1. Üzerinde **ClickTime etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ClickTime etki alanı ve URL'ler tek oturum açma bilgileri](./media/clicktime-tutorial/tutorial_clicktime_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://app.clicktime.com/sp/`
    
    b. İçinde **yanıt URL'si** metin kutusuna bir URL kullanarak aşağıdaki düzenler: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/clicktime-tutorial/tutorial_clicktime_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clicktime-tutorial/tutorial_general_400.png)

1. Üzerinde **ClickTime yapılandırma** bölümünde **yapılandırma ClickTime** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![ClickTime yapılandırma](./media/clicktime-tutorial/tutorial_clicktime_configure.png) 

1. Farklı bir web tarayıcı penceresinde ClickTime şirket sitenize yönetici olarak oturum.

1. Üst araç çubuğunda tıklatın **tercihleri**ve ardından **güvenlik ayarları**.

1. İçinde **tek oturum açma tercihlerini** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Güvenlik ayarları](./media/clicktime-tutorial/tic777280.png "güvenlik ayarları")
   
    a.  Seçin **izin** kullanarak çoklu oturum açma (SSO) ile oturum açın **Azure AD'ye**.
   
    b. İçinde **kimlik sağlayıcısı uç noktası** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    c.  Açık **base-64 kodlamalı sertifika** Azure portalından indirilen **not defteri**içeriği kopyalayın ve ardından yapıştırın **X.509 sertifikası** metin.
   
    d.  **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/clicktime-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/clicktime-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.
 
    ![Ekle düğmesi](./media/clicktime-tutorial/create_aaduser_03.png) 

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/clicktime-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-clicktime-test-user"></a>ClickTime test kullanıcısı oluşturma

ClickTime açarken Azure AD kullanıcılarının etkinleştirmek için bunların ClickTime sağlanması gerekir.  
ClickTime söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

> [!NOTE]
> Herhangi diğer ClickTime kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için ClickTime tarafından sağlanan.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**
1. Oturum açın, **ClickTime** Kiracı.
1. Üst araç çubuğunda tıklatın **şirket**ve ardından **kişiler**.
   
    ![Kişiler](./media/clicktime-tutorial/tic777282.png "kişiler")
1. Tıklayın **Kişi Ekle**.
   
    ![Kişi Ekle](./media/clicktime-tutorial/tic777283.png "Kişi Ekle")
1. Yeni bir kişiye bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kişiler](./media/clicktime-tutorial/tic777284.png "kişiler")
   
    a.  İçinde **tam adı** metin kutusuna tam adı gibi kullanıcı **Britta Simon**. 
  
    b.  İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta türünü ister **brittasimon@contoso.com**.
       
    > [!NOTE]
    > İsterseniz, yeni kişi nesnesinin ek özellikleri ayarlayabilirsiniz.
   
    c.  **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için ClickTime erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ClickTime için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ClickTime**.

    ![Uygulamalar listesinde ClickTimne bağlantı](./media/clicktime-tutorial/tutorial_clicktime_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ClickTime kutucuğa tıkladığınızda, otomatik olarak ClickTime uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clicktime-tutorial/tutorial_general_01.png
[2]: ./media/clicktime-tutorial/tutorial_general_02.png
[3]: ./media/clicktime-tutorial/tutorial_general_03.png
[4]: ./media/clicktime-tutorial/tutorial_general_04.png

[100]: ./media/clicktime-tutorial/tutorial_general_100.png

[200]: ./media/clicktime-tutorial/tutorial_general_200.png
[201]: ./media/clicktime-tutorial/tutorial_general_201.png
[202]: ./media/clicktime-tutorial/tutorial_general_202.png
[203]: ./media/clicktime-tutorial/tutorial_general_203.png

