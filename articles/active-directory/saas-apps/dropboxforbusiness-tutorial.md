---
title: 'Öğretici: Azure Active Directory Tümleştirme ile iş için Dropbox | Microsoft Docs'
description: Azure Active Directory ve iş için Dropbox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: d46f2aac5fb16b10f33cccabdcd76d60f0d6dfb9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438068"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Öğretici: Azure Active Directory Dropbox for Business ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iş için Dropbox tümleştirme konusunda bilgi edinin.

İş için Dropbox'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İş Dropbox erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan dropbox'a iş (çoklu oturum açma) için Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iş için Dropbox ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Dropbox iş çoklu oturum açma için abonelik etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. İş için Dropbox galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-dropbox-for-business-from-the-gallery"></a>İş için Dropbox galeri ekleme
Azure AD'de iş için Dropbox tümleştirmesini yapılandırmak için iş için Dropbox Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**İş için Dropbox Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **iş için Dropbox**seçin **iş için Dropbox** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde iş için Dropbox](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iş için Dropbox'ile test edin.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı iş için dropbox'ta bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve iş için dropbox'ta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

İş için Dropbox'ta değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ile iş için Dropbox sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Dropbox için iş test kullanıcısı oluşturma](#create-a-dropbox-for-business-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı iş için dropbox'ta bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Dropbox iş uygulaması için çoklu oturum açmayı yapılandırın.

**İş için Dropbox ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **iş için Dropbox** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

1. Üzerinde **iş etki alanı ve URL'ler için Dropbox** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Dropbox için iş etki alanı ve URL'ler tek oturum açma bilgileri](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.dropbox.com/sso/<id>`

    b. İçinde **tanımlayıcı** metin kutusuna bir değer girin: `Dropbox`

    > [!NOTE] 
    > Önceki oturum açma URL değeri, gerçek değeri değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek oturum açma URL'si ile değerini güncelleştirir. İlgili kişi [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact) değeri alınamıyor. 
 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

1. Üzerinde **iş yapılandırması için Dropbox** bölümünde **Dropbox'ı iş için yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![İş yapılandırması için Dropbox](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

1. Çoklu oturum açmayı yapılandırma **iş için Dropbox** yan, iş Kiracı için Dropbox gidin.

    a. Dropbox iş Kiracı için oturum açın. 
   
    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769509.png "çoklu oturum açmayı yapılandırın")
   
    b. Sol taraftaki gezinti bölmesinde **Yönetici Konsolu**. 
   
    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769510.png "çoklu oturum açmayı yapılandırın")
   
    c. Üzerinde **Yönetici Konsolu**, tıklayın **kimlik doğrulaması** sol gezinti bölmesinde. 
   
    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769511.png "çoklu oturum açmayı yapılandırın")
   
    d. İçinde **çoklu oturum açma** bölümünden **çoklu oturum açmayı etkinleştirme**ve ardından **daha fazla** bu bölümü genişletin.  
   
    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769512.png "çoklu oturum açmayı yapılandırın")
   
    e. URL'yi kopyalayabilirsiniz **kullanıcılar oturum açabilir e-posta adresi girerek veya doğrudan gidebilirsiniz** yapıştırın **oturum açma URL'si** textbox'ın **iş etki alanı ve URL'leriçinDropbox** bölümü Azure portalı. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/dropboxforbusiness-tutorial/ic769513.png)
    
1. İçinde **çoklu oturum açma** bölümünü **kimlik doğrulaması** sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/IC769516.png "çoklu oturum açmayı yapılandırın")
   
    a. Tıklayın **gerekli**.
   
    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** hangi Azure portaldan kopyaladığınız.

    c. Tıklayın **Sertifika Seç**ve ardından göz atın, **Base64 kodlu sertifika dosyası**.

    d. Tıklayın **değişiklikleri kaydetmek** iş Kiracı için DropBox yapılandırmasını tamamlamak için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Bir Dropbox için iş test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı, iş için dropbox'ta oluşturulur. İş için Dropbox tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. İş için dropbox'ta bir kullanıcı zaten mevcut değilse yeni bir iş için Dropbox'ı erişmeyi denediğinde oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, iş için dropbox'a erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İş için Dropbox Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **iş için Dropbox**.

    ![Uygulamalar listesini iş bağlantısı için Dropbox](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde iş kutucuk için Dropbox'ı tıkladığınızda, iş uygulaması için oturum açma sayfası, Dropbox'ın almanız gerekir.
 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

