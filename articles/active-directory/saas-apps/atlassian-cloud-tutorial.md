---
title: 'Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Atlassian bulut arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: d57d570aab34d029ec84279a8644b5f0beb243d8
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971890"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Atlassian bulut tümleştirme öğrenin.

Atlassian bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Atlassian bulut erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanmış kullanıcılarınıza etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına olan Atlassian buluta.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Atlassian bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD abonelik.
- Güvenlik onaylama işlemi biçimlendirme dili (SAML) çoklu oturum açma Atlassian bulut ürünleri için etkinleştirmek için Identity Manager ayarlamanız gerekir. Daha fazla bilgi edinmek [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bir üretim ortamında kullanmanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Öğreticide verilen senaryoda iki ana yapı taşlarını oluşur:

* Galeriden Atlassian bulut ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-atlassian-cloud-from-the-gallery"></a>Galeriden Atlassian bulut ekleme
Azure AD ile tümleştirme Atlassian bulutun yapılandırmak için Atlassian bulut aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **Atlassian bulut**, sonuçlar listesinde **Atlassian bulut**ve ardından **Ekle**.

    ![Sonuçlar listesinde Atlassian bulut](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Atlassian bulut, adlandırılmış bir test kullanıcı tabanlı ile test etme *Britta Simon*.

Tekli çalışmaya oturum için Azure AD'de Atlassian bulut kullanıcı ve kendisine karşılık gelen tanımlamak Azure AD gerekiyor. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı arasındaki bağlantıyı ilişki Atlassian bulutta oluşturmanız gerekir.

Bağlantı ilişkisi oluşturmak için Atlassian bulut olarak atamak *kullanıcıadı* Azure AD ile atanan aynı değere *kullanıcı adı*.

Yapılandırma ve Azure AD çoklu oturum açma Atlassian bulut ile test etmek için aşağıdaki bölümlerdeki yapı taşları tamamlamanız gerekir.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Atlassian bulut uygulamanızda yapılandırın.

Azure AD çoklu oturum açma Atlassian bulut ile yapılandırmak için aşağıdakileri yapın:

1. Azure portalında içinde **Atlassian bulut** uygulama tümleştirmesi bölmesinde, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** penceresi, **tek oturum açma modu** kutusunda **SAML tabanlı oturum açma**.

    ![Çoklu oturum açma penceresi](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Uygulama IDP başlatılan modda altında yapılandırmak için **Atlassian bulut etki alanı ve URL'leri**, aşağıdakileri yapın:

    ![Atlassian bulut etki alanı ve oturum açma URL'leri tek bilgi](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. İçinde **tanımlayıcısı** kutusuna **`https://auth.atlassian.com/saml/<unique ID>`**.
    
    b. İçinde **yanıt URL'si** kutusuna **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`**.

    c. İçinde **geçiş durumunu** kutusunda, aşağıdaki sözdizimini kullanarak URL'yi yazın: **`https://<instancename>.atlassian.net`**.

4. SP tarafından başlatılan modunda uygulama yapılandırmak için seçin **Göster Gelişmiş URL ayarları** , daha sonra **URL üzerinde oturum** kutusunda, aşağıdaki sözdizimini kullanarak URL'yi yazın: **`https://<instancename>.atlassian.net`** .

    ![Atlassian bulut etki alanı ve oturum açma URL'leri tek bilgi](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE]
    > Yukarıdaki değerleri gerçek değildir. Bunları, gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si değerlerini güncelleştirin. Gerçek değerlerin Atlassian bulut SAML Yapılandırma ekranından alabilirsiniz. Biz, daha sonra öğreticide değerleri açıklanmaktadır.

5. Altında **SAML imzalama sertifikası**seçin **Certificate(Base64)** ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png)

6. SAML belirteci öznitelikleri yapılandırmanızı özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar bulmak Atlassian bulut uygulamanızı bekliyor. 

    Varsayılan olarak, **kullanıcı tanımlayıcısı** değeri user.userprincipalname için eşlenmedi. Bu değer için User.Mail eşleştirmek için değiştirin. Diğer uygun değeri, kuruluşunuzun Kurulum göre de seçebilirsiniz, ancak bir e-posta taleplerini çoğunda çalışması gerekir.

    ![Sertifika indirme bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png)

7. **Kaydet**’i seçin.

    ![Yapılandırma çoklu oturum açma düğmesi Kaydet](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

8. Açmak için **yapılandırma oturum açma** penceresi, **Atlassian bulut Yapılandırması** bölümünde, select **Atlassian bulut yapılandırma**.

9. İçinde **hızlı başvuru** bölümünde, kopyalama **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmet URL'si**.

    ![Atlassian bulut yapılandırması](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png)

10. Uygulamanız için yapılandırılmış SSO almak için Atlassian portal yönetici kimlik bilgileriyle oturum açın.

11. Çoklu oturum açma yapılandırmak için geçmeden önce etki alanınızı doğrulamanız gerekir. Daha fazla bilgi için bkz: [Atlassian etki alanı doğrulama](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) belge.

12. Sol bölmede seçin **SAML çoklu oturum açma**. Zaten yapmadıysanız, Atlassian Identity Manager abone olun.

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

13. İçinde **eklemek SAML Yapılandırması** penceresinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** kutusunda, Azure portalından kopyalandığından SAML varlık kimliği yapıştırın.

    b. İçinde **kimlik sağlayıcısı SSO URL** kutusunda, Azure portalından kopyalandığından SAML çoklu oturum açma hizmeti URL'sini yapıştırın.

    c. Açık bir .txt dosyasında Azure Portalı'ndan indirilen Sertifikayı kopyalamak değeri (olmadan *başlamak sertifika* ve *son sertifikayı* satırları) ve ardından yapıştırın **ortak X509 Sertifika** kutusu.
    
    d. Seçin **yapılandırmasını kaydetmek**.
     
14. Doğru URL'leri ayarladığınızdan emin olmak için aşağıdakileri yaparak Azure AD ayarları güncelleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. SAML penceresindeki kopyalamak **SP kimlik kimliği** ve daha sonra Azure portalında, Atlassian bulut altında **etki alanı ve URL'leri**, yapıştırın **tanımlayıcısı** kutusu.
    
    b. SAML penceresindeki kopyalamak **SP onaylama tüketici hizmeti URL'si** ve daha sonra Azure portalında, Atlassian bulut altında **etki alanı ve URL'leri**, yapıştırın **yanıt URL'si** kutusu. Oturum açma URL'si Atlassian bulut Kiracı URL'sidir.

    > [!NOTE]
    > Güncelleştirdikten sonra varolan bir müşteri olup olmadığınızı **SP kimlik kimliği** ve **SP onaylama tüketici hizmeti URL'si** Azure portalında değerleri seçin **Evet, yapılandırmayıgüncelleştirme**. Yeni bir müşteri değilseniz, bu adımı atlayabilirsiniz.
    
15. Azure portalında seçin **kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

   ![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/atlassian-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/atlassian-cloud-tutorial/create_aaduser_02.png)

3. İçinde **tüm kullanıcılar** penceresinde, seçin **Ekle**.

    ![Ekle düğmesi](./media/atlassian-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/atlassian-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="create-an-atlassian-cloud-test-user"></a>Bir Atlassian bulut test kullanıcısı oluşturma

Azure AD Atlassian buluta oturum açmalarını etkinleştirmek için aşağıdakileri yaparak Atlassian bulutta el ile kullanıcı hesapları sağlama:

1. İçinde **Yönetim** bölmesinde, **kullanıcılar**.

    ![Atlassian bulut kullanıcıları bağlantısı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Bir kullanıcı Atlassian bulutta oluşturmak için seçin **davet kullanıcı**.

    ![Bir Atlassian bulut kullanıcı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. İçinde **e-posta adresi** kutusuna kullanıcının e-posta adresi girin ve ardından uygulama erişimi atayın.

    ![Bir Atlassian bulut kullanıcı oluşturun](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Kullanıcıya bir e-posta göndermek için seçin **kullanıcıları davet**. Kullanıcıya bir e-posta davet gönderilir ve daveti kabul ettikten sonra kullanıcı sistemde etkindir.

>[!NOTE]
>Ayrıca toplu-seçerek kullanıcıları oluşturun **Toplu oluşturma** düğmesini **kullanıcılar** bölümü.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta Atlassian buluta erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200]

1. Azure portalında açmak **uygulamaları** görüntülemek, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. İçinde **uygulamaları** listesinde **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantı](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** penceresinde, seçin **seçin**.

7. İçinde **eklemek atama** penceresinde, seçin **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Atlassian bulut** döşeme erişim panelinde oturumunuz otomatik olarak Atlassian bulut uygulamanız.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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
