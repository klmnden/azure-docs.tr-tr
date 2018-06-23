---
title: 'Öğretici: Azure Active Directory Tümleştirme ile iPass SmartConnect | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile iPass SmartConnect arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 5ae600142f7e90a7f6185c3a948a4174ce4c7797
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320530"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Öğretici: Azure Active Directory Tümleştirme iPass SmartConnect ile

Bu öğreticide, Azure Active Directory (Azure AD) ile iPass SmartConnect tümleştirmek öğrenin.

İPass SmartConnect Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İPass SmartConnect erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Azure AD hesaplarına sahip iPass SmartConnect (çoklu oturum açma) için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SmartConnect iPass ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik bir iPass SmartConnect çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden iPass SmartConnect ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Galeriden iPass SmartConnect ekleme
Azure AD iPass SmartConnect tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden iPass SmartConnect eklemeniz gerekir.

**Galeriden iPass SmartConnect eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **iPass SmartConnect**seçin **iPass SmartConnect** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde iPass SmartConnect](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SmartConnect "Britta Simon" adlı bir test kullanıcı tabanlı iPass ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen kullanıcı iPass SmartConnect Azure AD'de bir kullanıcı için olduğunu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının iPass ilgili kullanıcı arasında bir bağlantı ilişkisi SmartConnect kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iPass SmartConnect ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir iPass SmartConnect test kullanıcısı oluşturma](#create-an-ipass-smartconnect-test-user)**  - bir Britta Simon karşılık gelen iPass kullanıcı Azure AD gösterimini bağlı SmartConnect içinde olması.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma iPass SmartConnect uygulama yapılandırın.

**Azure AD çoklu oturum açma SmartConnect iPass ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **iPass SmartConnect** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

3. Üzerinde **iPass SmartConnect etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** başlatılan modu, herhangi bir adım gerçekleştirmeniz gerekmez.

    ![oturum açma bilgileri iPass SmartConnect etki alanı ve URL'leri tek](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

4. Göster Gelişmiş URL ayarlarını denetleyin ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modu tarafından başlatılan:

    ![oturum açma bilgileri iPass SmartConnect etki alanı ve URL'leri tek](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Oturum açma URL'si metin kutusuna bir URL yazın: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

5. iPass SmartConnect uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/attribute.png)

6. Tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusu **kullanıcı öznitelikleri** öznitelikleri genişletmek için bölüm. Her biri görüntülenen öznitelikleri - üzerinde aşağıdaki adımları uygulayın

    | Öznitelik Adı | Öznitelik Değeri | Namespace değeri|
    | ---------------| --------------- |----------------|
    | firstName | User.givenName |   |
    | Soyadı | User.surname | |
    | e-posta | User.userPrincipalName | |
    | kullanıcı adı | User.userPrincipalName | |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı değeri bu satır için boş bırakın.

    e. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

9. Çoklu oturum açma yapılandırmak için **iPass SmartConnect** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve **etki alanı adı** için [iPass SmartConnect Takım Destek](mailto:help@ipass.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-ipass-smartconnect-test-user"></a>Bir iPass SmartConnect test kullanıcısı oluşturma

Bu bölümde, Britta Simon iPass SmartConnect adlı bir kullanıcı oluşturun. Çalışmak [iPass SmartConnect destek ekibi](mailto:help@ipass.com) kullanıcıları veya iPass SmartConnect Platform güvenilir listesinde olması için gerekli olan etki alanı eklemek için. Etki alanı ekibi tarafından eklediyseniz, kullanıcıların otomatik olarak iPass SmartConnect platform için sağlanan. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta iPass SmartConnect erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**İPass SmartConnect Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **iPass SmartConnect**.

    ![Uygulamaları listeden SmartConnect bağlantıyı iPass](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

**SP tarafından başlatılan akışında uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Windows iPass SmartConnect istemci indirme [burada](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![Uygulamaları listeden SmartConnect bağlantıyı iPass](./media/ipasssmartconnect-tutorial/testing3.png)

b. İstemci ve başlatma yükleyin.

c. Tıklayın **başlama**.

![Uygulamaları listeden SmartConnect bağlantıyı iPass](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Etki alanı Azure kullanıcı adı girin. Tıklayın **devam**. Bu Azure oturum açma sayfasına yönlendirilir ve

![Uygulamaları listeden SmartConnect bağlantıyı iPass](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Başarılı kimlik doğrulamasından sonra istemci etkinleştirme başlatılır. İstemci devreye.

**IDP başlatılan akışında uygulamayı test etmek için aşağıdaki adımları gerçekleştirin:**

a. Oturum açma [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Üzerinde iPass SmartConnect uygulamaya tıklayın.

c. SSA sayfa başlatır, tıklayın **Windows için karşıdan uygulama** iPass SmartConnect istemci yüklemek için.

![Uygulamaları listeden SmartConnect bağlantıyı iPass](./media/ipasssmartconnect-tutorial/testing4.png)

d. Yükleme, istemci üzerinde ilk kez başlatıldığında otomatik olarak ayarlanır sonra hüküm ve koşulları kabul sonra etkinleştirme işlemini başlatır.

e. Etkinleştirme başlamazsa, etkinleştirmeyi başlatmak için SSA sayfasında Etkinleştir düğmesini tıklatın.

f. İstemci devreye.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

