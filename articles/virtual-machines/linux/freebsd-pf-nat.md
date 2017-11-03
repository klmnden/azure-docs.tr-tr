---
title: "Azure üzerinde bir güvenlik duvarı oluşturma FreeBSD'ın paket filtresini kullanma | Microsoft Docs"
description: "FreeBSD'ın kullanan bir NAT güvenlik duvarı dağıtmayı öğrenin azure'da PF."
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: cd777291a1321eabf4efe0d7b9b101f932d9398b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Güvenli güvenlik duvarı oluşturma FreeBSD'ın paket filtresini kullanma
Bu makalede Azure Resource Manager şablonu aracılığıyla FreeBSD'ın Packer filtresi için ortak web sunucu senaryosu kullanan bir NAT güvenlik duvarı dağıtma tanıtılır.

## <a name="what-is-pf"></a>PF nedir?
PF (paket filtresi, ayrıca pf yazılan) bir BSD lisanslı durum bilgisi olan paket filtre saldırısından için yazılım merkezi bir parçası olarak bilinir. PF beri hızlı bir şekilde gelişmiştir ve artık kullanılabilir diğer güvenlik duvarları üzerinden çeşitli avantajları vardır. Ağ adresi çevirisi (NAT) günden itibaren ardından Paket Zamanlayıcısı'nı PF içinde olan ve etkin kuyruk yönetimi tümleşik PF yapılarak ALTQ tümleştirme, PF'ın yapılandırması aracılığıyla yapılandırılabilir. Yük devretme ve artıklık, oturum kimlik doğrulaması ve zor FTP protokolünü saldırısından kolaylaştırmak için ftp-proxy authpf pfsync ve CARP gibi özellikleri de PF. genişletilmiş Kısacası, PF güçlü ve zengin bir Güvenlik Duvarı ' dir. 

## <a name="get-started"></a>başlarken
Web sunucuları için bulutta güvenli bir güvenlik duvarını ayarı ilgilendiğiniz sonra başlayalım durumunda. Bu Azure Resource Manager şablonunu ağ topolojinizi ayarlamak için kullanılan komut uygulayabilir.
Azure Resource Manager şablonu PF ve iki FreeBSD sanal makineye yüklenmiş ve yapılandırılmış Nginx web sunucusu ile kullanarak NAT /redirection gerçekleştiren bir FreeBSD sanal makine ayarlayın. NAT iki web sunucuları çıkış trafiği için gerçekleştirmeden, yanı sıra NAT/yeniden yönlendirme sanal makine HTTP isteklerini karşılar ve bunları iki web sunucularına hepsini şekilde yönlendirin. VNet özel yönlendirilemeyen IP adresi alanı 10.0.0.2/24 kullanır ve şablon parametrelerini değiştirebilirsiniz. Azure Resource Manager şablonu bir yol tablosu hedef IP adresine göre Azure varsayılan yolların geçersiz kılmak için kullanılan bir tekil yollar koleksiyonudur tüm VNet için aynı zamanda tanımlar. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Azure CLI aracılığıyla dağıtma
En son gereksinim [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/#login). [az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu adı oluşturur `myResourceGroup` içinde `West US` konumu.

```azurecli
az group create --name myResourceGroup --location westus
```

Ardından, şablonu dağıtmak [pf freebsd Kurulum](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) ile [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#create). Karşıdan [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) aynı yolunda ve kendi kaynak değerlerini aşağıdaki gibi tanımlayın `adminPassword`, `networkPrefix`, ve `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Yaklaşık beş dakika sonra bilgilerinin alırsınız `"provisioningState": "Succeeded"`. Ardından, ssh ön uç VM (NAT) ya da erişim Nginx web sunucusunun ortak IP adresi veya ön uç VM (NAT) FQDN'sini kullanarak bir tarayıcıda gerçekleştirebilirsiniz. Aşağıdaki örnek, FQDN ve VM (NAT) ön uç için atanan ortak IP adresi listeler `myResourceGroup` kaynak grubu. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Sonraki adımlar
Azure'da kendi NAT ayarlamak istiyor musunuz? Kaynak, ücretsiz ancak güçlü açılsın mı? Ardından PF iyi bir seçimdir. Şablon kullanarak [pf freebsd Kurulum](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), beş dakika yeterlidir hepsini bir kez deneme yük dengeleme FreeBSD kullanarak bir NAT güvenlik duvarını ayarlama için azure'da PF ortak web sunucusu senaryo için kullanıcının. 

Azure'da FreeBSD sunulması öğrenmek istiyorsanız, başvurmak [FreeBSD Azure ile ilgili giriş](freebsd-intro-on-azure.md).

PF hakkında daha fazla bilgi edinmek istiyorsanız, başvurmak [FreeBSD el kitabı](https://www.freebsd.org/doc/handbook/firewalls-pf.html) veya [PF-Kullanıcı Kılavuzu](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
