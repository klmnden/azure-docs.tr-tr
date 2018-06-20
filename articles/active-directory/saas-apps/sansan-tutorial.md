---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Sansan | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Sansan arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c8b5a84c853a974ede77e716b77f0a5007775ef7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231221"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Öğretici: Azure Active Directory Tümleştirme Sansan ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Sansan tümleştirmek öğrenin.

Sansan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Sansan erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Sansan (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Sansan ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Sansan çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Sansan ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sansan-from-the-gallery"></a>Galeriden Sansan ekleme
Azure AD Sansan tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Sansan eklemeniz gerekir.

**Galeriden Sansan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Sansan**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/tutorial_sansan_search.png)

5. Sonuçlar panelinde seçin **Sansan**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Sansan sınayın.

Tekli çalışmaya oturum için Azure AD Sansan karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Sansan ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Sansan içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sansan ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Sansan test kullanıcısı oluşturma](#creating-a-sansan-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Sansan sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Sansan uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Sansan yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Sansan** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. Üzerinde **Sansan etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın: 
    
    | Ortam | URL'si |
    |:--- |:--- |
    | Koruyun web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Yerel mobil uygulama |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Mobil tarayıcı ayarları |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL'si ile güncelleştirin. Kişi [Sansan istemci destek ekibi](https://www.sansan.com/form/contact) bu değerleri almak için. 
     
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_general_400.png)

6. Sansan uygulama bekler birden çok **tanımlayıcıları** ve **yanıt URL'leri** PowerShell kullanarak yapılandırılmış birden çok ortamlarını (PC web, yerel mobil uygulama, mobil tarayıcı ayarlarını) desteklemek için komut dosyası. Ayrıntılı adımlar aşağıda açıklanmıştır.

7. Birden çok yapılandırmak için **tanımlayıcıları** ve **yanıt URL'leri** PowerShell Betiği kullanılarak Sansan uygulaması için şu adımları gerçekleştirin:

    ![Çoklu oturum açma obj yapılandırın](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Git **özellikleri** sayfasında **Sansan** uygulama ve kopyalama **nesne kimliği** kullanarak **kopyalama** düğmesine tıklayın ve Not Defteri'ne yapıştırın.

    b. **Nesne kimliği**, Azure portalından kopyalanan olarak kullanılacak **ServicePrincipalObjectId** daha sonra öğreticide kullanılan PowerShell komut dosyası. 

    c. Şimdi yükseltilmiş bir Windows PowerShell komut istemi açın.
    
    >[!NOTE] 
    > Azuread'i modülü yüklemeniz gerekir (komutunu `Install-Module -Name AzureAD`). NuGet modülü veya yeni Azure Active Directory V2 PowerShell modülü yüklemek isteyip istemediğiniz sorulduğunda Y yazın ve ENTER tuşuna basın.

    d. Çalıştırma `Connect-AzureAD` ve bir genel yönetici kullanıcı hesabıyla oturum açın.

    e. Birden fazla URL bir uygulamayı güncelleştirmek için aşağıdaki komut dosyasını kullanın:

    ```poweshell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. PowerShell Betiği başarılı tamamlanmasıyla sonra aşağıda gösterildiği gibi komut dosyası sonucu şu şekilde olacaktır ve URL değerleri güncelleştirilmesi ancak bunlar Azure portalında yansıtılan olmaz. 

    ![Çoklu oturum açma komut dosyası yapılandırın](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. Üzerinde **Sansan yapılandırma** 'yi tıklatın **yapılandırma Sansan** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Çoklu oturum açma yapılandırmak için **Sansan** yan, indirilen göndermek için ihtiyacınız **sertifika**, **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** için [Sansan destek ekibi](https://www.sansan.com/form/contact). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

>[!NOTE]
>Bilgisayar Tarayıcı ayarını da iş mobil uygulama ve PC web birlikte mobil tarayıcı için. 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sansan-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sansan-test-user"></a>Sansan test kullanıcısı oluşturma

Bu bölümde, Sansan içinde Britta Simon adlı bir kullanıcı oluşturun. Sansan uygulama kullanıcının SSO yapmadan önce uygulamayı sağlanması gerekir. 

>[!NOTE]
>Bir kullanıcı el ile oluşturabilir veya toplu gerekiyorsa kullanıcıları, başvurmanız gerekir. [Sansan destek ekibi](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Sansan için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Sansan için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Sansan**.

    ![Çoklu oturum açmayı yapılandırın](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Sansan parçasında tıklattığınızda, otomatik olarak Sansan uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

