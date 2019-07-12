---
title: Dağıtma ve Azure Azure CLI kullanarak güvenlik duvarı yapılandırma
description: Bu makalede, dağıtma ve Azure Azure CLI kullanarak güvenlik duvarı yapılandırma hakkında bilgi edinin.
services: firewall
author: vhorne
ms.service: firewall
ms.date: 7/10/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: 24954eecde58c978fa3e14bb3a2d411d708687a3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707153"
---
# <a name="deploy-and-configure-azure-firewall-using-azure-cli"></a>Dağıtma ve Azure Azure CLI kullanarak güvenlik duvarı yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin, web sitelerine erişimi sınırlamak isteyebilirsiniz. Veya giden IP adresleri ve erişilebilen bağlantı noktalarını sınırlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları. Ayrıca FQDN'nin [SQL örnekleri dahil](sql-fqdn-filtering.md).
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu makale için kolay dağıtım için üç alt ağ ile Basitleştirilmiş tek bir VNet oluşturun. Üretim dağıtımları için bir [merkez ve ışınsal modelini](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) önerilir. Güvenlik Duvarı'nı kendi Vnet'ine ' dir. Bir veya daha fazla alt ağ ile aynı bölgedeki eşlenmiş Vnet'ler içindeki iş yükü sunucularıdır.

* **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
* **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
* **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * www.google.com erişmesine izin vermek için bir uygulama kuralı yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

Tercih ederseniz, bu yordamı kullanarak tamamlayabilirsiniz [Azure portalında](tutorial-firewall-deploy-portal.md) veya [Azure PowerShell](deploy-ps.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-cli"></a>Azure CLI

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.4 sürüm çalıştırın veya üzeri. Sürümü bulmak için çalıştırın **az--version**. Yükleme veya yükseltme hakkında daha fazla bilgi için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).

Azure güvenlik duvarı uzantıyı yükleyin:

```azurecli-interactive
az extension add -n azure-firewall
```


## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu dağıtımı için tüm kaynakları içerir.

```azurecli-interactive
az group create --name Test-FW-RG --location eastus
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağı, üç alt sahiptir.

> [!NOTE]
> En düşük AzureFirewallSubnet alt /26 boyutudur.

```azurecli-interactive
az network vnet create \
  --name Test-FW-VN \
  --resource-group Test-FW-RG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name AzureFirewallSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name Workload-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.2.0/24
az network vnet subnet create \
  --name Jump-SN \
  --resource-group Test-FW-RG \
  --vnet-name Test-FW-VN   \
  --address-prefix 10.0.3.0/24
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.
İstendiğinde, sanal makine için bir parola yazın.

Srv hızlı sanal makine oluşturun.

```azurecli-interactive
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Jump \
    --location eastus \
    --image win2016datacenter \
    --vnet-name Test-FW-VN \
    --subnet Jump-SN \
    --admin-username azureadmin
az vm open-port --port 3389 --resource-group Test-FW-RG --name Srv-Jump
```



Srv-belirli bir DNS sunucusu IP adreslerinin çalışmak için bir NIC ve genel IP adresi ile test etmek için oluşturun.

```azurecli-interactive
az network nic create \
    -g Test-FW-RG \
    -n Srv-Work-NIC \
   --vnet-name Test-FW-VN \
   --subnet Workload-SN \
   --public-ip-address "" \
   --dns-servers 209.244.0.3 209.244.0.4
```

Şimdi iş yükü sanal makine oluşturun.
İstendiğinde, sanal makine için bir parola yazın.

```azurecli-interactive
az vm create \
    --resource-group Test-FW-RG \
    --name Srv-Work \
    --location eastus \
    --image win2016datacenter \
    --nics Srv-Work-NIC \
    --admin-username azureadmin
```

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Şimdi sanal ağa Güvenlik Duvarı'nı dağıtın.

```azurecli-interactive
az network firewall create \
    --name Test-FW01 \
    --resource-group Test-FW-RG \
    --location eastus
az network public-ip create \
    --name fw-pip \
    --resource-group Test-FW-RG \
    --location eastus \
    --allocation-method static \
    --sku standard
az network firewall ip-config create \
    --firewall-name Test-FW01 \
    --name FW-config \
    --public-ip-address fw-pip \
    --resource-group Test-FW-RG \
    --vnet-name Test-FW-VN
az network firewall update \
    --name Test-FW01 \
    --resource-group Test-FW-RG 
az network public-ip show \
    --name fw-pip \
    --resource-group Test-FW-RG
fwprivaddr="$(az network firewall ip-config list -g Test-FW-RG -f Test-FW01 --query "[?name=='FW-config'].privateIpAddress" --output tsv)"
```

Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

BGP yol yaymayı devre dışı bir tablo oluşturma

```azurecli-interactive
az network route-table create \
    --name Firewall-rt-table \
    --resource-group Test-FW-RG \
    --location eastus \
    --disable-bgp-route-propagation true
```

Bir yol oluşturun.

```azurecli-interactive
az network route-table route create \
  --resource-group Test-FW-RG \
  --name DG-Route \
  --route-table-name Firewall-rt-table \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $fwprivaddr
```

Alt ağa yönlendirme tablosunu ilişkilendirme

```azurecli-interactive
az network vnet subnet update \
    -n Workload-SN \
    -g Test-FW-RG \
    --vnet-name Test-FW-VN \
    --address-prefixes 10.0.2.0/24 \
    --route-table Firewall-rt-table
```

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Uygulama kuralı www.google.com giden erişim sağlar.

```azurecli-interactive
az network firewall application-rule create \
   --collection-name App-Coll01 \
   --firewall-name Test-FW01 \
   --name Allow-Google \
   --protocols Http=80 Https=443 \
   --resource-group Test-FW-RG \
   --target-fqdns www.google.com \
   --source-addresses 10.0.2.0/24 \
   --priority 200 \
   --action Allow
```

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Ağ kuralı bağlantı noktası 53'ün (DNS) iki IP adreslerine giden erişime izin verir.

```azurecli-interactive
az network firewall network-rule create \
   --collection-name Net-Coll01 \
   --destination-addresses 209.244.0.3 209.244.0.4 \
   --destination-ports 53 \
   --firewall-name Test-FW01 \
   --name Allow-DNS \
   --protocols UDP \
   --resource-group Test-FW-RG \
   --priority 200 \
   --source-addresses 10.0.2.0/24 \
   --action Allow
```

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Şimdi beklendiği gibi çalıştığını doğrulamak için Güvenlik Duvarı'nı test edin.

1. Özel IP adresini Not **Srv iş** sanal makine:

   ```azurecli-interactive
   az vm list-ip-addresses \
   -g Test-FW-RG \
   -n Srv-Work
   ```

1. Bağlanmak için Uzak Masaüstü **Srv atlama** sanal makine ve oturum açın. Burada, bir Uzak Masaüstü Bağlantısı açın **Srv iş** özel IP adresi ve oturum açın.

3. Üzerinde **SRV iş**, bir PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:

   ```
   nslookup www.google.com
   nslookup www.microsoft.com
   ```

   Her iki komut yanıtları, DNS sorguları güvenlik duvarı üzerinden alıyorsanız gösteren, döndürmelidir.

1. Aşağıdaki komutları çalıştırın:

   ```
   Invoke-WebRequest -Uri https://www.google.com
   Invoke-WebRequest -Uri https://www.google.com

   Invoke-WebRequest -Uri https://www.microsoft.com
   Invoke-WebRequest -Uri https://www.microsoft.com
   ```

   www.google.com isteklerinin başarılı olması gerekir ve www.microsoft.com istekleri başarısız olması. Bu, güvenlik duvarı kurallarınız beklendiği gibi çalışıp çalışmadığını gösterir.

Şimdi güvenlik duvarı kuralları çalıştığını doğruladığınıza göre:

* Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.
* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik Duvarı kaynaklarınızı korumak için sonraki öğreticiye veya artık gerekli değilse silin **Test FW RG** güvenlik duvarı ile ilgili tüm kaynakları silmek için kaynak grubu:

```azurecli-interactive
az group delete \
  -n Test-FW-RG
```

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
