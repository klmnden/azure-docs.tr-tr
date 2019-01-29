---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iQualify LMS | Microsoft Docs'
description: Azure Active Directory ve LMS iQualify arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: 74242d5e539824b913a68a7a1116b16565fd1253
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55170130"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Öğretici: İQualify LMS ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iQualify LMS tümleştirme konusunda bilgi edinin.

Azure AD ile iQualify LMS tümleştirme ile aşağıdaki avantajları sağlar:

- İQualify LMS erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için iQualify LMS (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi LMS iQualify ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir iQualify LMS çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden iQualify LMS ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-iqualify-lms-from-the-gallery"></a>Galeriden iQualify LMS ekleme
Azure AD'de iQualify LMS tümleştirmesini yapılandırmak için iQualify LMS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden iQualify LMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **iQualify LMS**seçin **iQualify LMS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde iQualify LMS](./media/iqualify-tutorial/tutorial_iqualify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı LMS tabanlı iQualify ile test etme

Tek iş için oturum açma için Azure AD hangi karşılık gelen kullanıcı iQualify LMS bir kullanıcının Azure AD'de olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının iQualify ilgili kullanıcı arasında bir bağlantı ilişki LMS kurulması gerekir.

Değerini iQualify LMS, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma iQualify LMS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir iQualify LMS test kullanıcısı oluşturma](#create-an-iqualify-lms-test-user)**  - Britta Simon iQualify kullanıcı Azure AD gösterimini bağlı LMS içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve iQualify LMS uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma LMS iQualify ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **iQualify LMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/iqualify-tutorial/tutorial_iqualify_samlbase.png)

1. Üzerinde **iQualify LMS etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/iqualify-tutorial/tutorial_iqualify_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/`|
    | Test ortamı: `https://<yourorg>.iqualify.io`|
    
    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Test ortamı: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![iQualify LMS etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/iqualify-tutorial/tutorial_iqualify_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|--|
    | Üretim ortamı: `https://<yourorg>.iqualify.com/login` |
    | Test ortamı: `https://<yourorg>.iqualify.io/login` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [iQualify LMS istemci Destek ekibine](https://www.iqualify.com) bu değerleri almak için. 

1. Belirli bir biçimde görüntülenmesi için güvenlik onaylama işlemi biçimlendirme dili (SAML) onaylar iQualify LMS uygulama bekliyor. Talepleri yapılandırın ve özniteliklerin değerleri yönetme **kullanıcı öznitelikleri** bölümü aşağıdaki ekran görüntüsünde gösterildiği gibi iQualify uygulama tümleştirme sayfası:
    
    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | --- | --- |    
    | e-posta | User.userPrincipalName |
    | first_name | User.givenName |
    | Soyadı | User.surname |
    | person_id | "özniteliği" | 

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb2.png)

    ![Çoklu oturum açmayı yapılandırın](./media/iqualify-tutorial/atb3.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

    e. Sonraki tablo satırları için "a" ile "d" adımları yineleyin. 

    > [!Note]
    > "A" ile "d" adımları için yinelenen **person_id** özniteliği **isteğe bağlı**

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base 64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/iqualify-tutorial/tutorial_iqualify_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/iqualify-tutorial/tutorial_general_400.png)
    
1. Üzerinde **iQualify LMS yapılandırma** bölümünde **iQualify LMS yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![iQualify LMS yapılandırma](./media/iqualify-tutorial/tutorial_iqualify_configure.png) 

1.  Yeni bir tarayıcı penceresi açın ve ardından iQualify ortamınıza bir yönetici olarak oturum açın.

1. Oturum açtıktan sonra sağ üst köşedeki avatarınız tıklayın ve ardından tıklayarak **"Hesap ayarları."**

    ![Hesap ayarları](./media/iqualify-tutorial/setting1.png) 
1. Hesap ayarları alanında Şerit menüsünde sol tıklayın ve tıklayarak **"TÜMLEŞTİRMELER."**
    
    ![TÜMLEŞTİRMELER](./media/iqualify-tutorial/setting2.png)

1. TÜMLEŞTİRMELER altında tıklayarak **SAML** simgesi.

    ![SAML simgesi](./media/iqualify-tutorial/setting3.png)

1. İçinde **SAML kimlik doğrulaması ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![SAML kimlik doğrulama ayarları](./media/iqualify-tutorial/setting4.png)

    a. İçinde **SAML çoklu oturum açma hizmeti URL'si** kutusu, yapıştırma **SAML çoklu Sign‑On hizmet URL'si** değer Azure AD uygulama yapılandırması penceresinde kopyalanır.
    
    b. İçinde **SAML oturum kapatma URL'si** kutusu, yapıştırma **Sign‑Out URL** değer Azure AD uygulama yapılandırması penceresinde kopyalanır.
    
    c. İndirilen sertifika dosyasını Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **ortak sertifika** kutusu.
    
    d. İçinde **oturum açma düğmesi ETİKETİNİN** oturum açma sayfasında görüntülenecek düğme için bir ad girin.
    
    e. **KAYDET**'e tıklayın.

    f. Tıklayın **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/iqualify-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/iqualify-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/iqualify-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/iqualify-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-iqualify-lms-test-user"></a>Bir iQualify LMS test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı iQualify oluşturulur. iQualify LMS just‑in‑time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı iQualify içinde zaten mevcut değilse iQualify LMS erişmeye çalıştığında yeni bir tane oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iQualify LMS erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon LMS iQualify için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **iQualify LMS**.

    ![İQualify LMS uygulamalar listesinde bağlayın.](./media/iqualify-tutorial/tutorial_iqualify_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

İQualify LMS erişim Paneli'nde kutucuğuna tıkladığınızda, oturum açma sayfası iQualify LMS uygulamanızın almanız gerekir. 

   ![oturum açma sayfası](./media/iqualify-tutorial/login.png) 

Tıklayın **oturum Azure AD'de oturum** düğmesini otomatik olarak oturum açma iQualify LMS uygulamanıza.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iqualify-tutorial/tutorial_general_01.png
[2]: ./media/iqualify-tutorial/tutorial_general_02.png
[3]: ./media/iqualify-tutorial/tutorial_general_03.png
[4]: ./media/iqualify-tutorial/tutorial_general_04.png

[100]: ./media/iqualify-tutorial/tutorial_general_100.png

[200]: ./media/iqualify-tutorial/tutorial_general_200.png
[201]: ./media/iqualify-tutorial/tutorial_general_201.png
[202]: ./media/iqualify-tutorial/tutorial_general_202.png
[203]: ./media/iqualify-tutorial/tutorial_general_203.png

