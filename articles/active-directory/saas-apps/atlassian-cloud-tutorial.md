---
title: 'Öğretici: Azure Active Directory Atlassian bulut ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Atlassian bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: f0ad879bb084a8d3a50a0934557eae64621c0160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054271"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Öğretici: Azure Active Directory Atlassian bulut ile tümleştirme

Bu öğreticide, Atlassian bulut Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Atlassian bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Atlassian Bulutuna erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanmış kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına Atlassian buluta.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Atlassian Bulutla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz.
- Güvenlik onaylama işlemi biçimlendirme dili (SAML) çoklu oturum açma için Atlassian bulut ürünleri etkinleştirmek için Identity Manager'ı ayarlamak gerekir. Daha fazla bilgi edinin [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bir üretim ortamında kullanmanızı öneririz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

* Atlassian bulut galeri ekleme
* Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-atlassian-cloud-from-the-gallery"></a>Atlassian bulut Galeriden Ekle
Azure AD ile Atlassian bulut tümleştirmesini yapılandırmak için Atlassian bulut aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **Atlassian bulut**, sonuç listesinden **Atlassian bulut**ve ardından **Ekle**.

    ![Sonuç listesinde Atlassian bulut](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Atlassian adlı bir test kullanıcı tabanlı bulutla test *Britta Simon*.

Tek iş için oturum açma için Azure AD'de Atlassian bulut kullanıcısı ve kendisine karşılık gelen tanımlamak Azure AD'nin gerekiyor. Diğer bir deyişle, Atlassian buluttaki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Bağlantı kurmak için Atlassian bulut olarak atama *kullanıcıadı* Azure AD'ye atanan değerin *kullanıcı adı*.

Yapılandırma ve Azure AD çoklu oturum açma Atlassian Bulutu ile test etmek için aşağıdaki bölümlerde yapı taşlarını tamamlanması gerekir.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Atlassian bulut uygulamanızı yapılandırın.

Azure AD çoklu oturum açma Atlassian Bulutla yapılandırmak için aşağıdakileri yapın:

1. Azure portalında, **Atlassian bulut** uygulama tümleştirme bölmesinde **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** penceresi içinde **çoklu oturum açma modunu** kutusunda **SAML tabanlı oturum açma**.

    ![Çoklu oturum açma penceresi](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. IDP tarafından başlatılan modunda altında uygulamayı yapılandırmak için **Atlassian bulut etki alanı ve URL'ler**, aşağıdakileri yapın:

    ![Oturum açma bilgileri çoklu Atlassian bulut etki alanı ve URL'ler](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. İçinde **tanımlayıcı** kutusuna **`https://auth.atlassian.com/saml/<unique ID>`**.
    
    b. İçinde **yanıt URL'si** kutusuna **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`**.

    c. İçinde **geçiş durumu** kutusunda, aşağıdaki sözdizimini kullanarak bir URL yazın: **`https://<instancename>.atlassian.net`**.

4. SP tarafından başlatılan modunda uygulamayı yapılandırmak için seçin **Gelişmiş URL ayarlarını göster** ve daha sonra **oturum açma URL'si** kutusunda, aşağıdaki sözdizimini kullanarak bir URL yazın: **`https://<instancename>.atlassian.net`** .

    ![Oturum açma bilgileri çoklu Atlassian bulut etki alanı ve URL'ler](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE]
    > Yukarıdaki değerleri gerçek değildir. Bunları, gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si değeri ile güncelleştirin. Atlassian bulut SAML yapılandırma ekranında, gerçek değerleri alabilirsiniz. Biz, öğreticinin ilerleyen bölümlerinde değerleri açıklanmaktadır.

5. Altında **SAML imzalama sertifikası**seçin **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png)

6. Özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bulmak Atlassian bulut uygulamanızın bekliyor. 

    Varsayılan olarak, **kullanıcı tanımlayıcısı** değeri için user.userprincipalname eşlenir. İçin User.Mail eşlemek için bu değeri değiştirin. Diğer uygun değere göre kuruluşunuzun Kurulum de seçebilirsiniz, ancak bir e-posta taleplerini çoğunda çalışması gerekir.

    ![Sertifika indirme bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png)

7. **Kaydet**’i seçin.

    ![Yapılandırma çoklu oturum açma düğmesi Kaydet](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

8. Açmak için **yapılandırma oturum açma** penceresi içinde **Atlassian bulut Yapılandırması** bölümünden **Atlassian bulut yapılandırma**.

9. İçinde **hızlı başvuru** bölümünde, kopya **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL'si**.

    ![Atlassian bulut yapılandırması](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png)

10. Uygulamanız için yapılandırılmış SSO almak için Atlassian portalına yönetici kimlik bilgileriyle oturum açın.

11. Çoklu oturum açmayı yapılandırmak için geçmeden önce etki alanınızı doğrulayın gerekir. Daha fazla bilgi için [Atlassian etki alanı doğrulaması](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belge.

12. Sol bölmede seçin **SAML çoklu oturum açma**. Zaten yapmadıysanız, Atlassian Identity Manager'a abone olun.

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

13. İçinde **ekleme SAML yapılandırma** penceresinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** kutusuna, Azure Portalı'ndan kopyaladığınız SAML varlık kimliği yapıştırın.

    b. İçinde **kimlik sağlayıcısı SSO URL** kutusunda, SAML çoklu oturum açma hizmeti Azure portaldan kopyaladığınız URL'yi yapıştırın.

    c. Açık bir .txt dosyasında Azure portalından indirilen sertifikayı kopyalamanız değeri (olmadan *başlamak sertifika* ve *End Certıfıcate* satırları), ardından yapıştırın **genel X509 Sertifika** kutusu.
    
    d. Seçin **yapılandırmayı kaydetmek**.
     
14. Doğru URL'leri ayarladığınızdan emin olmak için aşağıdakileri yaparak Azure AD ayarlarını güncelleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. SAML penceresindeki kopyalamak **SP kimliği** ve ardından Azure portalında, Atlassian Bulut'un altında **etki alanı ve URL'ler**, yapıştırın **tanımlayıcı** kutusu.
    
    b. SAML penceresindeki kopyalamak **SP onay belgesi tüketici hizmeti URL'si** ve ardından Azure portalında, Atlassian Bulut'un altında **etki alanı ve URL'ler**, yapıştırın **yanıt URL'si** kutusu. Oturum açma URL'si Atlassian bulut Kiracı URL'sidir.

    > [!NOTE]
    > Güncelleştirdikten sonra varolan bir müşteri olmadığınızı **SP kimliği** ve **SP onay belgesi tüketici hizmeti URL'si** Azure portalında, değerler **Evet, yapılandırmayıgüncelleştirme**. Yeni bir müşteri iseniz, bu adımı atlayabilirsiniz.
    
15. Azure portalında **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

   ![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/atlassian-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/atlassian-cloud-tutorial/create_aaduser_02.png)

3. İçinde **tüm kullanıcılar** penceresinde **Ekle**.

    ![Ekle düğmesi](./media/atlassian-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/atlassian-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="create-an-atlassian-cloud-test-user"></a>Bir Atlassian bulut test kullanıcısı oluşturma

Atlassian buluta oturum açın, aşağıdakileri yaparak Atlassian bulutta el ile kullanıcı hesapları sağlamak Azure AD kullanıcılarının etkinleştirmek için:

1. İçinde **Yönetim** bölmesinde **kullanıcılar**.

    ![Atlassian bulut kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Bir kullanıcı Atlassian bulutta oluşturmak için Seç **davet kullanıcı**.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. İçinde **e-posta adresi** kutusuna kullanıcının e-posta adresini girin ve ardından uygulama erişimi atayın.

    ![Atlassian bulut kullanıcısı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Kullanıcıya bir e-posta davetiyesi göndermek için seçin **kullanıcıları davet**. Kullanıcıya bir davet e-postası gönderilir ve kullanıcı daveti kabul ettikten sonra sistemde etkin.

>[!NOTE]
>Ayrıca toplu-seçerek kullanıcılar oluşturma **Toplu oluşturma** düğmesine **kullanıcılar** bölümü.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta Simon, Atlassian buluta erişim vererek Azure çoklu oturum açmayı kullanmak için etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200]

1. Azure portalında açın **uygulamaları** görüntüleme, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. İçinde **uygulamaları** listesinden **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi içinde **kullanıcılar** listesinden **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** penceresinde **seçin**.

7. İçinde **atama Ekle** penceresinde **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Atlassian bulut** kutucuğuna erişim panelinde Atlassian bulut uygulamanız için otomatik olarak imzalanıp imzalanmayacağını.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/atlassian-cloud-tutorial/tutorial_general_203.png
