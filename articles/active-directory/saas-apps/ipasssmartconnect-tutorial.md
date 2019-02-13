---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iPass SmartConnect | Microsoft Docs'
description: Azure Active Directory ve iPass SmartConnect arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91a68a208496904fcc8bfe13a227c61bf313214f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198176"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Öğretici: İPass SmartConnect ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iPass SmartConnect tümleştirme konusunda bilgi edinin.

Azure AD ile iPass SmartConnect tümleştirme ile aşağıdaki avantajları sağlar:

- İPass SmartConnect erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için iPass SmartConnect (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SmartConnect iPass ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir iPass SmartConnect çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden iPass SmartConnect ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Galeriden iPass SmartConnect ekleme
Azure AD'de iPass SmartConnect tümleştirmesini yapılandırmak için iPass SmartConnect Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iPass SmartConnect eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **iPass SmartConnect**seçin **iPass SmartConnect** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı SmartConnect tabanlı iPass ile test edin.

Tek iş için oturum açma için Azure AD hangi karşılık gelen kullanıcı iPass SmartConnect bir kullanıcının Azure AD'de olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının iPass ilgili kullanıcı arasında bir bağlantı ilişki SmartConnect kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iPass SmartConnect ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir iPass SmartConnect test kullanıcısı oluşturma](#create-an-ipass-smartconnect-test-user)**  - Britta Simon iPass kullanıcı Azure AD gösterimini bağlı SmartConnect içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, iPass SmartConnect uygulama yapılandırın.

**Azure AD çoklu oturum açma SmartConnect iPass ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **iPass SmartConnect** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

1. Üzerinde **iPass SmartConnect etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, herhangi bir adım gerçekleştirmeniz gerekmez.

    ![iPass SmartConnect etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

1. Gelişmiş URL ayarlarını Göster'i denetleyin ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![iPass SmartConnect etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Oturum açma URL'si metin kutusuna bir URL yazın: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

1. iPass SmartConnect uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/attribute.png)

1. Tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu **kullanıcı öznitelikleri** bölüm öznitelikleri genişletin. Her birini görüntülenen öznitelikleri - aşağıdaki adımları gerçekleştirin

    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri|
    | ---------------| --------------- |----------------|
    | FirstName | User.givenName |   |
    | Soyadı | User.surname | |
    | e-posta | User.userPrincipalName | |
    | kullanıcı adı | User.userPrincipalName | |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı değeri bu satır için boş bırakın.

    e. **Tamam**’a tıklayın.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **iPass SmartConnect** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **etki alanı adı** için [iPass SmartConnect Destek](mailto:help@ipass.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-ipass-smartconnect-test-user"></a>Bir iPass SmartConnect test kullanıcısı oluşturma

Bu bölümde, Britta Simon SmartConnect iPass içinde adlı bir kullanıcı oluşturun. Çalışmak [iPass SmartConnect Destek ekibine](mailto:help@ipass.com) kullanıcı veya iPass SmartConnect platformunda Güvenilenler listesine eklenmek için gerekli olan etki alanı eklemek için. Etki alanı ekibi tarafından eklenirse, kullanıcıların otomatik olarak iPass SmartConnect platforma sağlanan. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iPass SmartConnect erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon SmartConnect iPass için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **iPass SmartConnect**.

    ![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

**SP tarafından başlatılan akış içinde uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Windows iPass SmartConnect istemci indirme [burada](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing3.png)

b. İstemci yükleyip başlatın.

c. Tıklayarak **başlama**.

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Etki alanı ile Azure kullanıcı adını girin. Tıklayarak **devam**. Azure oturum açma sayfasına yönlendirilirsiniz

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing2.png) 

e. İstemci etkinleştirme başarılı kimlik doğrulamadan sonra başlatılır. İstemci etkin.

**IDP tarafından başlatılan akış içinde uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Oturum açma [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Üzerinde iPass SmartConnect uygulamaya tıklayın.

c. SSA İyileştiricisine sayfa başlatır, tıklayarak **uygulama Windows için indirin** iPass SmartConnect istemci yüklemek için.

![Uygulamalar listesinde SmartConnect bağlantı iPass](./media/ipasssmartconnect-tutorial/testing4.png)

d. Hüküm ve koşulları kabul sonra etkinleştirme, yükleme, istemci ilk başlatmada otomatik olarak ayarlanır sonra başlar.

e. Etkinleştirme başlatılmazsa etkinleştirmeyi SSA İyileştiricisine sayfasında Etkinleştir düğmesini tıklatın.

f. İstemci etkin.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

