---
title: Uç nokta kotayı nda dil anlama (HALUK) - Azure artırmak için Microsoft Azure trafik Yöneticisi kullanın | Microsoft Docs
description: İçinde dil anlama (uç nokta Kotayı artırmak için HALUK) birkaç Aboneliklerdeki uç nokta kota yaymak için Microsoft Azure trafik Yöneticisi'ni kullanın
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/07/2018
ms.author: v-geberr
ms.openlocfilehash: b5849e0b4d9b7fb1f681a52aef2c403031ff0144
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356354"
---
# <a name="use-microsoft-azure-traffic-manager-to-manage-endpoint-quota-across-keys"></a>Microsoft Azure trafik Yöneticisi uç noktası kota anahtarlarını yönetmek için kullanın
Dil anlama (HALUK) tek bir anahtarın kota ötesinde uç nokta isteği Kotayı artırmak için olanak sağlar. Bu HALUK için daha fazla anahtarları oluşturarak ve bunları HALUK uygulamaya ekleyerek yapılır **Yayımla** sayfasındaki **kaynakları ve anahtarları** bölümü. 

Anahtarları arasında trafiği yönetmek istemci uygulaması sahiptir. HALUK, yapmaz. 

Bu makalede Azure ile anahtarlar arasındaki trafiği yönetmek nasıl açıklanmaktadır [trafik Yöneticisi][traffic-manager-marketing]. Zaten bir eğitilmiş ve yayımlanan HALUK uygulamanız gerekir. Önceden oluşturulmuş etki alanı bir yoksa izleyin [Hızlı Başlangıç](luis-get-started-create-app.md). 

## <a name="connect-to-powershell-in-the-azure-portal"></a>Azure portalında için PowerShell'i bağlayın
İçinde [Azure] [ azure-portal] portal, PowerShell penceresini açın. PowerShell penceresinde simgesi **> _** üst gezinti çubuğunda. Portaldan PowerShell kullanarak, son PowerShell sürümü almak ve kimlik doğrulaması yapılır. PowerShell portalında gerektiriyor bir [Azure Storage](https://azure.microsoft.com/services/storage/) hesabı. 

![Ekran görüntüsü, Azure portal ile Powershell penceresini açın](./media/traffic-manager/azure-portal-powershell.png)

Aşağıdaki bölümlerde kullanım [trafik Yöneticisi PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/?view=azurermps-6.2.0#traffic_manager).

## <a name="create-azure-resource-group-with-powershell"></a>PowerShell ile Azure kaynak grubu oluşturma
Azure kaynaklarını oluşturmadan önce tüm kaynakları içerecek şekilde bir kaynak grubu oluşturun. Kaynak grubu adı `luis-traffic-manager` ve bölgeyi kullanmak `West US`. Kaynak grubunun bölge grubu ile ilgili meta verileri depolar. Başka bir bölgede olmaları durumunda kaynaklarınızı yavaş olmaz. 

Kaynak grubu ile oluşturma **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-6.2.0)** cmdlet:

```PowerShell
New-AzureRmResourceGroup -Name luis-traffic-manager -Location "West US"
```

## <a name="create-luis-keys-to-increase-total-endpoint-quota"></a>HALUK toplam endpoint Kotayı artırmak için anahtarları oluşturma
1. Azure portalında iki oluşturma **dil anlama** anahtarları, birinde `West US` tane `East US`. Adlı önceki bölümde oluşturduğunuz var olan kaynak grubu kullanmak `luis-traffic-manager`. 

    ![Ekran görüntüsü, Azure portal Haluk trafik Yöneticisi kaynak grubunda iki HALUK anahtarlarla](./media/traffic-manager/luis-keys.png)

2. İçinde [HALUK] [ LUIS] Web sitesi, **Yayımla** sayfasında, uygulamaya anahtarları eklemeniz ve uygulamayı yeniden yayımlayın. 

    ![Yayımla Sayfası üzerinde iki HALUK anahtarlarla ekran görüntüsü, HALUK portalı](./media/traffic-manager/luis-keys-in-luis.png)

    Örnek URL'de **endpoint** sütun abonelik anahtarla bir GET isteğine bir sorgu parametresi olarak kullanır. İki yeni anahtarlar uç nokta URL'leri kopyalayın. Bunlar, bu makalenin sonraki bölümlerinde trafik Yöneticisi yapılandırmasının bir parçası olarak kullanılır.

## <a name="manage-luis-endpoint-requests-across-keys-with-traffic-manager"></a>Anahtarları Traffic Manager ile arasında HALUK uç nokta isteklerini yönet
Trafik Yöneticisi uç noktalarınızı için yeni bir DNS erişim noktası oluşturur. Bir ağ geçidi veya proxy olarak ancak DNS düzeyinde kesinlikle hareket değil. Bu örnekte, tüm DNS kayıtları değiştirmez. Bir DNS kitaplığı belirli bu istek için doğru uç noktası almak için trafik Yöneticisi'ile iletişim kurmak için kullanır. _Her_ HALUK ilk kullanmak için hangi HALUK endpoint belirlemek için bir trafik Yöneticisi isteği gerektirir yönelik istek. 

### <a name="polling-uses-luis-endpoint"></a>Yoklama HALUK uç nokta kullanır
Trafik Yöneticisi uç noktalar düzenli aralıklarla endpoint hala kullanılabilir olduğundan emin olmak için yoklar. Trafik Yöneticisi URL bir GET isteğiyle erişilebilir olması ve bir 200 dönmek için gereksinimlerini sorgulanan. Uç nokta URL'sini **Yayımla** sayfa yapar. Farklı bir yol ve sorgu dizesi parametreleri her abonelik anahtara sahip olduğundan, her abonelik anahtarı farklı yoklama yolu gerekiyor. Trafik Yöneticisi yoklar, her seferinde bir kota isteği maliyeti. Sorgu dizesi parametresi **q** HALUK HALUK için gönderilen utterance uç noktadır. Bir utterance göndermek yerine bu parametre, trafik Yöneticisi yoklama HALUK uç nokta günlüğe trafik yapılandırılmış Yöneticisi alınmaya çalışılırken hata ayıklama bir teknik olarak eklemek için kullanılır.

Her HALUK bitiş kendi yolu gerektiğinden, kendi trafik Yöneticisi profili gerekir. Oluşturma işlemi profillerini yönetmek için bir [ _iç içe geçmiş_ trafik Yöneticisi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-nested-profiles) mimarisi. Bir üst profil alt profillere işaret ve bunların arasında trafiği yönetin.

Trafik Yöneticisi yapılandırıldıktan sonra günlük kullanılacak yol değiştirmeyi unutmayın günlüğünüzün yoklamayı doluyor olmayan şekilde = false sorgu dizesi parametresi.

## <a name="configure-traffic-manager-with-nested-profiles"></a>Trafik Yöneticisi ile iç içe geçmiş profillerini yapılandırma
Aşağıdaki bölümlerde biri Doğu HALUK anahtar diğeri Batı HALUK anahtarı için iki alt profili oluşturun. Bir üst profil oluşturulduktan sonra iki alt profilleri üst profiline eklenir. 

### <a name="create-the-east-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Doğu ABD trafik Yöneticisi profili oluştur
Doğu ABD trafik Yöneticisi profili oluşturmak için birkaç adım vardır: profil oluşturma, uç nokta ekleyin ve uç nokta ayarlayın. Bir Traffic Manager profilini birçok uç noktaları olabilir, ancak her bitiş noktasıyla aynı doğrulama yolu vardır. Doğu ve Batı abonelikleri HALUK uç nokta URL'lerinin bölge ve abonelik anahtarı nedeniyle farklı olduğundan, her HALUK bitiş profilinde tek bir uç noktası olması gerekir. 

1. Profil oluşturma **[yeni AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet'i

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Değiştirdiğinizden emin olun `appIdLuis` ve `subscriptionKeyLuis`. SubscriptionKey için BİZE Doğu HALUK anahtardır. Yolun HALUK uygulama kimliği ve abonelik anahtarı dahil doğru değil, trafik Yöneticisi yoklama durumu ise `degraded` trafiği yönetmek HALUK uç noktası başarıyla istenemiyor olduğundan. Değerini emin olun `q` olan `traffic-manager-east` bu değer HALUK uç nokta günlüklerine görebilmeniz için.

    ```PowerShell
    $eastprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-eastus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-eastus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appID>?subscription-key=<subscriptionKey>&q=traffic-manager-east"
    ```
    
    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:
    
    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Adı|Haluk profili eastus|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|Haluk trafik Yöneticisi|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri][routing-methods]. Performans kullanıyorsanız, bir URL isteği için trafik Yöneticisi kullanıcı bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama giderek, bölge için trafik Yöneticisi çağrısında taklit etmek üzere chatbot's sorumluluk olur. |
    |-RelativeDnsName|Haluk dns eastus|Alt etki alanı hizmeti için budur: Haluk dns eastus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol HALUK için HTTPS/443'tür|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-east`|Değiştir <appIdLuis> ve <subscriptionKeyLuis> kendi değerlere sahip.|
    
    Başarılı bir istek yanıt aldı.

2. Doğu ABD noktayla eklemek **[Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/add-azurermtrafficmanagerendpointconfig?view=azurermps-6.2.0)** cmdlet'i

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-east-endpoint -TrafficManagerProfile $eastprofile -Type ExternalEndpoints -Target eastus.api.cognitive.microsoft.com -EndpointLocation "eastus" -EndpointStatus Enabled
    ```
    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-EndpointName|Haluk Doğu endpoint|Profil altında görüntülenen uç nokta adı|
    |-TrafficManagerProfile|$eastprofile|1. adımda oluşturduğunuz profil nesnesi kullanın|
    |-Türü|ExternalEndpoints|Daha fazla bilgi için bkz: [trafik Yöneticisi uç noktası][traffic-manager-endpoints] |
    |-Hedef|eastus.api.cognitive.microsoft.com|Bu HALUK uç noktası için etki alanıdır.|
    |-EndpointLocation|"eastus"|Uç noktanın bölge|
    |-EndpointStatus|Etkin|Oluşturulduğunda uç noktayı etkinleştirme|

    Başarılı yanıt şuna benzer:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-eastus
    Name                             : luis-profile-eastus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-eastus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/<luis-app-id>?subscription-key=f0517d185bcf467cba5147d6260bb868&q=traffic-manager-east
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-east-endpoint}
    ```

3. Doğu ABD noktayla ayarlamak **[kümesi AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/set-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet'i

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $eastprofile
    ```

    Başarılı yanıt aynı yanıt olarak 2. adım olacaktır.

### <a name="create-the-west-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Batı ABD trafik Yöneticisi profili oluştur
Batı ABD trafik Yöneticisi profili oluşturmak için aynı adımları izleyin: profil oluşturma, uç nokta ekleyin ve uç nokta ayarlayın.

1. Profil oluşturma **[yeni AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Değiştirdiğinizden emin olun `appIdLuis` ve `subscriptionKeyLuis`. SubscriptionKey için BİZE Doğu HALUK anahtardır. Yol HALUK uygulama kimliği ve abonelik anahtarı da dahil olmak üzere doğru değilse, trafik Yöneticisi yoklama durumu olan `degraded` trafiği yönetmek HALUK uç noktası başarıyla istenemiyor olduğundan. Değerini emin olun `q` olan `traffic-manager-west` bu değer HALUK uç nokta günlüklerine görebilmeniz için.

    ```PowerShell
    $westprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-westus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-westus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west"
    ```
    
    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:
    
    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Adı|Haluk profili westus|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|Haluk trafik Yöneticisi|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri][routing-methods]. Performans kullanıyorsanız, bir URL isteği için trafik Yöneticisi kullanıcı bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama giderek, bölge için trafik Yöneticisi çağrısında taklit etmek üzere chatbot's sorumluluk olur. |
    |-RelativeDnsName|Haluk dns westus|Alt etki alanı hizmeti için budur: Haluk dns westus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol HALUK için HTTPS/443'tür|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west`|Değiştir <appId> ve <subscriptionKey> kendi değerlere sahip. Bu abonelik anahtarı Doğu abonelik anahtar farklı olduğunu unutmayın|
    
    Başarılı bir istek yanıt aldı.

2. Batı ABD noktayla eklemek **[Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet'i

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-west-endpoint -TrafficManagerProfile $westprofile -Type ExternalEndpoints -Target westus.api.cognitive.microsoft.com -EndpointLocation "westus" -EndpointStatus Enabled
    ```

    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-EndpointName|Haluk Batı endpoint|Profil altında görüntülenen uç nokta adı|
    |-TrafficManagerProfile|$westprofile|1. adımda oluşturduğunuz profil nesnesi kullanın|
    |-Türü|ExternalEndpoints|Daha fazla bilgi için bkz: [trafik Yöneticisi uç noktası][traffic-manager-endpoints] |
    |-Hedef|westus.api.cognitive.microsoft.com|Bu HALUK uç noktası için etki alanıdır.|
    |-EndpointLocation|"westus"|Uç noktanın bölge|
    |-EndpointStatus|Etkin|Oluşturulduğunda uç noktayı etkinleştirme|

    Başarılı yanıt şuna benzer:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-westus
    Name                             : luis-profile-westus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-westus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/c3fc5d1e-5187-40cc-af0f-fbde328aa16b?subscription-key=e3605f07e3cc4bedb7e02698a54c19cc&q=traffic-manager-west
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-west-endpoint}
    ```

3. Batı ABD noktayla ayarlamak **[kümesi AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $westprofile
    ```

    Başarılı yanıt aynı yanıt olarak 2. adım:.

### <a name="create-parent-traffic-manager-profile"></a>Üst trafik Yöneticisi profili oluştur
Trafik Yöneticisi profili ana oluşturur ve iki alt trafik Yöneticisi profili üst öğeye bağlayın.

1. Üst profiliyle oluşturma **[yeni AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    ```PowerShell
    $parentprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-parent -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-parent -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/"
    ```

    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Adı|Haluk profili üst|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|Haluk trafik Yöneticisi|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri][routing-methods]. Performans kullanıyorsanız, bir URL isteği için trafik Yöneticisi kullanıcı bölgesinden gelmelidir. Bir chatbot veya başka bir uygulama giderek, bölge için trafik Yöneticisi çağrısında taklit etmek üzere chatbot's sorumluluk olur. |
    |-RelativeDnsName|Haluk dns üst|Alt etki alanı hizmeti için budur: Haluk dns parent.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol HALUK için HTTPS/443'tür|
    |-MonitorPath|`/`|Alt uç yolları yerine kullanıldığından bu yolu önemli değildir.|

    Başarılı bir istek yanıt aldı.

2. Üst öğesi ile Doğu ABD alt profil eklemek **[Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** ve **NestedEndpoints** türü

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-useast -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $eastprofile.Id -EndpointStatus Enabled -EndpointLocation "eastus" -MinChildEndpoints 1
    ```

    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-EndpointName|alt uç noktası useast|Doğu profili|
    |-TrafficManagerProfile|$parentprofile|Bu uç noktaya atamak için profil|
    |-Türü|NestedEndpoints|Daha fazla bilgi için bkz: [Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-Uç noktası Targetresourceıd|$eastprofile. Kimliği|Alt profil kimliği|
    |-EndpointStatus|Etkin|Üst öğeye ekledikten sonra uç nokta durumu|
    |-EndpointLocation|"eastus"|[Azure bölgesi adı](https://azure.microsoft.com/global-infrastructure/regions/) kaynağının|
    |-MinChildEndpoints|1|Alt uç noktaları için en az durum sayısı|

    Başarılı yanıt görünüm aşağıdaki gibi ve yeni içerir `child-endpoint-useast` uç noktası:    

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast}
    ```

3. Üst öğesi ile Batı ABD alt profil eklemek **[Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet'i ve **NestedEndpoints** türü

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-uswest -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $westprofile.Id -EndpointStatus Enabled -EndpointLocation "westus" -MinChildEndpoints 1
    ```

    Bu tabloda, her bir değişken cmdlet açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-EndpointName|alt uç noktası uswest|Batı profili|
    |-TrafficManagerProfile|$parentprofile|Bu uç noktaya atamak için profil|
    |-Türü|NestedEndpoints|Daha fazla bilgi için bkz: [Ekle AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-Uç noktası Targetresourceıd|$westprofile. Kimliği|Alt profil kimliği|
    |-EndpointStatus|Etkin|Üst öğeye ekledikten sonra uç nokta durumu|
    |-EndpointLocation|"westus"|[Azure bölgesi adı](https://azure.microsoft.com/global-infrastructure/regions/) kaynağının|
    |-MinChildEndpoints|1|Alt uç noktaları için en az durum sayısı|

    Başarılı yanıt Ara hem de önceki içerir ve gibi `child-endpoint-useast` endpoint ve yeni `child-endpoint-uswest` uç noktası:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast, child-endpoint-uswest}
    ```

4. Uç nokta ile ayarlamak **[kümesi AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i 

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $parentprofile
    ```

    Başarılı yanıt aynı yanıt olarak 3. adım:.

### <a name="powershell-variables"></a>PowerShell değişkenleri
Önceki bölümlerde üç PowerShell değişkenleri oluşturulan: `$eastprofile`, `$westprofile`, `$parentprofile`. Bu değişkenler trafik Yöneticisi yapılandırması sonuna doğru kullanılır. Değişkenleri oluşturmamayı seçtiniz veya unuttunuz veya PowerShell penceresi zaman aşımına uğradı, PowerShell cmdlet'ini kullanabilirsiniz  **[Get-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)**, profili yeniden elde ve atamak için bir değişkene. 

Köşeli parantez içindeki öğeleri değiştirmek `<>`, gereksinim duyduğunuz üç profilinden her birini için doğru değerlerle. 

```PowerShell
$<variable-name> = Get-AzureRmTrafficManagerProfile -Name <profile-name> -ResourceGroupName luis-traffic-manager
```

## <a name="verify-traffic-manager-works"></a>Trafik Yöneticisi'nin çalıştığını doğrulama
Trafik Yöneticisi çalışma profilleri, Profiller durumunu gerek doğrulamak için `Online` bu durum uç noktasının yoklama yola göre. 

### <a name="view-new-profiles-in-the-azure-portal"></a>Azure portalında yeni profilleri görünümü
Üç profil kaynaklarında bakarak oluşturulduğunu doğrulayabilirsiniz `luis-traffic-manager` kaynak grubu.

![Ekran görüntüsü, Azure kaynak grubu Haluk-trafik-Yöneticisi](./media/traffic-manager/traffic-manager-profiles.png)

### <a name="verify-the-profile-status-is-online"></a>Profili durumunun çevrimiçi olduğunu doğrulayın
Trafik Yöneticisi çevrimiçi olduğundan emin olmak için her uç nokta yolunu yoklar. Çevrimiçi ise alt profilleri durumunu olan `Online`. Bu gösterilir **genel bakış** her profili. 

![Ekran Azure trafik Yöneticisi profili, izleme durumu çevrimiçi gösteren genel bakış](./media/traffic-manager/profile-status-online.png)

### <a name="validate-traffic-manager-polling-works"></a>Trafik Yöneticisi works yoklama doğrula
Trafik Yöneticisi'ni Works yoklama doğrulamak için başka bir HALUK uç nokta günlükleriyle yoludur. Üzerinde [HALUK] [ LUIS] Web sitesi uygulamalar listesi sayfasında, uygulama için uç nokta günlüğünü dışarı aktar. Trafik Yöneticisi için iki uç nokta hangi sıklıkla yoklayacağını çünkü girişleri var. günlüklerde bunlar yalnızca birkaç dakikada olsa bile Sorgu başladığı ile girdilerini arayın hatırlamak `traffic-manager-`.

```text
traffic-manager-west    6/7/2018 19:19  {"query":"traffic-manager-west","intents":[{"intent":"None","score":0.944767}],"entities":[]}
traffic-manager-east    6/7/2018 19:20  {"query":"traffic-manager-east","intents":[{"intent":"None","score":0.944767}],"entities":[]}
```

### <a name="validate-dns-response-from-traffic-manager-works"></a>Trafik Yöneticisi Works'ten DNS yanıtını doğrula
DNS yanıtı HALUK endpoint döndürür doğrulamak için bir DNS istemci kitaplığı kullanılarak DNS trafiği yönetmek üst profili isteyin. DNS adı üst profil için `luis-dns-parent.trafficmanager.net`.

Aşağıdaki Node.js kodu üst profili için istekte bulunur ve HALUK son noktasını döndürür:

```javascript
const dns = require('dns');

dns.resolveAny('luis-dns-parent.trafficmanager.net', (err, ret) => {
  console.log('ret', ret);
});
```

Başarılı yanıt HALUK uç noktası ile verilmiştir:

```cmd
[
    {
        value: 'westus.api.cognitive.microsoft.com', 
        type: 'CNAME'
    }
]
```

## <a name="use-the-traffic-manager-parent-profile"></a>Trafik Yöneticisi üst profili kullan
Uç noktalar arasında trafiği yönetmek için trafik Yöneticisi DNS HALUK uç noktası bulmak için bir çağrı eklemeniz gerekir. Bu çağrı için her HALUK son nokta isteğinin yapılan ve HALUK istemci uygulamasının kullanıcı coğrafi konumunu benzetimini yapmak gerekiyor. DNS yanıtı kodu HALUK istemci uygulamanız ve istek Between HALUK için uç nokta tahmin için ekleyin. 


## <a name="clean-up"></a>Temizleme
İki HALUK Abonelik anahtarları, üç trafik Yöneticisi profillerinizi ve beş bu kaynakları içeren kaynak grubunu kaldırın. Bu Azure portalından gerçekleştirilir. Beş kaynakları kaynakları listeden silin. Kaynak grubunu silin. 

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [ara yazılımı](https://docs.microsoft.com/azure/bot-service/bot-builder-create-middleware?view=azure-bot-service-4.0&tabs=csaddmiddleware%2Ccsetagoverwrite%2Ccsmiddlewareshortcircuit%2Ccsfallback%2Ccsactivityhandler) BotFramework v4 seçeneklerinde Bu trafik yönetim kodu bir BotFramework bot nasıl eklenebileceğini anlamak için. 

[traffic-manager-marketing]: https://azure.microsoft.com/services/traffic-manager/
[traffic-manager-docs]: https://docs.microsoft.com/azure/traffic-manager/
[LUIS]:luis-reference-regions.md#luis-website
[azure-portal]:https://portal.azure.com/
[azure-storage]:https://azure.microsoft.com/services/storage/
[routing-methods]:https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods
[traffic-manager-endpoints]:https://docs.microsoft.com/azure/traffic-manager/traffic-manager-endpoint-types
