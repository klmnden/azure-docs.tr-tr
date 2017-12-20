---
title: "Bir Azure uygulama ağ geçidi - Azure CLI 1.0 oluşturma | Microsoft Docs"
description: "Resource Manager'da Azure CLI 1.0 kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: fe50fb3a7434702101dc5ae7a9dd176a33423119
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Azure CLI kullanarak bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Klasik PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)
> 
> 

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Uygulama ağ geçidi şu uygulama teslim özelliklerine sahiptir: HTTP Yük Dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi ve Güvenli Yuva Katmanı (SSL) yük boşaltma, özel sistem durumu araştırmalarının ve desteklemek için çok siteli.

## <a name="prerequisite-install-the-azure-cli"></a>Önkoşul: Azure CLI'yı yükleme

Bu makaledeki adımları gerçekleştirmek için gerek [Mac, Linux ve Windows (Azure CLI) için Azure komut satırı arabirimini yükleyin](../xplat-cli-install.md) ve gerek [Azure oturumunu](/cli/azure/authenticate-azure-cli). 

> [!NOTE]
> Bir Azure hesabınız yoksa, biri gerekir. [Buradaki ücretsiz deneme sürümüyle](../active-directory/sign-up-organization.md) kaydolun.

## <a name="scenario"></a>Senaryo

Bu senaryoda, Azure portalını kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin.

Bu senaryo aşağıdakileri yapar:

* Orta uygulama ağ geçidi ile iki örnek oluşturun.
* Bir ayrılmış 10.0.0.0/16 CIDR bloğu ile ContosoVNET adlı bir sanal ağ oluşturun.
* CIDR bloğu 10.0.0.0/28 kullanan subnet01 adlı bir alt ağ oluşturun.

> [!NOTE]
> Özel durumu da dahil olmak üzere uygulama ağ geçidinin ek yapılandırma araştırmaları, arka uç havuzu adresleri ve ek kurallar uygulama ağ geçidi yapılandırıldıktan sonra ve ilk dağıtım sırasında değil yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure uygulama ağ geçidi, kendi alt gerektirir. Bir sanal ağ oluştururken, birden çok alt ağa sahip için yeterli adres alanı bıraktığınızdan emin olun. Bir alt ağ için bir uygulama ağ geçidi dağıttığınızda, yalnızca ek uygulama ağ geçidi alt ağına eklemek kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Açık **Microsoft Azure komut istemi**ve oturum açın. 

```azurecli-interactive
azure login
```

Önceki örnekte yazın sonra bir kod sağlanır. Oturum açma işlemine devam etmek için bir tarayıcıda https://aka.ms/devicelogin gidin.

![cmd gösteren cihaz oturum açma][1]

Tarayıcıda, aldığınız kodu girin. Bir oturum açma sayfasına yönlendirilir.

![Tarayıcı kodu girin][2]

Kod girildikten sonra oturum açtığınızdan, üzerinde senaryoyla devam etmek için tarayıcıyı kapatın.

![başarıyla oturum açıldı][3]

## <a name="switch-to-resource-manager-mode"></a>Resource Manager moduna geçin

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Uygulama ağ geçidi oluşturmadan önce bir kaynak grubu, uygulama ağ geçidi içerecek şekilde oluşturulur. Aşağıda, komut gösterilmektedir.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Kaynak grubu oluşturulduktan sonra bir sanal ağ uygulama ağ geçidi için oluşturulur.  Aşağıdaki örnekte, önceki senaryo notları tanımlanan adres alanı 10.0.0.0/16 oluştu.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Alt ağ oluşturma

Sanal ağ oluşturulduktan sonra bir alt ağ için uygulama ağ geçidi eklenir.  Uygulama ağ geçidiyle aynı sanal ağ içinde barındırılan bir web uygulaması ile uygulama ağ geçidi kullanmayı planlıyorsanız, başka bir alt ağ için yeterli alan olduğundan emin olun.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Sanal ağ ve alt ağ oluşturulduktan sonra uygulama ağ geçidi için ön koşullar tamamlandı. Ayrıca daha önce dışarı aktarılan .pfx sertifika ve sertifika için parola için aşağıdaki adım gereklidir: arka uç için kullanılan IP adresleri arka uç sunucusu için IP adresleridir. Bu değerler, sanal ağ içinde kullanılabilecek özel IP, genel IP'ler veya arka uç sunucuları için tam etki alanı adları olabilir.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Aşağıdaki komutu çalıştırın oluşturma sırasında sağlanan parametrelerin listesi için: **azure ağ uygulama ağ geçidi oluşturma--Yardım**.

Bu örnek, dinleyicisi, arka uç havuzu, arka uç http ayarları ve kuralları için varsayılan ayarlarla temel uygulama ağ geçidi oluşturur. Bu ayarları sağlama başarılı olduktan sonra dağıtımınızı uyacak şekilde değiştirebilirsiniz.
Web uygulamanızın arka uç havuzu oluşturulduktan sonra önceki adımlarda tanımlanan zaten varsa, Yük Dengeleme başlar.

## <a name="next-steps"></a>Sonraki adımlar

Özel sistem durumu araştırmalarının ziyaret ederek oluşturmayı öğrenin [bir özel durum araştırması oluştur](application-gateway-create-probe-portal.md)

SSL boşaltma yapılandırmak ve web sunucularınızın kapalı maliyetli SSL şifre çözme ziyaret ederek ele öğrenin [SSL boşaltma yapılandırın](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
