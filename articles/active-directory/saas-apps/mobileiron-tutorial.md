---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle MobileIron | Microsoft Docs'
description: Azure Active Directory ve MobileIron arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3e4bbd5b-290e-4951-971b-ec0c1c11aaa2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: jeedes
ms.openlocfilehash: 8bdf49f4cea7c6f0ff30e37bcf1cf2fed3abc2bb
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963819"
---
# <a name="tutorial-azure-active-directory-integration-with-mobileiron"></a>Öğretici: Azure Active Directory MobileIron ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile MobileIron tümleştirme konusunda bilgi edinin.

Azure AD ile MobileIron tümleştirme ile aşağıdaki avantajları sağlar:

- MobileIron erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için MobileIron (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile MobileIron yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik MobileIron çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden MobileIron ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-mobileiron-from-the-gallery"></a>Galeriden MobileIron ekleme

Azure AD'de MobileIron tümleştirmesini yapılandırmak için MobileIron Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden MobileIron eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **MobileIron**seçin **MobileIron** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde MobileIron](./media/mobileiron-tutorial/tutorial_mobileiron_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı MobileIron ile test etme

Tek iş için oturum açma için Azure AD ne MobileIron karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının MobileIron ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

MobileIron içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma MobileIron ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[MobileIron test kullanıcısı oluşturma](#create-a-mobileiron-test-user)**  - kullanıcı Azure AD gösterimini bağlı MobileIron Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve MobileIron uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile MobileIron yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **MobileIron** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/mobileiron-tutorial/tutorial_mobileiron_samlbase.png)

1. Üzerinde **MobileIron etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![MobileIron etki alanı ve URL'ler tek oturum açma bilgileri](./media/mobileiron-tutorial/tutorial_mobileiron_url.png)

    1. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.mobileiron.com/<key>`

    1. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<host>.mobileiron.com/saml/SSO/alias/<key>`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açmayı MobileIron etki alanı ve URL'ler](./media/mobileiron-tutorial/tutorial_mobileiron_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<host>.mobileiron.com/user/login.html`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu öğreticinin ilerleyen bölümlerinde açıklanan MobileIron, Yönetim Portalı'ndan anahtarı ve ana bilgisayar değerlerini alırsınız.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/mobileiron-tutorial/tutorial_mobileiron_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/mobileiron-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde MobileIron şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **yönetici** > **kimlik**.

   - Seçin **AAD** seçeneğini **bulut IDP kurulumu hakkında bilgi** alan.

    ![Tek yönetici oturum açma düğmesi yapılandırın](./media/mobileiron-tutorial/tutorial_mobileiron_admin.png)

1. Değerlerini kopyalamayı **anahtarı** ve **konak** URL'lerinde tamamlama yapıştırın **MobileIron etki alanı ve URL'ler** bölümü Azure Portalı'nda.

    ![Tek yönetici oturum açma düğmesi yapılandırın](./media/mobileiron-tutorial/key.png)

1. İçinde **MobileIron bulut alana meta veri dosyası dışarı aktarma ve içeri aktarma** tıklayın **Dosya Seç** Azure portalından indirilen meta verilerini karşıya yüklemek için. Tıklayın **Bitti** yüklendikten sonra.

    ![Yönetim meta verileri düğmesi çoklu oturum açmayı yapılandırın](./media/mobileiron-tutorial/tutorial_mobileiron_adminmetadata.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/mobileiron-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/mobileiron-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/mobileiron-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/mobileiron-tutorial/create_aaduser_04.png)

    1. İçinde **adı** kutusuna **BrittaSimon**.

    1. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    1. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    1. **Oluştur**’a tıklayın.
  
### <a name="create-a-mobileiron-test-user"></a>MobileIron test kullanıcısı oluşturma

MobileIron için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların MobileIron sağlanması gerekir.  
MobileIron söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. MobileIron şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **kullanıcılar** tıklayın **ekleme** > **tek kullanıcı**.

    ![Kullanıcı düğmesi, çoklu oturum açmayı yapılandırın](./media/mobileiron-tutorial/tutorial_mobileiron_user.png)

1. Üzerinde **"Tek kullanıcı"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma Kullanıcı Ekle düğmesi](./media/mobileiron-tutorial/tutorial_mobileiron_useradd.png)

    1. İçinde **e-posta adresi** metin kutusuna, kullanıcının gibi e-posta girin brittasimon@contoso.com.

    1. İçinde **ad** metin kutusunda, Britta gibi kullanıcı adını girin.

    1. İçinde **Soyadı** metin kutusunda, son Simon gibi kullanıcı adını girin.

    1. **Bitti**’ye tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için MobileIron erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon MobileIron için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

1. Uygulamalar listesinde **MobileIron**.

    ![Uygulamalar listesinde MobileIron bağlantı](./media/mobileiron-tutorial/tutorial_mobileiron_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde MobileIron kutucuğa tıkladığınızda, otomatik olarak MobileIron uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mobileiron-tutorial/tutorial_general_01.png
[2]: ./media/mobileiron-tutorial/tutorial_general_02.png
[3]: ./media/mobileiron-tutorial/tutorial_general_03.png
[4]: ./media/mobileiron-tutorial/tutorial_general_04.png

[100]: ./media/mobileiron-tutorial/tutorial_general_100.png

[200]: ./media/mobileiron-tutorial/tutorial_general_200.png
[201]: ./media/mobileiron-tutorial/tutorial_general_201.png
[202]: ./media/mobileiron-tutorial/tutorial_general_202.png
[203]: ./media/mobileiron-tutorial/tutorial_general_203.png
