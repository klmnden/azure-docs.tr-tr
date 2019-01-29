---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile BitaBIZ | Microsoft Docs'
description: Azure Active Directory ve BitaBIZ arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 894363a48f0ba1f45664451d5508473f78aceb07
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55162217"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Öğretici: BitaBIZ ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BitaBIZ tümleştirme konusunda bilgi edinin.

Azure AD ile BitaBIZ tümleştirme ile aşağıdaki avantajları sağlar:

- BitaBIZ erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için BitaBIZ (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BitaBIZ yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik BitaBIZ çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden BitaBIZ ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-bitabiz-from-the-gallery"></a>Galeriden BitaBIZ ekleme
Azure AD'de BitaBIZ tümleştirmesini yapılandırmak için BitaBIZ Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden BitaBIZ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **BitaBIZ**seçin **BitaBIZ** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde BitaBIZ](./media/bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı BitaBIZ sınayın.

Tek iş için oturum açma için Azure AD ne BitaBIZ karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının BitaBIZ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BitaBIZ içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma BitaBIZ ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[BitaBIZ test kullanıcısı oluşturma](#create-a-bitabiz-test-user)**  - kullanıcı Azure AD gösterimini bağlı BitaBIZ Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve BitaBIZ uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile BitaBIZ yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **BitaBIZ** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

1. Üzerinde **BitaBIZ etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ etki alanı ve URL'ler tek oturum açma bilgileri](./media/bitabiz-tutorial/tutorial_bitabiz_url.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > Yalnızca gösterimi için yukarıdaki URL'deki değerdir. Öğreticinin ilerleyen bölümlerinde açıklanan gerçek tanımlayıcı değerini güncelleştirin.

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![BitaBIZ etki alanı ve URL'ler tek oturum açma bilgileri](./media/bitabiz-tutorial/tutorial_bitabiz_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://www.bitabiz.com/dashboard`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/bitabiz-tutorial/tutorial_general_400.png)
    
1. Üzerinde **BitaBIZ yapılandırma** bölümünde **yapılandırma BitaBIZ** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/tutorial_bitabiz_configure.png) 

1. Farklı bir web tarayıcı penceresinde BitaBIZ kiracınıza yönetici olarak oturum.

1. Tıklayarak **Kurulum yönetici**.

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings1.png)

1. Tıklayarak **Microsoft tümleştirmeler** altında **değer Ekle** bölümü.

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings2.png)

1. Bölümüne kaydırın **Microsoft Azure AD (etkinleştir çoklu oturum açma)** ve aşağıdaki adımları uygulayın:

    ![BitaBIZ yapılandırma](./media/bitabiz-tutorial/settings3.png)

    a. Değeri Şuradan Kopyala: **varlık kimliği (Azure AD'de "tanımlayıcı")** metin kutusuna yapıştırın **tanımlayıcı** metin üzerinde **BitaBIZ etki alanı ve URL'ler** bölümü Azure Portalı'nda. 
    
    b. İçinde **Azure AD çoklu oturum açma hizmeti URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **Azure AD SAML varlık kimliği** metin kutusu, yapıştırma **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **Azure AD imzalama sertifikası (şifreli Base64)** metin.

    e. İş e-posta etki alanınızı ekleme mycompany.com, diğer bir deyişle, ad **etki alanı adı** SSO bu e-posta etki alanı ile şirketinizdeki kullanıcılara atamak için metin (zorunlu).
    
    f. İşareti **SSO etkin** BitaBIZ hesabı.
    
    g. Tıklayın **Azure AD'ye yapılandırmayı kaydetmek** kaydedin ve SSO yapılandırma etkinleştirme.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/bitabiz-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/bitabiz-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/bitabiz-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/bitabiz-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-bitabiz-test-user"></a>BitaBIZ test kullanıcısı oluşturma

BitaBIZ için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların BitaBIZ sağlanması gerekir.  
BitaBIZ söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. BitaBIZ şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **Kurulum yönetici**.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/settings1.png)

1. Tıklayarak **kullanıcı ekleme** altında **kuruluş** bölümü.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user1.png)

1. Tıklayın **Ekle yeni çalışan**.

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user2.png)

1. Üzerinde **"Yeni çalışan Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ Kullanıcı Ekle](./media/bitabiz-tutorial/user3.png)

    a. İçinde **ad** metin kutusu, kullanıcının Britta gibi ilk tür adı.

    b. İçinde **Soyadı** metin son Simon gibi kullanıcı adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. Bir tarih seçin **gününü**.

    e. Kullanıcı için ayarlanabilir diğer zorunlu olmayan kullanıcı öznitelikleri vardır. Lütfen [çalışan Kurulum Doc](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) daha fazla ayrıntı için.    
    
    f. Tıklayın **Kaydet çalışan**.
    
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.
    
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için BitaBIZ erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon BitaBIZ için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **BitaBIZ**.

    ![Uygulamalar listesinde BitaBIZ bağlantı](./media/bitabiz-tutorial/tutorial_bitabiz_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde BitaBIZ kutucuğa tıkladığınızda, otomatik olarak BitaBIZ uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/bitabiz-tutorial/tutorial_general_203.png

