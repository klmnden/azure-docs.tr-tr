---
title: Öğretici PowerShell - Azure ayrılmış HSM kullanarak mevcut bir sanal ağa dağıtma | Microsoft Docs
description: PowerShell kullanarak mevcut bir sanal ağa ayrılmış bir HSM dağıtmayı gösteren öğretici.
services: dedicated-hsm
documentationcenter: na
author: barclayn
manager: barbkess
editor: ''
ms.service: key-vault
ms.topic: tutorial
ms.custom: mvc, seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 581ce6d75df8f42bb72bbfc93e85684d97620e3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159045"
---
# <a name="tutorial--deploying-hsms-into-an-existing-virtual-network-using-powershell"></a>Öğretici: PowerShell kullanarak mevcut sanal ağına HSM'ler dağıtma

Azure ayrılmış HSM hizmetini fiziksel bir cihaz için tam yönetim denetimi ve tam yönetim sorumluluk ile tek bir müşterinin kullanım sağlar. Fiziksel donanım sağlama nedeniyle Microsoft kapasite etkili bir şekilde yönetildiğinden emin olmak için bu cihazları nasıl ayrıldığını denetleyen gerekir. Sonuç olarak, bir Azure aboneliğinde, ayrılmış HSM hizmetini normalde kaynak sağlama için görünür olmaz. Ayrılmış HSM hizmetine erişim gerektiren herhangi bir Azure müşterisi ilk başvurmalısınız, Microsoft hesap yöneticinize istek kayıt için ayrılmış HSM hizmeti. Yalnızca bu işlemi başarıyla tamamlandıktan sonra sağlama mümkün olacaktır.
Tipik bir sağlama işlemini göstermek için bu öğreticiyi amaçlar burada:

- Bir müşteri sanal ağ zaten var
- Bir sanal makineye sahip oldukları
- Bunlar, mevcut ortamına HSM kaynakları eklemeniz gerekir.

Tipik, yüksek kullanılabilirlik, çok bölgeli dağıtım mimarisi şu şekilde görünebilir:

![Çok bölgeli dağıtım](media/tutorial-deploy-hsm-powershell/high-availability.png)

Bu öğreticide HSM'ler çifti üzerinde odaklanır ve gerekli ExpressRoute (bkz. Yukarıdaki alt ağ 1) bir sanal mevcut tümleştirilmektedir ağ geçidi ağ (VNET 1 yukarıdaki bakın).  Diğer tüm kaynaklar standart Azure kaynaklarıdır. Aynı tümleştirme işlemi HSM'ler için yukarıdaki VNET 3 4 ağda kullanılabilir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure ayrılmış HSM Azure portalında şu anda kullanılamıyor, bu nedenle tüm etkileşim hizmeti ile komut satırı veya kullanarak PowerShell olur. Bu öğreticide, Azure Cloud Shell'de PowerShell kullanacaksınız. PowerShell için yeni başladıysanız, izleme Buradaki yönergeleri Başlarken: [Azure PowerShell kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Varsayımlar:

- Azure ayrılmış HSM kayıt sürecinde olan ve hizmeti kullanmak için onaylanmış. Aksi durumda, ardından ayrıntılar için Microsoft hesap temsilcinize başvurun. 
- Bu kaynaklar için bir kaynak grubu oluşturdunuz ve Bu öğreticide dağıtılan yenilerini bu gruba katılacak.
- Gerekli sanal ağ, alt ağ ve sanal makineler Yukarıdaki diyagramda göre oluşturmuş ve artık 2 HSM'ler bu dağıtımı ile tümleştirmek istediğiniz.

Azure portalında zaten gittiğiniz ve Cloud Shell açtığınız tüm talimatları varsayılır (seçin "\>\_" en üste portal'ın sağ).

## <a name="provisioning-a-dedicated-hsm"></a>Ayrılmış HSM sağlama

HSM'ler sağlama ve var olan bir sanal ağı ExpressRoute ağ geçidi üzerinden oturum tümleştirme doğrulanmış kullanarak ssh erişimi ve diğer yapılandırma etkinlikleri için HSM cihazını temel kullanılabilirliğini sağlamak için komut satırı aracı. Aşağıdaki komutlar, HSM kaynaklar ve ilişkili ağ kaynakları oluşturmak için Resource Manager şablonu kullanır.

### <a name="validating-feature-registration"></a>Özellik kaydı doğrulanıyor

Yukarıda belirtildiği gibi herhangi bir sağlama etkinliği ayrılmış HSM hizmetini, aboneliğiniz için kayıtlı olmasını gerektiriyor. Doğrulamak için Azure portal cloud shell'de aşağıdaki PowerShell komutunu çalıştırın. 

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.HardwareSecurityModules -FeatureName AzureDedicatedHsm
```

Aşağıdaki komut, HSM adanmış hizmet için gereken ağ özellikleri doğrular.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowBaremetalServers
```

Herhangi devam etmeden önce her iki komutu (aşağıda gösterildiği gibi) "Kaydedildi" durumuna döndürmeniz gerekir.  Bu hizmet için kaydetmeniz gerekirse, Microsoft hesap temsilcinize başvurun.

![Abonelik durumu](media/tutorial-deploy-hsm-powershell/subscription-status.png)

### <a name="creating-hsm-resources"></a>HSM kaynakları oluşturma

Bir HSM cihazını bir müşterilerin sanal ağa sağlanır. Bu, bir alt ağ gereksinimi anlamına gelir. Bir ExpressRoute ağ geçidi için sanal ağ ile fiziksel cihazı arasındaki iletişimi etkinleştirmek HSM bağımlılıktır ve son olarak bir sanal makine Gemalto istemci yazılımını kullanarak HSM cihazınıza erişim hakkı gereklidir. Bu kaynaklar, kullanım kolaylığı için karşılık gelen parametre dosyası ile bir şablon dosyasına toplanana. Dosyaları doğrudan Microsoft'a başvurarak kullanılabilir HSMrequest@Microsoft.com.

Dosyaları oluşturduktan sonra kaynaklar için tercih edilen adlarınızı eklemek için parametre dosyasını düzenlemeniz gerekir. Bu "value" satırları düzenleme anlamına gelir: "".

- `namingInfix` HSM kaynakların adları için önek
- `ExistingVirtualNetworkName` HSM'ler için kullanılan sanal ağ adı
- `DedicatedHsmResourceName1` Veri Merkezi damganızda 1 HSM kaynağın adı
- `DedicatedHsmResourceName2` Veri Merkezi damganızda 2 HSM kaynağın adı
- `hsmSubnetRange` HSM'ler için alt ağ IP adresi aralığı
- `ERSubnetRange` VNET ağ geçidi alt ağı IP adresi aralığı

Bu değişiklikler örneği aşağıdaki gibidir:

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "namingInfix": {
      "value": "MyHSM"
    },
    "ExistingVirtualNetworkName": {
      "value": "MyHSM-vnet"
    },
    "DedicatedHsmResourceName1": {
      "value": "HSM1"
    },
    "DedicatedHsmResourceName2": {
      "value": "HSM2"
    },
    "hsmSubnetRange": {
      "value": "10.0.2.0/24"
    },
    "ERSubnetRange": {
      "value": "10.0.255.0/26"
    },
  }
}
```

İlişkili Resource Manager şablon dosyası 6 kaynakları bu bilgileri ile oluşturacak:

- Belirtilen sanal ağda HSM'ler için bir alt ağ
- Sanal ağ geçidi için bir alt ağ 
- HSM cihazlarına sanal AĞA bağlanan bir sanal ağ geçidi
- Ağ geçidi için genel bir IP adresi
- Bir HSM damgasında 1
- Bir HSM'de damga 2

Parametre değerlerini ayarladıktan sonra dosyaları kullanmak için Azure portalında cloud shell dosya paylaşımına yüklenmesi gerekir. Azure portalında "\>\_" cloud shell simgesi sağ üst ve bu komut ortam ekranın alt kısmına hale getirir. Bu seçenekleri BASH şunlardır ve PowerShell ve BASH seçin değilse zaten ayarlanmış.

Komut kabuğu araç çubuğundaki karşıya yükleme/indirme seçeneği vardır ve bu şablonu ve parametre dosyalarını karşıya dosya paylaşımınızı seçmeniz gerekir:

![Dosya Paylaşımı](media/tutorial-deploy-hsm-powershell/file-share.png)

Dosyalar yüklendiğinde bu kaynakları oluşturmaya hazır olursunuz.
Yeni HSM oluşturmadan önce emin olmanız gerekir bazı önkoşul kaynak yerinde kaynaklardır. İşlem, HSM'ler ve ağ geçidi alt ağı aralıklarına sahip bir sanal ağ olması gerekir. Aşağıdaki komutları ne tür bir sanal ağda oluşturacak bir örnek olarak görev yapar.

```powershell
$compute = New-AzVirtualNetworkSubnetConfig `
  -Name compute `
  -AddressPrefix 10.2.0.0/24
```

```powershell
$delegation = New-AzDelegation `
  -Name "myDelegation" `
  -ServiceName "Microsoft.HardwareSecurityModules/dedicatedHSMs"

```

```powershell
$hsmsubnet = New-AzVirtualNetworkSubnetConfig ` 
  -Name hsmsubnet ` 
  -AddressPrefix 10.2.1.0/24 ` 
  -Delegation $delegation 

```

```powershell

$gwsubnet= New-AzVirtualNetworkSubnetConfig `
  -Name GatewaySubnet `
  -AddressPrefix 10.2.255.0/26

```

```powershell

New-AzVirtualNetwork `
  -Name myHSM-vnet `
  -ResourceGroupName myRG `
  -Location westus `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $compute, $hsmsubnet, $gwsubnet

```

>[!NOTE]
>Unutmayın sanal ağ için en önemli yapılandırma olan alt ağ HSM cihazını için temsilciler "Microsoft.HardwareSecurityModules/dedicatedHSMs" için ayarlanmış olması gerekir.  HSM sağlama bu olmadan çalışmaz.

Tüm önkoşulların yerine getirildiğinden sonra değerleri kendi benzersiz adlara sahip güncelleştirilen sağlama Resource Manager şablonu kullanmak için aşağıdaki komutu çalıştırın (en az bir kaynak grubu adı):

```powershell

New-AzResourceGroupDeployment -ResourceGroupName myRG `
     -TemplateFile .\Deploy-2HSM-toVNET-Template.json `
     -TemplateParameterFile .\Deploy-2HSM-toVNET-Params.json `
     -Name HSMdeploy -Verbose

```

Bu komutun tamamlanması yaklaşık 20 dakika sürer. "-Verbose" seçeneğini kullanılan emin olmanızı durumunu sürekli olarak görüntülenir.

![sağlama durumu](media/tutorial-deploy-hsm-powershell/progress-status.png)

Başarıyla tamamlandığında, "provisioningState tarafından" gösterilen: "Başarılı", mevcut sanal makinenizde oturum açın ve HSM cihazını kullanılabilirliğini sağlamak için SSH kullanın.

## <a name="verifying-the-deployment"></a>Dağıtımı doğrulama

Cihazları sağlanmış olduğundan ve cihaz öznitelikleri doğrulamak için aşağıdaki komut kümesini çalıştırın. Kaynak grubu uygun şekilde ayarlanır ve tam olarak parametre dosyasında olduğundan, kaynak adı olduğundan emin olun.

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG"
$resourceName = "HSM1"  
Get-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName

```

![sağlama durumu](media/tutorial-deploy-hsm-powershell/progress-status2.png)

Ayrıca artık kullanarak kaynakları görmeye olacak [Azure kaynak Gezgini](https://resources.azure.com/).   Bir kez Gezgini'nde soldaki "abonelikler" genişletin, belirli ayrılmış HSM aboneliğinizin genişletin, "kaynak grupları" genişletin, kullandığınız kaynak grubunu genişletin ve son olarak "Kaynaklar" maddesi'ı seçin.

## <a name="testing-the-deployment"></a>Test etme ve dağıtım

Test etme ve dağıtım HSM(s) erişebilen bir sanal makineye bağlanmak ve ardından HSM cihaza doğrudan bağlanma bir durumdur. Bu Eylemler, HSM ulaşılabildiğinden onaylar.
Ssh aracı kullanarak sanal makineye bağlanmak için kullanılır. Komut aşağıdakine benzer olacaktır, ancak bir yönetici adı ve dns adı ile parametresinde belirtilen.

`ssh adminuser@hsmlinuxvm.westus.cloudapp.azure.com`

Kullanılacak parametre dosyasından bir paroladır.
Bir kez oturum oturum kaynak için Portalı'nda bulunan özel IP adresini kullanarak HSM Linux VM'de \<önek > hsm_vnic.

```powershell

(Get-AzResource -ResourceGroupName myRG -Name HSMdeploy -ExpandProperties).Properties.networkProfile.networkInterfaces.privateIpAddress

```
IP adresi varsa, aşağıdaki komutu çalıştırın:

`ssh tenantadmin@<ip address of HSM>`

Başarılı olursa için bir parola istenir. Varsayılan parola paroladır. HSM parolanızı değiştirmenizi isteriz kadar güçlü bir parola ayarlayın ve kuruluşunuz tercih parola depolayıp kaybını önlemek için hangi mekanizmasını kullanın.  

>[!IMPORTANT]
>Bu parola kaybederseniz, HSM sıfırlanması gerekir ve anahtarlarınızı kaybetmeden anlamına gelir.

HSM cihazda ssh kullanarak bağlandıktan sonra HSM çalıştığından emin olmak için aşağıdaki komutu çalıştırın.

`hsm show`

Çıktı, görüntüyü aşağıda gösterildiği gibi görünmelidir:

![sağlama durumu](media/tutorial-deploy-hsm-powershell/output.png)

Bu noktada, tüm kaynaklar için yüksek oranda kullanılabilir bir, iki HSM dağıtım ve doğrulanmış erişim ve işlemsel durumu ayırmış olmanız. Daha fazla yapılandırma veya test daha fazla iş HSM cihazını içerir. Bunun için HSM başlatıp bölümler oluşturmak için 7 Gemalto Luna ağ HSM 7 Yönetim Kılavuzu bölümdeki yönergeleri izlemelidir. Gemalto müşteri desteği Portalı'nda kayıtlı olan ve bir müşteri kimliği sahip tüm belgeler ve yazılım doğrudan Gemalto indirme için kullanılabilir İstemci tüm gerekli bileşenleri almak için yazılım sürüm 7.2 indirin.

## <a name="delete-or-clean-up-resources"></a>Silme veya kaynakları temizleme

Yalnızca HSM cihazla tamamlandıysa, sonra bir kaynak olarak silinir ve olması ücretsiz havuza geri döner. Bunu yaparken belirgin cihazda olan hassas müşteri verilerinin konusudur. Önemli müşteri kaldırmak için veri cihazı fabrika ayarlarına sıfırlama Gemalto istemcisini kullanarak olmalıdır. SafeNet ağ Luna 7 cihazın Gemalto yönetici kılavuzuna başvurun ve aşağıdaki komutları sırayla göz önünde bulundurun.

1. `hsm factoryReset -f`
2. `sysconf config factoryReset -f -service all`
3. `network interface delete -device eth0`
4. `network interface delete -device eth1`
5. `network interface delete -device eth2`
6. `network interface delete -device eth3`
7. `my file clear -f`
8. `my public-key clear -f`
9. `syslog rotate`


> [!NOTE]
> herhangi bir Gemalto cihaz yapılandırma ile ilgili sorun varsa başvurmalısınız [Gemalto müşteri desteği](https://safenet.gemalto.com/technical-support/).

Bu kaynak grubundaki kaynaklar ile tamamladınız, ardından şu komutla tüm kaldırabilirsiniz:

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG" 
$resourceName = "HSMdeploy"  
Remove-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName 

```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticide adımları tamamladıktan sonra özel HSM sağlandığı ve kullanılabilir sanal ağınızda kaynaklardır. Artık bu dağıtımın gerektiği gibi daha fazla kaynak tarafından tercih edilen dağıtım Mimarinizi ile tamamlayıcı konumuna demektir. Dağıtımınızı planlama yardımcı olacak daha fazla bilgi için kavramları belgelere bakın. Bir tasarım ile raf düzeyinde kullanılabilirlik ele alan bir birincil bölgede iki Hsm'leri ve iki ikincil bir bölgeye bölgesel kullanılabilirlik adresleme Hsm'lerde önerilir. Bu öğreticide kullanılan şablon dosyasının kolayca iki HSM dağıtımlar için temel olarak kullanılabilir, ancak gereksinimlerinizi karşılayacak şekilde değiştirilmiş parametrelerini sahip olması gerekir.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [İzleme](monitoring.md)
* [Desteklenebilirliği](supportability.md)
