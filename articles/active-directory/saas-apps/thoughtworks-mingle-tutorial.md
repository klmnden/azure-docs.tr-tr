---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Thoughtworks Mingle | Microsoft Docs'
description: Azure Active Directory ve Thoughtworks Mingle arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7652f0179725dbe04c3245028491fae5f8c4dac7
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813268"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Öğretici: Thoughtworks Mingle ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Thoughtworks Mingle öğrenin.

Azure AD ile tümleştirme Thoughtworks Mingle ile aşağıdaki avantajları sağlar:

- Thoughtworks Mingle erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Thoughtworks Mingle açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Thoughtworks Mingle ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Thoughtworks Mingle çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Thoughtworks Mingle ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Galeriden Thoughtworks Mingle ekleme
Thoughtworks Mingle tümleştirmesini Azure AD'de yapılandırmak için Thoughtworks Mingle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Thoughtworks Mingle eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Thoughtworks Mingle**seçin **Thoughtworks Mingle** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Thoughtworks Mingle](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Thoughtworks Mingle ile test edin.

Tek iş için oturum açma için Azure AD ne Thoughtworks Mingle karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili Thoughtworks Mingle kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değeri atamak Thoughtworks Mingle içinde **kullanıcı adı** değerini Azure AD'de **kullanıcı adı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Thoughtworks Mingle ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Thoughtworks Mingle test kullanıcısı oluşturma](#create-a-thoughtworks-mingle-test-user)**  - Thoughtworks kullanıcı Azure AD gösterimini bağlı Mingle Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Thoughtworks Mingle uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Thoughtworks Mingle ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Thoughtworks Mingle** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. Üzerinde **Thoughtworks Mingle etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Thoughtworks Mingle etki alanı ve URL'ler tek oturum açma bilgileri](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Thoughtworks Mingle istemci Destek ekibine](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Oturum açın, **Thoughtworks Mingle** şirketinizin sitesi yöneticisi olarak.

1. Tıklayın **yönetici** sekmesini tıklatıp, ardından **SSO yapılandırma**.
   
    ![Yönetim sekmesi](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO yapılandırma")

1. İçinde **SSO yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SSO yapılandırma](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO yapılandırma")
    
    a. Meta veri dosyası karşıya yüklemek için tıklayın **dosya**. 

    b. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Thoughtworks Mingle test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmesi, Azure Active Directory kullanıcı adlarını kullanarak Thoughtworks Mingle uygulaması sağlanmalıdır. Thoughtworks Mingle söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Şirketinizin Thoughtworks Mingle sitesi için yönetici olarak oturum açın.

1. tıklayın **profili**.
   
    ![İlk projenizi](./media/thoughtworks-mingle-tutorial/ic785160.png "ilk projenizi")

1. Tıklayın **yönetici** sekmesine ve ardından **kullanıcılar**.
   
    ![Kullanıcılar](./media/thoughtworks-mingle-tutorial/ic785161.png "kullanıcılar")

1. Tıklayın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/thoughtworks-mingle-tutorial/ic785162.png "yeni kullanıcı")

1. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/ic785163.png "yeni kullanıcı")  
 
    a. Tür **oturum açma adı**, **görünen ad**, **Seç parola**, **parolayı onayla** geçerli bir Azure AD hesabı sağlamak istediğiniz ilgili metin kutularına. 

    b. Olarak **kullanıcı türü**seçin **tam kullanıcı**.

    c. Tıklayın **bu profil oluşturma**.

>[!NOTE]
>Herhangi diğer Thoughtworks Mingle kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri Thoughtworks Mingle tarafından sağlanan AAD kullanıcı hesapları sağlamak için.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Thoughtworks Mingle için erişim izni verme kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Thoughtworks Mingle için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Thoughtworks Mingle**.

    ![Uygulamalar listesinde Thoughtworks Mingle bağlantı](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Thoughtworks Mingle kutucuğa tıkladığınızda, otomatik olarak Thoughtworks Mingle uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

