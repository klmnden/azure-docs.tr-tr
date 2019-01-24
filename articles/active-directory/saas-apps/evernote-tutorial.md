---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Evernote | Microsoft Docs'
description: Azure Active Directory ve Evernote arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 8ba80e113de8ea6754d8d2d6446fb26498904e12
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816815"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Öğretici: Evernote ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Evernote tümleştirme konusunda bilgi edinin.

Azure AD ile Evernote tümleştirme ile aşağıdaki avantajları sağlar:

- Evernote erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Evernote'a (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Evernote yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Evernote çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Evernote ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-evernote-from-the-gallery"></a>Galeriden Evernote ekleme
Azure AD'de Evernote tümleştirmesini yapılandırmak için Evernote Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Evernote eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Evernote**seçin **Evernote** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Evernote](./media/evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Evernote sınayın.

Tek iş için oturum açma için Azure AD ne Evernote karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Evernote ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Evernote içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Evernote ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Evernote test kullanıcısı oluşturma](#create-an-evernote-test-user)**  - kullanıcı Azure AD gösterimini bağlı Evernote Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Evernote uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Evernote yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Evernote** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/evernote-tutorial/tutorial_evernote_samlbase.png)

1. Üzerinde **Evernote etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Evernote etki alanı ve URL'ler tek oturum açma bilgileri](./media/evernote-tutorial/tutorial_evernote_url.png)

    İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://www.evernote.com/saml2`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Evernote etki alanı ve URL'ler tek oturum açma bilgileri](./media/evernote-tutorial/tutorial_evernote_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://www.evernote.com/Login.action`   

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/evernote-tutorial/tutorial_evernote_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/evernote-tutorial/tutorial_general_400.png)

1. Üzerinde **Evernote yapılandırma** bölümünde **yapılandırma Evernote** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Evernote yapılandırma](./media/evernote-tutorial/tutorial_evernote_configure.png) 

1. Farklı bir web tarayıcı penceresinde Evernote şirket sitenize yönetici olarak oturum.

1. Git **'Yönetici Konsolu'**

    ![Yönetici Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

1. Gelen **'Yönetici Konsolu'** Git **'Güvenlik'** seçip **' çoklu oturum açma '**

    ![SSO ayarı](./media/evernote-tutorial/tutorial_evernote_sso.png)

1. Aşağıdaki değerleri yapılandırın:

    ![Sertifika ayarları](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **SSO'yu etkinleştirin:** SSO, varsayılan olarak etkindir (tıklayın **devre dışı çoklu oturum açma** SSO gereksinimini kaldırmak için)

    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **SAML HTTP isteği URL'si** metin.

    c. İndirilen sertifika Azure AD'den bir Not Defteri'nde açın ve "Sertifika başlayın" ve "END CERTIFICATE" dahil olmak üzere içeriği kopyalayın ve yapıştırın **X.509 sertifikası** metin. 

    d.Click **Değişiklikleri Kaydet**

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/evernote-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/evernote-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/evernote-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/evernote-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-evernote-test-user"></a>Bir Evernote test kullanıcısı oluşturma

Evernote açarken Azure AD kullanıcılarının etkinleştirmek için bunların Evernote sağlanması gerekir.  
Evernote söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Evernote şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **'Yönetici Konsolu'**.

    ![Yönetici Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

1. Gelen **'Yönetici Konsolu'** Git **'kullanıcı ekleme'**.

    ![TestUser Ekle](./media/evernote-tutorial/create_aaduser_0001.png)

1. **Takım üyeleri ekleme** içinde **e-posta** metin kullanıcı hesabının e-posta adresini yazın ve tıklayın **davet edin.**

    ![TestUser Ekle](./media/evernote-tutorial/create_aaduser_0002.png)
    
1. Davet gönderildikten sonra Azure Active Directory hesap sahibinin davetiyeyi kabul etmek için e-posta alırsınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Evernote'a erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Evernote'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Evernote**.

    ![Uygulamalar listesinde Evernote bağlantı](./media/evernote-tutorial/tutorial_evernote_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Evernote kutucuğa tıkladığınızda, Evernote uygulamanıza açan. Ancak daha sonra hesabı gerekir kişisel hesabınızla oturum açmak için bir kuruluş olarak oturum açtıktan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/evernote-tutorial/tutorial_general_01.png
[2]: ./media/evernote-tutorial/tutorial_general_02.png
[3]: ./media/evernote-tutorial/tutorial_general_03.png
[4]: ./media/evernote-tutorial/tutorial_general_04.png

[100]: ./media/evernote-tutorial/tutorial_general_100.png

[200]: ./media/evernote-tutorial/tutorial_general_200.png
[201]: ./media/evernote-tutorial/tutorial_general_201.png
[202]: ./media/evernote-tutorial/tutorial_general_202.png
[203]: ./media/evernote-tutorial/tutorial_general_203.png

