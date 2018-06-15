---
title: Bir bölge olarak yedekli Azure uygulama ağ geçidi oluşturma
description: Her bölge seperarte ağ geçidi gerektirmeyen bir bölge olarak yedekli uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: amsriva
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/8/2018
ms.author: victorh
ms.openlocfilehash: 03bc58db813534fdd17c9567f6425ab7357ed901
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937759"
---
# <a name="create-a-zone-redundant-azure-application-gateway---private-preview"></a>Bölge olarak yedekli Azure uygulama ağ geçidi - özel Önizleme oluşturma

Uygulama ağ geçidi bölge olarak yedekli varolan uygulama ağ geçidi SKU'su dahil üzerinden çok sayıda geliştirmeleri sunan yeni bir SKU platformudur:
- **Bölge dayanıklılık** - bir uygulama ağ geçidi dağıtımı artık birden çok kullanılabilirlik sağlamak için gereken kaldırma bölgeleri kapsayabilir ve PIN ayrı bir uygulama ağ geçidi örnekleri her bölgede bir trafik Yöneticisi ile. Tek bir bölge veya uygulama ağ geçidi örnekleri dağıtıldığı birden fazla bölge böylece sağlama bölge hatası dayanıklılık seçebilirsiniz. Uygulamalar için arka uç havuzu kullanılabilirlik dilimlerinde benzer şekilde dağıtılabilir.
- **Performans iyileştirmeleri**
- **Statik VIP** -VIP uygulama ağ geçidi artık statik VIP türü özel olarak destekler. Bu uygulama ağ geçidi ile ilişkili VIP yeniden başlatmadan sonra değiştirmez sağlar.
- **Müşteri SSL sertifikaları için anahtar kasası tümleştirme**
- **Hızlı Dağıtım ve güncelleştirme zamanı**

> [!NOTE]
> Bölge olarak yedekli uygulama ağ geçidi şu anda özel önizlemede değil. Bu önizleme bir hizmet düzeyi sözleşmesi sağlanır ve üretim iş yükleri için önerilmez. Belirli özellikler desteklenmiyor olabilir veya özellikleri kısıtlanmış. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="supported-regions"></a>Desteklenen bölgeler

Bölge olarak yedekli uygulama ağ geçitleri, Doğu ABD 2 bölgede desteklenmemektedir. Daha fazla bölgeler yakında eklenecek.

## <a name="topologies"></a>Topolojileri

Geçerli sürümle birlikte, artık zonal artıklık almak için bölge sabitlenmiş uygulama ağ geçitleri oluşturmanız gerekir. Aynı uygulama ağ geçidi dağıtımı artık birden çok bölgenin yayılabilir.

En az üç örnekleri, tüm üç dilimlerinde yayılır emin olmak için gereklidir. Daha fazla örnekleri eklendikçe uygulama ağ geçidi örnekleri dilimlerinde dağıtır.

Aşağıdaki diyagram gibi önceki bölge olarak yedekli topolojileri Aranan:

![Eski yedek topolojisi](media/create-zone-redundant/old-redundant.png)

Yeni bölge olarak yedekli topoloji Bu diyagram gibi görünür:

![Yeni bölge olarak yedekli topolojisi](media/create-zone-redundant/new-redundant.png)

## <a name="deployment"></a>Dağıtım

### <a name="prerequisites"></a>Önkoşullar

Bölge olarak yedekli özelliği şu anda yalnızca özel önizleme olarak kullanılabilir. E-posta göndermeniz appgwxzone@microsoft.com Güvenilenler listesine olmalıdır. Bir onay aldıktan sonra sonraki adımlara geçebilirsiniz. E-posta uygulamaları güvenilir listeye almayı için aşağıdaki bilgileri ekleyin:

- Abonelik Kimliği
- Bölge adı
- Gerekli uygulama ağ geçitleri yaklaşık sayısı

### <a name="resource-manager-template-deployment"></a>Resource Manager şablonu dağıtımı

1. Kullandığınız abonelik daha önce belirtildiği gibi Güvenilenler listesine olduğundan emin olun.
2. Bir onay aldıktan sonra Azure hesabınızda oturum açın ve birden fazla aboneliğiniz varsa, uygun aboneliği seçin. Güvenilenler listesine olan abonelik seçtiğinizden emin olun.

   ```PowerShell
   Login-AzureRmAccount

   Select-AzureRmSubscription -Subscription "<whitelisted subscription name>”
   ```
3. Yeni bir kaynak grubu oluşturma

   ``` PowerShell
   New-AzureRmResourceGroup -Name <resource group name> -Location "East US 2"
   ```
4. Şablonlardan karşıdan [GitHub](https://github.com/amitsriva/CrossZonePreview) ve kaydettiğiniz bunları klasörü not edin.
5. Oluşturduğunuz kaynak grubunda yeni bir dağıtım oluşturun. Dağıtmadan önce şablonu ve parametre dosyasını uygun şekilde değiştirin. 

   Aşağıdaki diyagramda gösterildiği nerede Kiracı kimliği Azure Portal'da alabilirsiniz:

   ![Kiracı kimliği portalından](media/create-zone-redundant/tenant-id.png)

Şablon aşağıdaki kaynaklar oluşturur:

- **Atanan kullanıcı kimliğini** -bu uygulama ağ geçidi örnekleri anahtar kasası erişmek ve kullanıcı tarafından depolanan sertifikaları almak üzere etkinleştirmek için kullanılır.
- **KeyVault** – kullanıcının sertifikanın depolandığı anahtar kasası. Bu da önceden var olan bir anahtar kasası olabilir.
- **Gizli** – anahtar kasasında depolanan özel anahtar.
- **Erişim İlkesi** – uygulama ağ geçidi dağıtımları kullanıcı sertifikaları alabilmeleri atanan kullanıcı kimliğini kullanarak izin veren anahtar kasası uygulanan bir erişim ilkesi.
- **Genel IP adresi** – bir uygulama ağ geçidi erişmek için kullanılan IP adresi ayrılmış. Bu IP adresi, uygulama ağ geçidi yaşam döngüsü için hiçbir zaman değiştirir.
- **Ağ güvenlik grupları** – yapılandırılmış dinleyicisi bağlantı noktası trafiği açar uygulama ağ geçidi alt ağı üzerinde otomatik oluşturulan NSG bir. Bu açıkça oluşturulur ve bu NSG örtük bulunduğu önceki SKU karşılaştırıldığında yeni SKU yönetilebilir.
- **Sanal ağ** – uygulamalar ve uygulama ağ geçidi dağıtıldığı vnet.
- **Uygulama ağ geçidi** – gerekli bölgeleri durumlarda bir uygulama ağ geçidi oluşturur. Varsayılan olarak, tüm bölgeler (1,2,3) seçilir. SKU adı değiştirildi *Standard_v2*. Değiştirilebilir bu SKU adıdır. Şu anda, otomatik ölçeklendirme yapılandırma min ve max örnekleri gerekli sayıya ayarlayın yok. Otomatik ölçeklendirmeyi etkinleştirildikten sonra bu ayarlanabilir.

```PowerShell
New-AzureRmResourceGroupDeployment -Name Deployment1 -ResourceGroupName AmitVMSSLinuxTest9 -TemplateFile <complete path to template.json> -TemplateParameterFile <complete path to parameters.json>
```
### <a name="powershell-deployment"></a>PowerShell dağıtım

1. Kullanılan abonelik daha önce belirtildiği gibi önkoşulları Güvenilenler listesine olduğundan emin olun.
2. Gelen özel PowerShell MSI yükleyip [GitHub](https://github.com/amitsriva/CrossZonePreview/blob/master/Azure-Cmdlets-5.7.0.19009-x64.msi).
3. Önizleme kayıt onayı e-posta ile belirtilen konumdan özel PowerShell zip dosyasını indirin. Sürücünüze dosyanın sıkıştırmasını açın ve konumunu not edin.
4. Önizleme etkinleştirildikten sonra ilk önce hesabınızda oturum açmayı Önizleme modüllerini yükleyin:

   ```PowerShell
   $azurePSPath = "<complete path to Azure-PowerShell folder>"
   import-module "$azurePSPath\AzureRM.Profile\AzureRM.Profile.psd1"
   import-module "$azurePSPath\Azure.Storage\Azure.Storage.psd1"
   import-module "$azurePSPath\AzureRM.Resources\AzureRM.Resources.psd1"
   import-module "$azurePSPath\AzureRM.Network\AzureRM.Network.psd1"
   import-module "$azurePSPath\AzureRM.KeyVault\AzureRM.KeyVault.psd1"
   ```

4. Azure hesabınızda oturum açın ve birden fazla aboneliğiniz varsa, istediğiniz aboneliği seçin. Güvenilenler listesine olan uygun aboneliği seçin emin olun.
5. Çoğu için oluşturulan varlıkları adlarında ortak sabitleri oluşturmak için aşağıdaki komutları çalıştırın. 

   Girişleri adlandırma tercihinizi için gerekli olarak değiştirin.

   ```PowerShell
   $location = "eastus2"
   $version = "300"

   $rgname = "RG_A_$version"
   $appgwName = "AGW_A_$version"
   $vaultName = "KVA$version"
   $userAssignedIdentityName = "UI_A_$version"
   $certificateName = "KVCA$version"
   $nsgName = "NSG_A_$version"
   $vnetName = "VN_A_$version"
   $gwSubnetName = "SN_A_$version"
   $gipconfigname = "GC_A_$version"
   $publicIpName = "PIP_A_$version"
   $fipconfig01Name = "FC_A_$version"
   $poolName = "BP_A_$version"
   $frontendPort01Name = "FP1_A_$version"
   $frontendPort02Name = "FP2_A_$version"
   $poolSetting01Name = "BS_A_$version"
   $listener01Name = "HL1_A_$version"
   $listener02Name = "HL2_A_$version"
   $rule01Name = "RR1_A_$version"
   $rule02Name = "RR2_A_$version"
   $AddressPrefix = "111.111.222.0" 
   ```
6. Kaynak grubunu oluşturun:

   ```PowerShell
   $resourceGroup = New-AzureRmResourceGroup -Name $rgname -Location $location -Force
   ```
7. Kullanıcı atanmış uygulama ağ geçidi için erişim vermek için kullanılan anahtar Kasası'nı sertifikaları almak için kimlik oluşturma.

   ```PowerShell
   $userAssignedIdentity = New-AzureRmResource -ResourceGroupName $rgname -Location $location -ResourceName $userAssignedIdentityName -ResourceType Microsoft.ManagedIdentity/userAssignedIdentities -Force
   ```
8. Sertifikalarınızı depolamak için kullanılan anahtar kasası oluşturun:

   ```PowerShell
   $keyVault = New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgname -Location $location -EnableSoftDelete
   ```
9. Sertifika anahtar kasası için gizlilik olarak karşıya yükle:

   ```PowerShell
   $securepfxpwd = ConvertTo-SecureString -String "<password>" -AsPlainText -Force

   $cert = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name         $certificateName -FilePath ‘<path to pfx file>'  -Password $securepfxpwd
   ```
10. Erişim İlkesi atanan kullanıcı kimliğini kullanarak anahtar kasası atayın. Bu, anahtar kasası erişmek uygulama ağ geçidi örnekleri gizli sağlar:

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $rgname -PermissionsToSecrets get -ObjectId $userAssignedIdentity.Properties.principalId
   ```
11. Uygulama ağ geçidi alt ağının yeni dinleyicileri oluşturulduğu noktalarına erişmesine izin vermek için ağ güvenlik grubu (NSG) oluşturun.

    Örneğin, varsayılan HTTP/HTTPS için bağlantı noktalarını NSG 80, 443 ve 65200-65535 yönetim işlemleri için gelen erişim olanak tanır.

   ```PowerShell
   $srule01 = New-AzureRmNetworkSecurityRuleConfig -Name "listeners" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange 22,80,443 -Access Allow -Priority 100

   $srule02 = New-AzureRmNetworkSecurityRuleConfig -Name "managementPorts" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange "65200-65535" -Access Allow -Priority 101

   $nsg = New-AzureRmNetworkSecurityGroup -Name $nsgName -ResourceGroupName $rgname -Location $location -SecurityRules $srule01,$srule02 -Force
   ```

12. VNet ve alt ağları oluşturun:

   ```PowerShell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 
   $gwSubnetName -AddressPrefix "$AddressPrefix/24" -NetworkSecurityGroup $nsg

   $vnet = New-AzureRmvirtualNetwork -Name $vnetName -ResourceGroupName $rgname -Location $location -AddressPrefix "$AddressPrefix/24" -Subnet $gwSubnet -Force
   ```
13. Bir ortak türü ayrılmış/statik IP adresi oluşturun:

   ```PowerShell
   $publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgname -name $publicIpName -location $location -AllocationMethod Static -Sku Standard -Force
   ```

14. Uygulama ağ geçidi oluşturun:

   ```PowerShell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name $gipconfigname -Subnet $gwSubnet

   $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name $fipconfig01Name -PublicIPAddress $publicip

   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name $poolName -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

   $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort01Name -Port 443

   $fp02 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort02Name -Port 80

   $sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -KeyVaultSecretId $secret.Id

   $listener01 = New-AzureRmApplicationGatewayHttpListener -Name $listener01Name -Protocol Https -FrontendIPConfiguration
 $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01

   $listener02 = New-AzureRmApplicationGatewayHttpListener -Name $listener02Name -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02

   $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name $poolSetting01Name -Port 80 -Protocol Http -CookieBasedAffinity Disabled

   $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule01Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

   $rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule02Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool

   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2

   $listeners = @($listener02)

   $fps = @($fp01, $fp02)

   $fipconfigs = @($fipconfig01)

   $sslCerts = @($sslCert01)

   $rules = @($rule01, $rule02)

   $listeners = @($listener01, $listener02)

   $appgw = New-AzureRmApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Location $location -UserAssignedIdentityId $userAssignedIdentity.ResourceId -Probes $probeHttps -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfigs -FrontendPorts $fps -HttpListeners $listeners -RequestRoutingRules $rules -Sku $sku -SslPolicy $sslPolicy -sslCertificates $sslCerts -Force
   ```

15. Oluşturulan uygulama ağ geçidi genel IP adresi al:

   ```PowerShell
   $pip = Get-AzureRmPublicIpAddress -Name $publicIpName -ResourceGroupName $rgname $pip.IpAddress
   ```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

-  Uygulama ağ geçidi Önizleme için faturalandırılmaya?

   Önizleme sırasında ücretsizdir. Size sanal makineleri vb. gibi anahtar kasası, uygulama ağ geçidi dışındaki kaynaklar için faturalandırılır.
- Hangi bölgeleri Önizleme kullanılabilir?

   Önizleme Doğu ABD 2 bölgede şu anda kullanılamıyor. Daha fazla bölgeler yakında eklenecek.
- Portal Önizleme'de destekleniyor mu?

   Hayır, destek özel Önizleme sırasında özel bir PowerShell modülü ve Resource Manager şablonu ile sınırlıdır.

- Üretim iş yükü özel Önizleme sırasında destekleniyor mu?

   Hayır, SLA veya yoktur destek özel Önizleme sırasında. Önizlemeler sırasında üretim iş yükleri koymak için önerilmez. Önizleme için e-posta diğer adını kullanarak ürün grubu ile doğrudan etkileşim için destek sınırlıdır.

- Sorunları nasıl rapor edebilirim?

   Özel önizleme hatalar içerebilir ve sık kodu dağıtımlar olabilir. Destek diğer adlarını kullanma appgwxzone@microsoft.com sorunları ve Yardım raporlama.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar


|Sorun  |Ayrıntılar  |
|---------|---------|---------|
|Faturalandırma     |Şu anda hiçbir faturalama|
|Tanılama günlükleri (ölçümleri değil)     |Performans ve istek/yanıt günlükleri şu anda görünmüyor|
|CLI/Portal/SDK     |Portal, CLI veya SDK için destek yok. Portal, ağ geçitleri önizlemek için güncelleştirmeleri vermek için kullanılmamalıdır.|
|Şablon aracılığıyla güncelleştirme zaman zaman başarısız olur     |KeyVault erişim ilkesi ile yarış durumu nedeniyle budur|Atanan kullanıcı kimliğini ve anahtar kasası oluşturulur sonra şablonu kaldırılabilir ve yalnızca gizlilik ve kimlik başvurular şablonda gereklidir.|
|Otomatik ölçeklendirme     |Şu anda otomatik ölçeklendirmeyi desteği|
|WAF     |WAF şu anda desteklenmiyor|
|Kullanıcı sertifikaları ve dinamik VIP'ler sağlanan     |Bu yeni modelinde desteklenmez. Anahtar kasası, sertifikalar ve statik VIP'ler depolamak için kullanın.|
|Eski için aynı alt ağ ve uygulama ağ geçidi Önizleme sürümü     |Mevcut bir uygulama ağ geçidi (eski modeli) sahip bir alt ağa özel Önizleme sürümü için kullanılamaz.|
|HTTP/2, arka uç olarak FIPS modunda, WebSocket, Azure Web uygulamaları     |Şu anda desteklenmiyor |


## <a name="support-and-feedback"></a>Destek ve geri bildirim

İçin destek ve geri bildirim için başvurun appgwxzone@microsoft.com. Uygulama ağ geçidi ürün grubu geliştirmeleri için Geri bildirimlerinizi ve kılavuzluk mutluluk gerektiğinde.

## <a name="next-steps"></a>Sonraki adımlar

Diğer uygulama ağ geçidi özellikleri hakkında bilgi edinin:

- [Azure uygulama ağ geçidi nedir?](overview.md)