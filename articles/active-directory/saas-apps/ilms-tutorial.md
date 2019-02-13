---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iLMS | Microsoft Docs'
description: Azure Active Directory ve iLMS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf68f76f6ba451ff5f3e81b3aaabf3825155ba15
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201542"
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Öğretici: İLMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iLMS tümleştirme konusunda bilgi edinin.

Azure AD ile iLMS tümleştirme ile aşağıdaki avantajları sağlar:

- İLMS erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için iLMS (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile iLMS yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir iLMS çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden iLMS ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ilms-from-the-gallery"></a>Galeriden iLMS ekleme
Azure AD'de iLMS tümleştirmesini yapılandırmak için iLMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iLMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **iLMS**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/tutorial_ilms_search.png)

1. Sonuçlar panelinde seçin **iLMS**, ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iLMS sınayın.

Tek iş için oturum açma için Azure AD ne iLMS karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının iLMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** iLMS içinde.

Yapılandırma ve Azure AD çoklu oturum açma iLMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir iLMS test kullanıcısı oluşturma](#creating-an-ilms-test-user)**  - Britta Simon Azure AD'ye gösterimini her için bağlı iLMS içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve iLMS uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile iLMS yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **iLMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_samlbase.png)

1. Üzerinde **iLMS etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_url.png)

    a. İçinde **tanımlayıcı** metin kutusu, yapıştırma **tanımlayıcı** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS Yönetim Portalı'nda.

    b. İçinde **yanıt URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** SAML ayarlarının iLMS yönetim portalında şu desene sahip `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Bu '123456' tanımlayıcısı bir örnek değeri.

1. Denetleme **Gelişmiş URL ayarlarını göster**, uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_url1.png)

    İçinde **oturum açma URL'si** metin kutusu, yapıştırma **uç noktası (URL)** değer, kopyalama **hizmet sağlayıcısı** olarak iLMS Yönetim Portalı'nda SAML ayarları bölümü `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

1. JIT sağlamayı etkinleştirmek için belirli bir biçimde SAML onaylamalarını iLMS uygulama bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/4.png)
    
    Oluşturma **departman, bölge** ve **bölme** öznitelikleri ve iLMS içinde bu özniteliklerin adını ekleyin. Yukarıda gösterilen bu öznitelikleri gereklidir.  

    > [!NOTE] 
    > Etkinleştirmek sahip olduğunuz **Un-recognized kullanıcı hesabı oluşturma** bu öznitelikleri eşlemek için iLMS içinde. Yönergeleri izleyerek [burada](http://support.inspiredelearning.com/customer/portal/articles/2204526) öznitelikleri yapılandırma hakkında bir fikir edinmek için.

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | Bölme | User.Department |
    | bölge | User.State |
    | Bölüm | user.jobtitle |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde oturum açın, **iLMS Yönetici portalı** yönetici olarak.

1. Tıklayın **SSO:SAML** altında **ayarları** SAML ayarlarını açın ve aşağıdaki adımları gerçekleştirmek için sekmesinde:
    
    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/1.png) 

    a. Genişletin **hizmet sağlayıcısı** bölümü ve kopyalama **tanımlayıcı** ve **uç noktası (URL)** değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/2.png) 

    b. Altında **kimlik sağlayıcısı** bölümünde **meta verileri içeri aktarma**.
    
    c. Seçin **meta verileri** dosya, Azure Portalı'ndan indirildiği **SAML imzalama sertifikası** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Sağlama kaldırma için iLMS hesaplarını oluşturmak için JIT etkinleştirmek istiyorsanız-kullanıcıların tanıyacak, aşağıdaki adımları izleyin:
        
       - Denetleme **beklemediğiniz tanınan bir kullanıcı hesabı oluşturma**.
       
       ![Configure Single Sign-On](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Azure AD'de iLMS öznitelikleri ile harita öznitelikleri. Öznitelik sütunu, öznitelik adı veya varsayılan değeri belirtin.

    e. Git **iş kuralları** sekmesinde ve aşağıdaki adımları gerçekleştirin: 
        
       ![Configure Single Sign-On](./media/ilms-tutorial/5.png)

       - Denetleme **Un-recognized bölgeler oluşturun, bölümler ve Departmanlar** bölgeler, bölümler ve zamanında çoklu oturum açma zaten var olmayan bölümler oluşturmak için.
        
       - Denetleme **güncelleştirme kullanıcı profili sırasında oturum açma** her çoklu oturum açma ile kullanıcının profilini güncelleştirilip güncelleştirilmediğini belirtmek için. 
        
       - Varsa **"Güncelleştirme boş değerler için olmayan zorunlu alanlar, kullanıcı profili"** seçeneği, oturum açma temel boş isteğe bağlı profili alanlar da alanlar boş değerler içerecek şekilde kullanıcı iLMS profilinin neden.
        
       - Denetleme **hatası bildirim e-posta Gönder** ve hatası bildirim e-postaları almak istediğiniz kullanıcının e-posta girin.

1. Tıklayın **Kaydet** düğmesini kullanarak ayarları kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/save.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ilms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-ilms-test-user"></a>Bir iLMS test kullanıcısı oluşturma

Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulama kullanıcıları otomatik olarak uygulama oluşturulduktan sonra sadece destekler. JIT çalışır, tıkladıysanız **Un-recognized kullanıcı hesabı oluşturma** iLMS Yönetici portalı SAML yapılandırma ayarı sırasında onay kutusu.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, ardından aşağıdaki adımları uygulayın:

1. İLMS şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayın **"Kullanıcı kaydetme"** altında **kullanıcılar** açmak için sekmesinde **kullanıcı Kaydet** sayfası. 
   
   ![Çalışan Ekle](./media/ilms-tutorial/3.png)

1. Üzerinde **"Kullanıcı kaydetme"** sayfasında, aşağıdaki adımları gerçekleştirin.

    ![Çalışan Ekle](./media/ilms-tutorial/create_testuser_add.png)

    a. İçinde **ad** metin Britta ilk tür adı.
   
    b. İçinde **Soyadı** metin Soyadı Simon yazın.

    c. İçinde **e-posta kimliği** metin Britta Simon hesabı e-posta adresini yazın.

    d. İçinde **bölge** açılır listesinde, bölge için bir değer seçin.

    e. İçinde **bölme** açılır listesinde, bölme için bir değer seçin.

    f. İçinde **departmanı** açılır listesinde, departman için bir değer seçin.

    g. **Kaydet**’e tıklayın.

    > [!NOTE] 
    > Seçerek kullanıcı için kayıt e-posta gönderebilir **kayıt posta Gönder** onay kutusu.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma iLMS kendi erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon iLMS için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **iLMS**.

    ![Çoklu oturum açmayı yapılandırın](./media/ilms-tutorial/tutorial_ilms_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde iLMS kutucuğa tıkladığınızda, otomatik olarak iLMS uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ilms-tutorial/tutorial_general_01.png
[2]: ./media/ilms-tutorial/tutorial_general_02.png
[3]: ./media/ilms-tutorial/tutorial_general_03.png
[4]: ./media/ilms-tutorial/tutorial_general_04.png

[100]: ./media/ilms-tutorial/tutorial_general_100.png

[200]: ./media/ilms-tutorial/tutorial_general_200.png
[201]: ./media/ilms-tutorial/tutorial_general_201.png
[202]: ./media/ilms-tutorial/tutorial_general_202.png
[203]: ./media/ilms-tutorial/tutorial_general_203.png

