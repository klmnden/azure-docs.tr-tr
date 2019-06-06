---
title: Hızlı Başlangıç - Azure PowerShell kullanarak uygulamaların yüksek kullanılabilirlik için bir Traffic Manager profili oluşturma
description: Bu hızlı başlangıç makalesi, yüksek oranda kullanılabilir web uygulaması oluşturmak için bir Traffic Manager profilinin nasıl oluşturulacağını açıklar.
services: traffic-manager
author: KumudD
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/04/2019
ms.author: kumud
ms.openlocfilehash: f6ed2e03352a335022d99cf703240552fa34e732
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729007"
---
# <a name="quickstart-create-a-traffic-manager-profile-for-a-highly-available-web-application-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir yüksek oranda kullanılabilir web uygulaması için bir Traffic Manager profili oluşturma

Bu hızlı başlangıçta, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Traffic Manager profilinin nasıl oluşturulacağını açıklar.

Bu hızlı başlangıçta, bir web uygulamasının iki örneği oluşturacaksınız. Bunların her biri farklı bir Azure bölgesinde çalışıyor. Temel bir Traffic Manager profilini oluşturacağınız [uç nokta önceliği](traffic-manager-routing-methods.md#priority). Profil, kullanıcı trafiğini web uygulaması çalıştıran bir birincil siteye yönlendirir. Traffic Manager, web uygulaması sürekli olarak izler. Birincil site kullanılamıyorsa, otomatik yük devretme için yedekleme sitesi sağlar.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma
Kullanarak bir kaynak grubu oluşturmanız [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).

```azurepowershell-interactive


# Variables
$Location1="WestUS"

# Create a Resource Group
New-AzResourceGroup -Name MyResourceGroup -Location $Location1
```

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Kullanarak bir Traffic Manager profili oluşturma [yeni AzTrafficManagerProfile](/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) uç nokta önceliği temelinde kullanıcı trafiği yönlendirir.

```azurepowershell-interactive

# Generates a random value
$Random=(New-Guid).ToString().Substring(0,8)
$mytrafficmanagerprofile="mytrafficmanagerprofile$Random"

New-AzTrafficManagerProfile `
-Name $mytrafficmanagerprofile `
-ResourceGroupName MyResourceGroup `
-TrafficRoutingMethod Priority `
-MonitorPath '/' `
-MonitorProtocol "HTTP" `
-RelativeDnsName $mytrafficmanagerprofile `
-Ttl 30 `
-MonitorPort 80
```

## <a name="create-web-apps"></a>Create Web Apps

Bu hızlı başlangıçta, iki farklı Azure bölgelerinde dağıtılan web uygulamasının iki örneği gerekir (*Batı ABD* ve *Doğu ABD*). Traffic Manager için her birincil ve yük devretme uç noktalar olarak hizmet verecektir.

### <a name="create-web-app-service-plans"></a>Web App Service planları oluşturma
Hizmet planları kullanarak Web uygulaması oluşturma [yeni AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) için iki örnek web uygulamasının iki farklı Azure bölgelerinde dağıtacaksınız.

```azurepowershell-interactive

# Variables
$App1Name="AppServiceTM1$Random"
$App2Name="AppServiceTM2$Random"
$Location1="WestUS"
$Location2="EastUS"

# Create an App service plan
New-AzAppservicePlan -Name "$App1Name-Plan" -ResourceGroupName MyResourceGroup -Location $Location1 -Tier Standard
New-AzAppservicePlan -Name "$App2Name-Plan" -ResourceGroupName MyResourceGroup -Location $Location2 -Tier Standard

```
### <a name="create-a-web-app-in-the-app-service-plan"></a>App Service planında bir Web uygulaması oluşturma
İki örneği kullanarak web uygulaması oluşturma [yeni AzWebApp](/powershell/module/az.websites/new-azwebapp) App Service planlarında *Batı ABD* ve *Doğu ABD* Azure bölgeleri.

```azurepowershell-interactive
$App1ResourceId=(New-AzWebApp -Name $App1Name -ResourceGroupName MyResourceGroup -Location $Location1 -AppServicePlan "$App1Name-Plan").Id
$App2ResourceId=(New-AzWebApp -Name $App2Name -ResourceGroupName MyResourceGroup -Location $Location2 -AppServicePlan "$App2Name-Plan").Id

```

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme
İki Web uygulaması kullanarak Traffic Manager uç noktalar olarak eklemek [yeni AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) Traffic Manager profili gibi:
- Bulunan Web uygulaması Ekle *Batı ABD* tüm kullanıcı trafiği yönlendirmek için birincil uç nokta olarak Azure bölgesi. 
- Bulunan Web uygulaması Ekle *Doğu ABD* yük devretme uç nokta olarak Azure bölgesi. Birincil uç noktaya kullanılamadığında, trafiği otomatik olarak yük devretme uç noktasına yönlendirir.

```azurepowershell-interactive
New-AzTrafficManagerEndpoint -Name "$App1Name-$Location1" `
-ResourceGroupName MyResourceGroup `
-ProfileName "$mytrafficmanagerprofile" `
-Type AzureEndpoints `
-TargetResourceId $App1ResourceId `
-EndpointStatus "Enabled"

New-AzTrafficManagerEndpoint -Name "$App2Name-$Location2" `
-ResourceGroupName MyResourceGroup `
-ProfileName "$mytrafficmanagerprofile" `
-Type AzureEndpoints `
-TargetResourceId $App2ResourceId `
-EndpointStatus "Enabled"
```

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde, Traffic Manager profilinizin etki alanı adını kontrol edeceğiz. Ayrıca, kullanılamaz olarak birincil uç noktaya yapılandıracaksınız. Son olarak, web uygulaması hala kullanılabilir olduğunu görmek alın. Traffic Manager trafik yük devretme uç noktasına gönderir. olmasıdır.

### <a name="determine-the-dns-name"></a>DNS adını belirleme

Traffic Manager profili kullanarak DNS adını belirleme [Get-AzTrafficManagerProfile](/powershell/module/az.trafficmanager/get-aztrafficmanagerprofile).

```azurepowershell-interactive
Get-AzTrafficManagerProfile -Name $mytrafficmanagerprofile `
-ResourceGroupName MyResourceGroup
```

Kopyalama **RelativeDnsName** değeri. Traffic Manager profilinizin DNS adının *http://<* relativednsname *>. trafficmanager.net*. 

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
1. Bir web tarayıcısında, Traffic Manager profilinizin DNS adını girin (*http://<* relativednsname *>. trafficmanager.net*) Web uygulamasının varsayılan Web sitesini görüntülemek için.

    > [!NOTE]
    > Bu hızlı başlangıçta senaryoda, tüm istekleri birincil uç noktasına yönlendirme. Ayarlanmış **öncelik 1**.
2. Traffic Manager yük devretme uygulamada görüntülemek için birincil site kullanarak devre dışı bırak [devre dışı bırak AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/disable-aztrafficmanagerendpoint).

   ```azurepowershell-interactive
    Disable-AzTrafficManagerEndpoint -Name $App1Name-$Location1 `
    -Type AzureEndpoints `
    -ProfileName $mytrafficmanagerprofile `
    -ResourceGroupName MyResourceGroup `
    -Force
   ```
3. Traffic Manager profilinizin DNS adını kopyalayın (*http://<* relativednsname *>. trafficmanager.net*) Web sitesini yeni bir web tarayıcı oturumunda görüntülemek için.
4. Web uygulaması hala kullanılabilir olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, kaynak grupları, web uygulamaları ve tüm ilgili kaynakları kullanarak silme [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Traffic Manager profili oluşturuldu. Trafiği yönlendirme hakkında daha fazla bilgi için Traffic Manager öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)