---
title: Azure'da güvenlik duvarı oluşturma Freebsd'nin paket filtresini kullanma | Microsoft Docs
description: Freebsd'nin kullanan bir NAT güvenlik duvarı dağıtma hakkında bilgi edinin azure'da PF.
services: virtual-machines-linux
documentationcenter: ''
author: KylieLiang
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 03ef1ad3f81cfe7b11f74ace9ff2992535d5aad6
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667636"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Azure'da güvenli bir güvenlik duvarı oluşturma Freebsd'nin paket filtresini kullanma
Bu makale, yaygın web sunucusu senaryosu için Azure Resource Manager şablonu aracılığıyla Freebsd'nin Packer filtresini kullanarak NAT güvenlik duvarı dağıtma tanıtır.

## <a name="what-is-pf"></a>PF nedir?
PF (paket filtresi, pf de yazılır) BSD lisanslı durum bilgisi olan paket filtresi, merkezi özellikleri için bir yazılım parçasıdır ' dir. PF beri hızlı bir şekilde gelişmiştir ve artık kullanılabilir başka güvenlik duvarları üzerinden çeşitli avantajları vardır. Ağ adresi çevirisi (NAT) günden beri sonra Paket Zamanlayıcısı'nı PF içinde olan ve etkin kuyruk yönetimi tümleşik PF, ALTQ tümleştirme PF'ın yapılandırma yoluyla yapılandırılabilir yapılarak. Yük devretme ve yedeklilik, oturumu kimlik doğrulama ve zor FTP protokolünü saldırısından kolaylaştırmak için ftp-proxy authpf pfsync ve CARP gibi özellikleri de PF. genişletilmiş Kısacası, PF, güçlü ve zengin özelliklere sahip bir güvenlik duvarı olup. 

## <a name="get-started"></a>başlarken
Web sunucularınız için bulutta güvenli bir güvenlik duvarını ayarlamak istiyorsanız, ardından başlayalım seçerseniz. Ağ topolojinizi ayarlamak için bu Azure Resource Manager şablonunda kullanılan betikler de uygulayabilirsiniz.
Azure Resource Manager şablonu FreeBSD sanal makine, NAT /redirection PF ve FreeBSD sanal makineleri iki yüklenmiş ve yapılandırılmış Ngınx web sunucusu ile kullanarak gerçekleştirir ayarlayın. İki web sunucuları çıkış trafiği için NAT biçimlendirebilir NAT/yeniden yönlendirme sanal makine, HTTP istekleri alacak ve iki web sunucularından birinde ettirirsiniz yönlendirin. Sanal ağ özel yönlendirilemeyen IP adresi alanı 10.0.0.2/24 kullanır ve şablon parametrelerini değiştirebilirsiniz. Azure Resource Manager şablonu bir yol tablosu hedef IP adresini temel alan Azure varsayılan yolları geçersiz kılmak için kullanılan bir tekil yollar koleksiyonudur tam bir VNet için aynı zamanda tanımlar. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Azure CLI ile dağıtma
En son ihtiyacınız [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index). [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu adı oluşturur `myResourceGroup` içinde `West US` konumu.

```azurecli
az group create --name myResourceGroup --location westus
```

Ardından, şablonu dağıtmak [pf freebsd Kurulum](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) ile [az grubu dağıtım oluşturma](/cli/azure/group/deployment). İndirme [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) aynı yol altındaki ve kendi kaynak değerlerini tanımlamak, gibi `adminPassword`, `networkPrefix`, ve `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Yaklaşık beş dakika sonra bilgilerini alırsınız `"provisioningState": "Succeeded"`. Ardından ssh ön uç VM (NAT) veya erişim genel IP adresi veya FQDN ön uç VM (NAT) kullanarak bir tarayıcıda Ngınx web sunucusunu kullanabilirsiniz. Aşağıdaki örnek, FQDN ve ön uç VM (NAT) atanmış genel IP adresi listeler `myResourceGroup` kaynak grubu. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Sonraki adımlar
Azure'da kendi NAT ayarlamak istiyor musunuz? Ücretsiz ancak güçlü açık kaynak? Ardından PF iyi bir seçimdir. Şablon kullanarak [pf freebsd Kurulum](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), beş dakika yeterlidir FreeBSD kullanarak hepsini bir kez deneme yük dengelemeyi ile bir NAT güvenlik duvarı oluşturmak için azure'da PF yaygın web sunucusu senaryosu için kullanıcının. 

FreeBSD azure'da sunulması öğrenmek istiyorsanız, başvurmak [Azure üzerinde Freebsd'ye giriş](freebsd-intro-on-azure.md).

PF hakkında daha fazla bilgi edinmek istiyorsanız, başvurmak [FreeBSD el kitabı](https://www.freebsd.org/doc/handbook/firewalls-pf.html) veya [PF-Kullanıcı Kılavuzu](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
