---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Evernote | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Evernote arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 737382013a390dab329d2758a5296f084e38b39a
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35989197"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Öğretici: Azure Active Directory Tümleştirme Evernote ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Evernote tümleştirmek öğrenin.

Evernote Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Evernote erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Evernote (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Evernote ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Evernote çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Evernote ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-evernote-from-the-gallery"></a>Galeriden Evernote ekleme
Azure AD Evernote tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Evernote eklemeniz gerekir.

**Galeriden Evernote eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Evernote**seçin **Evernote** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Evernote](./media/evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Evernote sınayın.

Tekli çalışmaya oturum için Azure AD Evernote karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Evernote ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Evernote içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Evernote ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Evernote test kullanıcısı oluşturma](#create-an-evernote-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Evernote sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Evernote uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Evernote yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Evernote** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/evernote-tutorial/tutorial_evernote_samlbase.png)

3. Üzerinde **Evernote etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Evernote etki alanı ve URL'leri tek oturum açma bilgileri](./media/evernote-tutorial/tutorial_evernote_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın: `https://www.evernote.com/saml2`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Evernote etki alanı ve URL'leri tek oturum açma bilgileri](./media/evernote-tutorial/tutorial_evernote_url1.png)

    İçinde **URL üzerinde oturum** metin kutusuna, URL'yi yazın: `https://www.evernote.com/Login.action`   

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/evernote-tutorial/tutorial_evernote_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/evernote-tutorial/tutorial_general_400.png)

7. Üzerinde **Evernote yapılandırma** 'yi tıklatın **yapılandırma Evernote** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Evernote yapılandırma](./media/evernote-tutorial/tutorial_evernote_configure.png) 

8. Farklı web tarayıcısı penceresinde Evernote şirket sitenize yönetici olarak oturum açın.

9. Git **'Yönetici Konsolu'**

    ![Yönetim Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

10. Gelen **'Yönetici Konsolu'** gidin **'Security'** seçip **' çoklu oturum açma '**

    ![SSO ayarı](./media/evernote-tutorial/tutorial_evernote_sso.png)

11. Aşağıdaki değerleri yapılandırın:

    ![Sertifika ayarları](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **SSO etkinleştir:** SSO varsayılan olarak etkindir (tıklatın **devre dışı çoklu oturum açma** SSO gereksinimini kaldırmak için)

    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **SAML HTTP istek URL'si** metin kutusu.

    c. İndirilen sertifika Azure AD'den bir Not Defteri'nde açın ve "Sertifika başlayın" ve "Son SERTİFİKAYI" dahil olmak üzere içerik kopyalayıp yapıştırın **X.509 sertifikası** metin kutusu. 

    d.Click **Değişiklikleri Kaydet**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/evernote-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/evernote-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/evernote-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/evernote-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-evernote-test-user"></a>Bir Evernote test kullanıcısı oluşturma

Azure AD kullanıcıların Evernote oturum etkinleştirmek için bunların Evernote sağlanmalıdır.  
Evernote söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Evernote şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **'Yönetici Konsolu'**.

    ![Yönetim Konsolu](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Gelen **'Yönetici Konsolu'** gidin **'kullanıcıları eklemek'**.

    ![Ekleme testUser](./media/evernote-tutorial/create_aaduser_0001.png)

4. **Ekip üyeleri ekleme** içinde **e-posta** metin kutusu, kullanıcı hesabının e-posta adresini yazın ve'ı tıklatın **davet edin.**

    ![Ekleme testUser](./media/evernote-tutorial/create_aaduser_0002.png)
    
5. Davet gönderildikten sonra Azure Active Directory hesap sahibi daveti kabul etmek için e-posta alacaksınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Evernote için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Evernote için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Evernote**.

    ![Uygulamalar listesinde Evernote bağlantı](./media/evernote-tutorial/tutorial_evernote_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Evernote parçasında tıklattığınızda, Evernote uygulamanıza açan. Hesap ancak sonra gerekirse, kişisel hesabıyla oturum kuruluş olarak günlüğü. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/evernote-tutorial/tutorial_general_01.png
[2]: ./media/evernote-tutorial/tutorial_general_02.png
[3]: ./media/evernote-tutorial/tutorial_general_03.png
[4]: ./media/evernote-tutorial/tutorial_general_04.png

[100]: ./media/evernote-tutorial/tutorial_general_100.png

[200]: ./media/evernote-tutorial/tutorial_general_200.png
[201]: ./media/evernote-tutorial/tutorial_general_201.png
[202]: ./media/evernote-tutorial/tutorial_general_202.png
[203]: ./media/evernote-tutorial/tutorial_general_203.png

