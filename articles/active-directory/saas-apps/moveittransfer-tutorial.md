---
title: 'Öğretici: Azure Active Directory Tümleştirme MOVEit Transfer - Azure AD Tümleştirmesi ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory MOVEit Transfer - Azure AD tümleştirme arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: b43230ac8f4b8f6048319236adca56d0289bceb3
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982066"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Öğretici: Azure Active Directory Tümleştirme ile MOVEit Transfer - Azure AD tümleştirme

Bu öğreticide, MOVEit Transfer - Azure Active Directory (Azure AD) ile Azure AD tümleştirme tümleştirmek öğrenin.

MOVEit Transfer - Azure AD tümleştirme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- MOVEit Transfer - Azure AD tümleştirme erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak MOVEit aktarımı - Azure AD hesaplarına ile Azure AD tümleştirme (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme MOVEit Transfer - Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- MOVEit Transfer - Azure AD tümleştirme çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. MOVEit Transfer - Azure AD tümleştirme galerisinden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>MOVEit Transfer - Azure AD tümleştirme galerisinden ekleme
MOVEit Transfer - Azure AD, Azure AD tümleştirmeye tümleştirmesini yapılandırma MOVEit Transfer - Azure AD tümleştirme galerisinden listenize yönetilen SaaS uygulamalarının eklemeniz gerekir.

**Azure AD tümleştirme Galerisi'nden MOVEit Transfer - eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **MOVEit Transfer - Azure AD tümleştirme**seçin **MOVEit Transfer - Azure AD tümleştirme** sonuç panelinden ardından **Ekle** Ekle düğmesi uygulama.

    ![MOVEit Transfer - Azure AD tümleştirme sonuçlar listesinde](./media/moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma MOVEit Transfer - "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD Tümleştirmesi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen MOVEit transfer - Azure AD tümleştirme bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı MOVEit transfer - arasında bir bağlantı ilişkisi Azure AD tümleştirme kurulması gerekir.

Değeri MOVEit Transfer - Azure AD tümleştirme atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma ile MOVEit Transfer - sınamak için Azure AD tümleştirme, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[MOVEit Transfer - Azure AD tümleştirme test kullanıcısı oluşturma](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - Britta Simon, karşılık gelen MOVEit Transfer - kullanıcı Azure AD gösterimini bağlı Azure AD tümleştirme sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, MOVEit Transfer - Azure AD tümleştirme uygulaması yapılandırın.

**Azure AD çoklu oturum açma MOVEit Transfer - Azure AD tümleştirme yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **MOVEit Transfer - Azure AD tümleştirme** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. Üzerinde **MOVEit aktarım - Azure AD tümleştirme etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://contoso.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://contoso.com/<tenatid>`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu değerleri daha sonra da başvurabilir **servis sağlayıcı meta verileri URL'sini** bölüm veya kişi [MOVEit Transfer - Azure AD tümleştirme istemci destek ekibi](https://community.ipswitch.com/s/support) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/moveittransfer-tutorial/tutorial_general_400.png)
    
6. MOVEit aktarımı Kiracı yönetici olarak oturum açma.

7. Sol gezinti bölmesinde tıklatın **ayarları**.

    ![Ayarları bölümü üzerinde uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Tıklatın **tek oturum açma** altındaki bağlantı **kullanıcı kimlik doğrulama -> güvenlik ilkelerini**.

    ![Güvenlik ilkeleri üzerinde uygulama yan](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Meta veri belgesi indirmek için meta veri URL'sini bağlantısına tıklayın.

    ![Hizmet sağlayıcısı meta veri URL'si](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Doğrulayın **Entityıd** eşleşen **tanımlayıcısı** içinde **MOVEit aktarım - Azure AD tümleştirme etki alanı ve URL'leri** bölümü.
    * Doğrulayın **AssertionConsumerService** konumu URL ile eşleşip **yanıt URL'si** içinde **MOVEit aktarım - Azure AD tümleştirme etki alanı ve URL'leri** bölümü.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Tıklatın **kimlik sağlayıcı Ekle** yeni bir Federasyon kimlik sağlayıcısı eklemek için düğmeyi.

    ![Kimlik sağlayıcısı ekleyin](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Tıklatın **Gözat...**  Azure Portalı'ndan indirilen meta veri dosyası seçmek için ardından **kimlik sağlayıcı Ekle** indirilen dosya karşıya yüklemek için.

    ![SAML kimlik sağlayıcısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Seçin "**Evet**" olarak **etkin** içinde **Federasyon kimlik sağlayıcı ayarları Düzenle...**  sayfasında ve tıklayın **kaydetmek**.

    ![Federe kimlik sağlayıcı ayarları](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. İçinde **federe kimlik sağlayıcısı kullanıcı ayarlarını Düzenle** sayfasında, aşağıdaki eylemleri gerçekleştirin:
    
    ![Federe kimlik sağlayıcı ayarları Düzenle](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Seçin **SAML NameID** olarak **oturum açma adı**.
    
    b. Seçin **diğer** olarak **tam adı** ve **öznitelik adı** textbox değeri koyun: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Seçin **diğer** olarak **e-posta** ve **öznitelik adı** textbox değeri koyun: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Seçin **Evet** olarak **oturum açma hesabı otomatik olarak oluşturma**.
    
    e. Tıklatın **kaydetmek** düğmesi.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/moveittransfer-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/moveittransfer-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/moveittransfer-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/moveittransfer-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>MOVEit Transfer - Azure AD tümleştirme test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon MOVEit Transfer - Azure AD tümleştirme adlı bir kullanıcı oluşturmaktır. MOVEit Transfer - Azure AD tümleştirme yalnızca zaman sağlama, hangi etkinleştirdiğiniz destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı MOVEit Transfer - henüz yoksa Azure AD tümleştirme erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [MOVEit Transfer - Azure AD tümleştirme istemci destek ekibi](https://community.ipswitch.com/s/support).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta MOVEit Transfer - Azure AD tümleştirme erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon MOVEit Transfer - Azure AD tümleştirme atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **MOVEit Transfer - Azure AD tümleştirme**.

    ![MOVEit Transfer - Azure AD tümleştirme uygulamalar listesinde bağlantı](./media/moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

MOVEit Transfer - tıklattığınızda Azure AD tümleştirme kutucuğunu erişim panelinde, otomatik olarak imzalanmış-, MOVEit Transfer - Azure AD tümleştirme uygulaması için. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/moveittransfer-tutorial/tutorial_general_203.png

