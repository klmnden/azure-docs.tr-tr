---
title: Öğretici - Azure ayrılmış HSM Azure CLI kullanarak mevcut bir sanal ağa dağıtma | Microsoft Docs
description: CLI kullanarak mevcut bir sanal ağa ayrılmış bir HSM dağıtmayı gösteren öğretici.
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
ms.openlocfilehash: 84beac4eca44a274eecc032e4816e3ff57aeafe2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60688457"
---
# <a name="tutorial-deploying-hsms-into-an-existing-virtual-network-using-cli"></a>Öğretici: CLI kullanarak HSM'leri mevcut sanal ağa dağıtma

Azure ayrılmış HSM fiziksel bir cihaz için tam yönetim denetimi ve tam yönetim sorumluluk ile tek bir müşterinin kullanım sağlar. Fiziksel cihazlar kullanımını kapasite etkili bir şekilde yönetildiğinden emin olmak için cihaz ayırma denetlemek Microsoft gereksinimini oluşturur. Sonuç olarak, bir Azure aboneliğinde, ayrılmış HSM hizmetini normalde kaynak sağlama için görünür olmaz. Ayrılmış HSM hizmetine erişim gerektiren herhangi bir Azure müşterisi ilk başvurmalısınız, Microsoft hesap yöneticinize istek kayıt için ayrılmış HSM hizmeti. Yalnızca bu işlemi başarıyla tamamlandıktan sonra sağlama mümkün olacaktır. 

Bu öğreticide, tipik bir sağlama işlemi gösterilir. Burada:

- Bir müşteri sanal ağ zaten var
- Bir sanal makineye sahip oldukları
- Bunlar, mevcut ortamına HSM kaynakları eklemeniz gerekir.

Tipik, yüksek kullanılabilirlik, çok bölgeli dağıtım mimarisi şu şekilde görünebilir:

![Çok bölgeli dağıtım](media/tutorial-deploy-hsm-cli/high-availability-architecture.png)

Bu öğreticide HSM'ler çifti üzerinde odaklanır ve gerekli ExpressRoute (bkz. Yukarıdaki alt ağ 1) bir sanal mevcut tümleştirilmektedir ağ geçidi ağ (VNET 1 yukarıdaki bakın).  Diğer tüm kaynaklar standart Azure kaynaklarıdır. Aynı tümleştirme işlemi HSM'ler için yukarıdaki VNET 3 4 ağda kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Azure ayrılmış HSM, Azure portalında şu anda kullanılabilir değil. Tüm etkileşim hizmeti ile komut satırı veya kullanarak PowerShell olacaktır. Bu öğreticide, Azure Cloud shell'de komut satırı (CLI) arabirimini kullanır. Azure CLI'yı yeniyseniz izleyin Buradaki yönergeleri Başlarken: [Azure CLI 2.0 Başlarken](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).

Varsayımlar:

- Azure ayrılmış HSM kayıt işlemi tamamlandı
- Hizmeti kullanmak için onaylanmıştır. Aksi durumda, Ayrıntılar için Microsoft hesap temsilcinize başvurun.
- Bu kaynaklar için bir kaynak grubu oluşturulur ve Bu öğreticide dağıtılan yenilerini bu gruba katılacak.
- Gerekli sanal ağ, alt ağ ve sanal makineler Yukarıdaki diyagramda göre zaten oluşturduğunuz ve artık 2 HSM'ler bu dağıtımı ile tümleştirmek istediğiniz.

Azure portalında zaten gittiğiniz ve Cloud Shell açtığınız tüm talimatları varsayılır (seçin "\>\_" en üste portal'ın sağ).

## <a name="provisioning-a-dedicated-hsm"></a>Ayrılmış HSM sağlama

HSM'ler sağlama ve bunları ExpressRoute ağ geçidi üzerinden mevcut sanal ağına tümleştirmek için kullanılan ssh kullanarak doğrulanır. Bu doğrulama, erişilebilirlik ve başka yapılandırma etkinlikleri için HSM cihazını temel kullanılabilirliğini sağlamaya yardımcı olur. Aşağıdaki komutlar, HSM kaynaklar ve ilişkili ağ kaynakları oluşturmak için bir Azure Resource Manager şablonu kullanır.

### <a name="validating-feature-registration"></a>Özellik kaydı doğrulanıyor

Yukarıda belirtildiği gibi herhangi bir sağlama etkinliği ayrılmış HSM hizmetini, aboneliğiniz için kayıtlı olmasını gerektiriyor. Doğrulamak için Azure portal cloud shell'de aşağıdaki komutları çalıştırın.

```azurecli
az feature show \
   --namespace Microsoft.HardwareSecurityModules \
   --name AzureDedicatedHSM
```

Aşağıdaki komut, HSM adanmış hizmet için gereken ağ özellikleri doğrular.

```azurecli
az feature show \
   --namespace Microsoft.Network \
   --name AllowBaremetalServers
```

Her iki komutu (aşağıda gösterildiği gibi) "Kaydedildi" durumuna döndürmeniz gerekir. "Bu hizmet için kayıt olmanız gereklidir kayıtlı" komutları döndürmeyin, Microsoft hesap temsilcinize başvurun.

![Abonelik durumu](media/tutorial-deploy-hsm-cli/subscription-status.png)

### <a name="creating-hsm-resources"></a>HSM kaynakları oluşturma

HSM, bir müşterilerin sanal ağa bir sanal ağ ve alt ağ gerekli olacak şekilde sağlanır. Bir ExpressRoute ağ geçidi için sanal ağ ile fiziksel cihazı arasındaki iletişimi etkinleştirmek HSM bağımlılıktır ve son olarak bir sanal makine Gemalto istemci yazılımını kullanarak HSM cihazınıza erişim hakkı gereklidir. Bu kaynaklar, kullanım kolaylığı için karşılık gelen parametre dosyası ile bir şablon dosyasına toplanana. Dosyaları olarak doğrudan Microsoft'a başvurarak kullanılabilir HSMrequest@Microsoft.com.

Dosyaları oluşturduktan sonra kaynaklar için tercih edilen adlarınızı eklemek için parametre dosyasını düzenlemeniz gerekir. "Value" satırıyla Düzenle: "".

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

İlişkili Azure Resource Manager şablon dosyası 6 kaynakları bu bilgileri ile oluşturacak:

- Belirtilen sanal ağda HSM'ler için bir alt ağ
- Sanal ağ geçidi için bir alt ağ
- HSM cihazlarına sanal AĞA bağlanan bir sanal ağ geçidi
- Ağ geçidi için genel bir IP adresi
- Bir HSM damgasında 1
- Bir HSM'de damga 2

Parametre değerlerini ayarladıktan sonra dosyaları kullanmak için Azure portalında cloud shell dosya paylaşımına yüklenmesi gerekir. Azure portalında "\>\_" cloud shell simgesi sağ üst ve bu komut ortam ekranın alt kısmına hale getirir. Bu seçenekleri BASH şunlardır ve PowerShell ve BASH seçin değilse zaten ayarlanmış.

Komut kabuğu araç çubuğundaki karşıya yükleme/indirme seçeneği vardır ve bu şablonu ve parametre dosyalarını karşıya dosya paylaşımınızı seçmeniz gerekir:

![Dosya Paylaşımı](media/tutorial-deploy-hsm-cli/file-share.png)

Dosyalar yüklendiğinde bu kaynakları oluşturmaya hazır olursunuz. Yeni HSM oluşturmadan önce emin olmanız gerekir bazı önkoşul kaynak yerinde kaynaklardır. İşlem, HSM'ler ve ağ geçidi alt ağı aralıklarına sahip bir sanal ağ olması gerekir. Aşağıdaki komutları ne tür bir sanal ağda oluşturacak bir örnek olarak görev yapar.

```azurecli
az network vnet create \
  --name myHSM-vnet \
  --resource-group myRG \
  --address-prefix 10.2.0.0/16
  --subnet-name compute
  --subnet-prefix 10.2.0.0/24
```

```azurecli
--vnet-name myHSM-vnet \
  --resource-group myRG \
  --name hsmsubnet \
  --address-prefixes 10.2.1.0/24 \
  --delegations Microsoft.HardwareSecurityModules/dedicatedHSMs
```

```azurecli
az network vnet subnet create \
  --vnet-name myHSM-vnet \
  --resource-group myRG \
  --name GatewaySubnet \
  --address-prefixes 10.2.255.0/26
```

>[!NOTE]
>Unutmayın sanal ağ için en önemli yapılandırma olan alt ağ HSM cihazını için temsilciler "Microsoft.HardwareSecurityModules/dedicatedHSMs" için ayarlanmış olması gerekir.  HSM sağlama ayarlanan bu seçenek olmadan çalışmaz.

Tüm önkoşulların yerine getirildiğinden sonra değerleri kendi benzersiz adlara sahip güncelleştirilen sağlayarak Azure Resource Manager şablonu kullanmak için aşağıdaki komutu çalıştırın (en az bir kaynak grubu adı):

```azurecli
az group deployment create \
   --resource-group myRG  \
   --template-file ./Deploy-2HSM-toVNET-Template.json \
   --parameters ./Deploy-2HSM-toVNET-Params.json \
   --name HSMdeploy \
   --verbose
```

Bu dağıtım yaklaşık 25 için HSM cihazlarına olan bu süreyi toplu ile tamamlanması 30 dakika sürer

![sağlama durumu](media/tutorial-deploy-hsm-cli/progress-status.png)

Dağıtım başarıyla "provisioningState" tamamlandığında: "Başarılı" görüntülenir. Mevcut sanal makinenize bağlanın ve HSM cihazını kullanılabilirliğini sağlamak için SSH kullanın.

## <a name="verifying-the-deployment"></a>Dağıtımı doğrulama

Cihazları sağlanmış olduğundan ve cihaz öznitelikleri doğrulamak için aşağıdaki komut kümesini çalıştırın. Kaynak grubu uygun şekilde ayarlanır ve tam olarak parametre dosyasında olduğundan, kaynak adı olduğundan emin olun.

```azurecli
subid=$(az account show --query id --output tsv)
az resource show \
   --ids /subscriptions/$subid/resourceGroups/myRG/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/HSM1
az resource show \
   --ids /subscriptions/$subid/resourceGroups/myRG/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/HSM2
```

![sağlama çıkışı](media/tutorial-deploy-hsm-cli/progress-status2.png)

Ayrıca artık kullanarak kaynakları görmeye olacak [Azure kaynak Gezgini](https://resources.azure.com/).   Bir kez Gezgini'nde soldaki "abonelikler" genişletin, belirli ayrılmış HSM aboneliğinizin genişletin, "kaynak grupları" genişletin, kullandığınız kaynak grubunu genişletin ve son olarak "Kaynaklar" maddesi'ı seçin.

## <a name="testing-the-deployment"></a>Test etme ve dağıtım

Test etme ve dağıtım HSM(s) erişebilen bir sanal makineye bağlanmak ve ardından HSM cihaza doğrudan bağlanma bir durumdur. Bu Eylemler, HSM ulaşılabildiğinden onaylar.
Ssh aracı kullanarak sanal makineye bağlanmak için kullanılır. Komut aşağıdakine benzer olacaktır, ancak bir yönetici adı ve dns adı ile parametresinde belirtilen.

`ssh adminuser@hsmlinuxvm.westus.cloudapp.azure.com`

Sanal makinenin IP adresini, yukarıdaki komutu bir DNS adı yerine de kullanılabilir. Komut başarılı olursa, bir parola sorar ve girmeniz gerekir. Sanal makineye oturum açıldığında, hsm'ye HSM ile ilişkili ağ arabirimi kaynağı için Portalı'nda bulunan özel IP adresini kullanarak oturum açabilir.

![bileşenleri listesi](media/tutorial-deploy-hsm-cli/resources.png)

>[!NOTE]
>"Türleri Show hidden" onay kutusunu fark olan seçili olacak HSM kaynakları görüntüler.

Yukarıdaki ekran görüntüsünde "HSM1_HSMnic" veya "HSM2_HSMnic" tıklayarak uygun özel IP adresini gösterir. Aksi takdirde `az resource show` yukarıda kullanılan komutu, doğru IP adresini tanımlamak için bir yoludur. 

Doğru IP adresi varsa, bu adresi değiştirerek aşağıdaki komutu çalıştırın:

`ssh tenantadmin@10.0.2.4`

Başarılı olursa için bir parola istenir. Varsayılan parola paroladır ve HSM, parolanızı değiştirmek için önce sorar kadar güçlü bir parola ayarlayın ve kuruluşunuz tercih parola depolayıp kaybını önlemek için hangi mekanizmasını kullanın.

>[!IMPORTANT]
>Bu parola kaybederseniz, HSM sıfırlanması gerekir ve anahtarlarınızı kaybetmeden anlamına gelir.

Ssh kullanarak HSM'ye bağlandığınızda, HSM çalıştığından emin olmak için aşağıdaki komutu çalıştırın.

`hsm show`

Çıktı aşağıdaki resimde gösterildiği gibi görünmelidir:

![bileşenleri listesi](media/tutorial-deploy-hsm-cli/hsm-show-output.png)

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

```azurecli
az group deployment delete \
   --resource-group myRG \
   --name HSMdeploy \
   --verbose

```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide adımları tamamladıktan sonra ayrılmış HSM kaynaklar sağlanır ve HSM ile iletişimi etkinleştirmek için gerekli HSM'ler ve daha fazla ağ bileşenleri ile bir sanal ağ sahipsiniz.  Artık bu dağıtımın gerektiği gibi daha fazla kaynak tarafından tercih edilen dağıtım Mimarinizi ile tamamlayıcı konumuna demektir. Dağıtımınızı planlama yardımcı olacak daha fazla bilgi için kavramları belgelere bakın.
Bir tasarım ile raf düzeyinde kullanılabilirlik ele alan bir birincil bölgede iki Hsm'leri ve iki ikincil bir bölgeye bölgesel kullanılabilirlik adresleme Hsm'lerde önerilir. Bu öğreticide kullanılan şablon dosyasının kolayca iki HSM dağıtımlar için temel olarak kullanılabilir, ancak gereksinimlerinizi karşılayacak şekilde değiştirilmiş parametrelerini sahip olması gerekir.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)
