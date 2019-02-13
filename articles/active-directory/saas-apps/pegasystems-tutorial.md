---
title: 'Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Pega sistemleri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 31acf80f-1f4b-41f1-956f-a9fbae77ee69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7a6fd51ad14395b3c195ae1ceb5a188dd2c708c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162518"
---
# <a name="tutorial-azure-active-directory-integration-with-pega-systems"></a>Öğretici: Pega sistemler ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pega sistemleri tümleştirme konusunda bilgi edinin.

Azure AD ile Pega sistemleri tümleştirme ile aşağıdaki avantajları sağlar:

- Pega sistemlerine erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Pega sistemlerine (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Pega sistemleriyle yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Pega sistemleri çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Pega sistemleri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pega-systems-from-the-gallery"></a>Galeriden Pega sistemleri ekleme
Azure AD'de Pega sistemleri tümleştirmesini yapılandırmak için Pega sistemleri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pega sistemleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Pega sistemleri**seçin **Pega sistemleri** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Pega sistemleri](./media/pegasystems-tutorial/tutorial_pegasystems_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Pega "Britta Simon" adlı bir test kullanıcı tabanlı sistemler ile Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne Pega sistemlerinde karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Pega sistemleri arasında bir bağlantı ilişki kurulması gerekir.

Pega sistemlerinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pega sistemlerle sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Pega sistemleri test kullanıcısı oluşturma](#create-a-pega-systems-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pega sistemlerinde Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Pega sistemleri uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Pega sistemleriyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Pega sistemleri** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/pegasystems-tutorial/tutorial_pegasystems_samlbase.png)

1. Üzerinde **Pega sistemleri etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Pega sistemleri etki alanı ve URL'ler tek oturum açma bilgileri](./media/pegasystems-tutorial/tutorial_pegasystems_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/sp/<INSTANCEID>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<CUSTOMERNAME>.pegacloud.io:443/prweb/PRRestService/WebSSO/SAML/AssertionConsumerService`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Pega sistemleri etki alanı ve URL'ler tek oturum açma bilgileri](./media/pegasystems-tutorial/tutorial_pegasystems_url1.png)

    İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak: `https://<CUSTOMERNAME>.pegacloud.io/prweb/sso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve geçişi durumu URL'si ile güncelleştirin. Bu öğreticinin ilerleyen bölümlerinde açıklanan Pega uygulama tanımlayıcısı ve yanıt URL'si değerleri bulabilirsiniz. Geçiş durumu için temasa [Pega sistemlerini istemci Destek ekibine](https://www.pega.com/contact-us) değeri alınamıyor. 

1. Pega sistemleri uygulamanın SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Bu talep, müşteriye özgü olan ve ihtiyacınıza bağlıdır. Aşağıdaki isteğe bağlı taleplerdir örnek yalnızca, uygulamanız için yapılandırabilirsiniz. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. 

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
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
    > Müşteri belirli değerleri şunlardır. Lütfen, uygun değerleri sağlayın.

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/pegasystems-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/pegasystems-tutorial/tutorial_pegasystems_certificate.png) 
1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **Pega sistemleri** yanı, açık **Pega portalı** yönetici hesabıyla başka bir tarayıcı penceresinde.

1. Seçin **oluşturma** -> **SysAdmin** -> **kimlik doğrulama hizmeti**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin.png)
    
1. Aşağıdaki işlemleri gerçekleştirin **Aauthentication hizmet Oluştur** ekran:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin1.png)

    a. Seçin **SAML 2.0** türünden

    b. İçinde **adı** metin herhangi adı ör. Azure AD SSO girin

    c. İçinde **kısa açıklama** metin kutusuna bir açıklama girin  

    d. Tıklayarak **oluştur ve Aç** 
    
1. İçinde **kimlik sağlayıcıyı (IDP) bilgi** bölümünde, tıklayarak **IDP alma meta verileri** ve Azure portalından indirdiğiniz meta veri dosyasına göz atın. Tıklayın **Gönder** meta verileri yüklenemedi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin2.png)
    
1. Aşağıda gösterildiği gibi bu IDP verilerini doldurulur.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin3.png)
    
1. Aşağıdaki işlemleri gerçekleştirin **hizmet sağlayıcısı (SP) ayarları** bölümü:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/pegasystems-tutorial/tutorial_pegasystems_admin4.png)

    a. Kopyalama **varlık kimliği** geri Azure Portalı'nda kişinin yapıştırın ve değer **tanımlayıcı** metin.

    b.  Kopyalama **onaylama tüketici hizmeti (ACS) konumu** geri Azure Portalı'nda kişinin yapıştırın ve değer **yanıt URL'si** metin.

    c. Seçin **imzalama isteğini devre dışı**.

1. **Kaydet**’e tıklayın
    
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/pegasystems-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/pegasystems-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/pegasystems-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/pegasystems-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-pega-systems-test-user"></a>Pega sistemleri test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Pega sistemlerinde adlı bir kullanıcı oluşturmaktır. Lütfen birlikte çalışarak [Pega sistemlerini istemci Destek ekibine](https://www.pega.com/contact-us) Pega Sysyems içinde kullanıcıları oluşturmak için.


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Pega sisteme erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Pega sistemlerine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Pega sistemleri**.

    ![Uygulamalar listesinde Pega sistemleri bağlantı](./media/pegasystems-tutorial/tutorial_pegasystems_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Pega sistemleri kutucuğa tıkladığınızda, otomatik olarak Pega sistemleri uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



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

