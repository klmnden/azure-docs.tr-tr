---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle SuccessFactors | Microsoft Docs'
description: Azure Active Directory ve SuccessFactors arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 8401462ed4bf5ef2ac1ff10cf3f6750cbed7b4e5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050810"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Öğretici: Azure Active Directory SuccessFactors ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SuccessFactors tümleştirme konusunda bilgi edinin.

Azure AD ile SuccessFactors tümleştirme ile aşağıdaki avantajları sağlar:

- SuccessFactors erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için SuccessFactors (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SuccessFactors yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir SuccessFactors çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SuccessFactors ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-successfactors-from-the-gallery"></a>Galeriden SuccessFactors ekleme
Azure AD'de SuccessFactors tümleştirmesini yapılandırmak için SuccessFactors Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SuccessFactors eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SuccessFactors**seçin **SuccessFactors** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SuccessFactors](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SuccessFactors sınayın.

Tek iş için oturum açma için Azure AD ne SuccessFactors karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SuccessFactors ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SuccessFactors içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SuccessFactors ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SuccessFactors test kullanıcısı oluşturma](#create-a-successfactors-test-user)**  - kullanıcı Azure AD gösterimini bağlı SuccessFactors Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SuccessFactors uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SuccessFactors yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SuccessFactors** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/successfactors-tutorial/tutorial_successfactors_samlbase.png)

3. Üzerinde **SuccessFactors etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SuccessFactors etki alanı ve URL'ler tek oturum açma bilgileri](./media/successfactors-tutorial/tutorial_successfactors_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SuccessFactors istemci Destek ekibine](https://www.successfactors.com/en_us/support.html) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/successfactors-tutorial/tutorial_general_400.png)
    
6. Üzerinde **SuccessFactors yapılandırma** bölümünde **yapılandırma SuccessFactors** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/successfactors-tutorial/tutorial_successfactors_configure.png) 

7. Farklı bir web tarayıcı penceresinde oturum açın, **SuccessFactors Yönetici portalı** yönetici olarak.
    
8. Ziyaret **uygulama güvenliği** ve yerel **tek oturum açma özelliğini**. 

9. Herhangi bir değer yerleştirin **sıfırlama belirteci** tıklatıp **Kaydet belirteci** SAML SSO etkinleştirme.
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][11]

    > [!NOTE] 
    > Bu değer, açma/kapatma düğmesi kullanılır. Herhangi bir değer kaydedilirse, SAML SSO açık'tır. SAML SSO OFF ise boş bir değer kaydedilir.

10. Yerel ekran görüntüsü aşağıda ve aşağıdaki eylemleri gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][12]
   
    a. Seçin **SAML v2 SSO** radyo düğmesi
   
    b. Ayarlama **SAML sunduğundan taraf adı**(örneğin, SAML veren + şirket adı).
   
    c. İçinde **veren URL'si** metin kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.
   
    d. Seçin **yanıt (müşteri oluşturulan/IDP/AP)** olarak **zorunlu imzası iste**.
   
    e. Seçin **etkin** olarak **etkinleştirme SAML bayrağı**.
   
    f. Seçin **Hayır** olarak **oturum açma isteği imza (SF oluşturulan/SP/RP)**.
   
    g. Seçin **tarayıcı/Post profili** olarak **SAML profili**.
   
    h. Seçin **Hayır** olarak **sertifika geçerli süresi zorunlu**.
   
    i. Azure Portalı'ndan indirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **SAML sertifikası doğrulama** metin.

    > [!NOTE] 
    > Sertifika içeriği sahip başlamalıdır sertifika ve bitiş sertifika etiketleri.

11. SAML V2'ye gidin ve ardından aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma uygulama tarafında yapılandırma][13]
   
    a. Seçin **Evet** olarak **destek SP tarafından başlatılan genel oturum kapatma**.
   
    b. İçinde **genel oturum kapatma hizmeti URL'si (LogoutRequest hedef)** metin kutusu, yapıştırma **oturum kapatma URL'si** kopyaladığınız değeri form Azure portalı.
   
    c. Seçin **Hayır** olarak **sp gereken tüm Nameıd öğesi şifreleme gerektiren**.
   
    d. Seçin **belirtilmeyen** olarak **Nameıd biçimi**.
   
    e. Seçin **Evet** olarak **etkinleştir sp tarafından başlatılan oturum açma (AuthnRequest)**.
   
    f. İçinde **şirket çapında veren olarak gönderme isteği** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

12. Oturum açma kullanıcı adlarını hale getirmek isterseniz bu adımları uygulamadan büyük küçük harfe duyarlı.
   
    ![Çoklu oturum açmayı yapılandırın][29]
    
    a. Ziyaret **şirket ayarları**(ekranın altındaki).
   
    b. yanında onay kutusunu işaretleyin **etkinleştirme durumu hassas olmayan kullanıcı adı**.
   
    c.Click **Kaydet**.
   
    > [!NOTE] 
    > Bunu etkinleştirmek çalışırsanız, yinelenen bir SAML oturum açma adı oluşturur sistem denetler. Örneğin, müşteri Kullanıcı1 hem Kullanıcı1 kullanıcı adları varsa. Büyük/küçük harfe duyarlılık hemen almak, bu yinelemeler yapar. Sistem, bir hata iletisi verir ve özellik etkinleştirmez. Müşteri, farklı yazıldığından kullanıcı adlarını birini değiştirmek gerekiyor.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/successfactors-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/successfactors-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/successfactors-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/successfactors-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-successfactors-test-user"></a>SuccessFactors test kullanıcısı oluşturma

SuccessFactors için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların SuccessFactors sağlanması gerekir.  
SuccessFactors söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

İletişime geçmeniz SuccessFactors içinde oluşturulan kullanıcıları almak için [SuccessFactors Destek ekibine](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SuccessFactors erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SuccessFactors için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SuccessFactors**.

    ![Uygulamalar listesinde SuccessFactors bağlantı](./media/successfactors-tutorial/tutorial_successfactors_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SuccessFactors kutucuğa tıkladığınızda, otomatik olarak SuccessFactors uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/successfactors-tutorial/tutorial_general_01.png
[2]: ./media/successfactors-tutorial/tutorial_general_02.png
[3]: ./media/successfactors-tutorial/tutorial_general_03.png
[4]: ./media/successfactors-tutorial/tutorial_general_04.png

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/successfactors-tutorial/tutorial_general_05.png
[15]: ./media/successfactors-tutorial/tutorial_general_06.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/successfactors-tutorial/tutorial_general_100.png

[200]: ./media/successfactors-tutorial/tutorial_general_200.png
[201]: ./media/successfactors-tutorial/tutorial_general_201.png
[202]: ./media/successfactors-tutorial/tutorial_general_202.png
[203]: ./media/successfactors-tutorial/tutorial_general_203.png

