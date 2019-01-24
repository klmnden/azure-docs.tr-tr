---
title: 'Öğretici: Rally yazılımı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Rally yazılımı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: 68d5558ff5dcf5d7d0cae03fef6302f13048c923
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54824284"
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Öğretici: Rally yazılımı ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Rally yazılımı tümleştirme konusunda bilgi edinin.

RALLY yazılımı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Rally yazılımı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Rally yazılımı (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Rally yazılımı ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Rally yazılımı çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. RALLY yazılımı galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-rally-software-from-the-gallery"></a>RALLY yazılımı galeri ekleme
Azure AD'de Rally yazılımı tümleştirmesini yapılandırmak için Rally yazılımı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RALLY yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Rally yazılımı**seçin **Rally yazılımı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde rally yazılımı](./media/rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcısı üzerinde temel yazılım Rally sınayın.

Tek iş için oturum açma için Azure AD ne Rally yazılımı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Rally yazılımı arasında bir bağlantı ilişki kurulması gerekir.

RALLY yazılımı değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rally yazılımı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Rally yazılımı test kullanıcısı oluşturma](#create-a-rally-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı Rally yazılımı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Rally yazılımı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Rally yazılımı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Rally yazılımı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

1. Üzerinde **Rally yazılımı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Rally yazılımı etki alanı ve URL'ler tek oturum açma bilgileri](./media/rally-software-tutorial/tutorial_rallysoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.rally.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.rally.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Rally yazılımı istemci Destek ekibine](https://help.rallydev.com/) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/rally-software-tutorial/tutorial_general_400.png)

1. Üzerinde **Rally yazılımı Yapılandırması** bölümünde **Rally yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Rally yazılımı yapılandırması](./media/rally-software-tutorial/tutorial_rallysoftware_configure.png) 

1. Oturum açın, **Rally yazılımı** Kiracı.

1. Üst araç çubuğunda tıklatın **Kurulum**ve ardından **abonelik**.
   
    ![Abonelik](./media/rally-software-tutorial/ic769531.png "abonelik")

1. Tıklayın **eylem** düğmesi. Seçin **Düzenle abonelik** araç çubuğunun üst sağ tarafındaki.

1. Üzerinde **abonelik** iletişim sayfasında, aşağıdaki adımları uygulayın ve ardından **Kaydet ve Kapat**:
   
    ![Kimlik doğrulaması](./media/rally-software-tutorial/ic769542.png "kimlik doğrulaması")
   
    a. Seçin **yarışı veya SSO kimlik doğrulaması** kimlik doğrulaması açılır listesinden.

    b. İçinde **kimlik sağlayıcısı URL'si** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız. 

    c. İçinde **SSO oturum kapatma** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/rally-software-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/rally-software-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/rally-software-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/rally-software-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rally-software-test-user"></a>Rally yazılımı test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açabilmesi, Azure Active Directory kullanıcı adlarını kullanarak Rally yazılımı uygulamaya sağlanmalıdır.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Rally yazılımı kiracınızda oturum açın.

1. Git **Kurulum \> kullanıcılar**ve ardından **+ yeni Ekle**.
   
    ![Kullanıcılar](./media/rally-software-tutorial/ic781039.png "kullanıcılar")

1. Yeni kullanıcı metin kutusunda adı yazın ve ardından **Ekle ayrıntılarla**.

1. İçinde **Create User** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/rally-software-tutorial/ic781040.png "kullanıcı oluşturma")

    a. İçinde **kullanıcı adı** metin kutusuna kullanıcı adını yazın ister **Brittsimon**.
   
    b. İçinde **e-posta adresi** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

    c. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    d. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **Simon**.

    e. Tıklayın **Kaydet ve Kapat**.

   >[!NOTE]
   >Azure AD kullanıcı hesapları sağlamak için herhangi bir Rally yazılımı kullanıcı hesabı oluşturma araçları veya Rally yazılımı tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Rally yazılımı erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Rally yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Rally yazılımı**.

    ![Uygulamalar listesinde Rally yazılımı bağlantı](./media/rally-software-tutorial/tutorial_rallysoftware_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Rally yazılımı kutucuğa tıkladığınızda, otomatik olarak Rally yazılımı uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rally-software-tutorial/tutorial_general_01.png
[2]: ./media/rally-software-tutorial/tutorial_general_02.png
[3]: ./media/rally-software-tutorial/tutorial_general_03.png
[4]: ./media/rally-software-tutorial/tutorial_general_04.png

[100]: ./media/rally-software-tutorial/tutorial_general_100.png

[200]: ./media/rally-software-tutorial/tutorial_general_200.png
[201]: ./media/rally-software-tutorial/tutorial_general_201.png
[202]: ./media/rally-software-tutorial/tutorial_general_202.png
[203]: ./media/rally-software-tutorial/tutorial_general_203.png

