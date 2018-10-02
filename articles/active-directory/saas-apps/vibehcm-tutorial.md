---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Vibe HCM | Microsoft Docs'
description: Azure Active Directory ve Vibe HCM arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4379bef7-adc9-4b6d-9384-c46d9a914bfe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: jeedes
ms.openlocfilehash: 395f99c986e0a51e8dd2c440d258724e862f275c
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019867"
---
# <a name="tutorial-azure-active-directory-integration-with-vibe-hcm"></a>Öğretici: Azure Active Directory Vibe HCM ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Vibe HCM tümleştirme konusunda bilgi edinin.

Azure AD ile Vibe HCM tümleştirme ile aşağıdaki avantajları sağlar:

- Vibe HCM erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Vibe HCM açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Vibe HCM yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Vibe HCM çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Vibe HCM ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-vibe-hcm-from-the-gallery"></a>Galeriden Vibe HCM ekleme
Azure AD'de Vibe HCM tümleştirmesini yapılandırmak için Vibe HCM Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Vibe HCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vibe HCM**seçin **Vibe HCM** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde vibe HCM](./media/vibehcm-tutorial/tutorial_vibehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Vibe HCM sınayın.

Tek iş için oturum açma için Azure AD ne Vibe HCM karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Vibe HCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Vibe HCM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Vibe HCM test kullanıcısı oluşturma](#create-a-vibe-hcm-test-user)**  - kullanıcı Azure AD gösterimini bağlı Vibe HCM Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Vibe HCM uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Vibe HCM yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Vibe HCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/vibehcm-tutorial/tutorial_vibehcm_samlbase.png)

3. Üzerinde **Vibe HCM etki alanı ve URL'ler** bölümünde, he kullanıcısının clonedatabase'i uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirmek:

    ![Vibe HCM etki alanı ve URL'ler tek oturum açma bilgileri](./media/vibehcm-tutorial/tutorial_vibehcm_url.png)

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Vibe HCM etki alanı ve URL'ler tek oturum açma bilgileri](./media/vibehcm-tutorial/tutorial_vibehcm_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyName>.vibehcm.com/portal.jsp`
     
    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Vibe HCM Destek ekibine](mailto:support@vibehcm.com) değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/vibehcm-tutorial/tutorial_vibehcm_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/vibehcm-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **Vibe HCM** tarafı, kopyalanan göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Vibe HCM Destek ekibine](mailto:support@vibehcm.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/vibehcm-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/vibehcm-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/vibehcm-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/vibehcm-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-vibe-hcm-test-user"></a>Vibe HCM test kullanıcısı oluşturma

Bu bölümde, Britta Simon Vibe HCM adlı bir kullanıcı oluşturun. Çalışmak [Vibe HCM Destek ekibine](mailto:support@vibehcm.com) Vibe HCM platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Vibe HCM erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Vibe HCM için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Vibe HCM**.

    ![Uygulamalar listesinde Vibe HCM bağlantı](./media/vibehcm-tutorial/tutorial_vibehcm_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Vibe HCM kutucuğa tıkladığınızda, otomatik olarak Vibe HCM uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/vibehcm-tutorial/tutorial_general_01.png
[2]: ./media/vibehcm-tutorial/tutorial_general_02.png
[3]: ./media/vibehcm-tutorial/tutorial_general_03.png
[4]: ./media/vibehcm-tutorial/tutorial_general_04.png

[100]: ./media/vibehcm-tutorial/tutorial_general_100.png

[200]: ./media/vibehcm-tutorial/tutorial_general_200.png
[201]: ./media/vibehcm-tutorial/tutorial_general_201.png
[202]: ./media/vibehcm-tutorial/tutorial_general_202.png
[203]: ./media/vibehcm-tutorial/tutorial_general_203.png

