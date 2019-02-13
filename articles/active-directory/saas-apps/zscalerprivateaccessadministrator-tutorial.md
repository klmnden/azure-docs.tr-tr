---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim Yöneticisi | Microsoft Docs'
description: Azure Active Directory ve Zscaler özel erişim Yöneticisi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ba2bde039cec65a1afe33efac58752d26f22c2b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171902"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim Yöneticisi tümleştirme konusunda bilgi edinin.

Zscaler özel erişim Yöneticisi, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zscaler için özel erişim Yöneticisi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Zscaler özel erişim yöneticisine açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zscaler özel erişim Yöneticisi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Zscaler özel erişim Yöneticisi çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Zscaler özel erişim yönetici ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Galeriden Zscaler özel erişim yönetici ekleme
Azure AD'de Zscaler özel erişim Yöneticisi'nin tümleştirmesini yapılandırmak için Zscaler özel erişim Yöneticisi galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zscaler özel erişim yönetici eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Zscaler özel erişim Yöneticisi**seçin **Zscaler özel erişim Yöneticisi** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde Zscaler özel erişim Yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Zscaler özel erişim Yöneticisi'nde bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zscaler özel erişim Yöneticisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim Yöneticisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Zscaler özel erişim yönetici test kullanıcısı oluşturma](#create-a-zscaler-private-access-administrator-test-user)**  - Zscaler özel erişim kullanıcı Azure AD gösterimini bağlı Yöneticisi'nde Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zscaler özel erişim Yöneticisi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Zscaler özel erişim Yöneticisi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zscaler özel erişim Yöneticisi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

1. Üzerinde **Zscaler özel erişim yöneticisinin etki alanı ve URL'ler** uygulamada yapılandırmak istiyorsanız bölümünde **IDP** başlatılan modu:

    ![Zscaler özel erişim yöneticisinin etki alanı ve URL'ler tek oturum açma bilgileri](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Denetleme **Gelişmiş URL ayarlarını göster**

    d. İçinde **RelayState** metin kutusuna bir değer girin: `idpadminsso`

1.  Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Zscaler özel erişim Yöneticisi Destek ekibine](https://help.zscaler.com/zpa-submit-ticket) bu değerleri almak için.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

1. Bir başka web tarayıcı penceresinde için Zscaler özel erişim Yöneticisi Yönetici olarak oturum açın.

1. En üstte tıklayın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırmasını**.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

1. Sağ üst köşedeki, tıklayın **IDP Yapılandırması Ekle**. 

    ![Zscaler özel erişim Yöneticisi addidp](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

1. Üzerinde **IDP Yapılandırması Ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler özel erişim Yöneticisi idpselect](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Tıklayın **Dosya Seç** indirdiğiniz Azure AD'de meta verileri dosyadan karşıya yüklemek için **IDP meta veri dosyasını karşıya yükleyin.** alan.

    b. Okuduğu **IDP meta verileri** Azure AD'den ve aşağıda gösterildiği gibi tüm alanları bilgileri doldurur.

    ![Zscaler özel erişim Yöneticisi idpconfig](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Seçin **tekli oturum** olarak **yönetici**.

    d. Etki alanınızdan seçin **etki alanları** alan.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Zscaler özel erişim yönetici test kullanıcısı oluşturma

Zscaler özel erişim Yöneticisi için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Zscaler özel erişim Yöneticisi olarak sağlanması gerekir. Zscaler özel erişim Yöneticisi söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Zscaler özel erişim Yöneticisi şirketinizin sitesi için bir yönetici olarak oturum açın.

1. En üstte tıklayın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırmasını**.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

1. Tıklayın **Yöneticiler** sol tarafındaki menüyü.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

1. Sağ üst köşedeki, tıklayın **Yöneticisi Ekle**:

    ![Zscaler özel erişim yönetici, yönetici Ekle](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

1. İçinde **Yöneticisi Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zscaler özel erişim Yönetici Kullanıcı Yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. İçinde **kullanıcıadı** metin gibi kullanıcının e-posta girin **BrittaSimon@contoso.com**.

    b. İçinde **parola** metin kutusuna parolayı yazın.

    c. İçinde **parolayı onayla** metin kutusuna parolayı yazın.

    d. Seçin **rol** olarak **Zscaler özel erişim Yöneticisi**.

    e. İçinde **e-posta** metin gibi kullanıcının e-posta girin **BrittaSimon@contoso.com**.

    f. İçinde **telefon** metin telefon numarasını yazın.

    g. İçinde **saat dilimi** metin saat dilimi seçin.

    h. **Kaydet**’e tıklayın.  

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Zscaler özel erişim Yöneticisi erişimi vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Zscaler özel erişim yöneticisine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Zscaler özel erişim Yöneticisi**.

    ![Uygulamalar listesinde Zscaler özel erişim Yöneticisi bağlantı](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler özel erişim Yöneticisi kutucuğa tıkladığınızda, otomatik olarak Zscaler özel erişim Yöneticisi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_203.png

