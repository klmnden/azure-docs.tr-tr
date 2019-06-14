---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iWellnessNow | Microsoft Docs'
description: Azure Active Directory ve iWellnessNow arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02b831df98db5b9d63873a0da93e603cd7cbf308
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60269459"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Öğretici: İWellnessNow ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iWellnessNow tümleştirme konusunda bilgi edinin.

Azure AD ile iWellnessNow tümleştirme ile aşağıdaki avantajları sağlar:

- İWellnessNow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için iWellnessNow (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile iWellnessNow yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir iWellnessNow çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden iWellnessNow ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-iwellnessnow-from-the-gallery"></a>Galeriden iWellnessNow ekleme
Azure AD'de iWellnessNow tümleştirmesini yapılandırmak için iWellnessNow Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iWellnessNow eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **iWellnessNow**seçin **iWellnessNow** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde iWellnessNow](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iWellnessNow sınayın.

Tek iş için oturum açma için Azure AD ne iWellnessNow karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının iWellnessNow ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iWellnessNow ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir iWellnessNow test kullanıcısı oluşturma](#create-an-iwellnessnow-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı iWellnessNow içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve iWellnessNow uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile iWellnessNow yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **iWellnessNow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

1. Üzerinde **iWellnessNow etki alanı ve URL'ler** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası** ve uygulama yapılandırmak istediğiniz **IDP** modunda başlatılan gerçekleştirin Aşağıdaki adımlar:

    ![etki alanı ve URL'ler çoklu oturum açma iWellnessNow karşıya yükleme](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![etki alanı ve URL'ler çoklu oturum açma iWellnessNow uploadconfig](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.
    
    c. Karşıya yükleme işlemin başarıyla tamamlanmasından sonra **hizmet sağlayıcısı meta veri dosyası** **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş  **iWellnessNow etki alanı ve URL'ler** aşağıda gösterildiği gibi metin kutusu bölümünde:

    ![iWellnessNow etki alanı ve URL'ler tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

1. Yoksa **hizmet sağlayıcısı meta veri dosyası** ve uygulama yapılandırmak istediğiniz **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![iWellnessNow etki alanı ve URL'ler tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `http://<CustomerName>.iwellnessnow.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<CustomerName>.iwellnessnow.com/ssologin`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![iWellnessNow etki alanı ve URL'ler tek oturum açma bilgileri](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [iWellnessNow istemci Destek ekibine](mailto:info@iwellnessnow.com) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **iWellnessNow** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [iWellnessNow Destek ekibine](mailto:info@iwellnessnow.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/iwellnessnow-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/iwellnessnow-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/iwellnessnow-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/iwellnessnow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-iwellnessnow-test-user"></a>Bir iWellnessNow test kullanıcısı oluşturma

Bu bölümde, Britta Simon iWellnessNow içinde adlı bir kullanıcı oluşturun. Çalışmak [iWellnessNow Destek ekibine](mailto:info@iwellnessnow.com) iWellnessNow platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iWellnessNow erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon iWellnessNow için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **iWellnessNow**.

    ![Uygulamalar listesinde iWellnessNow bağlantı](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde iWellnessNow kutucuğa tıkladığınızda, otomatik olarak iWellnessNow uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

