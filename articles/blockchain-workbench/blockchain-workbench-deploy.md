---
title: Azure Blockchain Workbench'i dağıtma
description: Azure Blockchain Workbench'i dağıtma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 7/13/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 57b610b40edff56207617e212d0eb6e591ad50d4
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224305"
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain Workbench'i dağıtma

Azure Blockchain Workbench, Azure Market'te çözüm şablonu kullanılarak dağıtılır. Blok zinciri uygulamaları oluşturmak için gereken bileşenlerin dağıtımına ilişkin şablonu basitleştirir. Dağıtıldıktan sonra Blockchain Workbench'i istemci uygulamalar, kullanıcılar ve blok zinciri uygulamaları oluşturup yönetmek için erişim sağlar.

Blockchain Workbench'i bileşenleri hakkında daha fazla bilgi için bkz. [Azure Blockchain Workbench mimarisi](blockchain-workbench-architecture.md).

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

![Örnek dağıtım](media/blockchain-workbench-deploy/example-deployment.png)

Blockchain Workbench'i temel alınan Azure hizmetlerinin maliyetinin toplama maliyetidir. Azure Hizmetleri kullanılarak hesaplanabilir için fiyatlandırma bilgilerine [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/).

Azure Blockchain Workbench dağıtımdan önce bazı Önkoşullar gerektirir. Önkoşullar, Azure AD, yapılandırma ve uygulama kayıtları içerir.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench'i API uygulama kaydı

Blockchain Workbench'i dağıtımı, Azure AD uygulaması kaydı gerektirir. Uygulamayı kaydetmek için bir Azure Active Directory (Azure AD) kiracısı ihtiyacınız vardır. Yeni bir kiracı oluşturmanız ya da mevcut bir kiracıyı kullanın. Mevcut bir Azure AD kiracısı kullanıyorsanız, uygulamaları ve Azure AD kiracısı içinde Graph API izinleri vermek için yeterli izinlere ihtiyacınız var. Mevcut bir Azure AD kiracısında yeterli izinlere sahip değilsiniz, yeni Kiracı oluşturma. 

> [!IMPORTANT]
> Workbench olarak bir Azure AD uygulaması kaydetmek için kullandığınız aynı kiracıda dağıtılması gerekmez. Workbench kaynakları dağıtmak için yeterli izinlere sahip olduğu bir kiracıda dağıtılması gerekir. Azure AD kiracılarıyla hakkında daha fazla bilgi için bkz. [bir Active Directory kiracısı edinme](../active-directory/develop/active-directory-howto-tenant.md) ve [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kiracı hesabınız sağ üst köşe ve geçiş yapmak istediğiniz Azure AD'ye seçin. Kiracı abonelik yöneticinin kiracısı aboneliğin burada Workbench dağıtılır ve uygulamaları kaydetmek için yeterli izinlere sahip olması gerekir.
3. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti. Seçin **uygulama kayıtları** > **yeni uygulama kaydı**.

    ![Uygulama kaydı](media/blockchain-workbench-deploy/app-registration.png)

4. Sağlayan bir **adı** ve **oturum açma URL'si** uygulama için. Dağıtım sırasında değerleri değiştiğinden, yer tutucu değerlerini kullanabilirsiniz. 

    ![Uygulama kaydı oluşturma](media/blockchain-workbench-deploy/app-registration-create.png)

    |Ayar  | Değer  |
    |---------|---------|
    |Ad | `Blockchain API` |
    |Uygulama türü |Web uygulaması / API'si|
    |Oturum Açma URL'si | `https://blockchainapi` |

5. Seçin **Oluştur** Azure AD uygulaması kaydedilecek.

### <a name="modify-application-manifest"></a>Uygulama bildiriminde değişiklik yapma

Ardından, uygulama rolleri Blockchain Workbench'i yöneticileri belirlemek için Azure AD içinde kullanmak için uygulama bildirimini değiştirmeniz gerekir.  Uygulama bildirimleri hakkında daha fazla bilgi için bkz: [Azure Active Directory Uygulama bildirimini](../active-directory/develop/active-directory-application-manifest.md).

1. Uygulama için kaydettiğiniz seçin **bildirim** kayıtlı uygulama ayrıntıları bölmesinde.
2. Bir GUID oluşturun. [GUID] PowerShell komutunu kullanarak bir GUID oluşturabileceğiniz:: NewGuid () veya GUID yeni cmdlet. Başka bir seçenek, bir GUID generator Web sitesi kullanmaktır.
3. Güncelleştirilecek **appRoles** bildiriminin. Düzenleme bildirim bölmesinde seçin **Düzenle** değiştirin `"appRoles": []` sağlanan JSON ile. Değerini değiştirdiğinizden emin olun **kimliği** , oluşturulan GUID ile alan. 

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

    ![Bildirimi düzenle](media/blockchain-workbench-deploy/edit-manifest.png)

    > [!IMPORTANT]
    > Değer **yönetici** Blockchain Workbench'i Yöneticiler tanımlamak için gereklidir.

4.  Tıklayın **Kaydet** uygulama bildirim değişiklikleri kaydedin.

### <a name="add-graph-api-required-permissions"></a>Graph API'si gerekli izinleri Ekle

API uygulaması, kullanıcının dizine erişim için izin istemesi gerekir. API uygulaması için aşağıdaki gerekli izni ayarlayın:

1. Blok zinciri API uygulama kaydında seçin **Ayarları > gerekli izinler > bir API seçin > Microsoft Graph**.

    ![Bir API seçin](media/blockchain-workbench-deploy/client-app-select-api.png)

    **Seç**'e tıklayın.

2. İçinde **erişimini etkinleştir** altında **uygulama izinleri**, seçin **tüm kullanıcıların tüm profilini okuma**.

    ![Erişimi etkinleştir](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Tıklayın **seçin** ardından **Bitti**.

3. İçinde **gerekli izinler**seçin **izinler** seçip **Evet** doğrulama istemi için.

   ![İzinleri verme](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   İzin verme, Blockchain Workbench'i ' dizininde yer alan kullanıcılar erişim sağlar. Arama ve Blockchain Workbench'i üyeleri eklemek için okuma izni gereklidir.

### <a name="add-graph-api-key-to-application"></a>Graph API anahtarı uygulama ekleme

Blockchain Workbench ile blok zinciri uygulamaları etkileşimde bulunabilecek kullanıcı için ana kimlik yönetim sistemi olarak Azure AD kullanır. Azure AD erişim ve adları ve e-postalar gibi kullanıcı bilgileri almak Blockchain Workbench'i sırada bir erişim anahtarı eklemeniz gerekir. Blockchain Workbench'i Azure AD kimlik doğrulaması için anahtar kullanır.

1. Uygulama için kaydettiğiniz seçin **ayarları** kayıtlı uygulama ayrıntıları bölmesinde.
2. **Anahtarlar**’ı seçin.
3. Bir anahtarı belirterek yeni bir anahtar ekleyin **açıklama** seçip **süresi** süre değeri. 

    ![Anahtar oluştur](media/blockchain-workbench-deploy/app-key-create.png)

    |Ayar  | Değer  |
    |---------|---------|
    | Açıklama | `Service` |
    | Süre Sonu: | Sona erme süresi seçin |

4. **Kaydet**’i seçin. 
5. Anahtar değerini kopyalayın ve daha sonra kullanmak üzere saklayın. Bu dağıtım için gerekir.

    > [!IMPORTANT]
    >  Dağıtım için anahtar kaydetmezseniz, yeni bir anahtar oluşturmak gerekir. Daha sonra portaldan anahtar değeri alınamıyor.

### <a name="get-application-id"></a>Uygulama Kimliği alma

Dağıtım için uygulama kimliği ve Kiracı bilgileri gereklidir. Toplama ve kullanım bilgilerini dağıtımı sırasında depolayın.

1. Uygulama için kaydettiğiniz seçin **ayarları** > **özellikleri**.
2.  İçinde **özellikleri** bölmesinde, kopyalama ve aşağıdaki değerleri daha sonra kullanmak üzere dağıtım sırasında depolama.

    ![API uygulaması özellikleri](media/blockchain-workbench-deploy/app-properties.png)

    | Depolamak için ayarlama  | Kullanma |
    |------------------|-------------------|
    | Uygulama Kimliği | Azure Active Directory Kurulum > Uygulama Kimliği |

### <a name="get-tenant-domain-name"></a>Etki alanı adı kiracısı edinme

Toplama ve uygulamaları kayıtlı olduğu Active Directory Kiracı etki alanı adı depolayın. 

Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti. **Özel etki alanı adları**'nı seçin. Kopyala ve etki alanı adını depolar.

![Etki alanı adı](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Blockchain Workbench'i dağıtma

Önkoşul adımlarını tamamladıktan sonra Blockchain Workbench'i dağıtma hazırsınız. Aşağıdaki bölümlerde, framework dağıtmak nasıl yenileyeceğiniz özetlenmektedir.

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Azure Blockchain Workbench'i dağıtma istediğiniz köşe ve anahtar istediğiniz Azure ad Kiracı sağ üst bölümde hesabınızı seçin.
3.  Sol bölmede seçin **kaynak Oluştur**. Arama `Azure Blockchain Workbench` içinde **markette Ara** arama çubuğu. 

    ![Market arama çubuğu](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Seçin **Azure Blockchain Workbench'i**.

    ![Market arama sonuçları](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  **Oluştur**’u seçin.
5.  Temel ayarlar tamamlayın.

    ![Azure Blockchain Workbench'i oluşturma](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Kaynak ön eki | Dağıtımınız için kısa benzersiz tanımlayıcısı. Bu değer, kaynakları adlandırma için temel olarak kullanılır. |
    | VM kullanıcı adı | Kullanıcı adı, yönetici olarak tüm sanal makineler (VM) için kullanılır. |
    | Kimlik doğrulaması türü | Vm'lere bağlanmak için anahtar veya bir parola kullanmak isteyip istemediğinizi seçin. |
    | Parola | Parola, Vm'lere bağlanmak için kullanılır. |
    | SSH | RSA ortak anahtarını tek satırlı biçimde başlayarak kullanmak **ssh-rsa** veya çok satırlı PEM biçimi kullanın. Kullanarak SSH anahtarları oluşturabilirsiniz `ssh-keygen` Linux ve OS X veya Windows üzerinde puttygen araçlarını kullanarak. SSH anahtarları hakkında daha fazla bilgi [azure'da Windows ile SSH kullanma anahtarları](../virtual-machines/linux/ssh-from-windows.md). |
    | Veritabanı parolası / veritabanı parolasını onaylayın | Dağıtımın bir parçası oluşturduğunuz veritabanına erişim için kullanılacak parolayı belirtin. |
    | Dağıtım bölgesi | Blockchain Workbench'i kaynakların dağıtılacağı yeri belirtin. En iyi kullanılabilirlik için bu eşleşmelidir **konumu** ayarı. |
    | Abonelik | Dağıtımınız için kullanmak istediğiniz Azure aboneliği belirtin. |
    | Kaynak grupları | Seçerek yeni bir kaynak grubu oluşturma **Yeni Oluştur** ve benzersiz kaynak grubu adı belirtin. |
    | Konum | Framework dağıtmak istediğiniz bölgeyi belirtin. |

6.  Seçin **Tamam** temel yapılandırma bölümü tamamlanması.

7.  Tamamlamak **Azure Active Directory Kurulum**.

    ![Azure AD Kurulumu](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Etki Alanı Adı | Toplanan kullanım Azure AD Kiracı içinde [Get Kiracı etki alanı adı](#get-tenant-domain-name) konusunun önkoşul bölümüne. |
    | Uygulama Kimliği | Uygulama Kimliği blok zinciri istemci uygulama kaydını kullanmak toplanan içinde [uygulama kimliği alma](#get-application-id) konusunun önkoşul bölümüne. |
    | Uygulama Anahtarı | Uygulama anahtarı toplanan Blockchain istemci uygulama kaydını kullanmak [uygulamaya ekleme Graph API anahtarı](#add-graph-api-key-to-application) konusunun önkoşul bölümüne. |


8.  Tıklayın **Tamam** Azure AD parametreleri yapılandırma bölümü tamamlanması.

9.  İçinde **ağ ayarları ve performans**, yeni bir blok zinciri ağ oluşturma veya var olan bir yetki başlangıcı kavram blockchain ağı kullanmak isteyip istemediğinizi seçin.

    İçin **Yeni Oluştur**:

    *Yeni Oluştur* seçeneği, bir dizi içindeki tek bir üyenin abonelik Ethereum kavram, (PoA) yetkilisi düğümü oluşturur. 

    ![Ağ ayarları ve performans](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-new.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Blok zinciri düğüm sayısı | Ağınızda dağıtılan Doğrulayıcı düğümlerin Ethereum PoA sayısını seçin. |
    | Depolama performansı | Blok zinciri ağınız için tercih edilen VM depolama performansı seçin. |
    | Sanal makine boyutu | Blok zinciri ağınız için tercih edilen VM boyutunu seçin. |

    İçin **var olanı kullan**:

    *Var olanı kullan* seçeneği, bir Ethereum kavram yetki başlangıcı (PoA) blok zinciri ağ belirtmenize olanak sağlar. Uç noktaları aşağıdaki gereksinimlere sahiptir.

    * Uç nokta bir Ethereum kavram yetki başlangıcı (PoA) blok zinciri ağ olması gerekir.
    * Uç noktası, ağ üzerinden genel olarak erişilebilir olması gerekir.
    * PoA blockchain ağ sıfır gaz fiyat için yapılandırılmış olması gerekir (Not: Blockchain Workbench'i hesapları finanse değil. İşlem başarısız) fon gerekiyorsa.

    ![Ağ ayarları ve performans](media/blockchain-workbench-deploy/blockchain-workbench-settings-network-existing.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Ethereum RPC bitiş noktası | Var olan PoA blok zinciri ağı RPC uç noktası sağlar. Uç nokta, http:// ile başlayan ve bir bağlantı noktası numarası ile sona erer. Örneğin, `http://contoso-chain.onmicrosoft.com:8545` |
    | Depolama performansı | Blok zinciri ağınız için tercih edilen VM depolama performansı seçin. |
    | Sanal makine boyutu | Blok zinciri ağınız için tercih edilen VM boyutunu seçin. |

10. Seçin **Tamam** ağ ayarlarını ve performans tamamlamak için.

11. Tamamlamak **Azure İzleyici** ayarları.

    ![Azure İzleyici](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | İzleme | Azure blockchain ağınızı izlemek izleyicisini etkinleştirmek isteyip istemediğinizi seçin |
    | Mevcut Log Analytics örneğine bağlanın | Log Analytics, mevcut bir kullanmak isteyip istemediğinizi örneği veya yeni bir tane oluşturun seçin. Var olan bir örneğini kullanıyorsanız, çalışma alanı kimliği ve birincil anahtarınızı girin. |

12. Tıklayın **Tamam** Azure İzleyici bölümünde tamamlanması.

13. Parametrelerinizi doğru olduğunu doğrulamak için özeti gözden geçirin.

    ![Özet](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Seçin **Oluştur** , Azure Blockchain Workbench'i dağıtma ve koşulları kabul etmiş olursunuz.

Dağıtım 90 dakikaya kadar sürebilir. Azure portalı, ilerlemeyi izlemek için kullanabilirsiniz. Yeni oluşturulan kaynak grubunda seçin **dağıtımları > Genel Bakış** dağıtılan yapıtlar durumunu görmek için.

## <a name="blockchain-workbench-web-url"></a>Blockchain Workbench'i Web URL'si

Blockchain Workbench'i dağıtımı tamamlandıktan sonra yeni bir kaynak grubu Blockchain Workbench'i kaynaklarınızı içerir. Blockchain Workbench'i Hizmetleri web URL aracılığıyla erişilir. Aşağıdaki adımlar dağıtılan framework'ün web URL'si almak nasıl gösterir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol gezinti bölmesinde seçin **kaynak grupları**
3. Blockchain Workbench'i dağıtırken belirtilen kaynak grubu adı seçin.
4. Tıklayın **türü** türüne göre alfabetik olarak sıralamak için sütun başlığını.
5. İki kaynak türüyle **App Service**. Kaynak türü seçin **App Service** *olmadan* "-API" soneki.

    ![Uygulama hizmet listesi](media/blockchain-workbench-deploy/resource-group-list.png)

6.  App Service **Essentials** bölümünde, kopya **URL** web URL'si, dağıtılan Blockchain Workbench'i temsil eden bir değer.

    ![App service temel bileşenleri](media/blockchain-workbench-deploy/app-service.png)

Özel etki alanı Blockchain Workbench ile ilişkilendirmek için bkz: [Traffic Manager'ı kullanarak Azure App Service içinde bir web uygulaması için özel etki alanı adı yapılandırma](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>Yanıt URL'si yapılandırılıyor

Azure Blockchain Workbench dağıtıldıktan sonra sonraki adımda Azure Active Directory (Azure AD) istemci uygulamayı doğru kayıtlı emin olmaktır **yanıt URL'si** dağıtılan Blockchain Workbench web URL'si.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD istemci uygulaması kayıtlı olduğu kiracıda olduğunu doğrulayın.
3. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti. **Uygulama kayıtları**'nı seçin.
4. Önkoşul bölümünde kayıtlı Azure AD İstemci uygulamayı seçin.
5. Seçin **Ayarları > yanıt URL'lerinin**.
6. Aldığınız, Azure Blockchain Workbench dağıtım ana web URL'sini belirtin **Azure Blockchain Workbench Web URL'sini alma** bölümü. Yanıt URL'si ön ekine sahip `https://`. Örneğin, `https://myblockchain2-7v75.azurewebsites.net`

    ![Yanıt URL'leri](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Seçin **Kaydet** istemci kaydı güncelleştirmek için.

## <a name="remove-a-deployment"></a>Bir dağıtımını Kaldır

Bir dağıtımı artık gerekli olmadığında, bir dağıtım Blockchain Workbench'i kaynak grubunu silerek kaldırabilirsiniz.

1. Azure portalında gidin **kaynak grubu** sol gezinti bölmesi ve silmek istediğiniz kaynak grubunu seçin. 
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adı girerek silme doğrulamak **Sil**.

    ![Kaynak grubunu silme](media/blockchain-workbench-deploy/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, Azure Blockchain Workbench dağıtıldı. Bir blok zinciri uygulaması oluşturmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench uygulamasında bir blok zinciri uygulaması oluşturma](blockchain-workbench-create-app.md)
