---
title: Yönetilen kimlik VM uzantısını kullanmayı bırakmak ve Azure örnek meta veri Hizmeti uç noktası'nı kullanmaya başlayın
description: VM uzantısını kullanmayı bırakmak ve kimlik doğrulaması için Azure örnek meta veri hizmeti (IMDS) kullanmaya başlamak için yönergeleri adım adım.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/25/2018
ms.author: markvi
ms.openlocfilehash: 5b3c6c99b05320ee53c3ff49f5c299650c32e939
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498341"
---
# <a name="how-to-stop-using-the-virtual-machine-managed-identities-extension-and-start-using-the-azure-instance-metadata-service"></a>Sanal makineyi durdurmak nasıl yönetilen kimlikleri uzantısı ve Azure örnek meta veri hizmeti kullanmaya başlayın

## <a name="virtual-machine-extension-for-managed-identities"></a>Yönetilen kimlikleri için sanal makine uzantısı

Yönetilen kimlikleri için sanal makine uzantısı, sanal makine içinde yönetilen bir kimlik belirteci istemek için kullanılır. İş akışı, aşağıdaki adımlardan oluşur:

1. İlk olarak, yerel uç nokta kaynağı içindeki iş yükü çağırır `http://localhost/oauth2/token` bir erişim belirteci istemek için.
2. Sanal makine uzantısı sonra Azure AD'den bir erişim belirteci istemek için yönetilen kimlik için kimlik bilgilerini kullanır... 
3. Erişim belirteci, çağırana döndürülür ve Azure anahtar kasası veya Azure depolama gibi Azure AD kimlik doğrulamasını destekleyen hizmetler için kimlik doğrulaması için kullanılabilir.

Sonraki bölümde açıklanan tüm sınırlamaları nedeniyle, yönetilen kimlik VM uzantısını Azure örnek meta veri hizmeti (IMDS) eşdeğer uç noktayı kullanmak lehine kullanım dışıdır

### <a name="provision-the-extension"></a>Uzantı sağlama 

Bir sanal makine veya sanal makine ölçek kümesi yönetilen bir kimlik sağlamak için yapılandırdığınızda, isteğe bağlı olabilir karar isteğe bağlı olarak VM uzantısıyla Azure kaynakları için yönetilen kimlikleri sağlamak için seçtiğiniz `-Type` parametresi[ Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet'i. Geçirebilirsiniz `ManagedIdentityExtensionForWindows` veya `ManagedIdentityExtensionForLinux`, sanal makine türüne bağlı olarak ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
```

VM uzantısı, aşağıdaki JSON ekleyerek sağlamak için Azure Resource Manager dağıtım şablonu kullanabilirsiniz `resources` şablon bölümü (kullanın `ManagedIdentityExtensionForLinux` Linux sürümü için adı ve türü öğeler için).

    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
    
Sanal makine ölçek kümeleri ile çalışıyorsanız, uzantısını kullanarak Azure kaynaklarınızı sanal makine ölçek kümesi için de yönetilen kimlikleri sağlayabileceğiniz [Ekle AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet'i. Geçirebilirsiniz `ManagedIdentityExtensionForWindows` veya `ManagedIdentityExtensionForLinux`türüne bağlı olarak sanal makine ölçek kümesi ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzVmss
   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```
Sanal makine ölçek sağlamak üzere Azure Resource Manager dağıtım şablonu uzantısıyla ayarlama eklemek için aşağıdaki JSON `extensionpProfile` şablon bölümü (kullanın `ManagedIdentityExtensionForLinux` Linux sürümü için adı ve türü öğeler için).

    ```json
    "extensionProfile": {
        "extensions": [
            {
                "name": "ManagedIdentityWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
    ```

Sanal makine uzantısı sağlanıyor DNS arama hataları nedeniyle başarısız olabilir. Bu durumda, sanal makineyi yeniden başlatın ve yeniden deneyin. 

### <a name="remove-the-extension"></a>Uzantıyı kaldırın 
Uzantıyı kaldırmak için `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` anahtarı (sanal makine türünü bağlı olarak) [az vm uzantısı silme](https://docs.microsoft.com/cli/azure/vm/), veya [az vmss uzantısı silme](https://docs.microsoft.com/cli/azure/vmss) için sanal makine ölçek Azure CLI kullanarak ayarlar veya `Remove-AzVMExtension` Powershell için:

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

```powershell
Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
```

### <a name="acquire-a-token-using-the-virtual-machine-extension"></a>Sanal makine uzantısı aracılığıyla bir belirteç Al

VM uzantısı uç noktası Azure kaynakları için yönetilen kimliklerle bir örnek istek verilmiştir:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktasından veri almak istediğiniz gösteren HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:50342/oauth2/token` | Burada 50342 varsayılan bağlantı noktası ve yapılandırılabilir Azure kaynaklarını uç noktası için yönetilen kimlikleri. |
| `resource` | Hedef kaynağın uygulama kimliği URİ'sini belirten bir sorgu dizesi parametresi. Ayrıca şurada görünür `aud` verilen belirtecin (dinleyici) talep. Bu örnekte Azure Resource Manager'a erişmek için bir belirteç isteyen bir uygulama kimliği URI'si, sahip olduğu https://management.azure.com/. |
| `Metadata` | Azure kaynakları için yönetilen kimlik olarak sunucu tarafı istek sahteciliği (SSRF) saldırılara karşı bir risk azaltma gerekli bir HTTP isteği üstbilgisi alanının. Bu değer true", tamamen küçük için" olarak ayarlanmalıdır.|
| `object_id` | (İsteğe bağlı) Belirteç için istediğiniz yönetilen kimlik object_id belirten bir sorgu dizesi parametresi. Sanal makinenize birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, gerekmez.|
| `client_id` | (İsteğe bağlı) Belirteç için istediğiniz yönetilen kimlik client_id belirten bir sorgu dizesi parametresi. Sanal makinenize birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, gerekmez.|


Örnek yanıt:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim belirteci. Güvenli bir REST API'nin çağrılması durumunda, belirteç katıştırılmış `Authorization` isteği üstbilgisi alanının arayan kimliğini doğrulamak API izin vererek "bearer" Token olarak. | 
| `refresh_token` | Tarafından yönetilen kimlikleri, Azure kaynakları için kullanılmaz. |
| `expires_in` | Erişim belirteci, verme zamandan süresi dolmadan önce geçerli olması için devam saniye sayısı. Verme süresini belirtecin içinde bulunabilir `iat` talep. |
| `expires_on` | Erişim belirtecinin süresi dolduğunda TimeSpan değeri. Saniyeyi tarih gösterilir "1970-01-01T0:0:0Z UTC" (belirtecinin karşılık gelen `exp` talep). |
| `not_before` | Erişim belirteci etkinleşir ve kabul edilebilir süre. Saniyeyi tarih gösterilir "1970-01-01T0:0:0Z UTC" (belirtecinin karşılık gelen `nbf` talep). |
| `resource` | Erişim belirtecine hangi eşleşmelerini istendi kaynak `resource` sorgu dizesi parametresi istek. |
| `token_type` | Kaynak bu belirtecin taşıyıcı için erişim verebilirsiniz anlamına gelir "Bearer" erişim belirteci olan Belirtecin türü. |


### <a name="troubleshoot-the-virtual-machine-extension"></a>Sanal makine uzantısı sorunlarını giderme 

#### <a name="restart-the-virtual-machine-extension-after-a-failure"></a>Sanal makine uzantısı bir hatadan sonra yeniden başlatın.

Uzantı durursa, Windows ve Linux'ın, aşağıdaki cmdlet'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Uzantı adı ve türü için Windows şöyledir: `ManagedIdentityExtensionForWindows`
- Uzantı adı ve türü için Linux şöyledir: `ManagedIdentityExtensionForLinux`

#### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Otomasyon betiği" şema dışarı aktarma uzantısı Azure kaynakları için yönetilen kimlikleri için çalışırken başarısız olur.

Azure kaynakları için yönetilen kimlikleri etkin olduğunda bir sanal makinede, sanal makine veya kaynak grubu için "Otomasyon betiği" özelliğini kullanmaya çalışırken şu hata gösterilir:

![Otomasyon betiği Azure kaynakları için yönetilen kimlikleri dışarı aktarma hatası](./media/howto-migrate-vm-extension/automation-script-export-error.png)

Sanal makine uzantısını Azure kaynakları için yönetilen kimlik şu anda desteklemiyor ve şeması için bir kaynak grubu şablonu dışarı aktarma yeteneği. Sonuç olarak oluşturulan şablon kaynağında Azure kaynakları için yönetilen kimlikleri etkinleştirmek için yapılandırma parametreleri göstermez. Bu bölümlerde örneklerde izleyerek elle eklenebilir [yapılandırma yönetilen bir Azure sanal makine şablonları kullanarak Azure kaynakları için kimlikleri](qs-configure-template-windows-vm.md).

Şema dışarı aktarma işlevi Azure kaynaklarını sanal makine uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) için yönetilen kimlikleri için kullanılabilir hale geldiğinde, içinde listelenecektir [verme kaynak VM uzantıları içeren grupları ](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

## <a name="limitations-of-the-virtual-machine-extension"></a>Sanal makine uzantısı sınırlamaları 

Sanal makine uzantısını kullanmanın birkaç önemli sınırlamaları vardır. 

 * En önemli sınırlama belirteçler istemek için kullanılan kimlik bilgileri sanal makine üzerinde depolanan gerçeğidir. Sanal makine başarıyla breaches bir saldırgan, kimlik bilgilerini sızdırabilir olabilir. 
 * Ayrıca, sanal makine uzantısı ile değiştirmek için derleme ve bu dağıtımların tümünde uzantıyı test etmek için çok büyük bir geliştirme hala çeşitli Linux dağıtımları tarafından desteklenmiyor. Şu anda, yalnızca aşağıdaki Linux dağıtımları desteklenmektedir: 
    * CoreOS kararlı
    * CentOS 7.1 
    * Red Hat 7.2 
    * Ubuntu 15.04 
    * Ubuntu 16.04
 * Sanal makine uzantısı sağlanacak de olduğu gibi yönetilen kimliklerle sanal makineleri dağıtmak için bir performans etkisi yoktur. 
 * Son olarak, sanal makine uzantısı yalnızca kullanıcı tarafından atanan 32 yönetilen kimlikleri sanal makine başına sahip destekleyebilir. 

## <a name="azure-instance-metadata-service"></a>Azure örnek meta veri hizmeti

[Azure örnek meta veri hizmeti (IMDS)](/azure/virtual-machines/windows/instance-metadata-service) yönetmek ve sanal makinelerinizi yapılandırmak için kullanılan sanal makine örneklerini çalıştırma hakkında bilgi sağlayan bir REST uç noktası. İyi bilinen yönlendirilemeyen IP adresinde uç noktası kullanılabilir (`169.254.169.254`), erişilebilir yalnızca sanal makine içinde.

Azure IMDS isteği belirteçleri kullanarak çeşitli avantajları vardır. 

1. Sanal makineye dış bir hizmettir, bu nedenle tarafından yönetilen kimlikleri kullanılan kimlik bilgileri artık sanal makinede mevcut değil. Bunun yerine, bunlar barındırılan ve Azure sanal makinesi konak makinesi üzerinde güvenli.   
2. Tüm Windows ve Linux Azure Iaas üzerinde desteklenen işletim sistemleri, yönetilen kimlikleri kullanabilirsiniz.
3. Dağıtım, daha hızlı ve kolay, olduğundan VM uzantısı artık hazırlanması gerekir.
4. IMDS ile 1000 kullanıcı tarafından atanan yönetilen kimlikleri kadar uç noktası tek bir sanal makineye atanabilir.
5. Bu sanal makine uzantısı aracılığıyla aksine IMDS kullanarak istekleri için önemli bir değişiklik yoktur, bu nedenle şu anda sanal makine uzantısı kullanan var olan dağıtımlar bağlantı noktasına nispeten basittir.

Sanal makine uzantısı kullanımdan sonra bu nedenlerle, Azure IMDS hizmet isteği belirteçleri en doğru şekilde olacaktır. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Bir erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için yönetilen kimliklerini kullanma](how-to-use-vm-token.md)
* [Azure örnek meta veri hizmeti](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)
