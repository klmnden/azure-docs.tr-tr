---
title: Bir IP kısıtlama kuralı için ön kapı Azure web uygulaması güvenlik duvarı kuralı yapılandırma
description: Mevcut bir ön uç noktası için bir IP adresi kısıtlaması WAF kuralı yapılandırmayı öğrenin.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: kumud;tyao
ms.openlocfilehash: b129579916330a34a2a78d98f2c7653f129d3319
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523703"
---
# <a name="configure-an-ip-restriction-rule-with-web-application-firewall-for-azure-front-door-preview"></a>Bir IP kısıtlama kuralı Azure ön kapısı (Önizleme) web uygulaması güvenlik duvarı yapılandırma
 Bu makalede Azure CLI, Azure PowerShell veya Azure Resource Manager şablonu kullanarak Azure web uygulaması Güvenlik Duvarı (WAF) IP kısıtlama kuralları için ön kapı yapılandırma gösterilmektedir.

Bir IP adresi tabanlı erişim denetimi kuralı sınıfsız etki alanları arası yönlendirme (CIDR) biçiminde bir IP adresi veya IP adresi aralıkları listesi belirterek web uygulamalarınıza erişimini denetlemesine izin veren özel bir WAF kuralıdır.

Varsayılan olarak, web uygulamanıza internet'ten erişilebilir. Web uygulamalarınızı bilinen IP adresleri veya IP adresi aralıklarını yalnızca istemcilere bir listeden erişimini sınırlamak istiyorsanız, iki IP eşleşen kural oluşturmanız gerekir. İlk IP eşleşen kural, IP adresleri olarak eşleşen değerler listesini içerir ve "İzin ver" eylemini ayarlayın. İkincisi daha düşük önceliğe sahip olan diğer tüm IP adresleri "All" işleci kullanılarak engelleyin ve "BLOK" eylemini belirlemek için. Bir IP kısıtlama kuralı uygulandıktan sonra bu izin verilenler dışında adreslerinden gelen tüm istekleri bir 403 (Yasak) yanıt alır.  

> [!IMPORTANT]
> WAF IP kısıtlaması özelliği için Azure ön kapısı, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="configure-waf-policy-with-azure-cli"></a>Azure CLI ile WAF ilkesi yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesi yapılandırmak başlamadan önce CLI ortamınızı ayarlayın ve ön kapısı profili oluşturun.

#### <a name="set-up-azure-cli-environment"></a>Azure CLI ortamını ayarlama
1. Yükleme [Azure CLI](/cli/azure/install-azure-cli), veya Azure Cloud Shell'i kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Önceden yüklenmiş ve yapılandırılmış hesabınızla birlikte kullanmak için Azure CLI var. Seçin **deneyin** düğmesi CLI komutları anlatılmaktadır. Seçme **deneyin** , Azure hesabınızla oturum açarak bir Cloud Shell çağırır. Cloud shell oturumu başladıktan sonra girin `az extension add --name front-door` ön kapısı uzantısı eklemek için.
 2. CLI'yi yerel olarak Bash'te kullanıyorsanız, Azure ile oturum açın `az login`.

#### <a name="create-front-door-profile"></a>Ön kapısı profili oluşturma
Açıklanan yönergeleri izleyerek bir ön kapısı profili oluşturma [hızlı başlangıç: Bir ön kapısı profili oluşturma](quickstart-create-front-door.md)

### <a name="create-a-waf-policy"></a>Bir WAF ilkesi oluşturma

Bir WAF ilkesiyle oluşturma [az ağ waf ilkesi oluşturma](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) komutu. İçinde aşağıdaki örnekte, ilke adını değiştirin *IPAllowPolicyExampleCLI* ile benzersiz bir ilke adı.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-custom-ip-access-control-rule"></a>Özel IP erişim denetimi Kuralı Ekle

Bir özel IP erişim denetimi kuralı önceki adımda oluşturduğunuz WAF ilke ekleyin [az ağ ilkesi waf özel kural oluşturma](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) komutu. 

İçinde aşağıdaki örnekteki:
-  değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.
-  değiştirin *IP adresi aralığı 1*, *IP adres aralığı 2* kendi aralığına sahip.

İlk olarak oluşturmak izin verme kuralı için belirtilen adresleri.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ["<ip-address-range-1>","<ip-address-range-2>"] \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Ardından, bir blok kuralı önceki IP'ın izin verdiğinden daha düşük öncelikli tüm IP kuralı oluşturun. Değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
 ```

### <a name="find-waf-policy-id"></a>WAF ilke kimliği bulunamıyor
Bir WAF ilkesiyle Kimliğini bulun [az ağ waf-policy show](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) komutu. Değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Bir ön kapısı ön uç konağa bağlantı WAF İlkesi

Ön kapısı ayarlamak *WebApplicationFirewallPolicyLink* kimliği ile ilke kimliği için [az ağ ön kapısı güncelleştirme](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) komutu. Değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
Bu örnekte, WAF ilke FrontendEndpoints [0] için uygulanır. WAF İlkesi, ön uç birine bağlanabilir.
> [!Note]
> Yalnızca ayarlamanız gerekir **WebApplicationFirewallPolicyLink** bir WAF İlkesi bir ön kapısı ön uç bağlantısı için bir kez özelliği. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç için uygulanır.

## <a name="configure-waf-policy-with-azure-powershell"></a>WAF ilkesini Azure PowerShell ile yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesi yapılandırmak başlamadan önce PowerShell ortamınızı ayarlayın ve ön kapısı profili oluşturun.

#### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için bu sayfadaki yönergeleri izleyin ve Az PowerShell modülünü yükleyin.

##### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açma için etkileşimli bir iletişim kutusu ile Azure'a bağlanma
```
Connect-AzAccount

```
Ön kapısı modülünü yüklemeden önce geçerli sürümü PowerShellGet yüklü olduğundan emin olun. Aşağıdaki komut ve yeniden çalıştırma PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

##### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modülünü yükleme 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Bir ön kapısı profili oluşturma
Açıklanan yönergeleri izleyerek bir ön kapısı profili oluşturma [hızlı başlangıç: Bir ön kapısı profili oluşturma](quickstart-create-front-door.md)

### <a name="define-ip-match-condition"></a>IP eşleşme koşulu tanımla
Kullanım [yeni AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) bir IP eşleşme koşulu tanımlamak için komutu. İçinde aşağıdaki örnekte, değiştirin *IP adresi aralığı 1*, *IP adres aralığı 2* kendi aralığına sahip.

```powershell
  $IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty IPMatch `
    -MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Bir IP eşleşen tüm koşul kuralı oluşturma
```powershell
  $IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty Any
    
```

### <a name="create-a-custom-ip-allow-rule"></a>Bir özel Oluştur IP kuralı izin ver
   Kullanım [yeni AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) bir eylem tanımlayın ve bir önceliğini ayarlamak için komutu. Aşağıdaki örnekte, istemci IP'leri listesiyle eşleşen isteği izin verilir. 

```powershell
  $IPAllowRule = New-AzFrontDoorCustomRuleObject `
    -Name "IPAllowRule" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchCondition `
    -Action Allow -Priority 1
```
Bir blok kuralı önceki IP'ın izin verdiğinden daha düşük öncelikli tüm IP kuralı oluşturun.

```powershell
  $IPBlockAll = New-AzFrontDoorCustomRuleObject `
    -Name "IPDenyAll" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchALlCondition `
    -Action Block `
    -Priority 2
   ```

### <a name="configure-waf-policy"></a>WAF ilkesini yapılandırma
Ön kapısı profili kullanılarak içeren kaynak grubunun adını bulma `Get-AzResourceGroup`. Ardından, IP Blok kullanarak kural ile bir WAF ilkesini yapılandırma [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Bir ön kapısı ön uç konağa bağlantı WAF İlkesi

Mevcut bir ön uç konak WAF İlkesi nesnesini bağlama ve ön kapısı özelliklerini güncelleştirir. Ön kapısı nesnesini kullanarak ilk alınması [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). Ardından, ön uç Ayarla *WebApplicationFirewallPolicyLink* ResourceId özelliğini *$IPAllowPolicyExamplePS* önceki adımda oluşturulan [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutu.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> Bu örnekte, WAF ilke FrontendEndpoints [0] için uygulanır. WAF İlkesi, ön uç birine bağlanabilir. Yalnızca ayarlamanız gerekir *WebApplicationFirewallPolicyLink* bir WAF İlkesi bir ön kapısı ön uç bağlantısı için bir kez özelliği. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç için uygulanır.


## <a name="configure-waf-policy-with-resource-manager-template"></a>Resource Manager şablonu ile WAF ilkesini yapılandırma
Özel IP kısıtlama kurallarıyla ön kapısı hem de WAF ilkesi oluşturur şablonu görüntülemek [burada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [ön kapısı profil oluşturma](quickstart-create-front-door.md).
