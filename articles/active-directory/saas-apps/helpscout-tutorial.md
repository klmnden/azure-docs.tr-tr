---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Yardım Scout | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Yardımı Scout arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.openlocfilehash: 28611619340a0cf624c6ab841c6376a97325b2f8
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982190"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Öğretici: Yardım Scout Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Yardım Scout tümleştirmek öğrenin.

Yardım Scout Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yardım Scout erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Yardım Scout için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Yardım Scout ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Yardım Scout çoklu oturum açma etkin abonelik

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Yardım Scout ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-help-scout-from-the-gallery"></a>Galeriden Yardım Scout ekleme
Azure AD Yardım Scout tümleştirilmesi yapılandırmak için Yardım Scout Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Yardım Scout eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Yardım Scout**seçin **Yardım Scout** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Scout Yardım](./media/helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Scout Yardım ile test etme

Tekli çalışmaya oturum için Azure AD Yardım Scout karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Yardım Scout ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yardım Scout oturumları için e-posta adresini kullanır, bu nedenle bağlantı ilişkisi oluşturmak için aynı kullanın **e-posta adresi** olarak **kullanıcı adı** Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma Yardımı Scout ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yardım Scout test kullanıcısı oluşturma](#create-a-help-scout-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Yardım Scout sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Yardımı Scout uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Yardımı Scout ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Yardım Scout** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Üzerinde **Yardım Scout etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Scout etki alanı ve URL'leri tek oturum açma bilgileri Yardım](./media/helpscout-tutorial/tutorial_helpscout_url.png)

    a. **Tanımlayıcı** olan **"İzleyici URI (hizmet sağlayıcısı varlık kimliği)"** Yardım Scout ' başlar `urn:`

    b. **Yanıt URL'si** olan **"Sonrası geri URL (onaylama tüketici hizmeti URL)"** Yardım Scout ' başlar `https://` 

    > [!NOTE] 
    > Bu URL'leri yalnızca tanıtım değerler. Bu değerler gerçek yanıt URL'si ve tanımlayıcı güncelleştirmeniz gerekir. Bu değerleri almak **çoklu oturum açma** sekmesinde öğreticide daha sonra açıklanan kimlik doğrulaması bölümü altında.

4. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, onay **Göster Gelişmiş URL ayarları** ve aşağıdaki adımı gerçekleştirin:

    ![Scout etki alanı ve URL'leri tek oturum açma bilgileri Yardım](./media/helpscout-tutorial/tutorial_helpscout_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://secure.helpscout.net/members/login/`
     
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/helpscout-tutorial/tutorial_general_400.png)


7. Üzerinde **Yardım Scout yapılandırma** 'yi tıklatın **Yardım Scout yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümünde**.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/config.png) 

8. Farklı web tarayıcısı penceresinde Yardım Scout şirket sitenize yönetici olarak oturum açın.

9. Tıklatın oturum açtıktan sonra **"Manage"** seçin ve üstteki menüden **"Şirket"** açılan menüden.

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings1.png) 
 
10. Seçin **"Authentication"** sol taraftaki menüden. 

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings2.png) 

11. Bu SAML ayarları bölümüne alır ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings3.png) 
 
    a. Kopya **sonrası geri URL (onaylama tüketici hizmeti URL'si)** değer ve değeri yapıştırın **yanıt URL'si** Yardım Scout altında Azure portalında kutusunda **etki alanı ve URL'leri** bölüm.
    
    b. Kopya **İzleyici URI'si (hizmet sağlayıcısı varlık kimliği)** değer ve değeri yapıştırın **tanımlayıcısı** Yardım Scout altında Azure portalında kutusunda **etki alanı ve URL'leri** bölümü.

12. İki durumlu **etkinleştirmek SAML** üzerinde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/helpscout-tutorial/settings4.png) 
 
    a. İçinde **çoklu oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    
    b. Tıklatın **sertifikasını karşıya yükle** karşıya yüklemek için **Certificate(Base64)** Azure portalından indirdiğiniz.

    c. Kuruluşunuzun e-posta etki alanlarındaki e.x. - enter `contoso.com` içinde **e-posta etki alanlarını** metin kutusu. Birden çok etki alanı virgülle ayırabilirsiniz. Dilediğiniz zaman bir Yardım Scout kullanıcı veya belirli bir etki alanı girer yönetici [Yardım Scout oturum açma sayfası](https://secure.helpscout.net/members/login/) kendi kimlik bilgileriyle kimlik doğrulaması için kimlik sağlayıcısı için yönlendirilir.

    d. Son olarak, geçiş **zorla SAML oturum açma** yalnızca Yardım Scout bu yöntemle oturum açmasını istiyorsanız. Yine de bunları kendi Yardım Scout kimlik bilgileriyle oturum seçeneğini bırakın başlamayı tercih ederseniz, yükseğe devre dışı bırakabilirsiniz. Bu etkin olsa bile, bir hesap sahibi her zaman hesap parolalarını Yardım Scout oturum açamaz olacaktır.

    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/helpscout-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/helpscout-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/helpscout-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/helpscout-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-help-scout-test-user"></a>Yardım Scout test kullanıcısı oluşturma

Bu bölümün amacı Yardım Scout içinde Britta Simon adlı bir kullanıcı oluşturmaktır. Yardım Scout yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Bir kullanıcı Yardımı Scout içinde zaten yoksa, Yardım Scout erişmeyi denediğinde yeni bir tane oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Yardım Scout erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Yardım Scout Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Yardım Scout**.

    ![Uygulamalar listesinde Yardım Scout bağlantı](./media/helpscout-tutorial/tutorial_helpscout_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Yardım Scout parçasında tıklattığınızda, otomatik olarak yardımcı Scout uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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

