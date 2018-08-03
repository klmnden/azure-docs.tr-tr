---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Scout Yardım | Microsoft Docs'
description: Azure Active Directory ve Yardım Scout arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.openlocfilehash: 0bbdf576c38207349bb45e7b54f3ffc85ecf3d36
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449443"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Öğretici: Azure Active Directory Yardımı Scout ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Scout yardımcı tümleştirme konusunda bilgi edinin.

Scout yardımcı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Scout yardımcı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan yardımcı (çoklu oturum açma) için Scout açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Scout yardımcı Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Yardım Scout çoklu oturum açma etkin aboneliği

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Scout yardımcı galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-help-scout-from-the-gallery"></a>Scout yardımcı galeri ekleme
Azure AD'ye yardımcı Scout tümleştirmesini yapılandırmak için Yardım Scout Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden yardımcı Scout eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **yardımcı Scout**seçin **yardımcı Scout** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde Scout Yardım](./media/helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Scout Yardım ile test etme

Tek iş için oturum açma için Azure AD yardımcı Scout karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili Yardım Scout kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yardım Scout oturumları için e-posta adreslerini kullanır, bu nedenle bağlantı kurmak için aynı kullanın **e-posta adresi** olarak **kullanıcı adı** Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma Yardımı Scout ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Scout yardımcı test kullanıcısı oluşturma](#create-a-help-scout-test-user)**  - yardımcı kullanıcı Azure AD gösterimini bağlı Scout Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve yardımcı Scout uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Yardımı Scout ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **yardımcı Scout** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/helpscout-tutorial/tutorial_helpscout_samlbase.png)

1. Üzerinde **yardımcı Scout etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Scout etki alanı ve URL'ler tek oturum açma bilgileri Yardım](./media/helpscout-tutorial/tutorial_helpscout_url.png)

    a. **Tanımlayıcı** olduğu **"Hedef kitle URİ'si (hizmet sağlayıcı varlık kimliği)"** yardımcı Scout ' başlar `urn:`

    b. **Yanıt URL'si** olduğu **"Sonrası geri URL (onay belgesi tüketici hizmeti URL'si)"** yardımcı Scout ' başlar `https://` 

    > [!NOTE] 
    > Bu URL'ler gösterimi için değerler. Bu değerler gerçek yanıt URL'si ve tanımlayıcı güncelleştirmeniz gerekir. Bu değerleri almak **çoklu oturum açma** sekmesi altında kimlik doğrulaması bölümünde, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

1. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, onay **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımı uygulayın:

    ![Scout etki alanı ve URL'ler tek oturum açma bilgileri Yardım](./media/helpscout-tutorial/tutorial_helpscout_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://secure.helpscout.net/members/login/`
     
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/helpscout-tutorial/tutorial_helpscout_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/helpscout-tutorial/tutorial_general_400.png)


1. Üzerinde **yardımcı Scout yapılandırma** bölümünde **yardımcı Scout yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümüne**.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/config.png) 

1. Farklı bir web tarayıcı penceresinde Yardım Scout şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklama açtıktan sonra **"Manage"** seçin ve üstteki menüden **"Şirket"** açılan menüden.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings1.png) 
 
1. Seçin **"Authentication"** sol taraftaki menüden. 

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings2.png) 

1. Bu SAML ayarları bölümüne alır ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings3.png) 
 
    a. Kopyalama **sonrası arka URL (onay belgesi tüketici hizmeti URL'si)** değeri ve değer yapıştırın **yanıt URL'si** Azure portalında, Yardım Scout kutusunda **etki alanı ve URL'ler** bölümü.
    
    b. Kopyalama **hedef kitle URİ'si (hizmet sağlayıcı varlık kimliği)** değeri ve değer yapıştırın **tanımlayıcı** Azure portalında, Yardım Scout kutusunda **etki alanı ve URL'ler** bölümü.

1. İki durumlu **etkinleştirme SAML** üzerinde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings4.png) 
 
    a. İçinde **çoklu oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    
    b. Tıklayın **sertifikasını karşıya yükle** yüklenecek **Certificate(Base64)** Azure portalından indirdiğiniz.

    c. Kuruluşunuzun e-posta etki alanları ör - enter `contoso.com` içinde **e-posta etki alanları** metin. Birden çok etki alanı virgülle ayırabilirsiniz. Dilediğiniz zaman yardımcı Scout kullanıcı veya belirli bir etki alanı girer üzerinde yönetici [yardımcı Scout oturum açma sayfası](https://secure.helpscout.net/members/login/) kendi kimlik bilgileriyle kimlik doğrulaması için kimlik sağlayıcısına yeniden yönlendirilir.

    d. Son olarak, geçiş **zorla SAML oturum açma** yalnızca Scout yardımcı olmak için bu yöntemle oturum açmasını istiyorsanız. Yine de bunları kendi Yardım Scout kimlik bilgileriyle oturum seçeneğini bırakın istiyorsanız, açılıp devre dışı bırakılabilir. Bu etkin olsa bile, bir hesap sahibi her zaman Scout yardımcı olmak için hesap parolalarını oturum açamaz olur.

    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/helpscout-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/helpscout-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/helpscout-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/helpscout-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-help-scout-test-user"></a>Scout yardımcı test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon yardımcı Scout içinde adlı bir kullanıcı oluşturmaktır. Yardım Scout tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Kullanıcı Yardım Scout içinde zaten mevcut değilse Yardım Scout erişmeye çalıştığında yeni bir tane oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Scout yardımcı olmak için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Scout yardımcı olmak için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **yardımcı Scout**.

    ![Uygulamalar listesinde Scout Yardım bağlantısı](./media/helpscout-tutorial/tutorial_helpscout_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde yardımcı Scout kutucuğa tıkladığınızda, otomatik olarak Yardım Scout uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/helpscout-tutorial/tutorial_general_01.png
[2]: ./media/helpscout-tutorial/tutorial_general_02.png
[3]: ./media/helpscout-tutorial/tutorial_general_03.png
[4]: ./media/helpscout-tutorial/tutorial_general_04.png

[100]: ./media/helpscout-tutorial/tutorial_general_100.png

[200]: ./media/helpscout-tutorial/tutorial_general_200.png
[201]: ./media/helpscout-tutorial/tutorial_general_201.png
[202]: ./media/helpscout-tutorial/tutorial_general_202.png
[203]: ./media/helpscout-tutorial/tutorial_general_203.png

