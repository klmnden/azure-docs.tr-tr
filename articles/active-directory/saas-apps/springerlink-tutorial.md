---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Springer bağlantıyla | Microsoft Docs'
description: Azure Active Directory ve Springer bağlantı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: jeedes
ms.openlocfilehash: d4d8d61f5e8834b679eeb68cb416bb6358a5f53a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180152"
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a>Öğretici: Springer bağlantı ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Springer bağlantı tümleştirme konusunda bilgi edinin.

Azure AD ile Springer bağlantı tümleştirme ile aşağıdaki avantajları sağlar:

- Springer bağlantı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Springer bağlantı açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Springer bağlantısını yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Springer bağlantı çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Springer bağlantı ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-springer-link-from-the-gallery"></a>Galeriden Springer bağlantı ekleme
Azure AD'de Springer bağlantı tümleştirmesini yapılandırmak için Springer bağlantı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Springer bağlantısı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Springer bağlantı**seçin **Springer bağlantı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde springer bağlantı](./media/springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı Springer bağlantıyı sınayın.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Springer bağlantısında bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Springer bağlantısında arasında bir bağlantı ilişki kurulması gerekir.

Değerini Springer bağlantıya atamak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile Springer bağlantısını sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Springer bağlantıyı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Springer bağlantısını yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Springer bağlantı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/springerlink-tutorial/tutorial_springerlink_samlbase.png)

1. Üzerinde **Springer bağlantı etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Springer bağlantı etki alanı ve URL'ler tek oturum açma bilgileri](./media/springerlink-tutorial/tutorial_springerlink_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://fsso.springer.com`

    b. İçinde **yanıt URL'si** metin kutusuna URL'yi yazın: `https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Springer bağlantı etki alanı ve URL'ler tek oturum açma bilgileri](./media/springerlink-tutorial/tutorial_springerlink_url.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın. 

    ![Sertifika indirme bağlantısı](./media/springerlink-tutorial/tutorial_springerlink_certificate.png)    

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/springerlink-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Springer bağlantı** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Springer bağlantı Destek ekibine](mailto:identity@springernature.com).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/springerlink-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/springerlink-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/springerlink-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/springerlink-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, erişim Springer bağlantı vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Springer bağlantıya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Springer bağlantı**.

    ![Uygulamalar listesini Springer bağlantı bağlantıdaki](./media/springerlink-tutorial/tutorial_springerlink_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Springer bağlantı kutucuğa tıkladığınızda, otomatik olarak Springer bağlantı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/springerlink-tutorial/tutorial_general_01.png
[2]: ./media/springerlink-tutorial/tutorial_general_02.png
[3]: ./media/springerlink-tutorial/tutorial_general_03.png
[4]: ./media/springerlink-tutorial/tutorial_general_04.png

[100]: ./media/springerlink-tutorial/tutorial_general_100.png

[200]: ./media/springerlink-tutorial/tutorial_general_200.png
[201]: ./media/springerlink-tutorial/tutorial_general_201.png
[202]: ./media/springerlink-tutorial/tutorial_general_202.png
[203]: ./media/springerlink-tutorial/tutorial_general_203.png

