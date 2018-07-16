---
title: 'Öğretici: Azure Active Directory Tümleştirme ile SilkRoad yaşam paketi | Microsoft Docs'
description: Azure Active Directory ve SilkRoad yaşam Suite arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 962b3f0e18c2dbedf80c0cfca6bc8275b394307b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046024"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Öğretici: Azure Active Directory SilkRoad yaşam Suite ile tümleştirme

Bu öğreticide, SilkRoad yaşam Suite Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile SilkRoad yaşam Suite tümleştirme ile aşağıdaki avantajları sağlar:

- SilkRoad yaşam Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) SilkRoad yaşam paketine açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SilkRoad yaşam Suite ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir SilkRoad yaşam Suite çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SilkRoad yaşam paketi ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Galeriden SilkRoad yaşam paketi ekleme
Azure AD'ye SilkRoad yaşam Suite tümleştirmesini yapılandırmak için SilkRoad yaşam Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SilkRoad yaşam paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SilkRoad yaşam Suite**seçin **SilkRoad yaşam Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SilkRoad yaşam paketi](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve SilkRoad ömrü "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile Azure AD çoklu oturum açma testi.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı SilkRoad yaşam grubundaki bir kullanıcı için Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SilkRoad yaşam paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SilkRoad yaşam paketindeki değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SilkRoad yaşam Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SilkRoad yaşam Suite test kullanıcısı oluşturma](#create-a-silkroad-life-suite-test-user)**  - SilkRoad yaşam paketindeki, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SilkRoad yaşam Suite uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma SilkRoad yaşam Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SilkRoad yaşam Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_samlbase.png)

3. Üzerinde **SilkRoad yaşam Suite etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SilkRoad yaşam Suite etki alanı ve URL'ler tek oturum açma bilgileri](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.silkroad-eng.com/Authentication/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP` |
    | `https://<subdomain>.silkroad.com/Authentication/SP` |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/` |
    | `https://<subdomain>.silkroad.com/Authentication/` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SilkRoad yaşam Suite istemci Destek ekibine](https://www.silkroad.com/locations/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/silkroad-life-suite-tutorial/tutorial_general_400.png)
    
6. Üzerinde **SilkRoad yaşam paketi yapılandırması** bölümünde **SilkRoad yaşam paketini Yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SilkRoad yaşam paketi yapılandırması](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_configure.png) 

7. SilkRoad şirketinizin sitesi için yönetici olarak oturum. 
 
    >[!NOTE] 
    > Microsoft Azure AD ile Federasyonu yapılandırma için SilkRoad yaşam Suite kimlik doğrulaması uygulamaya erişim elde etmek için lütfen SilkRoad desteği veya SilkRoad Hizmetleri temsilcinize başvurun.

8. Git **hizmet sağlayıcısı**ve ardından **Federasyon ayrıntıları**. 
   
    ![Azure AD çoklu oturum açma][10]

9. Tıklayın **Federasyon meta verileri indirme**ve bilgisayarınızda meta veri dosyasını kaydedin.
   
    ![Azure AD çoklu oturum açma][11] 

10. İçinde **SilkRoad** uygulama tıklayın **kimlik doğrulaması kaynakları**.
   
    ![Azure AD çoklu oturum açma][12] 

11. Tıklayın **kimlik doğrulaması kaynağı ekleme**. 
   
    ![Azure AD çoklu oturum açma][13] 

12. İçinde **kimlik doğrulaması kaynağı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
    ![Azure AD çoklu oturum açma][14]
  
    a. Altında **seçeneği 2 - meta veri dosyası**, tıklayın **Gözat** Azure portalından indirilen meta veri dosyası karşıya yüklemek için.
  
    b. Tıklayın **oluşturma kimlik sağlayıcısı kullanarak dosya verilerini**.

13. İçinde **kimlik doğrulaması kaynakları** bölümünde **Düzenle**. 
    
     ![Azure AD çoklu oturum açma][15] 

14. Üzerinde **kimlik doğrulaması kaynağı Düzenle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 
    
     ![Azure AD çoklu oturum açma][16] 

    a. Olarak **etkin**seçin **Evet**.

    b. İçinde **Entityıd** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **IDP açıklama** metin kutusuna bir açıklama yapılandırmanızı (örneğin: *Azure AD SSO*).

    d. İçinde **meta veri dosyası** metin kutusuna, karşıya yükleme **meta verileri** Azure portalından indirdiğiniz dosyası.
  
    e. İçinde **IDP adı** metin kutusu, bir yapılandırma için belirli bir ad yazın (örneğin: *Azure SP*).
  
    f. İçinde **oturum kapatma hizmeti URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    g. İçinde **oturum açma hizmeti URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    h. **Kaydet**’e tıklayın.

15. Diğer tüm kimlik doğrulaması kaynakları devre dışı bırakın. 
    
     ![Azure AD çoklu oturum açma][17]

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/silkroad-life-suite-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/silkroad-life-suite-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/silkroad-life-suite-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/silkroad-life-suite-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-silkroad-life-suite-test-user"></a>SilkRoad yaşam Suite test kullanıcısı oluşturma

Bu bölümde, Britta Simon SilkRoad yaşam paketindeki adlı bir kullanıcı oluşturun. Çalışmak [SilkRoad yaşam Suite istemci Destek ekibine](https://www.silkroad.com/locations/) SilkRoad yaşam Suite platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma SilkRoad yaşam paketine erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SilkRoad yaşam paketine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SilkRoad yaşam Suite**.

    ![Uygulamalar listesinde SilkRoad yaşam Suite bağlantısı](./media/silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SilkRoad yaşam Suite kutucuğa tıkladığınızda, otomatik olarak SilkRoad yaşam paketini uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/silkroad-life-suite-tutorial/tutorial_general_04.png

[100]: ./media/silkroad-life-suite-tutorial/tutorial_general_100.png

[200]: ./media/silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/silkroad-life-suite-tutorial/tutorial_general_202.png
[203]: ./media/silkroad-life-suite-tutorial/tutorial_general_203.png
[10]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png
