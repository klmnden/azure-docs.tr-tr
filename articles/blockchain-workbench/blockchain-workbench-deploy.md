---
title: Azure Blockchain çalışma ekranı dağıtma
description: Azure Blockchain çalışma ekranı dağıtma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/17/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e226aadbe499d5905b1814bec5d042f67d898c18
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294858"
---
# <a name="deploy-azure-blockchain-workbench"></a>Azure Blockchain çalışma ekranı dağıtma

Azure Blockchain çalışma ekranı, Azure Marketi'nde bir çözüm şablonu kullanılarak dağıtılır. Şablon blockchain uygulamaları oluşturmak için gerekli olan bileşenlerin dağıtımını basitleştirir. Uygulama dağıtıldıktan sonra Blockchain çalışma ekranı kullanıcılar ve blockchain uygulamalar oluşturmak ve yönetmek için istemci uygulamaları için erişim sağlar.

Blockchain çalışma ekranı bileşenleri hakkında daha fazla bilgi için bkz: [Azure Blockchain çalışma ekranı mimarisi](blockchain-workbench-architecture.md).

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma

Blockchain çalışma ekranı blockchain defter blockchain tabanlı bir uygulama oluşturmak için en sık kullanılan ilgili Azure Hizmetleri kümesi ile birlikte dağıtmanıza olanak tanır. Blockchain çalışma ekranı dağıtma Azure aboneliğinizde bir kaynak grubu içinde sağlanan aşağıdaki Azure hizmetlerinin sonuçlanır.

* 1 olay kılavuz konusu
* 1 Service Bus Namespace
* 1 application Insights
* 1 SQL veritabanı (standart S0)
* 2 uygulama Hizmetleri (standart)
* 2 azure anahtar kasası
* 2 azure depolama hesapları (standart LRS)
* 2 sanal makine ölçek kümeleri (için Doğrulayıcı ve çalışan düğümleri)
* (Yük dengeleyici, ağ güvenlik grubu ve her sanal ağ için genel IP adresi dahil) 2 sanal ağlar
* İsteğe bağlı: Azure İzleyicisi

Oluşturulan örnek dağıtımı aşağıdadır **myblockchain** kaynak grubu.

![Örnek dağıtım](media/blockchain-workbench-deploy/example-deployment.png)

Blockchain çalışma ekranı temel Azure Hizmetleri maliyetini toplama maliyetidir. Azure Hizmetleri kullanılarak hesaplanabilir için fiyatlandırma bilgileri [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).

Azure Blockchain çalışma ekranı dağıtımından önce bazı Önkoşullar gerektirir. Önkoşullar, Azure AD yapılandırma ve uygulama kayıtları içerir.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain çalışma ekranı API uygulama kaydı

Azure AD uygulaması kaydını Blockchain çalışma ekranının dağıtım gerektirir. Bir Azure Active Directory (Azure AD) Kiracı uygulamanızı kaydetmeniz gerekir. Mevcut bir kiracı kullanabilir veya yeni bir kiracı oluşturabilirsiniz. Var olan Azure AD kiracısı kullanıyorsanız, uygulamaları kaydetmek ve Azure AD kiracısı içinde grafik API'si izinleri vermek için yeterli izinleri gerekir. Var olan bir Azure AD kiracısında yeterli izinleri yoksa yeni bir kiracı oluşturun. 

> [!IMPORTANT]
> Çalışma ekranı Azure AD uygulaması kaydetmek için kullandığınız hesapla aynı Kiracı dağıtılması gerekmez. Çalışma ekranı kaynakları dağıtmak için yeterli izinlere sahip olduğu bir kiracı dağıtılması gerekir. Azure AD kiracılarıyla hakkında daha fazla bilgi için bkz: [bir Active Directory kiracısı alma](../active-directory/develop/active-directory-howto-tenant.md) ve [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kiracı hesabınızda sağ üst köşe ve geçiş yapmak istediğiniz Azure AD seçin. Kiracı abonelik yöneticisinin Kiracı abonelik burada çalışma ekranı dağıtılır ve uygulamalarını kaydetmek için yeterli izinlere sahip olması gerekir.
3. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet. Seçin **uygulama kayıtlar** > **yeni uygulama kaydı**.

    ![Uygulama kaydı](media/blockchain-workbench-deploy/app-registration.png)

4. Sağlayan bir **adı** ve **oturum açma URL'si** uygulama için. Dağıtım sırasında değerleri değiştiğinden yer tutucu değerlerini kullanabilirsiniz. 

    ![Uygulama kaydı oluşturun](media/blockchain-workbench-deploy/app-registration-create.png)

    |Ayar  | Değer  |
    |---------|---------|
    |Ad | `Blockchain API` |
    |Uygulama türü |Web uygulaması / API'si|
    |Oturum Açma URL'si | `https://blockchainapi` |

5. Seçin **oluşturma** Azure AD uygulaması kaydetmek için.

### <a name="modify-application-manifest"></a>Uygulama bildirimini değiştirin

Ardından, uygulama bildirimini uygulama rolleri Blockchain çalışma ekranı yöneticileri belirlemek için Azure AD içinde kullanmak için değiştirmeniz gerekir.  Uygulama bildirimleri hakkında daha fazla bilgi için bkz: [Azure Active Directory Uygulama bildirimini](../active-directory/develop/active-directory-application-manifest.md).

1. Uygulamanın, kayıtlı seçin **bildirim** kayıtlı uygulama Ayrıntılar bölmesinde.
2. Bir GUID oluşturur. [GUID] PowerShell komutunu kullanarak bir GUID oluşturabileceğiniz:: NewGuid () veya GUID yeni cmdlet. GUID Oluşturucu Web sitesi başka bir seçenektir.
3. Güncelleştirilecek **appRoles** bildirim bölümü. Düzen bildirim bölmesinde seçin **Düzenle** ve değiştirme `"appRoles": []` sağlanan JSON ile. Değeri değiştirdiğinizden emin olun **kimliği** , oluşturulan GUID ile alan. 

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
    > Değer **yönetici** Blockchain çalışma ekranı Yöneticiler tanımlamak için gereklidir.

4.  Tıklatın **kaydetmek** uygulama bildirim değişiklikleri kaydedin.

### <a name="add-graph-api-required-permissions"></a>Grafik API'si gerekli izinleri ekleyin

Dizine erişmek için kullanıcıdan izin istemek API uygulaması gerekir. API uygulaması için aşağıdaki gerekli izinleri ayarlayın:

1. Blockchain API uygulama kaydı seçin **ayarlar > gerekli izinleri > bir API seçin > Microsoft Graph**.

    ![Bir API seçin](media/blockchain-workbench-deploy/client-app-select-api.png)

    **Seç**'e tıklayın.

2. İçinde **erişimi etkinleştir** altında **uygulama izinleri**, seçin **tüm kullanıcıların tüm profilini okuma**.

    ![Erişimi etkinleştir](media/blockchain-workbench-deploy/client-app-read-perms.png)

    Tıklatın **seçin** ardından **Bitti**.

3. İçinde **gerekli izinleri**seçin **izinler** seçip **Evet** doğrulama istemi için.

   ![İzinleri verme](media/blockchain-workbench-deploy/client-app-grant-permissions.png)

   İzin verme Blockchain dizindeki kullanıcıları erişmek çalışma ekranı sağlar. Arama ve Blockchain çalışma ekranına üye eklemek için okuma izni gereklidir.

### <a name="add-graph-api-key-to-application"></a>Grafik API'si anahtarı için uygulama ekleme

Blockchain çalışma ekranı blockchain uygulamaları ile etkileşim kullanıcılar için asıl kimlik yönetimi sistemi olarak Azure AD kullanır. Blockchain Azure AD erişim ve adlarını ve e-postalar, gibi kullanıcı bilgilerini almak çalışma ekranı sırada erişim tuşu eklemeniz gerekir. Blockchain çalışma ekranı anahtarı Azure AD ile kimlik doğrulaması yapmak için kullanır.

1. Uygulamanın, kayıtlı seçin **ayarları** kayıtlı uygulama Ayrıntılar bölmesinde.
2. **Anahtarlar**’ı seçin.
3. Bir anahtarı belirterek yeni bir anahtar ekleyin **açıklama** ve seçme **süresi** süre değeri. 

    ![Anahtar oluştur](media/blockchain-workbench-deploy/app-key-create.png)

    |Ayar  | Değer  |
    |---------|---------|
    | Açıklama | `Service` |
    | Süre Sonu: | Bir sona erme süresini seçin |

4. **Kaydet**’i seçin. 
5. Anahtarın değerini kopyalayın ve daha sonra kullanmak üzere saklayın. Bu dağıtım için gerekir.

    > [!IMPORTANT]
    >  Dağıtım için anahtar kaydetmezseniz, yeni bir anahtar oluşturmak gerekir. Portaldan daha sonra anahtar değeri alınamıyor.

### <a name="get-application-id"></a>Uygulama Kimliği alma

Dağıtım için uygulama kimliği ve Kiracı bilgileri gereklidir. Toplama ve kullanım için dağıtım sırasında bilgileri depolar.

1. Uygulamanın, kayıtlı seçin **ayarları** > **özellikleri**.
2.  İçinde **özellikleri** bölmesi, kopyalama ve aşağıdaki değerleri daha sonra kullanmak için dağıtım sırasında deposu.

    ![API uygulama özellikleri](media/blockchain-workbench-deploy/app-properties.png)

    | Depolamak için ayarlama  | Dağıtımda kullanın |
    |------------------|-------------------|
    | Uygulama Kimliği | Azure Active Directory Kurulum > Uygulama Kimliği |

### <a name="get-tenant-domain-name"></a>Kiracı etki alanı adını Al

Toplamak ve mağaza uygulamaları, kayıtlı Active Directory Kiracı etki alanı adı. 

Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet. Seçin **özel etki alanı adları**. Kopyalama ve etki alanı adını depolar.

![Etki alanı adı](media/blockchain-workbench-deploy/domain-name.png)

## <a name="deploy-blockchain-workbench"></a>Blockchain çalışma ekranı dağıtma

Önkoşul adımları tamamladıktan sonra Blockchain çalışma ekranı dağıtmaya hazır olursunuz. Aşağıdaki bölümlerde framework dağıtma konusunda verilmiştir.

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Hesabınızı Azure Blockchain çalışma ekranı dağıtmak istediğiniz köşe ve anahtar istenen Azure ad Kiracı sağ üst seçin.
3.  Sol bölmede seçin **kaynak oluşturma**. Arama `Azure Blockchain Workbench` içinde **Market arama** arama çubuğu. 

    ![Market arama çubuğu](media/blockchain-workbench-deploy/marketplace-search-bar.png)

4.  Seçin **Azure Blockchain çalışma ekranı**.

    ![Market arama sonuçları](media/blockchain-workbench-deploy/marketplace-search-results.png)

4.  **Oluştur**’u seçin.
5.  Temel ayarlar tamamlayın.

    ![Azure Blockchain çalışma ekranı oluşturma](media/blockchain-workbench-deploy/blockchain-workbench-settings-basic.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Kaynak öneki | Dağıtımınız için kısa benzersiz tanımlayıcısı. Bu değer, kaynakları adlandırmak için temel olarak kullanılır. |
    | VM kullanıcı adı | Kullanıcı adı yönetici olarak tüm sanal makineler (VM) için kullanılır. |
    | Kimlik doğrulaması türü | Bir parola kullanın veya Vm'lere bağlanması için anahtar isteyip istemediğinizi seçin. |
    | Parola | Parola, Vm'lere bağlanması için kullanılır. |
    | SSH | Tek satırlı biçimi başlayarak bir RSA ortak anahtarı kullanmak **ssh-rsa** veya çok satırlı PEM biçimini kullanın. SSH anahtarlarını kullanarak oluşturabileceğiniz `ssh-keygen` Linux ve OS X ya da Windows'da PuTTYGen kullanarak. SSH anahtarları hakkında daha fazla bilgi [SSH kullanma anahtarları azure'da Windows ile](../virtual-machines/linux/ssh-from-windows.md). |
    | Parola veritabanı / veritabanı parolayı onaylayın | Dağıtımın bir parçası oluşturulan veritabanına erişim için kullanılacak parolayı belirtin. |
    | Dağıtım bölgesi | Blockchain çalışma ekranı kaynakları dağıtmak istediğiniz yeri belirtin. En iyi kullanılabilirlik için bu eşleşmelidir **konumu** ayarı. |
    | Abonelik | Dağıtımınız için kullanmak istediğiniz Azure aboneliği belirtin. |
    | Kaynak grupları | Seçerek yeni bir kaynak grubu oluşturun **Yeni Oluştur** ve benzersiz kaynak grubu adı belirtin. |
    | Konum | Framework dağıtmak istediğiniz bölgeyi belirtin. |

6.  Seçin **Tamam** temel yapılandırma bölümü tamamlamak için.

7.  Tamamlamak **Azure Active Directory Kurulum**.

    ![Azure AD Kurulumu](media/blockchain-workbench-deploy/blockchain-workbench-settings-aad.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Etki Alanı Adı | Kullanım Azure AD Kiracı toplanan [Get Kiracı etki alanı adı](#get-tenant-domain-name) önkoşul bölümü. |
    | Uygulama Kimliği | Kullanım Blockchain istemci uygulaması kaydını uygulama Kimliğinden toplanan [uygulama kimliği alma](#get-application-id) önkoşul bölümü. |
    | Uygulama Anahtarı | İçinde toplanan Blockchain istemci uygulaması kaydını uygulama anahtarından kullanmak [uygulama Ekle grafik API'si anahtarına](#add-graph-api-key-to-application) önkoşul bölümü. |


8.  Tıklatın **Tamam** Azure AD parametreleri yapılandırma bölümü tamamlamak için.

9.  Tamamlamak **ağ boyutu ve performans** ayarlar.

    ![Ağ ve performans ayarları](media/blockchain-workbench-deploy/blockchain-workbench-settings-network.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Blockchain düğüm sayısı | Ethereum PoA sayısını ağınızda dağıtılacak Doğrulayıcı düğümleri seçin. |
    | Depolama performansı | Tercih edilen VM depolama performansı blockchain ağınız için seçin. |
    | Sanal makine boyutu | Blockchain ağınız için tercih edilen VM boyutunu seçin. |

10. Seçin **Tamam** ağ boyutu ve performans bölümünde tamamlamak için.

11. Tamamlamak **Azure İzleyici** ayarlar.

    ![Azure İzleyici](media/blockchain-workbench-deploy/blockchain-workbench-settings-oms.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | İzleme | Azure blockchain ağınızda izlemek izleyicisini etkinleştirmek isteyip istemediğinizi seçin |
    | Var olan günlük analizi bağlanın | Varolan bir kullanmak mı istediğinizi günlük analizi örneği veya yeni bir tane oluşturun seçin. Var olan bir örneğini kullanıyorsanız, çalışma alanı kimliği ve birincil anahtarınızı girin. |

12. Tıklatın **Tamam** Azure İzleyici bölüm tamamlamak için.

13. Parametrelerinizi doğru doğrulamak için özeti gözden geçirin.

    ![Özet](media/blockchain-workbench-deploy/blockchain-workbench-summary.png)

14. Seçin **oluşturma** , Azure Blockchain çalışma ekranı dağıtıp koşullarını kabul ediyorsunuz.

Dağıtım 90 dakikaya kadar sürebilir. İlerleme durumunu izlemek için Azure portalını kullanabilirsiniz. Yeni oluşturulan kaynak grubunda seçin **dağıtımlar > Genel Bakış** dağıtılan yapıları durumunu görmek için.

## <a name="blockchain-workbench-web-url"></a>Blockchain çalışma ekranı Web URL'si

Blockchain çalışma ekranının dağıtım tamamlandıktan sonra yeni bir kaynak grubu Blockchain çalışma ekranı kaynaklarınızı içerir. Blockchain çalışma ekranı Hizmetleri web URL aracılığıyla erişilir. Aşağıdaki adımlar dağıtılan framework web URL'sini alma gösterir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol gezinti bölmesinde seçin **kaynak grupları**
3. Blockchain çalışma ekranı dağıtırken belirtilen kaynak grubu adı seçin.
4. Tıklatın **türü** türüne göre alfabetik olarak sıralamak için sütun başlığını.
5. İki kaynak türüyle **uygulama hizmeti**. Kaynak türü seçin **uygulama hizmeti** *olmadan* "-API" soneki.

    ![Uygulama hizmet listesi](media/blockchain-workbench-deploy/resource-group-list.png)

6.  App Service'te **Essentials** bölümünde, kopyalama **URL** web URL'si, dağıtılan Blockchain çalışma ekranına gösteren değer.

    ![Uygulama hizmeti temelleri](media/blockchain-workbench-deploy/app-service.png)

Özel etki alanı adı Blockchain çalışma ekranı ile ilişkilendirmek için bkz: [trafik Yöneticisi'ni kullanarak Azure App Service içinde bir web uygulaması için bir özel etki alanı adı yapılandırma](../app-service/web-sites-traffic-manager-custom-domain-name.md).

## <a name="configuring-the-reply-url"></a>Yanıt URL'si yapılandırma

Azure Blockchain çalışma ekranı dağıtıldıktan sonra sonraki adım Azure Active Directory (Azure AD) istemci uygulamasının doğru kaydedildiğinden emin olmaktır **yanıt URL'si** dağıtılan Blockchain çalışma ekranı URL web.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD istemci uygulaması, kayıtlı Kiracı içinde olduğundan emin olun.
3. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet. **Uygulama kayıtları**'nı seçin.
4. Önkoşul bölümünde kayıtlı Azure AD istemci uygulaması'nı seçin.
5. Seçin **ayarlar > Yanıtla URL'leri**.
6. Alınan içinde Azure Blockchain çalışma ekranının dağıtım ana web URL'sini belirtin **Azure Blockchain çalışma ekranı Web URL'sini alma** bölümü. Yanıt URL'si önekine sahip `https://`. Örneğin, `https://myblockchain2-7v75.azurewebsites.net`

    ![Yanıt URL'leri](media/blockchain-workbench-deploy/configure-reply-url.png)

7. Seçin **kaydetmek** istemci kaydını güncelleştirmek için.

## <a name="remove-a-deployment"></a>Bir dağıtım Kaldır

Bir dağıtım artık gerekli olmadığında Blockchain çalışma ekranı kaynak grubunu silerek bir dağıtım kaldırabilirsiniz.

1. Azure portalında gidin **kaynak grubu** sol gezinti bölmesinde ve silmek istediğiniz kaynak grubunu seçin. 
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adı girerek silme işlemini doğrulamak ve seçin **silmek**.

    ![Kaynak grubunu silme](media/blockchain-workbench-deploy/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede, Azure Blockchain çalışma ekranı dağıtıldı. Blockchain uygulama oluşturmayı öğrenmek için sonraki nasıl yapılır makalesi için devam edin.

> [!div class="nextstepaction"]
> [İçinde Azure Blockchain çalışma ekranı blockchain uygulaması oluşturma](blockchain-workbench-create-app.md)
