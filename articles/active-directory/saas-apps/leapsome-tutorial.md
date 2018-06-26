---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Leapsome | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Leapsome arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: b23a93db7912aa25b420157241c41533f4f48a27
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938503"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Öğretici: Azure Active Directory Tümleştirme Leapsome ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Leapsome tümleştirmek öğrenin.

Leapsome Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Leapsome erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Leapsome (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Leapsome ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Leapsome çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Leapsome ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-leapsome-from-the-gallery"></a>Galeriden Leapsome ekleme
Azure AD Leapsome tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Leapsome eklemeniz gerekir.

**Galeriden Leapsome eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Leapsome**seçin **Leapsome** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Leapsome](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Leapsome sınayın.

Tekli çalışmaya oturum için Azure AD Leapsome karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Leapsome ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Leapsome ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Leapsome test kullanıcısı oluşturma](#create-a-leapsome-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Leapsome sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Leapsome uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Leapsome yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Leapsome** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

3. Üzerinde **Leapsome etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Leapsome etki alanı ve URL'leri tek oturum açma bilgileri](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://www.leapsome.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Leapsome etki alanı ve URL'leri tek oturum açma bilgileri](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > Önceki yanıt URL'si ve oturum açma URL değeri gerçek değeri değil. Bunlar gerçek değerlerle daha sonra öğreticide açıklanan güncelleştirir.

5. Leapsome uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Ad alanı |
    | ---------------| --------------- | --------- |   
    | firstName | User.givenName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Soyadı | User.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | başlık | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | resmi | Çalışanın resim URL'si | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > Resim özniteliğinin değerini gerçek değil. Bu değer gerçek resim URL'si ile güncelleştirin. Bu değer kişi almak için [Leapsome istemci destek ekibi](mailto:support@leapsome.com).
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, ilgili satır için ad alanı URI yazın.
    
    e. Tıklatın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/leapsome-tutorial/tutorial_general_400.png)
    
9. Üzerinde **Leapsome yapılandırma** 'yi tıklatın **yapılandırma Leapsome** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Leapsome yapılandırma](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

10. Farklı web tarayıcısı penceresinde Leapsome için bir güvenlik yöneticisi olarak oturum açın.

11. Sağ üstte ayarları logosu tıklayın ve ardından **yönetici ayarları**. 

    ![Leapsome ayarlama](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

12. Sol menü çubuğunda **çoklu oturum açma (SSO)** ve **SAML tabanlı çoklu oturum açma (SSO)** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Seçin **etkinleştirmek SAML tabanlı çoklu oturum açma**.

    b. Kopya **oturum açma URL'si (oturum açma başlatmak için kullanıcılarınızın burada noktası)** değer ve yapıştırın **oturum açma URL'si** metin kutusuna **Leapsome etki alanı ve URL'leri** Azure Portal'daki bölüm.

    c. Kopya **yanıt URL'si (kimlik sağlayıcınızı yanıttan recieves)** değer ve yapıştırın **yanıt URL'si** metin kutusuna **Leapsome etki alanı ve URL'leri** Azure Portal'daki bölüm.

    d. İçinde **SSO oturum açma URL'si (Kimlik sağlayıcısı tarafından sağlanan)** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    e. --Sertifika başlar ve son sertifika--yorumlar olmadan Azure portalından indirmiş ve yapıştırın Sertifikayı kopyalamak **(Kimlik sağlayıcısı tarafından sağlanan) sertifika** metin kutusu.

    f. Tıklatın **güncelleştirme SSO ayarlarını**.
    
### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/leapsome-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/leapsome-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/leapsome-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/leapsome-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-leapsome-test-user"></a>Leapsome test kullanıcısı oluşturma

Bu bölümde, Leapsome içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Leapsome istemci destek ekibi](mailto:support@leapsome.com) kullanıcıları veya Leapsome Platform güvenilir listesinde olması gereken etki alanı eklemek için. Etki alanı ekibi tarafından eklediyseniz, kullanıcıların otomatik olarak Leapsome platform için sağlanan. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Leapsome için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Leapsome için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Leapsome**.

    ![Uygulamalar listesinde Leapsome bağlantı](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Leapsome parçasında tıklattığınızda, otomatik olarak Leapsome uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

