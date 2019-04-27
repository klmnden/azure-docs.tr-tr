---
title: 'Öğretici: Bulut güvenlik yapısı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve bulut güvenlik doku arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a556b38ca4947b71555ba7b023607b392900bdaf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60429601"
---
# <a name="tutorial-azure-active-directory-integration-with-the-cloud-security-fabric"></a>Öğretici: Bulut güvenlik yapısı ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile bulut güvenlik yapısı tümleştirme konusunda bilgi edinin.

Bulut güvenlik yapısı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Bulut güvenlik yapısı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak bulut güvenlik Dokusuna (çoklu oturum açma) açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile bulut güvenlik yapısı yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir bulut güvenliği yapısı çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Bulut güvenlik yapısı galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-the-cloud-security-fabric-from-the-gallery"></a>Bulut güvenlik yapısı galeri ekleme
Azure AD bulut güvenlik yapısı tümleştirilmesi yapılandırmak için bulut güvenliği yapısı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Bulut güvenlik yapısı Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **bulut güvenlik yapısı**seçin **bulut güvenlik yapısı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde bulut güvenlik yapısı](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma bulut güvenlik "Britta Simon" adlı bir test kullanıcı tabanlı yapı ile test edin.

Tek iş için oturum açma için Azure AD bulut güvenlik dokusunda karşılığı kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve bulut güvenlik dokusunda ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma bulut güvenlik yapı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bulut güvenlik yapısı test kullanıcısı oluşturma](#create-a-the-cloud-security-fabric-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bulut güvenlik yapısı içinde bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve bulut güvenlik yapısı uygulamanızda çoklu oturum açma yapılandırın.

**Azure AD çoklu oturum açma ile bulut güvenlik yapısı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **bulut güvenlik yapısı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

1. Üzerinde **bulut güvenlik Fabric etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Bulut güvenlik Fabric etki alanı ve URL'ler tek oturum açma bilgileri](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Değerini gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [bulut güvenlik Fabric istemci Destek ekibine](mailto:support@cloudlock.com) değeri alınamıyor. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/ciscocloudlock-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **bulut güvenlik yapısı** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [bulut güvenlik Fabric destek ekibi](mailto:support@cloudlock.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ciscocloudlock-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/ciscocloudlock-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ciscocloudlock-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ciscocloudlock-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-the-cloud-security-fabric-test-user"></a>Bulut güvenlik yapısı test kullanıcısı oluşturma

Bu bölümde, bulut güvenliği Dokusunda Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [bulut güvenlik Fabric destek ekibi](mailto:support@cloudlock.com) bulut güvenlik Fabric platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, bulut güvenliği yapısı için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon bulut güvenlik dokuya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **bulut güvenlik yapısı**.

    ![Uygulamalar listesini bulut güvenliği yapısı bağlantıdaki](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli bulut güvenlik yapısı kutucuğa tıkladığınızda, size otomatik olarak bulut güvenlik Fabric uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloudlock-tutorial/tutorial_general_203.png
