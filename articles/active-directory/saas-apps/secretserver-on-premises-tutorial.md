---
title: 'Öğretici: Azure Active Directory Tümleştirme (şirket içi) gizli sunucusuyla | Microsoft Docs'
description: Çoklu oturum açma gizli anahtarı sunucusu (şirket içi) ile Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: be4ba84a-275d-4f71-afce-cb064edc713f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: c9229afd7bd8ebad85ce9e329fb11f992236bce0
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220110"
---
# <a name="tutorial-azure-active-directory-integration-with-secret-server-on-premises"></a>Öğretici: Azure Active Directory Tümleştirme gizli sunucusuyla (şirket içi)

Bu öğreticide, gizli sunucusunu (şirket içi) Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Gizli sunucu (şirket içi) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Gizli sunucu (şirket içi) erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak gizli sunucuya (şirket içi) (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme (şirket içi) gizli sunucusuyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir gizli sunucu (şirket içi) çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden gizli sunucu (şirket içi) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Galeriden gizli sunucu (şirket içi) ekleme
Azure AD ile tümleştirme gizli sunucusunun (şirket içi) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden gizli sunucu (şirket içi) eklemeniz gerekir.

**Galeriden gizli sunucu (şirket içi) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **gizli sunucu (şirket içi)** seçin **gizli sunucu (şirket içi)** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde gizli sunucu (şirket içi)](./media/secretserver-on-premises-tutorial/tutorial_secretserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma gizli anahtarı "Britta Simon" adlı bir test kullanıcı tabanlı sunucuyu (şirket içi) ile test etme.

Tekli çalışmaya oturum için Azure AD gizli sunucu (şirket içi) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının gizliliği sunucu (şirket içi) ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma gizli anahtarı sunucusu (şirket içi) ile test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Gizli sunucu (şirket içi) test kullanıcısı oluşturma](#create-a-secret-server-on-premises-test-user)**  - Britta Simon, karşılık gelen gizli anahtarı kullanıcı Azure AD gösterimini bağlantılı sunucu (şirket içi) sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma gizli anahtarı sunucu (şirket içi) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma gizli anahtarı sunucusuyla (şirket içi) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **gizli sunucu (şirket içi)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/secretserver-on-premises-tutorial/tutorial_secretserver_samlbase.png)

3. Üzerinde **gizli sunucu (şirket içi) etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Gizli sunucu (şirket içi) etki alanı ve URL'ler tek oturum açma bilgileri](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, değer bir örnek olarak seçilen kullanıcı girin: `https://secretserveronpremises.azure`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<SecretServerURL>/SAML/AssertionConsumerService.aspx `

    > [!NOTE]
    > Yukarıda gösterilen varlık kimliği yalnızca bir örnek ve gizli Server örneğinizi Azure AD içinde tanımlayan benzersiz bir değer seçmek boş. Bu varlık kimliği göndermesi gerekir. [gizli sunucu (şirket içi) istemci destek ekibi](https://thycotic.force.com/support/s/) ve bunlar kendi tarafında yapılandırın. Daha fazla ayrıntı için lütfen okuyun [bu makalede](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server).

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Gizli sunucu (şirket içi) etki alanı ve URL'ler tek oturum açma bilgileri](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<SecretServerURL>/login.aspx`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [gizli sunucu (şirket içi) istemci destek ekibi](https://thycotic.force.com/support/s/) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/secretserver-on-premises-tutorial/tutorial_secretserver_certificate.png)

6. Denetleme **Göster gelişmiş sertifika imzalama ayarları** seçip **imzalama seçeneği** olarak **oturum SAML yanıtını ve onaylama**.

    ![İmzalama Seçenekleri](./media/secretserver-on-premises-tutorial/signing.png)

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/secretserver-on-premises-tutorial/tutorial_general_400.png)
    
8. Üzerinde **gizli sunucu (şirket içi) yapılandırması** 'yi tıklatın **gizli sunucusunu yapılandırma (şirket içi)** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Gizli sunucu (şirket içi) yapılandırması](./media/secretserver-on-premises-tutorial/tutorial_secretserver_configure.png)

9. Çoklu oturum açma yapılandırmak için **gizli sunucu (şirket içi)** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64), Sign-Out URL SAML çoklu oturum açma hizmet URL'si**, ve **SAML varlık Kimliği** için [gizli sunucu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/secretserver-on-premises-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/secretserver-on-premises-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/secretserver-on-premises-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/secretserver-on-premises-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-secret-server-on-premises-test-user"></a>Gizli sunucu (şirket içi) test kullanıcısı oluşturma

Bu bölümde, Britta Simon gizli sunucu (şirket içi) adlı bir kullanıcı oluşturun. Çalışmak [gizli sunucu (şirket içi) destek ekibi](https://thycotic.force.com/support/s/) gizli sunucu (şirket içi) platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma gizli anahtarı sunucuya (şirket içi) erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Gizli sunucuya (şirket içi) Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **gizli sunucu (şirket içi)**.

    ![Uygulamalar listesinde gizli sunucu (şirket içi) bağlantısı](./media/secretserver-on-premises-tutorial/tutorial_secretserver_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde gizli sunucu (şirket içi) Bölmesi'ı tıklattığınızda, otomatik olarak gizli sunucu (şirket içi) uygulamanız açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/secretserver-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/secretserver-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/secretserver-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/secretserver-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/secretserver-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/secretserver-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/secretserver-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/secretserver-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/secretserver-on-premises-tutorial/tutorial_general_203.png

