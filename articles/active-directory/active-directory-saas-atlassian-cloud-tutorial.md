---
title: "Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory Atlassian bulut arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
<<<<<<< HEAD
manager: femila
=======
manager: mtillman
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
<<<<<<< HEAD
ms.openlocfilehash: 8a68f68cd08587ae9377c92e2154e3b8024b227c
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
=======
ms.openlocfilehash: db9e9c7ae8380612bac9d0aeaaaf6df78cba523f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Atlassian bulut tümleştirme öğrenin.

Atlassian bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Atlassian bulut erişimi olan Azure AD'de kontrol edebilirsiniz.
<<<<<<< HEAD
- Azure AD hesaplarına otomatik olarak Atlassian buluta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.
=======
- Otomatik olarak imzalanmış kullanıcılarınıza etkinleştirebilirsiniz (çoklu oturum açma) Azure AD hesaplarına olan Atlassian buluta.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Atlassian bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

<<<<<<< HEAD
- Bir Azure AD aboneliği
- SAML çoklu oturum açma Atlassian bulut ürünleri için etkinleştirmek için Identity Manager ayarlamanız gerekir. Daha fazla bilgi edinmek [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)
=======
- Bir Azure AD abonelik.
- Güvenlik onaylama işlemi biçimlendirme dili (SAML) çoklu oturum açma Atlassian bulut ürünleri için etkinleştirmek için Identity Manager ayarlamanız gerekir. Daha fazla bilgi edinmek [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bir üretim ortamında kullanmanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Öğreticide verilen senaryoda iki ana yapı taşlarını oluşur:

* Galeriden Atlassian bulut ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-atlassian-cloud-from-the-gallery"></a>Galeriden Atlassian bulut ekleme
Azure AD ile tümleştirme Atlassian bulutun yapılandırmak için Atlassian bulut aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** düğmesi. 

    ![Azure Active Directory düğmesi][1]

<<<<<<< HEAD
    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Atlassian bulut**seçin **Atlassian bulut** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Atlassian bulut](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Atlassian bulut ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.
=======
2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **Atlassian bulut**, sonuçlar listesinde **Atlassian bulut**ve ardından **Ekle**.

    ![Sonuçlar listesinde Atlassian bulut](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Atlassian bulut, adlandırılmış bir test kullanıcı tabanlı ile test etme *Britta Simon*.

<<<<<<< HEAD
Değeri Atlassian buluta atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.
=======
Tekli çalışmaya oturum için Azure AD'de Atlassian bulut kullanıcı ve kendisine karşılık gelen tanımlamak Azure AD gerekiyor. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı arasındaki bağlantıyı ilişki Atlassian bulutta oluşturmanız gerekir.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

Bağlantı ilişkisi oluşturmak için Atlassian bulut olarak atamak *kullanıcıadı* Azure AD ile atanan aynı değere *kullanıcı adı*.

<<<<<<< HEAD
1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Atlassian bulut test kullanıcısı oluşturma](#create-an-atlassian-cloud-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Atlassian bulut sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.
=======
Yapılandırma ve Azure AD çoklu oturum açma Atlassian bulut ile test etmek için aşağıdaki bölümlerdeki yapı taşları tamamlamanız gerekir.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Atlassian bulut uygulamanızda yapılandırın.

Azure AD çoklu oturum açma Atlassian bulut ile yapılandırmak için aşağıdakileri yapın:

1. Azure portalında içinde **Atlassian bulut** uygulama tümleştirmesi bölmesinde, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** penceresi, **tek oturum açma modu** kutusunda **SAML tabanlı oturum açma**.
 
<<<<<<< HEAD
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)
=======
    ![Çoklu oturum açma penceresi](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Uygulama IDP başlatılan modda altında yapılandırmak için **Atlassian bulut etki alanı ve URL'leri**, aşağıdakileri yapın:

    ![Atlassian bulut etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. İçinde **tanımlayıcısı** kutusuna  **`https://auth.atlassian.com/saml/<unique ID>`** .
    
    b. İçinde **yanıt URL'si** kutusuna  **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** .
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    c. İçinde **geçiş durumunu** kutusunda, aşağıdaki sözdizimini kullanarak URL'yi yazın:  **`https://<instancename>.atlassian.net`** .

<<<<<<< HEAD
    ![Atlassian bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://auth.atlassian.com/saml/<unique ID>`
    
    b. İçinde **yanıt URL'si** metin kutusuna, URL'yi yazın:`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. İçinde **geçiş durumunu** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<instancename>.atlassian.net`
=======
4. SP tarafından başlatılan modunda uygulama yapılandırmak için seçin **Göster Gelişmiş URL ayarları** , daha sonra **URL üzerinde oturum** kutusunda, aşağıdaki sözdizimini kullanarak URL'yi yazın:  **`https://<instancename>.atlassian.net`**  .

    ![Atlassian bulut etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları, gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si değerlerini güncelleştirin. Gerçek değerlerin Atlassian bulut SAML Yapılandırma ekranından alabilirsiniz. Biz, daha sonra öğreticide değerleri açıklanmaktadır.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

5. Altında **SAML imzalama sertifikası**seçin **Certificate(Base64)**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

<<<<<<< HEAD
    ![Atlassian bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    İçinde **oturum açma URL'si metin kutusuna**, şu biçimi kullanarak bir URL yazın:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Öğreticinin sonraki adımlarda anlatılan Atlassian bulut SAML yapılandırma ekranında bu değerleri alır.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Atlassian bulut uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanızı özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde bekliyor. Varsayılan olarak kullanıcı tanımlayıcısı user.userprincipalname ile eşleştirilir. Lütfen bu ile eşleştirmek için değiştirin **user.mail**. Kuruluş kurulumunuzu uygun herhangi bir değer de seçebilirsiniz, ancak çoğu durumda, e-posta çalışması gerekir.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Üzerinde **Atlassian bulut Yapılandırması** 'yi tıklatın **Atlassian bulut yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Atlassian bulut yapılandırması](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

9. Yönetici hakları kullanarak Atlassian portalı oturum açma, uygulamanız için yapılandırılmış SSO almak için.

10. Gidin **Atlassian Site Yönetimi** > **kuruluşlar ve güvenlik**. Henüz yapmadıysanız oluşturabilir ve kuruluşunuzun adını. Sol gezinti bölmesinde, ardından **etki alanları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

11. Etki alanınızı - doğrulamak istediğiniz yolu seçin **DNS** veya **HTTPS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

12. DNS doğrulamak üzere seçtiğiniz **DNS** sekmesi **etki alanları** sayfasında ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Tıklatın **kopyalama** TXT kaydı değeri kopyalanacak.

    b. Yeni bir kayıt eklemek için ayarları sayfası, DNS'den Bul.

    c. Yeni bir kayıt ekleme seçeneğini seçin ve kopyaladığınız değeri yapıştırın **etki alanları** sayfasına **değeri** alan. DNS sunucunuzun olarak da başvurabilir **yanıt** veya **açıklama**.

    d. DNS kaydı ayrıca aşağıdaki alanları içerebilir:
    
    * **Kayıt türü**: girin **TXT**
    * **Ana/ad/diğer**: (@ veya boş) varsayılan adı bırakın
    * **Yaşam süresi (TTL)**: girin **86400**
    
    e.  Kaydı kaydedin.

13. Geri dönüp **etki alanları sayfasına** kuruluş yönetimi ve tıklatın **etki alanını doğrula** düğmesi. Açılır ve tıklatın, etki alanı adınızı girin **etki alanını doğrula** düğmesi.
=======
    ![Sertifika indirme bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. SAML belirteci öznitelikleri yapılandırmanızı özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar bulmak Atlassian bulut uygulamanızı bekliyor. 

    Varsayılan olarak, **kullanıcı tanımlayıcısı** değeri user.userprincipalname için eşlenmedi. Bu değer için User.Mail eşleştirmek için değiştirin. Diğer uygun değeri, kuruluşunuzun Kurulum göre de seçebilirsiniz, ancak bir e-posta taleplerini çoğunda çalışması gerekir.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. **Kaydet**’i seçin.

    ![Yapılandırma çoklu oturum açma düğmesi Kaydet](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Açmak için **yapılandırma oturum açma** penceresi, **Atlassian bulut Yapılandırması** bölümünde, select **Atlassian bulut yapılandırma**. 

9. İçinde **hızlı başvuru** bölümünde, kopyalama **SAML varlık kimliği** ve **SAML çoklu oturum açma hizmet URL'si**. 

    ![Atlassian bulut yapılandırması](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

10. Uygulamanız için yapılandırılmış SSO almak için Atlassian portal yönetici kimlik bilgileriyle oturum açın.

11. Git **Atlassian Site Yönetimi** > **kuruluşlar ve güvenlik**. Zaten yapmadıysanız, oluşturma ve kuruluşunuzun adını ve ardından, sol bölmede, **etki alanları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

12. Etki alanınızı doğrulamak istediğiniz yolu seçin: **DNS** veya **HTTPS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

13. DNS doğrulama içinde **etki alanları** penceresinde, seçin **DNS** sekmesini ve ardından aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Değeri, metin kaydı (TXT) kopyalamak için seçin **kopya**.

    b. Bir kayıt eklemek için DNS ayarları sayfasına gidin.

    c. Yeni bir kayıt ekleme seçeneğini seçin ve ardından, kopyaladığınız değeri yapıştırın **etki alanları** penceresine **değeri** alan. DNS kaydı olarak da başvurabilir **yanıt** veya **açıklama**.

    d. DNS kaydı ayrıca aşağıdaki alanları içerebilir:
    
    * İçinde **kayıt türü** kutusuna **TXT**.
    * İçinde **konak/ad/diğer** kutusunda, varsayılan değer (@ veya boş) bırakın.
    * İçinde **yaşam süresi (TTL)** kutusuna **86400**.
    
    e.  Kaydı kaydedin.

14. Geri dönüp **etki alanları** kuruluş yönetimi ve ardından penceresinde **etki alanını doğrula**. İçinde **etki alanı** kutusunda, etki alanı adınızı yazın ve ardından **etki alanını doğrula**.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
<<<<<<< HEAD
    > Etki alanı doğrulama başarılı olup olmadığını TXT kaydı değişikliklerin etkili olması için 72 saat kadar sürebilir, hemen bilemezsiniz. Denetleyin, **etki alanları** yakında doğrulama durumunuz için bu adımları gerçekleştirdikten sonra sayfa. Aşağıdaki ekran güncelleştirilmiş durumundaki bkz **doğrulandı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

14. HTTPS doğrulamak üzere seçtiğiniz **HTTPS** sekmesi **etki alanları** sayfasında ve şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Tıklatın **karşıdan yükleme dosyası** HTML dosyası indirilemedi.

    b.  HTML dosyası, etki alanının kök dizinine yükleyin.

15. Geri dönüp **etki alanları** sayfasında Kuruluş Yönetimi'nde ve tıklayın **etki alanını doğrula** düğmesi. Girin, **etki alanı adı** tıklayın ve açılan **etki alanını doğrula** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

16. Doğrulama işlemi kök dizininde karşıya dosya bulabiliyorsa, etki alanının durumu güncelleştirmeleri için **doğrulandı**.
=======
    > TXT kaydı değişikliklerin etkili olması için 72 saat kadar sürebilir çünkü hemen, etki alanı doğrulama başarılı olup olmadığını bilemezsiniz. Doğrulama durumunu görüntülemek için kontrol **etki alanları** yakında bu yordamı tamamladıktan sonra penceresi. Güncelleştirilmiş durum olarak görüntülenen *doğrulandı*aşağıdaki görüntüde gösterildiği gibi:
    > 
    > ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)
    > 
    > 

15. HTTPS doğrulaması için de **etki alanları** penceresinde, seçin **HTTPS** sekmesini ve ardından aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a. HTML dosyasını yüklemek üzere seçin **karşıdan yükleme dosyası**.

    b. HTML dosyası, etki alanının kök dizinine yükleyin.

16. Geri dönüp **etki alanları** sayfasında Kuruluş Yönetimi'nde ve seçin **etki alanını doğrula**. İçinde **etki alanını doğrula** penceresi, **etki alanı** kutusuna, **etki alanı adı**ve ardından **etki alanını doğrula**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

17. Etki alanının durumu güncelleştirilmesi doğrulama işlemi kök dizininde karşıya dosya bulabiliyorsa, *doğrulandı*, aşağıda gösterildiği gibi:
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
<<<<<<< HEAD
    > Etki alanı doğrulama hakkında daha fazla bilgi için bkz [Atlassian'ın etki alanı doğrulama belgeleri](https://confluence.atlassian.com/cloud/domain-verification-873871234.html)

17. Sol gezinti çubuğunda **SAML çoklu oturum açma**. Henüz yapmadıysanız, Atlassian'ın Identity Manager abone olun.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

18. İçinde **eklemek SAML Yapılandırması** kutusu iletişim kutusunda, aşağıdaki gibi kimlik sağlayıcı ayarları ekleyin:
=======
    > Daha fazla bilgi için bkz: [Atlassian etki alanı doğrulama](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

18. Sol bölmede seçin **SAML çoklu oturum açma**. Zaten yapmadıysanız, Atlassian Identity Manager abone olun.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

19. İçinde **eklemek SAML Yapılandırması** penceresinde aşağıdakileri yapın:
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** kutusunda, Azure portalından kopyalandığından SAML varlık kimliği yapıştırın.

    b. İçinde **kimlik sağlayıcısı SSO URL** kutusunda, Azure portalından kopyalandığından SAML çoklu oturum açma hizmeti URL'sini yapıştırın.

<<<<<<< HEAD
    c. İndirilen sertifika Azure portalından bir Not Defteri'nde açın, sertifika başlar ve son sertifikayı çizgileri olmayan değerleri kopyalayın ve yapıştırın **ortak X509 sertifika** kutusu.
    
    d. Tıklatın **yapılandırmasını kaydetmek**.
     
19. Doğru URL'leri Kurulum sahip olduğunuzdan emin olmak için Azure AD ayarlarını güncelleştirin.
  
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopya **SP kimlik kimliği** SAML ekranında ve değeri yapıştırın **tanımlayıcısı** Atlassian bulut altında Azure portalında kutusunda **etki alanı ve URL'leri** bölüm.
    
    b. Kopya **SP onaylama tüketici hizmeti URL'si** SAML ekranında ve değeri yapıştırın **yanıt URL'si** Atlassian bulut altında Azure portalında kutusunda **etki alanı ve URL'leri** bölüm.
    
    c. Oturum üzerinde Kiracı Atlassian bulutun URL'dir. 

    > [!NOTE]
    > Var olan müşteriler tıklayın gerek **Evet, yapılandırmasını güncelleştirmek** güncelleştirdikten sonra **SP kimlik kimliği** ve **SP onaylama tüketici hizmeti URL'si** Azure Portalı'nda değerleri. Yeni müşteriler, bu adımı gerçekleştirmeniz gerekmez. 
    
20. Azure portalında tıklatın **kaydetmek** düğmesi.
=======
    c. Açık bir .txt dosyasında Azure Portalı'ndan indirilen Sertifikayı kopyalamak değeri (olmadan *başlamak sertifika* ve *son sertifikayı* satırları) ve ardından yapıştırın **ortak X509 Sertifika** kutusu.
    
    d. Seçin **yapılandırmasını kaydetmek**.
     
20. Doğru URL'leri ayarladığınızdan emin olmak için aşağıdakileri yaparak Azure AD ayarları güncelleştirin:
  
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. SAML penceresindeki kopyalamak **SP kimlik kimliği** ve daha sonra Azure portalında, Atlassian bulut altında **etki alanı ve URL'leri**, yapıştırın **tanımlayıcısı** kutusu.
    
    b. SAML penceresindeki kopyalamak **SP onaylama tüketici hizmeti URL'si** ve daha sonra Azure portalında, Atlassian bulut altında **etki alanı ve URL'leri**, yapıştırın **yanıt URL'si** kutusu.  
        Oturum açma URL'si Atlassian bulut Kiracı URL'sidir. 

    > [!NOTE]
    > Güncelleştirdikten sonra varolan bir müşteri olup olmadığınızı **SP kimlik kimliği** ve **SP onaylama tüketici hizmeti URL'si** Azure portalında değerleri seçin **Evet, yapılandırmayıgüncelleştirme**. Yeni bir müşteri değilseniz, bu adımı atlayabilirsiniz. 
    
21. Azure portalında seçin **kaydetmek**.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Uygulaması kuruluyor gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve sonra katıştırılmış erişim belgelerde **yapılandırma** penceresinin alt kısmına. Daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
<<<<<<< HEAD

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]
=======

Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

   ![Bir Azure AD test kullanıcısı oluşturma][100]

<<<<<<< HEAD
1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Bir Atlassian bulut test kullanıcısı oluşturma

Azure AD kullanıcılarının Atlassian bulut oturum açmayı etkinleştirmek için bunlar Atlassian bulutunu sağlanmalıdır. Atlassian bulut durumunda sağlama bir el ile bir görevdir.
=======
1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. İçinde **tüm kullanıcılar** penceresinde, seçin **Ekle**.

    ![Ekle düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Bir Atlassian bulut test kullanıcısı oluşturma

Azure AD Atlassian buluta oturum açmalarını etkinleştirmek için aşağıdakileri yaparak Atlassian bulutta el ile kullanıcı hesapları sağlama:

1. İçinde **Yönetim** bölmesinde, **kullanıcılar**.

<<<<<<< HEAD
2. Tıklatın **davet kullanıcı** Atlassian bulutta bir kullanıcı oluşturmak için düğmesi.
=======
    ![Atlassian bulut kullanıcıları bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

2. Bir kullanıcı Atlassian bulutta oluşturmak için seçin **davet kullanıcı**.

<<<<<<< HEAD
3. Kullanıcının girin **e-posta adresi** ve uygulama erişimi atayın. 
=======
    ![Bir Atlassian bulut kullanıcı oluşturun](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

3. İçinde **e-posta adresi** kutusuna kullanıcının e-posta adresi girin ve ardından uygulama erişimi atayın. 

    ![Bir Atlassian bulut kullanıcı oluşturun](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
<<<<<<< HEAD
4. Tıklatın **kullanıcıları davet** düğmesi, kullanıcı için e-posta daveti gönderir ve daveti kabul ettikten sonra kullanıcı sistemde etkin olacaktır. 

>[!NOTE] 
>Toplu kullanıcılar tıklatarak da oluşturabilirsiniz **Toplu oluşturma** kullanıcılar bölümünde düğmesi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın
=======
4. Kullanıcıya bir e-posta göndermek için seçin **kullanıcıları davet**.  
    Kullanıcıya bir e-posta davet gönderilir ve daveti kabul ettikten sonra kullanıcı sistemde etkindir. 

>[!NOTE] 
>Ayrıca toplu-seçerek kullanıcıları oluşturun **Toplu oluşturma** düğmesini **kullanıcılar** bölümü.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

<<<<<<< HEAD
![Kullanıcı rolü atayın][200] 
=======
Bu bölümde, kullanıcı Britta Atlassian buluta erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

![Kullanıcı rolü atayın][200] 

1. Azure portalında açmak **uygulamaları** görüntülemek, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. İçinde **uygulamaları** listesinde **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** penceresinde, seçin **seçin**.

7. İçinde **eklemek atama** penceresinde, seçin **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

<<<<<<< HEAD
Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Atlassian bulut parçasında tıklattığınızda, otomatik olarak Atlassian bulut uygulamanıza açan.
=======
Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Atlassian bulut** döşeme erişim panelinde oturumunuz otomatik olarak Atlassian bulut uygulamanız.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

