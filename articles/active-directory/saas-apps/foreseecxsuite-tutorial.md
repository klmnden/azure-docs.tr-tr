---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Öngörüyor CX paketi | Microsoft Docs'
description: Azure Active Directory ve Öngörüyor CX Suite arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeedes
ms.openlocfilehash: b288bcbe14050c0f764f348d5e20186570e32866
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442209"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Öğretici: Azure Active Directory Öngörüyor CX Suite ile tümleştirme

Bu öğreticide, CX Suite Öngörüyor Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

CX Suite Öngörüyor Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- CX Suite Öngörüyor erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Öngörüyor CX paketine (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Öngörüyor CX Suite ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Öngörüyor CX Suite çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Öngörüyor CX paketi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>Galeriden Öngörüyor CX paketi ekleme
Azure AD'de Öngörüyor CX takımının tümleştirmesini yapılandırmak için CX Suite Öngörüyor Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Öngörüyor CX paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Öngörüyor CX Suite**seçin **Öngörüyor CX Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde CX Suite Öngörüyor](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Öngörüyor CX "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Öngörüyor CX grubundaki bir kullanıcı için Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Öngörüyor CX paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Öngörüyor CX Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[CX Suite Öngörüyor test kullanıcısı oluşturma](#create-a-foresee-cx-suite-test-user)**  - paketindeki Öngörüyor CX kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Öngörüyor CX Suite uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Öngörüyor CX Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Öngörüyor CX Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_samlbase.png)

1. Üzerinde **Öngörüyor CX Suite etki alanı ve URL'ler** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    ![CX Suite etki alanı ve URL'ler tek oturum açma bilgileri Öngörüyor.](./media/foreseecxsuite-tutorial/upload.png)

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![CX Suite etki alanı ve URL'ler tek oturum açma bilgileri Öngörüyor.](./media/foreseecxsuite-tutorial/tutorial_foreseen_uploadconfig.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    c. Karşıya yükleme işlemin başarıyla tamamlanmasından sonra **hizmet sağlayıcısı meta veri dosyası** **tanımlayıcı** değeri get otomatik doldurulur **Öngörüyor CX Suite etki alanı ve URL'ler** bölümü Aşağıda gösterildiği gibi metin:

    ![CX Suite etki alanı ve URL'ler tek oturum açma bilgileri Öngörüyor.](./media/foreseecxsuite-tutorial/urlupload.png)

1. Öğeniz yoksa **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    ![CX Suite etki alanı ve URL'ler tek oturum açma bilgileri Öngörüyor.](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://cxsuite.foresee.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.okta.com/saml2/service-provider/<UniqueID>`

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Bu değer, gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [Öngörüyor CX Suite istemci Destek ekibine](mailto:support@foresee.com) bu değeri alınamıyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/foreseecxsuite-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Öngörüyor CX Suite** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Öngörüyor CX Suite Destek ekibine](mailto:support@foresee.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/foreseecxsuite-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/foreseecxsuite-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/foreseecxsuite-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/foreseecxsuite-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-foresee-cx-suite-test-user"></a>CX Suite Öngörüyor test kullanıcısı oluşturma

Bu bölümde, Britta Simon Öngörüyor CX paketindeki adlı bir kullanıcı oluşturun. Çalışmak [Öngörüyor CX Suite Destek ekibine](mailto:support@foresee.com) kullanıcı veya Öngörüyor CX Suite platformunda Güvenilenler listesine eklenmek için gerekli olan etki alanı eklemek için. Etki alanı ekibi tarafından eklenirse, kullanıcıların otomatik olarak Öngörüyor CX Suite platforma sağlandı. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Öngörüyor CX paketine erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Öngörüyor CX paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **Öngörüyor CX Suite**.

    ![Uygulamalar listesinde Öngörüyor CX Suite bağlantısı](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_app.png)

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Öngörüyor CX Suite kutucuğa tıkladığınızda, otomatik olarak Öngörüyor CX paketini uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/foreseecxsuite-tutorial/tutorial_general_01.png
[2]: ./media/foreseecxsuite-tutorial/tutorial_general_02.png
[3]: ./media/foreseecxsuite-tutorial/tutorial_general_03.png
[4]: ./media/foreseecxsuite-tutorial/tutorial_general_04.png

[100]: ./media/foreseecxsuite-tutorial/tutorial_general_100.png

[200]: ./media/foreseecxsuite-tutorial/tutorial_general_200.png
[201]: ./media/foreseecxsuite-tutorial/tutorial_general_201.png
[202]: ./media/foreseecxsuite-tutorial/tutorial_general_202.png
[203]: ./media/foreseecxsuite-tutorial/tutorial_general_203.png

