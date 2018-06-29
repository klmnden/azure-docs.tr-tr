---
title: Azure trafik analytics sık sorulan sorular | Microsoft Docs
description: Bazı trafiği analytics hakkında sık sorulan soruların yanıtlarını alın.
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
ms.openlocfilehash: a4b87d92751c84d96bc70915d16adae7943c145e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062886"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Trafik analytics sık sorulan sorular

Bu makalede tek bir yerde birçok Azure Ağ İzleyicisi içinde trafiği analytics hakkında sık sorulan soruların toplar.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Trafik analytics kullanmak için Önkoşullar nelerdir?

Trafik analytics aşağıdaki önkoşulları gerektirir:

- Ağ İzleyicisi abonelik etkin.
- Ağ güvenlik grubu (NSG) akış günlükleri, izlemek istediğiniz Nsg'ler için etkinleştirilmiş.
- Ham depolamak için bir Azure depolama hesabınızın günlükleri flog.
- Okuma ve yazma erişimi olan bir Azure günlük analizi çalışma.

Hesabınızı trafiği analytics etkinleştirmek için aşağıdakilerden birini karşılaması gerekir:

- Bir abonelik düzeyinde aşağıdaki rollerin hesabınızı atanmalıdır: Hesap Yöneticisi, Hizmet Yöneticisi veya ortak yönetici.
- Hesabınızda herhangi biri aşağıdaki rol tabanlı erişim denetimi (RBAC) rolleri aboneliği kapsamında olması gerekir: sahibi, katkıda bulunan, okuyucu veya ağ Katılımcısı.
- Hesabınızı daha önce listelenen rollerinden birini atanmamışsa, aşağıdaki eylemler, abonelik düzeyinde atanan özel bir rol atanması gerekir.
            
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

1. Azure'a kullanarak oturum **Login-AzureRmAccount**. 

2. Gerekli olan abonelik kullanarak seçme **Select-AzureRmSubscription**. 

3. Belirtilen bir kullanıcıya atanmış olan tüm rolleri listelemek için kullanın **Get-AzureRmRoleAssignment - SignInName [kullanıcı e-postası] - IncludeClassicAdministrators**. 

Herhangi bir çıktı görmüyorsanız, komutları çalıştırmak için erişim sağlamak için ilgili aboneliği yöneticisine başvurun. Daha fazla ayrıntı için bkz: [rol tabanlı erişim denetimini Azure PowerShell ile yönetme](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>Hangi Azure trafiğini analiz bölgelerde kullanılabilir?

Nsg'ler aşağıdaki desteklenen bölgeler hiçbirinde trafiği analytics kullanabilirsiniz: Batı Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Orta ABD, Orta Güney ABD, Orta ABD, Batı ABD, Batı ABD 2, Batı Avrupa, Kuzey Avrupa, Birleşik Krallık Batı, Birleşik Krallık Güney, Avustralya Doğu, Avustralya Güneydoğu ve Güneydoğu Asya. Günlük analizi çalışma alanı, Batı Orta ABD, Doğu ABD, Batı Avrupa, Birleşik Krallık Güney, Avustralya Güneydoğu veya Güneydoğu Asya bölge içinde bulunmalıdır.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Akış etkinleştirebilirim Nsg'ler olabilir günlüklerini olması çalışma Alanım'den farklı bölgelerdeki?

Evet, bu Nsg'ler günlük analizi çalışma alanınızı daha farklı bölgelerdeki olabilir.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Birden çok Nsg'ler tek bir çalışma alanı içinde yapılandırılabilir mi?

Evet.

## <a name="can-i-use-an-existing-workspace"></a>Varolan bir çalışma alanı kullanabilir miyim?

Evet. Var olan bir çalışma öğesini seçerseniz, yeni sorgu dili geçirildiğinden emin olun. Çalışma alanına yükseltmeye istemiyorsanız, yeni bir tane oluşturmanız gerekir. Yeni sorgu dili hakkında daha fazla bilgi için bkz: [Azure günlük analizi yükseltmek için yeni günlük arama](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-operations-management-suite-workspace-be-in-a-different-subscription"></a>Azure depolama Hesabımı bir abonelikte kullanılabilir ve farklı bir abonelikte Operations Management Suite çalışma Alanım olabilir?

Evet, Azure depolama hesabınıza bir abonelikte olabilir ve farklı bir abonelikte Operations Management Suite çalışma alanınız olabilir.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Farklı bir abonelikte ham günlükleri depolayabilir miyim?

Hayır. Bir NSG'yi akış günlükleri için etkinleştirdiğiniz herhangi bir depolama hesabı ham günlükleri depolayabilirsiniz. Ancak, depolama hesabı ve raw günlükleri aynı abonelikte ve bölgede olması gerekir.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>Bir NSG'yi bir "Bulunamadı" hatası nedeniyle trafiğini analiz için yapılandırma olamaz

Desteklenen bir bölge seçin. Desteklenmeyen bir bölge seçerseniz, "Bulunamadı" hatasını alıyorsunuz. Desteklenen bölgeleri bu makalenin önceki bölümlerinde listelenir.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>Ne durum aldığımı "yükleyemedi," NSG akış günlükleri sayfanın altında?

Microsoft.ınsights sağlayıcısı için düzgün çalışması için günlük akışı kayıtlı olması gerekir. Microsoft.ınsights sağlayıcısı, aboneliğiniz için kayıtlı olup olmadığından emin değilseniz, yerini *xxxxx-xxxxx-xxxxxx-xxxx* aşağıdaki komutu yazın ve PowerShell aşağıdaki komutları çalıştırın:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Çözüm yapılandırmış olduğunuz. Neden bir şey Panoda görüyorum değil mi?

Panoyu ilk kez görünmesi 30 dakika sürebilir. Çözümü ilk anlamlı bilgiler türetilen kendisine için yeterli veri toplaması gerekir. Ardından raporlar oluşturur. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Bu ileti alırsam ne olur: "hiçbir veri bu çalışma alanında seçilen zaman aralığı için bulamadık. Zaman aralığını değiştirmeyi deneyin veya farklı bir çalışma alanı seçin. "?

Aşağıdaki seçenekleri deneyin:
- Üst çubuğunda zaman aralığını değiştirebilirsiniz.
- Farklı bir günlük analizi çalışma alanı, üst araç çubuğunda seçin.
- Yakın zamanda etkinleştirildiyse trafiği analytics 30 dakika sonra erişmeyi deneyin.
    
Sorun devam ederse, sorunları Yükselt [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Bu ileti alırsam ne olur: ", NSG çözümleme akış günlükleri ilk kez. Bu işlemin tamamlanması 20-30 dakika sürebilir. Bir süre sonra yeniden denetleyin. 2) yukarıdaki adım çalışmaz ve çalışma alanınızı ücretsiz SKU ise, kota doğrulamak için çalışma alanı kullanımınızı denetleyin, aksi takdirde, daha fazla bilgi için SSS için başvurun. "?

Çünkü bu iletiyi görebilirsiniz:
- Trafik analytics son etkinleştirildi ve henüz yeterli daha anlamlı bilgiler çıkarmaya toplanmış verilerine değil.
- Operations Management Suite çalışma ücretsiz sürümünü kullanıyorsanız ve kota sınırlarını aştı. Daha büyük bir kapasiteye sahip bir çalışma alanı kullanmanız gerekebilir.
    
Sorun devam ederse, sorunları Yükselt [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Bu ileti alırsam ne olur: "sahip olduğumuz kaynakları verileri (topoloji) ve akış bilgisi yok gibi görünüyor. Bu sırada, kaynak verileri görebilir ve sık sorulan sorular için daha fazla bilgi için bkz için burayı tıklatın. "?

Panoda kaynak bilgileri görüyorsunuz; Ancak, hiçbir akışıyla ilgili istatistikleri mevcut değildir. Veri kaynakları arasında hiçbir iletişim akışını nedeniyle mevcut olmayabilir. 60 dakika bekleyin ve durum yeniden denetleyin. Sorun devam ederse ve kaynaklar arasında iletişimi akışları var olduğundan emin olarak sorunları Oluştur [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>PowerShell kullanarak trafiği analytics yapılandırabilmeniz için veya bir Azure Resource Manager şablonu ya da istemci?

Trafik analytics sürümünden 6.2.1 başlayarak Windows PowerShell kullanarak yapılandırabilirsiniz. Akış günlüğü ve trafik analizi için belirli bir NSG kümesi cmdlet'ini kullanarak yapılandırmak için bkz [kümesi AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Akış günlüğe kaydetme ve trafik analytics durumu için belirli bir NSG almak için bkz: [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

Şu anda, trafiği analytics yapılandırmak için bir Azure Resource Manager şablonunu kullanamazsınız.

Bir Azure Resource Manager istemcisi kullanarak trafiği analytics yapılandırmak için aşağıdaki örneklere bakın.

**Cmdlet örnek ayarlayın:**
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
**Cmdlet örnek alın:**
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



## <a name="how-is-traffic-analytics-priced"></a>Trafik analytics nasıl fiyatlandırılır?

Trafik analytics ölçülen. Ölçüm üzerindeki akış günlük verileri işleme hizmeti tarafından temel alır ve elde edilen depolama günlükleri günlük analizi çalışma alanındaki Gelişmiş. Daha fazla ayrıntı için bkz: [planı fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/network-watcher/). 

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Coğrafi harita görünümünde klavyeyi kullanarak nasıl gidebilirsiniz?

Coğrafi harita sayfa iki ana bölümleri içerir:
    
- **Başlık**: coğrafi harita üstündeki başlık trafik dağıtım filtreleri (örneğin, dağıtım, trafiği ülkelerinden ve kötü amaçlı) seçmek için düğmeler sağlar. Bir düğme seçtiğinizde, ilgili filtre haritada uygulanır. Örneğin, etkin düğmesini seçerseniz, harita, dağıtımınızı active Veri merkezlerindeki vurgular.
- **Harita**: başlığı altında Azure veri merkezleri ile ülkelerde arasında trafik dağılımı harita bölümünde gösterilir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Başlıktaki klavye gezinme
    
- Varsayılan olarak, başlık coğrafi harita sayfasında "Azure DC'leri" filtre seçimdir.
- Başka bir filtre taşımak için kullanın ya da `Tab` veya `Right arrow` anahtarı. Geri gitmek için kullanın ya da `Shift+Tab` veya `Left arrow` anahtarı. İleri gezinti sağa sol alta üst tarafından izlenen.
- Tuşuna `Enter` veya `Down` seçilen filtre uygulamak için ok tuşu. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğüm eşlemesi bölümü altında vurgulanır.
- Harita başlığı arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Harita üzerinde klavye gezinme
    
- Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**Azure veri merkezi** veya **ülke/bölge**) eşlemesi görünümünde.
- Vurgulanan diğer düğümlere eşlemesindeki taşımak için kullanın ya da `Tab` veya `Right arrow` İleri hareketi için anahtar. Kullanım `Shift+Tab` veya `Left arrow` geriye dönük taşıma için anahtar.
- Vurgulanan düğümlerinden eşlemesinde seçmek için kullanın `Enter` veya `Down arrow` anahtarı.
- Bu tür bir düğüm seçimde odağı taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak kapalı düğmesi odağı taşır **bilgileri araç kutusu**. Daha fazla içinde taşımak için **kutusunu** görüntülemek için kullanmak `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
- Bastığınızda `Tab` odağı açıkken **bilgileri araç kutusu**, Seçili düğümün olarak aynı kıtada uç noktaları odağı taşır. Kullanım `Right arrow` ve `Left arrow` Bu uç noktaları yoluyla taşımak için anahtarları.
- Diğer akış uç noktaları veya kıtadan geçmeden kümelere taşıma için kullanmak `Tab` İleri hareketi için ve `Shift+Tab` geriye dönük taşıma için.
- Odağı olduğunda üzerinde **Kıtada kümeleri**, kullanın `Enter` veya `Down` kıtadan geçmeden küme içindeki uç noktaları vurgulamak için ok tuşlarını. Uç noktaları ve Kapat düğmesini kıtadan geçmeden küme bilgi kutusunu taşımak için kullanın ya da `Right arrow` veya `Left arrow` ileriye ve geriye doğru taşıma için sırasıyla anahtar. Herhangi bir noktadaki kullandığınız `Shift+L` seçili düğümden uç noktası için bağlantı hattı geçmek için. Basabilirsiniz `Shift+L` yeniden seçili uç noktasına taşımak için.
        
### <a name="keyboard-navigation-at-any-stage"></a>Herhangi bir aşamada klavye gezinme
    
- `Esc` Genişletilmiş seçimi daraltır.
- `Up arrow` Anahtar ile aynı eylemi gerçekleştirir `Esc`. `Down arrow` Anahtar ile aynı eylemi gerçekleştirir `Enter`.
- Kullanım `Shift+Plus` , yakınlaştırma ve `Shift+Minus` uzaklaştırmak için.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Sanal ağ topolojisi görünümünde klavyeyi kullanarak nasıl gidebilirsiniz?

Sanal ağ topolojisi sayfasında, iki ana bölümleri içerir:
    
- **Başlık**: sanal ağ topolojisi üstündeki başlık trafik dağıtım filtreleri (örneğin, bağlı sanal ağlar, bağlantısı kesilmiş sanal ağları ve genel IP'ler) seçmek için düğmeler sağlar. Bir düğme seçtiğinizde, ilgili filtre topolojisine uygulanır. Örneğin, etkin düğmesini seçerseniz, dağıtımınızdaki etkin sanal ağlar topoloji vurgular.
- **Topoloji**: başlığı altında sanal ağlar arasında trafiği dağıtım topolojisi bölümü gösterir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Başlıktaki klavye gezinme
    
- Varsayılan olarak, başlık sanal ağ topolojisi sayfasında "Bağlı sanal ağlar" filtre seçimdir.
- Başka bir filtre taşımak için kullanın `Tab` ilerlemek için anahtar. Geri gitmek için kullanmak `Shift+Tab` anahtarı. İleri gezinti sağa sol alta üst tarafından izlenen.
- Tuşuna `Enter` seçilen filtre uygulamak için. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğümler (sanal ağ) topolojisi bölümü altında vurgulanır.
- Başlık ve topoloji arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Klavye gezinti topolojisi hakkında
    
- Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**VNet**) topoloji görünümünde.
- Vurgulanan diğer düğümlere topoloji görünümünde taşımak için kullanın `Shift+Right arrow` İleri hareketi için anahtar. 
- Odağı vurgulanan düğümlerinde taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak, odağı taşır **daha fazla ayrıntı** düğmesini **bilgileri araç kutusu**. Daha fazla içinde taşımak için **kutusunu** görüntülemek için kullanmak `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
- Bu tür bir düğüm seçimde kendi bağlantıları ziyaret edebilirsiniz basarak bir `Shift+Left arrow` anahtarı. Odağı taşır **bilgileri araç kutusu** o bağlantı. Herhangi bir noktada odağı düğüme geri tuşlarına basarak gölgeye `Shift+Right arrow` yeniden.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Alt ağ topolojisi görünümünde klavyeyi kullanarak nasıl gidebilirsiniz?

Sanal ağlarla topolojisi sayfasında, iki ana bölümleri içerir:
    
- **Başlık**: sanal ağlarla topoloji üstündeki başlık trafik dağıtım filtreleri (örneğin, etkin, Orta ve ağ geçidi alt ağlar) seçmek için düğmeler sağlar. Bir düğme seçtiğinizde, ilgili filtre topolojisine uygulanır. Örneğin, etkin düğmesini seçerseniz, dağıtımınızdaki etkin sanal alt ağ topolojisi vurgular.
- **Topoloji**: başlığı altında sanal alt ağlar arasında trafiği dağıtım topolojisi bölümü gösterir.
    
### <a name="keyboard-navigation-on-the-banner"></a>Başlıktaki klavye gezinme
    
- Varsayılan olarak, başlık sanal ağlarla topolojisi sayfasında "Alt" filtre seçimdir.
- Başka bir filtre taşımak için kullanın `Tab` ilerlemek için anahtar. Geri gitmek için kullanmak `Shift+Tab` anahtarı. İleri gezinti sağa sol alta üst tarafından izlenen.
- Tuşuna `Enter` seçilen filtre uygulamak için. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğüm (alt ağ) topolojisi bölümü altında vurgulanır.
- Başlık ve topoloji arasında geçiş yapmak için basın `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Klavye gezinti topolojisi hakkında
    
- Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**alt**) topoloji görünümünde.
- Vurgulanan diğer düğümlere topoloji görünümünde taşımak için kullanın `Shift+Right arrow` İleri hareketi için anahtar. 
- Odağı vurgulanan düğümlerinde taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak, odağı taşır **daha fazla ayrıntı** düğmesini **bilgileri araç kutusu**. Daha fazla içinde taşımak için **kutusunu** görüntülemek için kullanmak `Right arrow` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
- Bu tür bir düğüm seçimde kendi bağlantıları ziyaret edebilirsiniz basarak bir `Shift+Left arrow` anahtarı. Odağı taşır **bilgileri araç kutusu** o bağlantı. Herhangi bir noktada odağı düğüme geri tuşlarına basarak gölgeye `Shift+Right arrow` yeniden.    

