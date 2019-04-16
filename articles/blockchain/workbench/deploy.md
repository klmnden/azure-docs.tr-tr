---
title: Azure Blockchain Workbench'i dağıtma
description: Azure Blockchain Workbench'i dağıtma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 5f488811e57ee20cb25db56b2d9e04202b17ffb2
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59579538"
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain Workbench'i dağıtma

Azure Blockchain Workbench, Azure Market'te çözüm şablonu kullanılarak dağıtılır. Blok zinciri uygulamaları oluşturmak için gereken bileşenlerin dağıtımına ilişkin şablonu basitleştirir. Dağıtıldıktan sonra Blockchain Workbench'i istemci uygulamalar, kullanıcılar ve blok zinciri uygulamaları oluşturup yönetmek için erişim sağlar.

Blockchain Workbench'i bileşenleri hakkında daha fazla bilgi için bkz. [Azure Blockchain Workbench mimarisi](architecture.md).

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma

Blockchain Workbench'i, ilgili Azure Hizmetleri blok zinciri tabanlı bir uygulama oluşturmak için en sık kullanılan bir dizi ile birlikte bir blok zinciri defter dağıtmanıza olanak tanır. Blockchain Workbench'i dağıtma, Azure aboneliğinizde bir kaynak grubu içinde sağlanan aşağıdaki Azure Hizmetleri ile sonuçlanır.

* 1 olay Kılavuzu konusu
* 1 Service Bus Namespace
* 1 application Insights
* 1 SQL veritabanı (standart S0)
* 2 uygulama Hizmetleri (standart)
* 2 azure anahtar kasaları
* 2 azure depolama hesapları (standart LRS)
* 2 sanal makine ölçek kümeleri (için Doğrulayıcı ve çalışan düğümleri)
* (Yük dengeleyici, ağ güvenlik grubu ve her bir sanal ağ için genel IP adresi gibi) 2 sanal ağ
* İsteğe bağlı: Azure İzleyici

Oluşturulan örnek bir dağıtım verilmiştir **myblockchain** kaynak grubu.

![Örnek dağıtım](media/deploy/example-deployment.png)

Blockchain Workbench'i temel alınan Azure hizmetlerinin maliyetinin toplama maliyetidir. Azure Hizmetleri kullanılarak hesaplanabilir için fiyatlandırma bilgilerine [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/).

> [!IMPORTANT]
> Bir Azure ücretsiz katman abonelik gibi düşük hizmet sınırları ile bir abonelik kullanıyorsanız, dağıtım yeterli VM çekirdek kotası nedeniyle başarısız olabilir. Dağıtımdan önce kılavuzdan kullanarak kotanızı denetleyin [sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) makalesi. Varsayılan VM seçim 6 VM çekirdek gerektiriyor. Daha küçük bir boyut VM gibi değiştirme *standart DS1 v2* 4 çekirdek sayısını azaltır.

## <a name="prerequisites"></a>Önkoşullar

Azure Blockchain Workbench, Azure AD, yapılandırma ve uygulama kayıtlarını gerektirir. Azure AD yapmak seçebileceğiniz [yapılandırmalar el ile](#azure-ad-configuration) dağıtım sonrasında otomatik olarak bir komut çalıştırın veya dağıtım önce. Blockchain Workbench'i yeniden dağıtıyorsanız bkz [Azure AD Yapılandırması](#azure-ad-configuration) Azure AD'ye yapılandırmanızı doğrulamak üzere.

> [!IMPORTANT]
> Workbench olarak bir Azure AD uygulaması kaydetmek için kullandığınız aynı kiracıda dağıtılması gerekmez. Workbench kaynakları dağıtmak için yeterli izinlere sahip olduğu bir kiracıda dağıtılması gerekir. Azure AD kiracılarıyla hakkında daha fazla bilgi için bkz. [bir Active Directory kiracısı edinme](../../active-directory/develop/quickstart-create-new-tenant.md) ve [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).



## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench'i dağıtma

Önkoşul adımlarını tamamladıktan sonra Blockchain Workbench'i dağıtma hazırsınız. Aşağıdaki bölümlerde, framework dağıtmak nasıl yenileyeceğiniz özetlenmektedir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sağ üst köşedeki hesabınızı seçin ve istenen geçiş, Azure Blockchain Workbench'i dağıtma istediğiniz Azure AD kiracısı.
3. Sol bölmede seçin **kaynak Oluştur**. Arama `Azure Blockchain Workbench` içinde **markette Ara** arama çubuğu. 

    ![Market arama çubuğu](media/deploy/marketplace-search-bar.png)

4. Seçin **Azure Blockchain Workbench'i**.

    ![Market arama sonuçları](media/deploy/marketplace-search-results.png)

5. **Oluştur**’u seçin.
6. Temel ayarlar tamamlayın.

    ![Azure Blockchain Workbench'i oluşturma](media/deploy/blockchain-workbench-settings-basic.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Kaynak ön eki | Dağıtımınız için kısa benzersiz tanımlayıcısı. Bu değer, kaynakları adlandırma için temel olarak kullanılır. |
    | VM kullanıcı adı | Kullanıcı adı, yönetici olarak tüm sanal makineler (VM) için kullanılır. |
    | Kimlik doğrulaması türü | Vm'lere bağlanmak için anahtar veya bir parola kullanmak isteyip istemediğinizi seçin. |
    | Parola | Parola, Vm'lere bağlanmak için kullanılır. |
    | SSH | RSA ortak anahtarını tek satırlı biçimde başlayarak kullanmak **ssh-rsa** veya çok satırlı PEM biçimi kullanın. Kullanarak SSH anahtarları oluşturabilirsiniz `ssh-keygen` Linux ve OS X veya Windows üzerinde puttygen araçlarını kullanarak. SSH anahtarları hakkında daha fazla bilgi [azure'da Windows ile SSH kullanma anahtarları](../../virtual-machines/linux/ssh-from-windows.md). |
    | Veritabanı parolası / veritabanı parolasını onaylayın | Dağıtımın bir parçası oluşturduğunuz veritabanına erişim için kullanılacak parolayı belirtin. |
    | Dağıtım bölgesi | Blockchain Workbench'i kaynakların dağıtılacağı yeri belirtin. En iyi kullanılabilirlik için bu eşleşmelidir **konumu** ayarı. |
    | Abonelik | Dağıtımınız için kullanmak istediğiniz Azure aboneliği belirtin. |
    | Kaynak grupları | Seçerek yeni bir kaynak grubu oluşturma **Yeni Oluştur** ve benzersiz kaynak grubu adı belirtin. |
    | Konum | Framework dağıtmak istediğiniz bölgeyi belirtin. |

7. Seçin **Tamam** temel yapılandırma bölümü tamamlanması.

8. İçinde **Gelişmiş ayarlar**, yeni bir blok zinciri ağ oluşturma veya var olan bir yetki başlangıcı kavram blockchain ağı kullanmak isteyip istemediğinizi seçin.

    İçin **Yeni Oluştur**:

    *Yeni Oluştur* seçeneği, bir dizi içindeki tek bir üyenin abonelik Ethereum kavram, (PoA) yetkilisi düğümü oluşturur. 

    ![Yeni blok zinciri ağ için Gelişmiş ayarları](media/deploy/advanced-blockchain-settings-new.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | İzleme | Azure blockchain ağınızı izlemek izleyicisini etkinleştirmek isteyip istemediğinizi seçin |
    | Azure Active Directory ayarları | Seçin **ekleyebilirsiniz**.</br>Not: İçin seçerseniz, [önceden Azure AD'yi yapılandırma](#azure-ad-configuration) veya tercih yeniden dağıtmaya gerek, *artık ekleme*. |
    | Sanal makine seçimi | Blok zinciri ağınız için tercih edilen VM boyutunu seçin. Gibi daha küçük bir VM boyutu seçin *standart DS1 v2* Azure ücretsiz katmanı gibi düşük hizmet sınırları olan bir abonelik kullanıyorsanız. |

    İçin **var olanı kullan**:

    *Var olanı kullan* seçeneği, bir Ethereum kavram yetki başlangıcı (PoA) blok zinciri ağ belirtmenize olanak sağlar. Uç noktaları aşağıdaki gereksinimlere sahiptir.

   * Uç nokta bir Ethereum kavram yetki başlangıcı (PoA) blok zinciri ağ olması gerekir.
   * Uç noktası, ağ üzerinden genel olarak erişilebilir olması gerekir.
   * PoA blockchain ağ sıfır olarak ayarlanmış gaz fiyat için yapılandırılmalıdır.

     > [!NOTE]
     > Blockchain Workbench'i hesapları finanse değil. Fon gerekiyorsa, işlem başarısız.

     ![Blok zinciri ağınız için Gelişmiş ayarları](media/deploy/advanced-blockchain-settings-existing.png)

     | Ayar | Açıklama  |
     |---------|--------------|
     | Ethereum RPC Endpoint | Var olan PoA blok zinciri ağı RPC uç noktası sağlar. Uç nokta https:// veya http:// ile başlayan ve bir bağlantı noktası numarası ile sona erer. Örneğin, `http<s>://<network-url>:<port>` |
     | Azure Active Directory ayarları | Seçin **ekleyebilirsiniz**.</br>Not: İçin seçerseniz, [önceden Azure AD'yi yapılandırma](#azure-ad-configuration) veya tercih yeniden dağıtmaya gerek, *artık ekleme*. |
     | Sanal makine seçimi | Blok zinciri ağınız için tercih edilen VM boyutunu seçin. |

9. Seçin **Tamam** Gelişmiş ayarlar tamamlanması.

10. Parametrelerinizi doğru olduğunu doğrulamak için özeti gözden geçirin.

    ![Özet](media/deploy/blockchain-workbench-summary.png)

11. Seçin **Oluştur** , Azure Blockchain Workbench'i dağıtma ve koşulları kabul etmiş olursunuz.

Dağıtım 90 dakikaya kadar sürebilir. Azure portalı, ilerlemeyi izlemek için kullanabilirsiniz. Yeni oluşturulan kaynak grubunda seçin **dağıtımları > Genel Bakış** dağıtılan yapıtlar durumunu görmek için.

> [!IMPORTANT]
> POST dağıtımı, Active Directory ayarları tamamlamanız gerekir. Seçerseniz, **daha sonra ekleme**, çalıştırmanız gereken [Azure AD'ye yapılandırma betiğini](#azure-ad-configuration-script).  Seçerseniz, **şimdi ekleyin**, yapmanız [yanıt URL'si yapılandırma](#configuring-the-reply-url).

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench'i Web URL'si

Blockchain Workbench'i dağıtımı tamamlandıktan sonra yeni bir kaynak grubu Blockchain Workbench'i kaynaklarınızı içerir. Blockchain Workbench'i Hizmetleri web URL aracılığıyla erişilir. Aşağıdaki adımlar dağıtılan framework'ün web URL'si almak nasıl gösterir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol gezinti bölmesinde seçin **kaynak grupları**
3. Blockchain Workbench'i dağıtırken belirtilen kaynak grubu adı seçin.
4. Seçin **türü** türüne göre alfabetik olarak sıralamak için sütun başlığını.
5. İki kaynak türüyle **App Service**. Kaynak türü seçin **App Service** *olmadan* "-API" soneki.

    ![Uygulama hizmet listesi](media/deploy/resource-group-list.png)

6. App Service **Essentials** bölümünde, kopya **URL** web URL'si, dağıtılan Blockchain Workbench'i temsil eden bir değer.

    ![App service temel bileşenleri](media/deploy/app-service.png)

Özel etki alanı Blockchain Workbench ile ilişkilendirmek için bkz: [Traffic Manager'ı kullanarak Azure App Service içinde bir web uygulaması için özel etki alanı adı yapılandırma](../../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="azure-ad-configuration-script"></a>Azure AD yapılandırma betiği

Azure AD, Blockchain Workbench'i dağıtımınızı tamamlamak için yapılandırılmalıdır. Yapılandırma yapmak için bir PowerShell Betiği kullanacaksınız.

1. Bir tarayıcıda gidin [Blockchain Workbench'i Web URL'si](#blockchain-workbench-web-url).
2. Cloud Shell kullanarak Azure AD'yi ayarlama yönergeleri görürsünüz. Komutu kopyalayıp Cloud Shell'i başlatın.

    ![AAD betiği Başlat](media/deploy/launch-aad-script.png)

3. Blockchain Workbench'i dağıttığınız Azure AD kiracısı seçin.
4. Cloud shell'de yapıştırın ve şu komutu çalıştırın.
5. İstendiğinde, Blockchain Workbench için kullanmak istediğiniz Azure AD kiracısı girin. Bu kullanıcılar için Blockchain Workbench'i içeren Kiracı olacaktır.

    > [!IMPORTANT]
    > Kimliği doğrulanmış kullanıcı, Azure AD uygulama kayıtları oluşturmak ve Kiracı yetkilendirilmiş uygulama izinleri vermek için izinleri gerektirir. Bir yöneticiden kiracısının Azure AD yapılandırma betiğini çalıştırın ya da yeni bir kiracı oluşturmanız gerekebilir.

    ![Azure AD kiracısı girin](media/deploy/choose-tenant.png)

6. Bir tarayıcı kullanarak Azure AD Kiracı kimlik doğrulaması istenir. Web URL'si bir tarayıcıda açtığınızda, kodu girin ve kimlik doğrulaması.

    ![Kod ile kimlik doğrulaması](media/deploy/authenticate.png)

7. Betik, birkaç durum iletileri çıkarır. Size bir **başarı** Kiracı başarıyla sağladıysanız durum iletisi.
8. Blockchain Workbench'i URL'ye gidin. Dizini için Okuma izinleri vermek için onay istenir. Bu Blockchain Workbench'i kiracıda kullanıcılara web uygulaması erişim sağlar. Kiracı yöneticisiyseniz, tüm kuruluş için onay seçebilirsiniz. Bu seçenek kiracıdaki tüm kullanıcılar için onay kabul eder. Aksi takdirde, her bir kullanıcı ilk kullanım Blockchain Workbench'i web uygulamasının onay istenir.
9. Seçin **kabul** onay verme.

     ![Kullanıcı profilini okuma izni](media/deploy/graph-permission-consent.png)

10. Onay, Blockchain Workbench'i web uygulaması kullanılabilir.

## <a name="azure-ad-configuration"></a>Azure AD yapılandırması

El ile yapılandırmak veya Azure AD ayarlarını dağıtımından önce doğrulamak isterseniz, bu bölümde açıklanan tüm adımları tamamlayın. Otomatik olarak Azure AD ayarlarını yapılandırmak isterseniz, kullanmak [Azure AD'ye yapılandırma betiğini](#azure-ad-configuration-script) Blockchain Workbench'i dağıtma sonra.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench API'si uygulama kaydı

Blockchain Workbench'i dağıtımı, Azure AD uygulaması kaydı gerektirir. Uygulamayı kaydetmek için bir Azure Active Directory (Azure AD) kiracısı ihtiyacınız vardır. Yeni bir kiracı oluşturmanız ya da mevcut bir kiracıyı kullanın. Mevcut bir Azure AD kiracısı kullanıyorsanız, uygulamaları, Graph API izinleri ve Azure AD kiracısı içinde Konuk erişime izin vermek için yeterli izinlere ihtiyacınız var. Mevcut bir Azure AD kiracısında yeterli izinlere sahip değilsiniz, yeni Kiracı oluşturma.


1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sağ üst köşedeki hesabınızı seçin ve geçiş yapmak istediğiniz Azure AD kiracısına. Kiracı abonelik yöneticinin kiracısı aboneliğin burada Workbench dağıtılır ve uygulamaları kaydetmek için yeterli izinlere sahip olması gerekir.
3. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. Seçin **uygulama kayıtları** > **yeni uygulama kaydı**.

    ![Uygulama kaydı](media/deploy/app-registration.png)

4. Sağlayan bir **adı** ve **oturum açma URL'si** uygulama için. Dağıtım sırasında değerleri değiştiğinden, yer tutucu değerlerini kullanabilirsiniz. 

    ![Uygulama kaydı oluşturma](media/deploy/app-registration-create.png)

    |Ayar  | Değer  |
    |---------|---------|
    |Ad | `Blockchain API` |
    |Uygulama türü |Web uygulaması / API'si|
    |Oturum açma URL'si | `https://blockchainapi` |

5. Seçin **Oluştur** Azure AD uygulaması kaydedilecek.

### <a name="modify-manifest"></a>Bildirimini değiştirin

Ardından, uygulama rolleri Blockchain Workbench'i yöneticileri belirlemek için Azure AD içinde kullanmak için bildirim değiştirmeniz gerekir.  Uygulama bildirimleri hakkında daha fazla bilgi için bkz: [Azure Active Directory Uygulama bildirimini](../../active-directory/develop/reference-app-manifest.md).

1. Uygulama için kaydettiğiniz seçin **bildirim** kayıtlı uygulama ayrıntıları bölmesinde.
2. Bir GUID oluşturun. [GUID] PowerShell komutunu kullanarak bir GUID oluşturabilirsiniz: NewGuid (') veya GUID yeni cmdlet. Başka bir seçenek, bir GUID generator Web sitesi kullanmaktır.
3. Güncelleştirilecek **appRoles** bildiriminin. Düzenleme bildirim bölmesinde seçin **Düzenle** değiştirin `"appRoles": []` sağlanan JSON ile. Değerini değiştirdiğinizden emin olun **kimliği** , oluşturulan GUID ile alan. 

    ![Bildirimi düzenle](media/deploy/edit-manifest.png)

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    > [!IMPORTANT]
    > Değer **yönetici** Blockchain Workbench'i Yöneticiler tanımlamak için gereklidir.

4. Bildirimde, ayrıca değişiklik **Oauth2AllowImplicitFlow** değerini **true**.

    ``` json
    "oauth2AllowImplicitFlow": true,
    ```

5. Seçin **Kaydet** bildirim değişiklikleri kaydedin.

### <a name="add-graph-api-required-permissions"></a>Graph API'si gerekli izinleri Ekle

API uygulaması, kullanıcının dizine erişim için izin istemesi gerekir. API uygulaması için aşağıdaki gerekli izni ayarlayın:

1. Blok zinciri API uygulama kaydında seçin **Ayarları > gerekli izinler > bir API seçin > Microsoft Graph**.

    ![Bir API seçin](media/deploy/client-app-select-api.png)

    **Seç**'e tıklayın.

2. İçinde **erişimini etkinleştir** altında **temsilci izinleri**, seçin **tüm kullanıcıların temel profillerini okuma**.

    ![Erişimi etkinleştir](media/deploy/client-app-read-perms.png)

    Seçin **Kaydet** seçip **Bitti**.

3. İçinde **gerekli izinler**seçin **izinler** seçip **Evet** doğrulama istemi için.

   ![İzinleri verme](media/deploy/client-app-grant-permissions.png)

   İzin verme, Blockchain Workbench'i ' dizininde yer alan kullanıcılar erişim sağlar. Arama ve Blockchain Workbench'i üyeleri eklemek için okuma izni gereklidir.

### <a name="get-application-id"></a>Uygulama Kimliği alma

Dağıtım için uygulama kimliği ve Kiracı bilgileri gereklidir. Toplama ve kullanım bilgilerini dağıtımı sırasında depolayın.

1. Uygulama için kaydettiğiniz seçin **ayarları** > **özellikleri**.
2. İçinde **özellikleri** bölmesinde, kopyalama ve aşağıdaki değerleri daha sonra kullanmak üzere dağıtım sırasında depolama.

    ![API uygulaması özellikleri](media/deploy/app-properties.png)

    | Depolamak için ayarlama  | Kullanma |
    |------------------|-------------------|
    | Uygulama Kimliği | Azure Active Directory Kurulum > Uygulama Kimliği |

### <a name="get-tenant-domain-name"></a>Etki alanı adı kiracısı edinme

Toplama ve uygulamaları kayıtlı olduğu Active Directory Kiracı etki alanı adı depolayın. 

Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. **Özel etki alanı adları**'nı seçin. Kopyala ve etki alanı adını depolar.

![Etki alanı adı](media/deploy/domain-name.png)

### <a name="guest-user-settings"></a>Konuk kullanıcı ayarları

Azure AD kiracınıza Konuk kullanıcılar varsa, Blockchain Workbench'i kullanıcı atama emin olmak için ek adımlar uygulayın ve yönetim düzgün çalışır.

1. Azure AD kiracınıza geçiş ve seçin **Azure Active Directory > Kullanıcı Ayarları > Dış işbirliği ayarlarını yönetin**.
2. Ayarlama **Konuk kullanıcı izinlerini sınırlı** için **Hayır**.
    ![Dış işbirliği ayarları](media/deploy/user-collaboration-settings.png)

## <a name="configuring-the-reply-url"></a>Yanıt URL'si yapılandırılıyor

Azure Blockchain Workbench dağıtıldıktan sonra Azure Active Directory (Azure AD) istemci uygulamasını yapılandırmak zorunda **yanıt URL'si** dağıtılan Blockchain Workbench web URL'si.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD istemci uygulaması kayıtlı olduğu kiracıda olduğunu doğrulayın.
3. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. **Uygulama kayıtları**'nı seçin.
4. Önkoşul bölümünde kayıtlı Azure AD İstemci uygulamayı seçin.
5. Seçin **Ayarları > yanıt URL'lerinin**.
6. Aldığınız, Azure Blockchain Workbench dağıtım ana web URL'sini belirtin **Azure Blockchain Workbench Web URL'sini alma** bölümü. Yanıt URL'si ön ekine sahip `https://`. Örneğin, `https://myblockchain2-7v75.azurewebsites.net`

    ![Yanıt URL'leri](media/deploy/configure-reply-url.png)

7. Seçin **Kaydet** istemci kaydı güncelleştirmek için.

## <a name="remove-a-deployment"></a>Bir dağıtımını Kaldır

Bir dağıtımı artık gerekli olmadığında, bir dağıtım Blockchain Workbench'i kaynak grubunu silerek kaldırabilirsiniz.

1. Azure portalında gidin **kaynak grubu** sol gezinti bölmesi ve silmek istediğiniz kaynak grubunu seçin. 
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adı girerek silme doğrulamak **Sil**.

    ![Kaynak grubunu silme](media/deploy/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, Azure Blockchain Workbench dağıtıldı. Bir blok zinciri uygulaması oluşturmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](create-app.md)
