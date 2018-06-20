---
title: 'Öğretici: Azure Active Directory Tümleştirme ile PagerDuty | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile PagerDuty arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 37409ee72591d943a834ff38f077a002a1724ab9
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215401"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Öğretici: Azure Active Directory Tümleştirme PagerDuty ile

Bu öğreticide, Azure Active Directory (Azure AD) ile PagerDuty tümleştirmek öğrenin.

PagerDuty Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- PagerDuty erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için PagerDuty (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme PagerDuty ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir PagerDuty çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden PagerDuty ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-pagerduty-from-the-gallery"></a>Galeriden PagerDuty ekleme
Azure AD PagerDuty tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden PagerDuty eklemeniz gerekir.

**Galeriden PagerDuty eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **PagerDuty**seçin **PagerDuty** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı PagerDuty sınayın.

Tekli çalışmaya oturum için Azure AD PagerDuty karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının PagerDuty ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

PagerDuty içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma PagerDuty ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[PagerDuty test kullanıcısı oluşturma](#create-a-pagerduty-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı PagerDuty sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma PagerDuty uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile PagerDuty yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **PagerDuty** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. Üzerinde **PagerDuty etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![PagerDuty etki alanı ve URL'leri tek oturum açma bilgileri](./media/pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenant-name>.pagerduty.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenant-name>.pagerduty.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [PagerDuty istemci destek ekibi](https://www.pagerduty.com/support/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pagerduty-tutorial/tutorial_general_400.png)

6. Üzerinde **PagerDuty yapılandırma** 'yi tıklatın **yapılandırma PagerDuty** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![PagerDuty yapılandırma](./media/pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. Farklı web tarayıcısı penceresinde Pagerduty şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **hesap ayarlarını**.
   
    ![Hesap ayarları](./media/pagerduty-tutorial/ic778535.png "hesap ayarları")

9. Tıklatın **çoklu oturum açma**.
   
    ![Çoklu oturum açma](./media/pagerduty-tutorial/ic778536.png "çoklu oturum açma")

10. Üzerinde **etkinleştirmek çoklu oturum açma (SSO)** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı etkinleştir](./media/pagerduty-tutorial/ic778537.png "çoklu oturum açmayı etkinleştir")
   
    a. Not Defteri'nde Azure portalından indirdiğiniz, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu
  
    b. İçinde **oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
  
    c. İçinde **oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyalanan.
 
    d. Seçin **tek oturum açma kapatma**.
 
    e. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/pagerduty-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/pagerduty-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/pagerduty-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/pagerduty-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-pagerduty-test-user"></a>PagerDuty test kullanıcısı oluşturma

Azure AD kullanıcıları için PagerDuty oturum açmak etkinleştirmek için bunların PagerDuty sağlanmalıdır.  
PagerDuty söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>API tarafından Pagerduty sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer Pagerduty kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Pagerduty** Kiracı.

2. Üstteki menüde tıklatın **kullanıcılar**.

3. Tıklatın **kullanıcıları eklemek**.
   
    ![Kullanıcıları ekleme](./media/pagerduty-tutorial/ic778539.png "kullanıcı ekleme")

4.  Üzerinde **ekibinizin davet** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Davet Et ekibinizin](./media/pagerduty-tutorial/ic778540.png "ekibinizin davet et")

    a. Tür **ilk ve son adı** gibi kullanıcının **Britta Simon**. 
   
    b. Girin **e-posta** kullanıcının adresi ister **brittasimon@contoso.com**.
   
    c. Tıklatın **Ekle**ve ardından **Gönder başvurulmasını**.
   
    >[!NOTE]
    >Tüm eklenen kullanıcıların PagerDuty hesabı oluşturmak için bir davet alır.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta PagerDuty için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**PagerDuty için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **PagerDuty**.

    ![Uygulamalar listesinde PagerDuty bağlantı](./media/pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tıkladığınızda erişim Panelyou PagerDuty döşemede otomatik olarak PagerDuty uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/pagerduty-tutorial/tutorial_general_203.png

