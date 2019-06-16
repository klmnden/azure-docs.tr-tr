---
title: Azure Klasik CLI - Azure Application Gateway oluşturma
description: Resource Manager'da Azure Klasik CLI kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 4/15/2019
ms.author: victorh
ms.openlocfilehash: 7107f45253c4f13b3378489726bf5034e104fa30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095991"
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Azure CLI kullanarak bir uygulama ağ geçidi oluşturma

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Application gateway şu uygulama teslim özelliklerine sahiptir: HTTP Yük Dengeleme, tanımlama bilgilerine dayalı oturum benzeşimi ve Güvenli Yuva Katmanı (SSL) boşaltma, özel sistem durumu araştırmaları ve çoklu site desteği.

## <a name="prerequisite-install-the-azure-cli"></a>Önkoşul: Azure CLI'yı yükleme

Bu makaledeki adımları gerçekleştirmek için yapmanız [Azure CLI'yı yükleme](../xplat-cli-install.md) ve gerekiyorsa [Azure'da oturum](/cli/azure/authenticate-azure-cli). 

> [!NOTE]
> Bir Azure hesabınız yoksa, bir gerekir. [Buradaki ücretsiz deneme sürümüyle](../active-directory/fundamentals/sign-up-organization.md) kaydolun.

## <a name="scenario"></a>Senaryo

Bu senaryoda, Azure portalını kullanarak bir uygulama ağ geçidi oluşturma konusunda bilgi edinin.

Bu senaryo olur:

* Orta uygulama ağ geçidi ile iki örnek oluşturun.
* Ayrılmış 10.0.0.0/16 CIDR bloğu ile ContosoVNET adlı bir sanal ağ oluşturun.
* CIDR bloğu 10.0.0.0/28 kullanan subnet01 adlı bir alt ağ oluşturun.

> [!NOTE]
> Ek yapılandırma özel durumu dahil olmak üzere uygulama ağ geçidinin araştırmaları, arka uç havuzu adresleri ve ek kurallar uygulama ağ geçidi yapılandırıldıktan sonra ve ilk dağıtım sırasında değil yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure Application Gateway, kendi alt ağına gerektirir. Bir sanal ağ oluştururken, birden çok alt ağ için yeterli adres alanı bırakın emin olun. Bir alt ağ için bir uygulama ağ geçidi dağıttığınızda, yalnızca ek uygulama ağ geçidi alt ağa eklenmesi olanağına sahip olursunuz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Açık **Microsoft Azure komut istemi**ve oturum açın.

```azurecli-interactive
az login
```

Yukarıdaki örnekte yazın sonra bir kod sağlanır. Gidin https://aka.ms/devicelogin bir tarayıcıda oturum açma işlemine devam etmek için.

![cmd gösteren cihaz oturum açma][1]

Tarayıcıda aldığınız kodu girin. Bir oturum açma sayfasına yönlendirilirsiniz.

![kodu girmek için tarayıcı][2]

Kod girildikten sonra oturumunuz, üzerinde bir senaryoyla devam etmek için tarayıcınızı kapatın.

![başarıyla oturum açıldı][3]

## <a name="switch-to-resource-manager-mode"></a>Resource Manager moduna geçin

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Uygulama ağ geçidi oluşturmadan önce uygulama ağ geçidi içerecek bir kaynak grubu oluşturulur. Aşağıda, komut gösterilmektedir.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Kaynak grubu oluşturulduktan sonra uygulama ağ geçidi için bir sanal ağ oluşturulur.  Aşağıdaki örnekte, yukarıdaki senaryo notları tanımlanan 10.0.0.0/16 adres alanı oluştu.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Alt ağ oluşturma

Sanal ağ oluşturulduktan sonra uygulama ağ geçidi için bir alt ağ eklenir.  Uygulama ağ geçidiyle aynı sanal ağda barındırılan bir web uygulaması ile uygulama ağ geçidini kullanmayı planlıyorsanız, başka bir alt ağ için yeterli alan bırakın emin olun.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Sanal ağ ve alt ağ oluşturulduktan sonra uygulama ağ geçidi için ön koşullar tamamlandı. Ayrıca daha önce dışarı aktarılan .pfx sertifika ve sertifika için parola için aşağıdaki adım gereklidir: Arka uç için kullanılan IP adresleri, arka uç sunucusu için IP adresleridir. Bu değerler, sanal ağ özel IP'ler, genel IP'ler veya, arka uç sunucuları için tam etki alanı adları olabilir.

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
> Aşağıdaki komutu çalıştırarak oluşturma sırasında sağlanan parametrelerin listesi için: **azure ağı application-gateway create--yardımcı**.

Bu örnekte, dinleyici, arka uç havuzu, arka uç http ayarları ve kuralları için varsayılan ayarlarla temel uygulama ağ geçidi oluşturur. Bu ayarlar, sağlama başarılı olduktan sonra dağıtımınız uyacak şekilde değiştirebilirsiniz.
Web uygulamanız oluşturulduktan sonra önceki adımlarda, arka uç havuzu ile tanımlanan zaten varsa, Yük Dengeleme başlar.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek özel sistem durumu araştırmaları oluşturma konusunda bilgi edinin [özel durum araştırması oluşturun](application-gateway-create-probe-portal.md)

SSL boşaltma yapılandırmak ve ziyaret ederek maliyetli SSL şifre çözme web sunucularınızın kapalı olması öğrenin [SSL yük boşaltma yapılandırın](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
