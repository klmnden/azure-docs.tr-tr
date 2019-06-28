---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Amazon Web Services (AWS) | Microsoft Docs'
description: Amazon Web Services (AWS) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9fe362eb90793c831fc48d6fdc5a871c12e1b560
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442754"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Öğretici: Amazon Web Services (AWS) Azure Active Directory ile tümleştirme

Bu öğreticide, Amazon Web Services (AWS) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Amazon Web Services (AWS) Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Amazon Web Services (AWS) erişimi, Azure AD'de denetler.
* Otomatik olarak Amazon Web Services (AWS) için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Birden çok örnek için birden çok tanımlayıcıları aşağıdaki gibi yapılandırabilirsiniz.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Bu değerleri ile Azure AD değerini kaldıracak **#** ve doğru değeri gönderme `https://signin.aws.amazon.com/saml` SAML belirteç hedef kitlesi URL olarak.

**Aşağıdaki nedenlerle Bu yaklaşımın kullanılması önerilir:**

a. Her uygulamanın benzersiz X509 sağlayacak sertifika. AWS Uygulama örneğinin her örneği, ardından kişi AWS hesabı temelinde yönetilebilir farklı sertifika sona erme tarihi olabilir. Genel sertifika geçişi bu durumda daha kolay olacaktır.

b. Kullanıcı sağlamayı AWS uygulamasıyla Azure AD'de etkinleştirebilirsiniz ve sonra tüm roller AWS hesaptan hizmetimiz getirir. El ile eklemek veya uygulama AWS rollerinde güncelleştirmek gerekmez.

c. Doğrudan Azure AD'de uygulamayı yöneten uygulama sahibi uygulama için ayrı ayrı atayabilirsiniz.

> [!Note]
> Galeri yalnızca kullandığınızdan emin olun

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Amazon Web Services (AWS) çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Amazon Web Services (AWS) destekleyen **SP ve IDP** SSO başlattı.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) galeri ekleme

Azure AD'de Amazon Web Services (AWS) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden Amazon Web Services (AWS) eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Amazon Web Services (AWS)** arama kutusuna.
1. Seçin **Amazon Web Services (AWS)** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile Amazon Web Services (adlı bir test kullanıcı kullanarak AWS) test **B.Simon**. Çalışmak SSO için Amazon Web Services'te (AWS) Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO Amazon Web Services (AWS) ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. **[Amazon Web Services (AWS) yapılandırma](#configure-amazon-web-services-aws)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma B.Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  B.Simon Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Amazon Web Services (AWS) test kullanıcısı oluşturma](#create-amazon-web-services-aws-test-user)**  B.Simon bir karşılığı Amazon Web Services (AWS) kullanıcı Azure AD gösterimini bağlı olması.
6. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Amazon Web Services (AWS)** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamanın önceden yapılandırılmış olduğu ve gerekli URL'ler zaten Azure ile önceden doldurulur. Tıklayarak yapılandırmayı kaydetmek kullanıcının erişmesi **Kaydet** düğmesi.

5. Birden fazla örnek yapılandırılırken Lütfen tanımlayıcı değeri girin. İkinci örneğinden ve sonraki sürümlerde, lütfen şu biçimde tanımlayıcı değeri belirtin. Lütfen bir **#** benzersiz bir SPN değeri belirtmek oturum açın.

    `https://signin.aws.amazon.com/saml#2`

6. Amazon Web Services (AWS) uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

    ![image](common/edit-attribute.png)

7. Yukarıdaki için Ayrıca, Amazon Web Services (AWS) uygulama SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:

    | Ad  | Kaynak özniteliği  | Ad Alanı |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "900 saniyenin (15 dakika) arasında bir değer 43200 saniye (12 saat) sağla" |  https://aws.amazon.com/SAML/Attributes |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. İçinde **Namespace** metin kutusuna, bu satır için gösterilen Namespace değeri yazın.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **Federasyon meta verileri XML** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Üzerinde **Amazon Web Services (AWS) kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Amazon Web Services (AWS) yapılandırma

1. Farklı bir tarayıcı penceresinde, Amazon Web Services (AWS) şirketinizin sitesi için yönetici olarak oturum.

2. Tıklayın **AWS giriş**.

    ![Giriş, çoklu oturum açmayı yapılandırın][11]

3. Tıklayın **kimlik ve erişim yönetimi**.

    ![Çoklu oturum açma kimliği yapılandırın][12]

4. Tıklayın **kimlik sağlayıcıları**ve ardından **sağlayıcısı oluşturma**.

    ![Çoklu oturum açma sağlayıcısı yapılandırma][13]

5. Üzerinde **yapılandırma sağlayıcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma iletişim yapılandırın][14]

    a. Olarak **sağlayıcı türü**seçin **SAML**.

    b. İçinde **sağlayıcı adı** metin kutusuna sağlayıcı adını yazın (örneğin: *WAAD*).

    c. İndirilen yüklenecek **meta veri dosyası** Azure portalından tıklayın **Dosya Seç**.

    d. Tıklayın **sonraki adım**.

6. Üzerinde **doğrulama sağlayıcı bilgileri** iletişim sayfa tıklayın **Oluştur**.

    ![Çoklu oturum açmayı yapılandırma doğrulayın][15]

7. Tıklayın **rolleri**ve ardından **rol oluşturma**.

    ![Çoklu oturum açma rollerini yapılandırma][16]

8. Üzerinde **rol oluşturma** sayfasında, aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açma güven yapılandırın][19]

    a. Seçin **SAML 2.0 Federasyon** altında **güvenilir varlık türü seçin**.

    b. Altında **SAML 2.0 sağlayıcı bölüm seçin**seçin **SAML sağlayıcısı** önceden oluşturulmuş (örneğin: *WAAD*)

    c. Seçin **izin programlı ve AWS Yönetim Konsolu erişim**.
  
    d. Tıklayın **sonraki: İzinleri**.

9. Üzerinde **eklemek izinleri ilkeleri** iletişim kutusunda, lütfen uygun İlkesi uyarınca kuruluşunuz bağlayın. Tıklayın **sonraki: Gözden geçirme**.  

    ![Çoklu oturum açma ilkesi yapılandırma][33]

10. Üzerinde **gözden geçirme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma gözden geçirme yapılandırın][34]

    a. İçinde **rol adı** metin rol adınızı girin.

    b. İçinde **rol açıklamasını** metin kutusuna bir açıklama girin.

    c. Tıklayın **rolü oluşturma**.

    d. Gerektiğinde kadar rolleri oluşturun ve bunları kimlik sağlayıcısına eşleyin.

11. AWS hizmet hesabı kimlik bilgileri, Azure AD kullanıcı sağlama AWS hesabı rollerini getirmek için kullanın. Bunun için ev AWS konsolu açın.

12. Tıklayarak **Hizmetleri** -> **güvenlik, kimlik ve Uyumluluk** -> **IAM**.

    ![AWS hesabı rolleri getirilirken](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Seçin **ilkeleri** IAM bölümünde sekmesi.

    ![AWS hesabı rolleri getirilirken](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Üzerine tıklayarak yeni bir ilke oluşturun **ilkesi oluşturma** AWS hesabı Azure AD kullanıcı sağlama rolleri getirilirken için.

    ![Yeni ilke oluşturma](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. AWS hesapları aşağıdaki adımları gerçekleştirerek tüm rolleri getirilecek kendi ilkenizi oluşturun:

    ![Yeni ilke oluşturma](./media/amazon-web-service-tutorial/policy1.png)

    a. İçinde **"ilkesi oluşturma"** bölümüne tıklayın **"JSON"** sekmesi.

    b. İlke belgeye eklediğiniz JSON aşağıda.

    ```

    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",

    "Action": [

    "iam:ListRoles"

    ],

    "Resource": "*"

    }

    ]

    }

    ```

    c. Tıklayarak **İnceleme İlkesi düğme** ilke doğrulamak için.

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy5.png)

16. Tanımlama **yeni ilke** aşağıdaki adımları gerçekleştirerek:

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy2.png)

    a. Sağlamak **ilke adı** olarak **AzureAD_SSOUserRole_Policy**.

    b. Sağlayabilirsiniz **açıklama** İlkesi **AWS hesapları rollerini getirmek için bu ilke sağlayacak**.

    c. Tıklayarak **"İlke oluştur"** düğmesi.

17. Yeni bir kullanıcı hesabı, aşağıdaki adımları uygulayarak AWS IAM hizmetinde oluşturun:

    a. Tıklayarak **kullanıcılar** AWS IAM konsolundaki gezinti.

    ![Yeni ilkeyi tanımlayın](./media/amazon-web-service-tutorial/policy3.png)

    b. Tıklayarak **Kullanıcı Ekle** yeni kullanıcı oluşturma düğmesi.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/policy4.png)

    c. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser1.png)

    * Kullanıcı adı olarak girin **AzureADRoleManager**.

    * Erişim türü seçmek **programlı erişim** seçeneği. Böylece kullanıcı, API'leri çağırmak ve rolleri AWS hesaptan alın.

    * Tıklayarak **sonraki izinler** alt sağ köşesindeki düğme.

18. Şimdi aşağıdaki adımları uygulayarak bu kullanıcı için yeni bir ilke oluşturun:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser2.png)

    a. Tıklayarak **mevcut ilkeleri doğrudan Ekle** düğmesi.

    b. Filtre bölümünde yeni oluşturulan ilke arama **AzureAD_SSOUserRole_Policy**.

    c. Seçin **ilke** ve ardından **sonraki: Gözden geçirme** düğmesi.

19. Bağlı kullanıcı ilkeyi, aşağıdaki adımları gerçekleştirerek gözden geçirin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser3.png)

    a. Kullanıcı adı, erişim türü ve kullanıcıyla eşlenen İlkesi gözden geçirin.

    b. Tıklayarak **kullanıcı oluşturma** kullanıcı oluşturmak için alt sağ köşesindeki düğme.

20. Bir kullanıcının kullanıcı kimlik bilgilerini, aşağıdaki adımları uygulayarak yükleyin:

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kullanıcı kopyalama **erişim anahtarı kimliği** ve **gizli erişim anahtarı**.

    b. AWS konsolunda rollerini getirmek için Azure AD kullanıcı bölümü hazırlama bu kimlik bilgilerini girin.

    c. Tıklayarak **Kapat** altındaki düğmesini.

21. Gidin **kullanıcı sağlamayı** Amazon Web Services uygulamasının Azure AD yönetim portalında bölümü.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning.png)

22. Girin **erişim anahtarı** ve **gizli** içinde **gizli** ve **gizli belirteç** sırasıyla alan.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning1.png)

    a. AWS kullanıcı erişim anahtarı girin **clientsecret** alan.

    b. AWS kullanıcı gizliliği girin **gizli belirteç** alan.

    c. Tıklayarak **Bağlantıyı Sına** düğmesini gereken başarıyla bu bağlantıyı test etmek kullanabilirsiniz.

    d. Tıklayarak ayarı kaydedin **Kaydet** üstünde düğme.

23. Artık sağlama durumu etkinleştirdiğinizden emin olun **üzerinde** geçiş yaptıktan ve ardından tıklayarak ayarları bölümünde **Kaydet** üstünde düğme.

    ![Kullanıcı ekle](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Amazon Web Services (AWS) için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Amazon Web Services (AWS)** .
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-amazon-web-services-aws-test-user"></a>Amazon Web Services (AWS) test kullanıcısı oluşturma

Bu bölümün amacı, B.Simon Amazon Web Services'te (AWS) adlı bir kullanıcı oluşturmaktır. Amazon Web Services (AWS), burada herhangi bir eylem gerçekleştirmeniz gerekmez, sistemde SSO için oluşturulan açmasına gerek yoktur.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Amazon Web Services (AWS) kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama Amazon Web Services (AWS) için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Bilinen sorunlar

 * İçinde **sağlama** bölümünde **eşlemeleri** alt bölümünün "Yükleniyor..." iletisini göster ve hiçbir zaman öznitelik eşlemelerini görüntülemez. Şu anda desteklenen tek sağlama iş akışı, Azure AD Kullanıcı/Grup ataması sırasında seçimi için AWS rollerden içe ' dir. Öznitelik eşlemeleri için bu önceden tanımlanmış ve yapılandırılamaz.
 
 * **Sağlama** bölüm yalnızca destekleyen bir dizi kimlik bilgisi bir AWS Kiracı için aynı anda girme. İçeri aktarılan tüm rolleri Azure AD appRoles özelliğine yazılır [servicePrincipal nesnesi](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) için AWS Kiracı. Azure AD'ye (servicePrincipals tarafından gösterilen) birden çok AWS kiracılar sağlama, ancak olduğu bilinen bir sorunu otomatik olarak tüm içeri aktarılan rolleri için kullanılan birden fazla AWS servicePrincipals yazılacak boyutlandırılmamışsa ile Galeriden eklenebilir Çoklu oturum açma için kullanılan tek servicePrincipal içine sağlama. Geçici bir çözüm olarak [Microsoft Graph API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) her AWS servicePrincipal alınan appRoles ayıklanacak sağlama yapılandırıldığı kullanılabilir. Bu rol dizeler sonradan çoklu oturum açma yapılandırıldığı AWS servicePrincipal eklenebilir.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png