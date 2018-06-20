---
title: 'Öğretici: New Relic Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve New Relic arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: a311a99522f5a47290cbf60993a2d1aa1997dcca
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215639"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Öğretici: New Relic Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile New Relic tümleştirmek öğrenin.

New Relic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- New Relic erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için New Relic açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile New Relic yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir New Relic çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. New Relic Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-new-relic-from-the-gallery"></a>New Relic Galeriden ekleme
Azure AD New Relic tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden New Relic eklemeniz gerekir.

**New Relic Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **New Relic**seçin **New Relic** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde New Relic](./media/new-relic-tutorial/tutorial_new-relic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı New Relic ile test etme.

Tekli çalışmaya oturum için Azure AD New Relic karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının New Relic ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

New Relic içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma New Relic ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[New Relic test kullanıcısı oluşturma](#create-a-new-relic-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı New Relic sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma New Relic uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile New Relic yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **New Relic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/new-relic-tutorial/tutorial_new-relic_samlbase.png)

3. Üzerinde **yeni Relic etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir yeni Relic etki alanı ve URL'leri](./media/new-relic-tutorial/tutorial_new-relic_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -kendi yeni Relic hesabı kimliği değiştirdiğinizden emin olun

    b. İçinde **tanımlayıcısı** metin değeri yazın: `rpm.newrelic.com`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/new-relic-tutorial/tutorial_new-relic_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/new-relic-tutorial/tutorial_general_400.png)

6. Üzerinde **yeni Relic yapılandırma** 'yi tıklatın **yapılandırma New Relic** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Yeni Relic yapılandırma](./media/new-relic-tutorial/tutorial_new-relic_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **New Relic** yönetici olarak şirket site.

8. Üstteki menüde tıklatın **hesap ayarlarını**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797036.png "hesap ayarları")

9. Tıklatın **güvenlik ve kimlik doğrulama** sekmesini ve ardından **çoklu oturum açmayı** sekmesi.
   
    ![Çoklu oturum açma](./media/new-relic-tutorial/ic797037.png "çoklu oturum açma")

10. SAML iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
   a. Tıklatın **Dosya Seç** indirilen Azure Active Directory sertifikanızı karşıya yüklemek için.

   b. İçinde **uzaktan oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
   
   c. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

   d. Tıklatın **my değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/new-relic-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/new-relic-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/new-relic-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/new-relic-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-new-relic-test-user"></a>New Relic test kullanıcısı oluşturma

New Relic oturum açmak Azure Active Directory Kullanıcıları etkinleştirmek için bunların New Relic sağlanmalıdır. New Relic söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabına New Relic sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **New Relic** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **hesap ayarlarını**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797040.png "hesap ayarları")

3. İçinde **hesap** bölmesi sol tarafta **Özet**ve ardından **Kullanıcı Ekle**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797041.png "hesap ayarları")

4. Üzerinde **etkin kullanıcılar** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Etkin kullanıcılar](./media/new-relic-tutorial/ic797042.png "etkin kullanıcılar")
   
    a. İçinde **e-posta** metin kutusuna, kullanmak istediğiniz sağlamak için geçerli bir Azure Active Directory kullanıcı e-posta adresini yazın.

    b. Olarak **rol** seçin **kullanıcı**.

    c. Tıklatın **bu kullanıcıyı eklemek**.

>[!NOTE]
>API tarafından New Relic sağlama AAD kullanıcı hesaplarına sağlanan veya herhangi diğer New Relic kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta New Relic erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**New Relic Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **New Relic**.

    ![Uygulamalar listesinde New Relic bağlantı](./media/new-relic-tutorial/tutorial_new-relic_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli New Relic parçasında tıklattığınızda, otomatik olarak New Relic uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/new-relic-tutorial/tutorial_general_01.png
[2]: ./media/new-relic-tutorial/tutorial_general_02.png
[3]: ./media/new-relic-tutorial/tutorial_general_03.png
[4]: ./media/new-relic-tutorial/tutorial_general_04.png

[100]: ./media/new-relic-tutorial/tutorial_general_100.png

[200]: ./media/new-relic-tutorial/tutorial_general_200.png
[201]: ./media/new-relic-tutorial/tutorial_general_201.png
[202]: ./media/new-relic-tutorial/tutorial_general_202.png
[203]: ./media/new-relic-tutorial/tutorial_general_203.png

