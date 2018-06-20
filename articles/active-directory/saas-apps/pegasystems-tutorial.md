---
title: 'Öğretici: Azure Active Directory Tümleştirme Pega sistemleriyle | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Pega sistemleri arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: 005f487b46ca3894ed2d94ec032bb009c3051796
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36223391"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Öğretici: Azure Active Directory Tümleştirme Pega sistemleriyle

Bu öğreticide, Azure Active Directory (Azure AD) ile Pega sistemleri tümleştirme öğrenin.

Azure AD ile Pega sistemleri tümleştirme ile aşağıdaki avantajları sağlar:

- Pega sistemleri erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Pega sistemlere (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Pega sistemleriyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Pega sistemleri çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Pega sistemleri ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-pega-systems-from-the-gallery"></a>Galeriden Pega sistemleri ekleme
Azure AD Pega sistemleri tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Pega sistemleri eklemeniz gerekir.

**Galeriden Pega sistemleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Pega sistemleri**seçin **Pega sistemleri** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Pega sistemleri](./media/pegasystems-tutorial/tutorial_pegasystems_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Pega "Britta Simon" adlı bir test kullanıcı tabanlı sistemler ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Pega sistemlerinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Pega sistemlerinde arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Pega sistemlerinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Pega sistemleriyle sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Pega sistemleri test kullanıcısı oluşturma](#create-a-pega-systems-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Pega sistemleri sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Pega sistemleri uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Pega sistemleriyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Pega sistemleri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/pegasystems-tutorial/tutorial_pegasystems_samlbase.png)

3. Üzerinde **Pega sistemleri etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Pega sistemleri etki alanı ve URL'leri tek oturum açma bilgileri](./media/pegasystems-tutorial/tutorial_pegasystems_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Pega sistemleri etki alanı ve URL'leri tek oturum açma bilgileri](./media/pegasystems-tutorial/tutorial_pegasystems_url1.png)

    İçinde **geçiş durumunu** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve geçiş durumu URL ile güncelleştirin. Bu öğreticinin ilerleyen bölümlerinde açıklanan Pega uygulama tanımlayıcısı ve yanıt URL'si değerleri bulabilirsiniz. İçin geçiş durumu, temasa [Pega sistemlerini istemci destek ekibi](https://www.pega.com/contact-us) değeri alınamıyor. 

5. Pega sistemleri uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Bu talep belirli müşterisiyseniz ve gereksinimi temel bağlıdır. Aşağıdaki isteğe bağlı taleplerdir örneği yalnızca, uygulamanız için yapılandırabilirsiniz. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. 

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | Kullanıcı Kimliği | *********** |
    | CN =  | *********** |
    | posta | *********** |
    | accessgroup | *********** |
    | Kuruluş | *********** |
    | orgdivision | *********** |
    | orgunit | *********** |
    | Çalışma grubu | *********** |
    | Telefon | *********** |

    > [!NOTE]
    > Bunlar müşteri belirli değerlerdir. Lütfen uygun değerleri sağlayın.

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/pegasystems-tutorial/tutorial_pegasystems_certificate.png) 
8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_general_400.png)
    
9. Çoklu oturum açma yapılandırmak için **Pega sistemleri** yan, açık **Pega Portal** başka bir tarayıcı penceresinde yönetici hesabıyla.

10. Seçin **oluşturma** -> **SysAdmin** -> **kimlik doğrulama hizmeti**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
11. Aşağıdaki işlemleri gerçekleştirin **Aauthentication hizmet oluşturma** ekran:

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    a. Seçin **SAML 2.0** türünden

    b. İçinde **adı** metin kutusuna, tüm ad örneğin Azure AD SSO girin

    c. İçinde **kısa bir açıklama** metin kutusuna, herhangi bir açıklama girin  

    d. Tıklayın **oluşturma ve Aç** 
    
12. İçinde **kimlik sağlayıcıyı (IDP) bilgi** bölümünde, tıklayın **alma IDP meta veri** ve Azure Portalı'ndan indirilen meta veri dosyasına göz atın. Tıklatın **gönderme** meta verileri yüklenemedi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
13. Aşağıda gösterildiği gibi bu IDP veri doldurulacaktır.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
14. Aşağıdaki işlemleri gerçekleştirin **hizmet sağlayıcısı (SP) ayarları** bölümü:

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    a. Kopya **varlık kimliği** değer ve geri Azure Portalı'nda 's yapıştırma **tanımlayıcısı** metin kutusu.

    b.  Kopya **onaylama tüketici Hizmeti'ni (ACS) konumu** değer ve geri Azure Portalı'nda 's yapıştırma **yanıt URL'si** metin kutusu.

    c. Seçin **imzalama isteği devre dışı**.

15. **Kaydet**’e tıklayın
    
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/pegasystems-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/pegasystems-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/pegasystems-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/pegasystems-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-pega-systems-test-user"></a>Pega sistemleri test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Pega sistemlerinde adlı bir kullanıcı oluşturmaktır. Lütfen çalışmak [Pega sistemlerini istemci destek ekibi](https://www.pega.com/contact-us) Pega Sysyems kullanıcıları oluşturmak için.


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Pega sisteme erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Pega sistemlere atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Pega sistemleri**.

    ![Uygulamalar listesinde Pega sistemleri bağlantı](./media/pegasystems-tutorial/tutorial_pegasystems_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Pega sistemleri parçasında tıklattığınızda, otomatik olarak Pega sistemleri uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/pegasystems-tutorial/tutorial_general_01.png
[2]: ./media/pegasystems-tutorial/tutorial_general_02.png
[3]: ./media/pegasystems-tutorial/tutorial_general_03.png
[4]: ./media/pegasystems-tutorial/tutorial_general_04.png

[100]: ./media/pegasystems-tutorial/tutorial_general_100.png

[200]: ./media/pegasystems-tutorial/tutorial_general_200.png
[201]: ./media/pegasystems-tutorial/tutorial_general_201.png
[202]: ./media/pegasystems-tutorial/tutorial_general_202.png
[203]: ./media/pegasystems-tutorial/tutorial_general_203.png

