---
title: Bir IP kısıtlama kuralı Azure ön kapısı hizmeti için bir web uygulaması güvenlik duvarı kuralı yapılandırma
description: Mevcut bir Azure ön kapısı hizmet uç noktası için IP adreslerini kısıtlamak için bir web uygulaması güvenlik duvarı kuralı yapılandırmayı öğrenin.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 88c5c284f26203ff3d6c39810a7b2810c1ebbc5a
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743157"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Bir IP kısıtlama kuralı ile bir web uygulaması güvenlik duvarı Azure ön kapısı hizmeti için yapılandırma
Bu makalede, IP kısıtlama kuralları, Azure CLI, Azure PowerShell veya Azure Resource Manager şablonu kullanarak Azure ön kapısı hizmeti için bir web uygulaması Güvenlik Duvarı (WAF) yapılandırma işlemini göstermektedir.

Bir IP adresi tabanlı erişim denetimi kuralı olanak sağlayan bir özel WAF kuralıdır web uygulamalarınıza erişimi denetler. IP adresleri veya IP adresi aralıkları listesi sınıfsız etki alanları arası yönlendirme (CIDR) biçiminde belirterek bunu yapar.

Varsayılan olarak, web uygulamanıza internet'ten erişilebilir. Bilinen IP adresleri veya IP adresi aralıklarını listesinden istemcilerine erişimi sınırlandırmak istiyorsanız, iki IP eşleşen kurallar oluşturmanız gerekir. IP eşleşen ilk kural eşleşen değerler olarak IP adreslerinin listesini içerir ve eylem ayarlar **izin**. İkincisi, daha düşük önceliğe sahip diğer tüm IP adresleri kullanarak engeller **tüm** işleci ve eylem ayarını **blok**. Bir IP kısıtlama kuralı uygulandıktan sonra bu izin verilen liste dışındaki adresleri kaynaklanan isteklerini bir 403 Yasak yanıtı alır.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Azure CLI ile bir WAF ilkesini yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesi yapılandırmak başlamadan önce CLI ortamınızı ayarlamak ve bir Azure ön kapısı hizmet profilini oluşturun.

#### <a name="set-up-the-azure-cli-environment"></a>Azure CLI ortamını ayarlama
1. Yükleme [Azure CLI](/cli/azure/install-azure-cli), veya Azure Cloud Shell kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Seçin **deneyin** izleyin ve ardından Azure hesabınızda oturum açar Cloud Shell'i oturumunda CLI komutlarında düğmesi. Oturumu başlatıldıktan sonra girin `az extension add --name front-door` Azure ön kapısı hizmeti uzantısı eklemek için.
 2. CLI'yi yerel olarak Bash'te kullanıyorsanız, Azure'da kullanarak oturum `az login`.

#### <a name="create-an-azure-front-door-service-profile"></a>Bir Azure ön kapısı hizmet profilini oluşturma
Açıklanan yönergeleri izleyerek bir Azure ön kapısı hizmet profili oluşturma [hızlı başlangıç: Yüksek oranda kullanılabilir bir küresel web uygulaması için bir ön kapı oluşturmak](quickstart-create-front-door.md).

### <a name="create-a-waf-policy"></a>Bir WAF ilkesi oluşturma

Bir WAF İlkesi kullanarak oluşturma [az ağ waf ilkesi oluşturma](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) komutu. Örnekte aşağıdaki gibi ilke adını değiştirin *IPAllowPolicyExampleCLI* ile benzersiz bir ilke adı.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Bir özel IP erişim denetimi Kuralı Ekle

Kullanım [az ağ ilkesi waf özel kural oluşturma](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) yeni oluşturduğunuz özel IP erişim denetim için bir kural WAF ilkesi eklemek için komutu.

Aşağıdaki örneklerde:
-  değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.
-  değiştirin *IP adresi aralığı 1*, *IP adres aralığı 2* kendi aralığına sahip.

İlk olarak oluşturmak izin verme kuralı için belirtilen adresleri.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch "<ip-address-range-1>","<ip-address-range-2>" \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Ardından, oluşturun bir **tüm block** önceki daha düşük önceliğe sahip kural **izin** kuralı. Yeniden değiştirin *IPAllowPolicyExampleCLI* aşağıdaki örnekte, daha önce oluşturduğunuz, benzersiz bir ilke ile.

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
    
### <a name="find-the-id-of-a-waf-policy"></a>Bir WAF ilke Kimliğini bulun 
Bir WAF ilkesinin kimliği kullanarak bulma [az ağ waf-policy show](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) komutu. Değiştirin *IPAllowPolicyExampleCLI* aşağıdaki örnekte, daha önce oluşturduğunuz, benzersiz bir ilke ile.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Bir WAF ilke Azure ön kapısı hizmeti ön uç konağa bağlantı

Azure ön kapısı hizmeti *WebApplicationFirewallPolicyLink* kullanarak ilke kimliği için kimlik [az ağ ön kapısı güncelleştirme](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) komutu. Değiştirin *IPAllowPolicyExampleCLI* daha önce oluşturduğunuz, benzersiz bir ilke ile.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
Bu örnekte, için WAF ilkenin geçerli olduğu **FrontendEndpoints [0]** . WAF İlkesi, ön uçlar birine bağlayabilirsiniz.
> [!Note]
> Ayarlanacak ihtiyacınız **WebApplicationFirewallPolicyLink** WAF ilke için bir Azure ön kapısı hizmeti ön ucu bağlamak için yalnızca bir kez özelliği. Sonraki ilke güncelleştirmeleri için ön uç otomatik olarak uygulanır.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Azure PowerShell ile bir WAF ilkesi yapılandırma

### <a name="prerequisites"></a>Önkoşullar
Bir IP kısıtlama ilkesi yapılandırmak başlamadan önce PowerShell ortamınızı ayarlamak ve bir Azure ön kapısı hizmet profilini oluşturun.

#### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell kullanan cmdlet kümesi sağlar [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) Azure kaynaklarını yönetmek için model.

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. PowerShell için Azure kimlik bilgilerinizi kullanarak oturum açmak için sayfadaki yönergeleri izleyin ve ardından Az modülünü yükleyin.

1. Aşağıdaki komutu kullanarak Azure'a bağlanma ve daha sonra oturum açmak için etkileşimli bir iletişim kutusunu kullanın.
    ```
    Connect-AzAccount
    ```
 2. Bir Azure ön kapısı hizmeti modülü yüklemeden önce geçerli sürümü PowerShellGet Modülü yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve ardından PowerShell açın.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Az.FrontDoor modülü, aşağıdaki komutu kullanarak yükleyin. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Bir Azure ön kapısı hizmet profilini oluşturma
Açıklanan yönergeleri izleyerek bir Azure ön kapısı hizmet profili oluşturma [hızlı başlangıç: Yüksek oranda kullanılabilir bir küresel web uygulaması için bir ön kapı oluşturmak](quickstart-create-front-door.md).

### <a name="define-an-ip-match-condition"></a>Bir IP eşleşme koşulu tanımla
Kullanım [yeni AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) bir IP eşleşme koşulu tanımlamak için komutu.
Aşağıdaki örnekte, değiştirin *IP adresi aralığı 1*, *IP adres aralığı 2* kendi aralığına sahip.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Bir IP oluşturma *tüm koşulla eşleşen* aşağıdaki komutu kullanarak kuralı:
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
Oluşturma bir **tüm block** önceki IP daha düşük önceliğe sahip kural **izin** kuralı.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>Bir WAF ilkesini yapılandırma
Azure ön kapısı hizmet profilini kullanarak bulunduğu kaynak grubunun adını bulma `Get-AzResourceGroup`. Ardından, bir WAF ilke IP ile yapılandırın **tüm block** kuralı kullanarak [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Bir WAF ilke Azure ön kapısı hizmeti ön uç konağa bağlantı

Bir WAF İlkesi nesnesini, bir var olan ön uç konak ve güncelleştirme Azure ön kapısı hizmet özelliklerine bağlayın. İlk olarak, Azure ön kapısı hizmet nesnesi kullanarak almak [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). Ardından, ayarlama **WebApplicationFirewallPolicyLink** kaynak kimliği özelliğini *$IPAllowPolicyExamplePS*, kullanarak önceki adımda oluşturulan [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)komutu.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> Bu örnekte, için WAF ilkenin geçerli olduğu **FrontendEndpoints [0]** . Bir WAF İlkesi, ön uçlar birine bağlayabilirsiniz. Ayarlanacak ihtiyacınız **WebApplicationFirewallPolicyLink** WAF ilke için bir Azure ön kapısı hizmeti ön ucu bağlamak için yalnızca bir kez özelliği. Sonraki ilke güncelleştirmeleri için ön uç otomatik olarak uygulanır.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Resource Manager şablonu ile bir WAF ilkesini yapılandırma
Özel IP kısıtlama kurallarıyla bir Azure ön kapısı hizmet İlkesi ve bir WAF ilkesini oluşturan şablonu görüntülemek üzere Git [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure ön kapısı hizmet profili oluşturma](quickstart-create-front-door.md).
