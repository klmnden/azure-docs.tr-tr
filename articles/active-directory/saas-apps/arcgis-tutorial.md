---
title: 'Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Arcgıs Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: 24a82bbaf47153791da2f21a0b68c2f81c0670e7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446365"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Arcgıs Online tümleştirme konusunda bilgi edinin.

Arcgıs Online Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Arcgıs Online erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Arcgıs Online için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Arcgıs Online ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Arcgıs Online çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Arcgıs Online galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-arcgis-online-from-the-gallery"></a>Arcgıs Online galeri ekleme
Azure AD'de Arcgıs Online'nın tümleştirmesini yapılandırmak için Arcgıs Online Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Arcgıs Online Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Arcgıs Online**seçin **Arcgıs Online** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Arcgıs Online sonuç listesinde](./media/arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Arcgıs Online ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Arcgıs Online bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Arcgıs Online arasında bir bağlantı ilişki kurulması gerekir.

Arcgıs Online içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Arcgıs Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Arcgıs Online test kullanıcısı oluşturma](#create-a-arcgis-online-test-user)**  - Arcgıs kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Arcgıs Online uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Arcgıs Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Arcgıs Online** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

1. Üzerinde **Arcgıs Online etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Arcgıs Online etki alanı ve URL'ler tek oturum açma bilgileri](./media/arcgis-tutorial/tutorial_arcgisonline_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.maps.arcgis.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `<companyname>.maps.arcgis.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Arcgıs Online istemci Destek ekibine](http://support.esri.com/en/) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/arcgis-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde bir Arcgıs şirket sitenize yönetici olarak oturum.

1. Tıklayın **ayarları düzenleme**.

    ![Ayarları Düzenle](./media/arcgis-tutorial/ic784742.png "ayarlarını Düzenle")

1. Tıklayın **güvenlik**.

    ![Güvenlik](./media/arcgis-tutorial/ic784743.png "güvenlik")

1. Altında **Kurumsal oturum açma bilgileri**, tıklayın **AYARLANMIŞ bir kimlik sağlayıcı**.

    ![Kurumsal oturum açma bilgileri](./media/arcgis-tutorial/ic784744.png "Kurumsal oturum açma bilgileri")

1. Üzerinde **ayarlanmış bir kimlik sağlayıcı** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısını Ayarla](./media/arcgis-tutorial/ic784745.png "kimlik sağlayıcısını Ayarla")
   
    a. İçinde **adı** metin kutusuna kuruluşunuzun adını yazın.

    b. İçin **Kurumsal kimlik sağlayıcısı meta verileri kullanarak belirtilmelidir**seçin **dosya**.

    c. İndirilen meta verileri dosyanızı karşıya yüklemek için tıklayın **dosya**.

    d. Tıklayın **KÜMESİ kimlik SAĞLAYICISI**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/arcgis-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/arcgis-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/arcgis-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/arcgis-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-arcgis-online-test-user"></a>Arcgıs Online test kullanıcısı oluşturma

Arcgıs Online'da oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Arcgıs Online sağlanması gerekir.  
Arcgıs Online söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Arcgıs** Kiracı.

1. Tıklayın **davet ÜYELERİ**.
   
    ![Üyeler davet](./media/arcgis-tutorial/ic784747.png "üyeler davet")

1. Seçin **üyeleri otomatik olarak bir e-posta göndermeden ekleme**ve ardından **sonraki**.
   
    ![Üyeleri otomatik olarak Ekle](./media/arcgis-tutorial/ic784748.png "üyeleri otomatik olarak Ekle")

1. Üzerinde **üyeleri** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
     ![Ekleme ve gözden geçirme](./media/arcgis-tutorial/ic784749.png "ekleme ve gözden geçirme")
    
     a. Girin **e-posta**, **ad**, ve **Soyadı** sağlamak istediğiniz geçerli bir AAD hesabı.
  
     b. Tıklayın **ekleme ve gözden geçirme**.
1. Girdiğiniz ve ardından verileri gözden **Üye Ekle**.
   
    ![Üye Ekle](./media/arcgis-tutorial/ic784750.png "Üye Ekle")
        
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Arcgıs Online erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Arcgıs Online atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Arcgıs Online**.

    ![Uygulamalar listesinde Arcgıs Online bağlantısı](./media/arcgis-tutorial/tutorial_arcgisonline_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Arcgıs Online kutucuğa tıkladığınızda, size otomatik olarak Arcgıs Online uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/arcgis-tutorial/tutorial_general_01.png
[2]: ./media/arcgis-tutorial/tutorial_general_02.png
[3]: ./media/arcgis-tutorial/tutorial_general_03.png
[4]: ./media/arcgis-tutorial/tutorial_general_04.png

[100]: ./media/arcgis-tutorial/tutorial_general_100.png

[200]: ./media/arcgis-tutorial/tutorial_general_200.png
[201]: ./media/arcgis-tutorial/tutorial_general_201.png
[202]: ./media/arcgis-tutorial/tutorial_general_202.png
[203]: ./media/arcgis-tutorial/tutorial_general_203.png

