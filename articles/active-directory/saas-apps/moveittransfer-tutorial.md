---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Moveıt aktarımı - Azure AD tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Moveıt aktarımı - Azure AD tümleştirmesi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 671d3c9954321bf19d0fd56057d05d8b4475d8d2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166076"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Öğretici: Azure Active Directory tümleştirmesiyle Moveıt aktarımı - Azure AD tümleştirmesi

Bu öğreticide, Moveıt aktarımı - Azure Active Directory (Azure AD) ile Azure AD tümleştirmesi tümleştirmeyi öğrenin.

Tümleştirme Moveıt aktarımı - Azure AD ile Azure AD Tümleştirmesi ile aşağıdaki avantajları sağlar:

- Moveıt aktarımı - Azure AD tümleştirmesi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Moveıt aktarımı - Azure AD hesaplarıyla (çoklu oturum açma) Azure AD tümleştirmesi için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Moveıt aktarımı - Azure AD Tümleştirmesi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Moveıt aktarımı - Azure AD tümleştirmesi çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Moveıt aktarımı - Azure AD tümleştirmesi galerisinden ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Moveıt aktarımı - Azure AD tümleştirmesi galerisinden ekleme
Moveıt aktarımı - Azure AD, Azure AD tümleştirme tümleştirmesini yapılandırmak için Moveıt aktarımı - Azure AD tümleştirmesi galerisinden listenize yönetilen SaaS uygulamalarının eklemeniz gerekir.

**Galerisi, Azure AD tümleştirmesi Moveıt aktarımı - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Moveıt aktarımı - Azure AD tümleştirmesi**seçin **Moveıt aktarımı - Azure AD tümleştirmesi** sonucu panelinden ardından **Ekle** eklemek için Ekle düğmesine uygulama.

    ![Moveıt aktarımı - sonuç listesinde Azure AD tümleştirmesi](./media/moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Moveıt aktarımı - "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD Tümleştirmesi ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Moveıt transfer - Azure AD tümleştirmesi için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Moveıt transfer - arasında bir bağlantı ilişki kurulması Azure AD tümleştirmesi gerekir.

Değerini Moveıt aktarımı - Azure AD tümleştirmesi, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Moveıt aktarımı ile-test etmek için Azure AD tümleştirmesi, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Moveıt aktarımı - Azure AD tümleştirme test kullanıcısı oluşturma](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - bir karşılığı Britta simon'un Moveıt aktarımı - kullanıcı Azure AD gösterimini bağlı olduğu Azure AD tümleştirmesi sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Moveıt aktarımı - Azure AD tümleştirme uygulaması yapılandırın.

**Azure AD çoklu oturum açma Moveıt aktarımı - Azure AD Tümleştirmesi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Moveıt aktarımı - Azure AD tümleştirmesi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

1. Üzerinde **Moveıt aktarımı - Azure AD tümleştirmesi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://contoso.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://contoso.com/<tenatid>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu değerler daha sonra da başvurabilirsiniz **servis sağlayıcı meta verileri URL'sini** bölüm veya kişi [Moveıt aktarımı - Azure AD tümleştirme istemci Destek ekibine](https://community.ipswitch.com/s/support) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/moveittransfer-tutorial/tutorial_general_400.png)
    
1. Moveıt aktarımı Kiracı yönetici olarak oturum açın.

1. Sol gezinti bölmesinde **ayarları**.

    ![Ayarlar bölümünde bulunan uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

1. Tıklayın **tek oturum açma** altında olan bağlantı **kullanıcı kimlik doğrulama -> güvenlik ilkelerini**.

    ![Güvenlik ilkeleri üzerinde uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

1. Meta veri belgesini indirmek için meta veri URL'si bağlantıya tıklayın.

    ![Hizmet sağlayıcısı meta veri URL'si](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Doğrulama **Entityıd** eşleşen **tanımlayıcı** içinde **Moveıt aktarımı - Azure AD tümleştirmesi etki alanı ve URL'ler** bölümü.
    * Doğrulama **AssertionConsumerService** konumu URL ile eşleşen **yanıt URL'si** içinde **Moveıt aktarımı - Azure AD tümleştirmesi etki alanı ve URL'ler** bölümü.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

1. Tıklayın **kimlik sağlayıcısı Ekle** düğmesini yeni bir Federasyon kimlik sağlayıcısı ekleyin.

    ![Kimlik sağlayıcısı Ekle](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

1. Tıklayın **Gözat...**  Azure portalından indirilen meta veri dosyası seçmek için ardından **kimlik sağlayıcısı Ekle** indirilen dosyayı karşıya yüklemek için.

    ![SAML kimlik sağlayıcısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

1. Seçin "**Evet**" olarak **etkin** içinde **Federasyon kimlik sağlayıcı ayarları Düzenle...**  sayfasında ve tıklayın **Kaydet**.

    ![Federe kimlik sağlayıcı ayarları](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

1. İçinde **federe kimlik sağlayıcısı kullanıcı ayarlarını Düzenle** sayfasında, aşağıdaki eylemleri gerçekleştirin:
    
    ![Federe kimlik sağlayıcı ayarları Düzenle](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Seçin **SAML Nameıd** olarak **oturum açma adı**.
    
    b. Seçin **diğer** olarak **tam adı** ve **öznitelik adı** textbox değeri koyun: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Seçin **diğer** olarak **e-posta** ve **öznitelik adı** textbox değeri koyun: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Seçin **Evet** olarak **oturum açma hesabı otomatik olarak oluşturma**.
    
    e. Tıklayın **Kaydet** düğmesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/moveittransfer-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/moveittransfer-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/moveittransfer-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/moveittransfer-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Moveıt aktarımı - Azure AD tümleştirme test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Moveıt aktarımı - Azure AD tümleştirmesi adlı bir kullanıcı oluşturmaktır. Moveıt aktarımı - Azure AD tümleştirmesi tam zamanında sağlama, etkin destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı Moveıt aktarımı - henüz mevcut değilse Azure AD tümleştirmesi erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Moveıt aktarımı - Azure AD tümleştirme istemci Destek ekibine](https://community.ipswitch.com/s/support).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Moveıt aktarımı - Azure AD tümleştirmesi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Moveıt aktarımı - Azure AD tümleştirmesi, atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Moveıt aktarımı - Azure AD tümleştirmesi**.

    ![Azure AD tümleştirmesi Moveıt aktarımı - uygulamalar listesindeki bağlantılardan](./media/moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Moveıt aktarımı - tıkladığınızda Azure AD tümleştirmesi kutucuğunu erişim panelinde, otomatik olarak imzalanmış-Moveıt aktarımınız için - Azure AD tümleştirme uygulaması. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/moveittransfer-tutorial/tutorial_general_203.png

