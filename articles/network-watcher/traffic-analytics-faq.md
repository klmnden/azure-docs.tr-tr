---
title: Azure trafik analizi hakkında sık sorulan sorular | Microsoft Docs
description: Trafik analizi hakkında sık sorulan soruların yanıtlarını alın.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: 65948b1de3a972687e738b011acf3542073db277
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59046999"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Trafik analizi hakkında sık sorulan sorular

Bu makale Azure Ağ İzleyicisi'nde trafik analizi hakkında sık sorulan soruların birçoğu tek bir yerde toplar.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Trafik analizi kullanmak için Önkoşullar nelerdir?

Trafik analizi, aşağıdaki önkoşulları gerektirir:

- Ağ İzleyicisi abonelik etkin.
- Ağ güvenlik grubu (NSG) akış günlüklerini izlemek istediğiniz Nsg'ler için etkin.
- Ham akış günlüklerini depolamak için bir Azure depolama hesabı.
- Okuma ve yazma erişimi ile bir Azure Log Analytics çalışma.

Hesabınızı trafik Analizi'ni etkinleştirmek için aşağıdakilerden birini karşılaması gerekir:

- Hesabınıza abonelik kapsamında aşağıdaki rol tabanlı erişim denetimi (RBAC) rollerinden herhangi biri olmalıdır: sahibi, katkıda bulunan, okuyucu veya ağ Katılımcısı.
- Hesabınızın daha önce listelenen rollerden biri atanmamışsa, aşağıdaki eylemler, abonelik düzeyinde atanmış bir özel rol atanması gerekir.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Bir abonelik için bir kullanıcıya atanan rollerin denetlemek için:

1. Azure'a kullanarak oturum açın **oturum açma AzAccount**. 

2. Gerekli olan abonelik kullanarak seçme **seçin AzSubscription**. 

3. Belirli bir kullanıcıya atanmış olan tüm rolleri listelemek için kullanın **AzRoleAssignment Get - SignInName [kullanıcı e-postası] - IncludeClassicAdministrators**. 

Herhangi bir çıktı görmediğinizden, komutları çalıştırmak için erişim elde etmek için ilgili abonelik yöneticisine başvurun. Daha fazla ayrıntı için [rol tabanlı erişim denetimini Azure PowerShell ile yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-is-traffic-analytics-available"></a>Hangi Azure bölgelerinde trafik analizi kullanılabilir mi?

Trafik analizi, aşağıdaki desteklenen bölgelerden'nde Nsg'ler için kullanabilirsiniz:
- Orta Kanada
- Batı Orta ABD
- Doğu ABD
- Doğu ABD 2
- Orta Kuzey ABD
- Orta Güney ABD
- Orta ABD
- Batı ABD
- Batı ABD 2
- Fransa Orta
- Batı Avrupa
- Kuzey Avrupa
- Güney Brezilya
- Birleşik Krallık Batı
- Birleşik Krallık Güney
- Avustralya Doğu
- Avustralya Güneydoğu 
- Doğu Asya
- Güneydoğu Asya
- Kore Orta
- Orta Hindistan
- Güney Hindistan
- Japonya Doğu
- Japonya Batı
- ABD Devleti Virginia

Log Analytics çalışma alanı şu bölgelerde bulunmalıdır:
- Orta Kanada
- Batı Orta ABD
- Batı ABD 2
- Doğu ABD
- Fransa Orta
- Batı Avrupa
- Birleşik Krallık Güney
- Avustralya Güneydoğu
- Güneydoğu Asya 
- Kore Orta
- Orta Hindistan
- Japonya Doğu
- ABD Devleti Virginia

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Akış etkinleştirebilirim Nsg'ler için günlükleri için olması çalışma Alanım'dan farklı bölgelerdeki?

Evet, bu Nsg, Log Analytics çalışma alanınızı daha farklı bölgelerde olabilir.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Birden fazla Nsg tek bir çalışma alanı içinde yapılandırılabilir mi?

Evet.

## <a name="can-i-use-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanabilir miyim?

Evet. Mevcut bir çalışma öğesini seçerseniz, yeni sorgu diline geçirilmiş olan emin olun. Çalışma alanı yükseltmek istiyor musunuz, yeni bir tane oluşturmanız gerekir. Yeni sorgu diline hakkında daha fazla bilgi için bkz: [yeni günlük araması için Azure İzleyici günlükleri yükseltme](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-log-analytics-workspace-be-in-a-different-subscription"></a>Bir abonelikte Azure depolama Hesabımı kullanılabilir ve Log Analytics çalışma Alanım'ı farklı bir abonelikte olabilir?

Evet, Azure depolama hesabınızdaki bir abonelikte olabilir ve Log Analytics çalışma alanınızın farklı bir abonelikte olabilir.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Farklı bir abonelikte ham günlükleri depolayabilir miyim?

Hayır. Bir NSG akış günlükleri için etkinleştirdiğiniz tüm depolama hesabında ham günlükleri depolayabilirsiniz. Ancak, hem depolama hesabı hem de ham günlükleri aynı abonelik ve aynı bölgede olması gerekir.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>Bir "Bulunamadı" hatası nedeniyle trafik analizi için bir NSG yapılandırma olamaz

Desteklenen bir bölge seçin. Desteklenmeyen bir bölge seçin, bir "Bulunamadı" hatasını alıyorsunuz. Bu makalenin önceki bölümlerinde listelenen desteklenen bölgeler.

## <a name="why-am-i-getting-the-error-failed-to-update-flow-logs-settings-for--internalservererror-when-enabling-nsgs-in-us-gov-virginia"></a>"İçin akış günlüğü ayarları güncelleştirmek için... başarısız hata neden alıyorum İç sunucu hatası..." NSG'DE'ın ABD Devleti Virginia etkinleştirilirken?

'Microsoft.Network' kaynak sağlayıcısı ABD Devleti Virginia bir abonelik için yeniden kayıtlı olduğu bir hata nedeniyle budur. Ekip, sorunun üzerinde çalışıyor. Geçici bir çözüm olarak, gerekir ['Microsoft.Network' RP el ile yeniden kaydetmeniz](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-register-provider-errors). 

Sorun devam ederse lütfen desteğe başvurun. 

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>Durum ne alıyorum "yüklenemedi," altında NSG akış günlükleri sayfası?

Düzgün çalışması için günlük akışı Microsoft.ınsights sağlayıcısına kayıtlı olması gerekir. Microsoft.Insights sağlayıcısı, aboneliğiniz için kayıtlı olup olmadığını emin değilseniz değiştirin *xxxxx-xxxxx-xxxxxx-xxxx* aşağıdaki komut, powershell'den aşağıdaki komutları çalıştırın:

```powershell-interactive
**Select-AzSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Çözüm yapılandırdım. Neden hiçbir şey Panoda görüyorum değil mi?

Panoyu ilk kez görünmesi 30 dakika kadar sürebilir. Çözümü ilk kez, anlamlı Öngörüler için yeterli veri toplaması gerekir. Ardından bu raporları oluşturur. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Peki bu ileti alıyorum: "Tüm veriler bu çalışma alanında seçili zaman aralığı için bulamadık. Zaman aralığını değiştirmeyi deneyin veya farklı bir çalışma alanı seçin. "?

Aşağıdaki seçenekler deneyin:
- Üst çubuğunda zaman aralığını değiştirebilirsiniz.
- Üst araç çubuğunda farklı bir Log Analytics çalışma alanı seçin.
- Yakın zamanda etkin trafik analizi 30 dakika sonra erişim deneyin.
    
Sorun devam ederse, sorunları yükseltmek [User voice forumunu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Peki bu ileti alıyorum: "NSG akış günlüklerinizi ilk kez çözümleniyor. Bu işlemin tamamlanması 20-30 dakika sürebilir. Süre sonra tekrar kontrol edin. 2) yukarıdaki adım işe yaramazsa çalışma alanınız ücretsiz SKU ise, ardından kota doğrulamak için burada çalışma alanı kullanımınızı denetleyin, başka SSS için daha fazla bilgi için bkz. "?

Çünkü bu iletiyi görebilirsiniz:
- Trafik analizi, yakın zamanda etkinleştirildi ve henüz bu anlamlı Öngörüler için yeterli veri toplanan değil.
- Log Analytics çalışma alanı ücretsiz sürümünü kullanıyorsanız ve kota sınırları aşıldı. Bir çalışma alanı ile daha büyük bir kapasite kullanmanız gerekebilir.
    
Sorun devam ederse, sorunları yükseltmek [User voice forumunu](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Peki bu ileti alıyorum: "Sahip olduğumuz kaynak verilerimiz (topoloji) ve akış bilgisi yok gibi görünüyor. Bu arada kaynak verilerini görmek ve daha fazla bilgi için SSS için başvurmak için buraya tıklayın. "?

Kaynak bilgileri Panoda gördüğünüz; Ancak, hiçbir akış ile ilgili istatistikleri yok. Veri kaynakları arasındaki iletişimin akış nedeniyle mevcut olmayabilir. 60 dakika bekleyin ve durumu yeniden denetleyin. Sorun devam ederse ve kaynaklar arasında iletişimi akışlar mevcut olduğundan emin olarak sorunları yükseltmek [User voice forumunu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>PowerShell kullanarak trafik analizi yapılandırabilir miyim veya Azure Resource Manager şablonu veya istemci?

Trafik analizi sürümünden 6.2.1 başlayarak Windows PowerShell kullanarak yapılandırabilirsiniz. Akış günlüğe kaydetme ve trafik analizi için belirli bir NSG kümesi cmdlet'ini kullanarak yapılandırmak için bkz. [kümesi AzNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkwatcherconfigflowlog). İçin belirli bir NSG akış günlüğe kaydetme ve trafik analizi durumu almak için bkz. [Get-AzNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkwatcherflowlogstatus).

Şu anda, trafik analizi yapılandırmak için bir Azure Resource Manager şablonu kullanamazsınız.

Trafik analizi, bir Azure Resource Manager istemcisi'ni kullanarak yapılandırmak için aşağıdaki örneklere bakın.

**Cmdlet örneğinde ayarlayın:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Cmdlet örneğinde alın:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>Trafik analizi nasıl fiyatlandırılır?

Trafik analizi ölçülür. Kullanım ölçümü akış günlük verisi işleme hizmeti tarafından temel alır ve ortaya çıkan depolama günlükleri Log Analytics çalışma alanında Gelişmiş. 

Örneğin, olarak başına [fiyatlandırma planını](https://azure.microsoft.com/pricing/details/network-watcher/), akış günlükleri verilerini trafik analizi tarafından işlenen bir depolama hesabında depolanan, Batı Orta ABD bölgesinde dikkate 10 GB'tır ve Log Analytics çalışma alanında alınan Gelişmiş günlük 1 GB'tır sonra Geçerli ücretleri şunlardır: 10 x 2.3$ + 1 x 2.76$ 25.76 = $

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Coğrafi harita Görünümü'nde klavyeyi kullanarak nasıl gidebilirsiniz?

Coğrafi harita sayfasında iki ana bölümleri içerir:
    
- **Başlık**: Coğrafi harita üst kısmındaki başlık trafik dağıtım filtreleri (örneğin, dağıtım, ülkelerinden trafiğini ve kötü amaçlı) seçmek için düğmeler sağlar. Bir düğmeyi seçerek ilgili filtre harita üzerinde uygulanır. Örneğin, etkin düğmesini seçerseniz, harita etkin veri merkezleri, dağıtımınızdaki vurgular.
- **Harita**: Başlığın altında harita bölümünde Azure veri merkezleri ve ülkeler arasındaki trafik dağılımı gösterilir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Klavye ile gezinme başlığındaki
    
- Varsayılan olarak, seçimi başlığı coğrafi harita sayfasındaki "Azure DC'leri" filtresidir.
- Başka bir filtre taşımak için kullanın `Tab` veya `Right arrow` anahtarı. Geri taşımak için kullanın `Shift+Tab` veya `Left arrow` anahtarı. İleri gezinme soldan sağa, yukarıdan üst tarafından izlenen.
- Tuşuna `Enter` veya `Down` ok tuşunu, seçili filtre uygulamak için. Filtre Seçimi ve dağıtım bağlı olarak, bir veya birden çok düğümleri eşlemesi bölümü altında vurgulanır.
- Harita başlığı arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Harita üzerinde klavye gezintisi
    
- Herhangi bir filtre başlıktaki seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**Azure veri merkezi** veya **ülke/bölge**) eşlemesi görünümünde.
- Haritadaki başka vurgulanmış düğümler taşımak için ya da kullanın `Tab` veya `Right arrow` ileri taşıma için anahtar. Kullanım `Shift+Tab` veya `Left arrow` geriye taşıma için anahtar.
- Haritada vurgulanan herhangi bir düğümü seçmek için kullanın `Enter` veya `Down arrow` anahtarı.
- Böyle bir düğümleri seçime göre odağı taşır **bilgileri araç kutusu** düğüm. Varsayılan olarak kapalı düğme odak taşır **bilgileri araç kutusu**. Daha fazla içine taşımak için **kutusu** görünümünde, `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahip **bilgileri araç kutusu**.
- Bastığınızda `Tab` odağı açıkken **bilgileri araç kutusu**, seçili düğüm olarak aynı Kıta, uç noktalara odağı taşır. Kullanım `Right arrow` ve `Left arrow` anahtarları Bu uç noktaları taşınır.
- Diğer akış uç noktaları veya kümelerini Kıta taşımak için kullanmak `Tab` ileri taşıma için ve `Shift+Tab` geriye taşıma için.
- Odağı olduğunda üzerinde **kümelerini Kıta**, kullanın `Enter` veya `Down` Kıta kümesi içinde uç noktaları vurgulamak için ok tuşlarını. Kıta kümesi bilgi kutusunu Kapat düğmesine ve uç noktaları ile taşımak için kullanın `Right arrow` veya `Left arrow` ileriye ve geriye taşıma için sırasıyla anahtar. Herhangi bir uç noktaya, kullandığınız `Shift+L` seçili düğümü uç noktasına bağlantı satırına geçmek için. Basabilirsiniz `Shift+L` yeniden seçili uç noktaya gitme.
        
### <a name="keyboard-navigation-at-any-stage"></a>Her aşamada klavye gezintisi
    
- `Esc` bir yelpaze daraltır.
- `Up arrow` Anahtarı aynı eylemi gerçekleştirir `Esc`. `Down arrow` Anahtarı aynı eylemi gerçekleştirir `Enter`.
- Kullanım `Shift+Plus` , yakınlaştırma ve `Shift+Minus` uzaklaştırmak için.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Sanal ağ topolojisi Görünümü'nde klavyeyi kullanarak nasıl gidebilirsiniz?

Sanal ağ topolojisi sayfasında, iki ana bölümleri içerir:
    
- **Başlık**: Sanal ağlar topoloji üst kısmındaki başlık trafik dağıtım filtreleri (örneğin, bağlı sanal ağlar, bağlantısı kesilen sanal ağlar ve genel IP'ler) seçmek için düğmeler sağlar. Bir düğmeyi seçerek ilgili filtre topolojisini uygulanır. Örneğin, etkin düğmesini seçerseniz, dağıtımınızdaki etkin sanal ağlar topoloji vurgular.
- **Topoloji**: Başlığın altında topolojisi bölümünde, sanal ağlar arasında trafik dağılımı gösterilir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Klavye ile gezinme başlığındaki
    
- Varsayılan olarak, sanal ağ topolojisi sayfasında başlığı için seçimi "Bağlı sanal ağlar" filtresidir.
- Başka bir filtre taşımanız `Tab` ilerlemek için anahtar. Geriye doğru gidin, `Shift+Tab` anahtarı. İleri gezinme soldan sağa, yukarıdan üst tarafından izlenen.
- Tuşuna `Enter` seçili filtre uygulamak için. Filtre Seçimi ve dağıtım bağlı olarak, bir veya birden çok düğümleri (sanal ağ için) topolojisi bölümü altında vurgulanır.
- Başlığı ve topoloji arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Klavye ile gezinme topolojisini
    
- Herhangi bir filtre başlıktaki seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**VNet**) topoloji görünümünde.
- Topoloji görünümünde başka vurgulanmış düğümler taşımak için kullanın `Shift+Right arrow` ileri taşıma için anahtar. 
- Odağı vurgulanmış düğümler üzerinde taşır **bilgileri araç kutusu** düğüm. Varsayılan olarak, odağı taşır **daha fazla ayrıntı** düğmesini **bilgileri araç kutusu**. Daha fazla içine taşımak için **kutusu** görünümünde, `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahip **bilgileri araç kutusu**.
- Böyle bir düğümleri seçime göre onun tüm bağlantıları ziyaret edebilirsiniz tuşuna basarak bir `Shift+Left arrow` anahtarı. Odağı hareket etmesini **bilgileri araç kutusu** , bu bağlantının. Herhangi bir noktada odak düğüme geri tuşuna basarak kaydırılmasına `Shift+Right arrow` yeniden.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Alt ağ topolojisi Görünümü'nde klavyeyi kullanarak nasıl gidebilirsiniz?

Sanal alt ağlar topolojisi sayfasında, iki ana bölümleri içerir:
    
- **Başlık**: Sanal alt ağlar topoloji üst kısmındaki başlık trafik dağıtım filtreleri (örneğin, etkin, Orta ve ağ geçidi alt ağlar) seçmek için düğmeler sağlar. Bir düğmeyi seçerek ilgili filtre topolojisini uygulanır. Örneğin, etkin düğmesini seçerseniz, dağıtımınızdaki etkin sanal alt ağ topolojisi vurgular.
- **Topoloji**: Başlığın altında sanal alt ağlar arasında trafik dağıtım topolojisi bölümünde gösterilir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Klavye ile gezinme başlığındaki
    
- Varsayılan olarak, sanal alt ağlar topolojisi sayfasında başlığı için seçimi "Alt" filtresidir.
- Başka bir filtre taşımanız `Tab` ilerlemek için anahtar. Geriye doğru gidin, `Shift+Tab` anahtarı. İleri gezinme soldan sağa, yukarıdan üst tarafından izlenen.
- Tuşuna `Enter` seçili filtre uygulamak için. Filtre Seçimi ve dağıtım bağlı olarak, bir veya birden çok düğüm (alt ağ) topolojisi bölümü altında vurgulanır.
- Başlığı ve topoloji arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Klavye ile gezinme topolojisini
    
- Herhangi bir filtre başlıktaki seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**alt**) topoloji görünümünde.
- Topoloji görünümünde başka vurgulanmış düğümler taşımak için kullanın `Shift+Right arrow` ileri taşıma için anahtar. 
- Odağı vurgulanmış düğümler üzerinde taşır **bilgileri araç kutusu** düğüm. Varsayılan olarak, odağı taşır **daha fazla ayrıntı** düğmesini **bilgileri araç kutusu**. Daha fazla içine taşımak için **kutusu** görünümünde, `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahip **bilgileri araç kutusu**.
- Böyle bir düğümleri seçime göre onun tüm bağlantıları ziyaret edebilirsiniz tuşuna basarak bir `Shift+Left arrow` anahtarı. Odağı hareket etmesini **bilgileri araç kutusu** , bu bağlantının. Herhangi bir noktada odak düğüme geri tuşuna basarak kaydırılmasına `Shift+Right arrow` yeniden.    

