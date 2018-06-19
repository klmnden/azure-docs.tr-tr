---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Autotask çalışma | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Autotask çalışma arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5bc72964b24ffd3b8c53f752e822b95d6694e44e
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971925"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Öğretici: Azure Active Directory Tümleştirme ile Autotask çalışma

Bu öğreticide, Azure Active Directory (Azure AD) ile Autotask çalışma alanına tümleştirmek öğrenin.

Autotask çalışma alanına Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Autotask çalışma alanına erişimi olan Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Autotask çalışma alanına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Autotask çalışma alanınız ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Autotask çalışma alanı çoklu oturum açma etkin abonelik
- Bir yönetici ya da çalışma Süper yönetici olması gerekir.
- Azure AD'de yönetici hesabı olması gerekir.
- Bu özellik yararlanacak olan kullanıcıların çalışma alanına ve Azure AD içinde hesapların olmalıdır ve e-posta adreslerini her ikisi için aynı olmalıdır.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Autotask çalışma alanına ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-autotask-workplace-from-the-gallery"></a>Galeriden Autotask çalışma alanına ekleme
Azure AD Autotask çalışma alanına tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Autotask çalışma alanına eklemeniz gerekir.

**Galeriden Autotask çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Autotask çalışma alanına**seçin **Autotask çalışma alanına** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Autotask çalışma](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Autotask çalışma alanı ile Azure AD çoklu oturum açmayı test "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Autotask çalışma bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Autotask çalışma arasında bir bağlantı ilişkisi kurulması gerekir.

Autotask çalışma alanı'ndaki değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Autotask çalışma alanı ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Autotask çalışma alanına test kullanıcısı oluşturma](#create-an-autotask-workplace-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Autotask çalışma sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Autotask çalışma alanına uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Autotask çalışma alanınız ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Autotask çalışma alanına** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. Uygulamada yapılandırmak istiyorsanız **IDP** modunda başlatılan aşağıdaki adımları gerçekleştirin **Autotask çalışma alanına etki alanı ve URL'leri** bölümü:

    ![Autotask çalışma alanına etki alanı ve URL'leri tek oturum açma bilgilerini IDP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

4. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, onay **Göster Gelişmiş URL ayarları** ve aşağıdaki adımları gerçekleştirin:

    ![Autotask çalışma alanına etki alanı ve URL'leri tek oturum açma bilgilerini SP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Autotask çalışma istemci destek ekibi](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/autotaskworkplace-tutorial/tutorial_general_400.png)

7. Farklı web tarayıcısı penceresinde çalışma alanı yönetici kimlik bilgilerini kullanarak Online'a oturum açın.

    >[!Note]
    >IDP yapılandırırken, bir alt etki alanı belirtilmesi gerekir. Doğru alt etki alanı, çalışma alanına çevrimiçi oturum açma onaylamak için. Oturum açtıktan sonra URL'deki alt etki alanı için not edin.
    >Alt etki alanı "https://" ve ".awp.autotask.net/" arasında bir parçasıdır ve bize, AB, ca veya au olmalıdır.

8. Git **yapılandırma** > **çoklu oturum açma** ve aşağıdaki adımları gerçekleştirin:

    ![Autotask tek oturum açma yapılandırması](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Seçin **XML meta veri dosyası** seçeneğini ve ardından karşıya **meta veri XML** Azure portalından indirdiğiniz.

    b. Tıklatın **etkinleştirmek SSO**.
    
    ![Autotask çoklu oturum açma yapılandırması Onayla](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Seçin **bu bilgilerin doğru olduğundan ve bu IDP güven onaylayın** onay kutusu.

    d. Tıklatın **onaylama**.
     
>[!Note]
>Autotask çalışma alanına yapılandırma ile ilgili Yardım gerekiyorsa, lütfen bkz [bu sayfayı](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) iş yeri hesabınızı Yardım almak için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/autotaskworkplace-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/autotaskworkplace-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/autotaskworkplace-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/autotaskworkplace-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-autotask-workplace-test-user"></a>Autotask çalışma alanına test kullanıcısı oluşturma

Bu bölümde, Autotask içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak [Autotask çalışma alanına destek ekibi](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) Autotask çalışma alanına platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Autotask çalışma alanına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Autotask çalışma alanına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Autotask çalışma alanına**.

    ![Uygulamalar listesinde Autotask çalışma alanına bağlantısı](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Autotask çalışma alanına parçasında tıklattığınızda, otomatik olarak Autotask çalışma alanına uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/autotaskworkplace-tutorial/tutorial_general_203.png

