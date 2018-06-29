---
title: 'ExpressRoute bağlantı hatlarını Klasikten Resource Manager taşıma: PowerShell: Azure | Microsoft Docs'
description: Bu sayfa, Klasik hattı PowerShell kullanarak Resource Manager dağıtım modeline taşıma açıklar.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 20914eec070452186295f6d87a85ea0675ebaf4c
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060095"
---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model-using-powershell"></a>ExpressRoute bağlantı hatları Klasikten Resource Manager dağıtım modeline PowerShell kullanarak Taşı

Bir expressroute bağlantı hattı Klasik ve Resource Manager dağıtım modelleri için kullanmak için bağlantı hattı Resource Manager dağıtım modeline taşımanız gerekir. Aşağıdaki bölümlerde, PowerShell kullanarak hattınız taşımanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure PowerShell modüllerinin en son sürümüne sahip olduğunu doğrulayın (en az sürüm 1.0). Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Gözden geçirdiğinizden emin olun [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Altında sağlanan bilgileri gözden [bir expressroute bağlantı hattını Klasikten Resource Manager taşıma](expressroute-move.md). Tam olarak sınırları ve kısıtlamaları anladığınızdan emin olun.
* Bağlantı hattı Klasik dağıtım modelinde tam olarak işlevsel olduğunu doğrulayın.
* Resource Manager dağıtım modelinde oluşturulmuş bir kaynak grubu olduğundan emin olun.

## <a name="move-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı taşıma

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Adım 1: Klasik dağıtım modelinden bağlantı hattı ayrıntılarını toplama

Azure Klasik ortama oturum açabilir ve hizmet anahtarı toplayın.

1. Azure hesabınızda oturum açın.

  ```powershell
  Add-AzureAccount
  ```

2. Uygun Azure aboneliğini seçin.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Azure ve ExpressRoute için PowerShell modülleri içeri aktarın.

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. Tüm ExpressRoute bağlantı hatları için hizmet anahtarları almak için aşağıdaki cmdlet'i kullanın. Anahtarları aldıktan sonra kopyalama **hizmet anahtarı** Resource Manager dağıtım modeline taşımak istediğiniz bağlantı hattının.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>2. adım: Oturum açın ve bir kaynak grubu oluşturun

Resource Manager ortamına oturum açın ve yeni bir kaynak grubu oluşturun.

1. Azure Resource Manager ortamınız için oturum açın.

  ```powershell
  Connect-AzureRmAccount
  ```

2. Uygun Azure aboneliğini seçin.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Bir kaynak grubu zaten yoksa yeni bir kaynak grubu oluşturmak için aşağıdaki kod parçacığında değiştirin.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>3. adım: expressroute bağlantı hattı Resource Manager dağıtım modeline taşıma

Şimdi, expressroute bağlantı hattı Klasik dağıtım modelinden Resource Manager dağıtım modeline taşıma hazırsınız. Devam etmeden önce sağlanan bilgileri gözden [bir expressroute bağlantı hattını Klasikten Resource Manager dağıtım modeline taşıma](expressroute-move.md).

Bağlantı hattınız taşımak için değiştirin ve aşağıdaki kod parçacığında çalıştırın:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> Klasik modda bir expressroute bağlantı hattı bir bölgeye bağlı kavramına sahip değil. Resource Manager (ARM içinde), her kaynak için bir Azure bölgesi eşlenmesi gerekir. Taşıma AzureRmExpressRouteCircuit cmdlet belirtilen bölgeyi teknik olarak herhangi bir bölgeyi olabilir. Kuruluş amacıyla yakından eşleme konumunuzu temsil eden bir bölge seçmek isteyebilirsiniz.
> 

> [!NOTE]
> Taşıma tamamlandıktan sonra önceki cmdlet'te listelenen yeni adı kaynak adres için kullanılır. Bağlantı hattı temelde yeniden adlandırılacak.
> 

## <a name="modify-circuit-access"></a>Bağlantı hattı erişimi değiştirme

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a>Her iki dağıtım modeli için ExpressRoute bağlantı hattı erişimini etkinleştirmek için

Klasik expressroute bağlantı hattı Resource Manager dağıtım modeline taşıdıktan sonra her iki dağıtım modeline erişimi etkinleştirebilirsiniz. Her iki dağıtım modeline erişimi etkinleştirmek için aşağıdaki cmdlet'leri çalıştırın:

1. Bağlantı hattı ayrıntıları öğrenin.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. "Klasik işlemleri TRUE izin ver" olarak ayarlayın.

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Bağlantı hattı güncelleştirin. Bu işlemi başarıyla tamamlandıktan sonra Klasik dağıtım modelinde bağlantı hattına görmeye olacaktır.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Expressroute bağlantı hattı ayrıntılarını almak için aşağıdaki cmdlet'i çalıştırın. Listelenen hizmet anahtarı görüyor olmanız gerekir.

  ```powershell
  get-azurededicatedcircuit
  ```

5. Resource Manager sanal ağlar için Klasik sanal ağlar ve Resource Manager komutları için Klasik dağıtım modeli komutları kullanarak expressroute bağlantı hattı bağlantıları artık yönetebilirsiniz. Aşağıdaki makaleleri expressroute bağlantı hattı bağlantıları yönetmenize yardımcı olur:

    * [Expressroute bağlantı hattı Resource Manager dağıtım modelinde sanal ağınıza bağlamak](expressroute-howto-linkvnet-arm.md)
    * [Expressroute bağlantı hattı Klasik dağıtım modelinde sanal ağınıza bağlamak](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a>Klasik dağıtım modeli için ExpressRoute bağlantı hattı erişimi devre dışı bırakma

Klasik dağıtım modeline erişimi devre dışı bırakmak için aşağıdaki cmdlet'leri çalıştırın.

1. Expressroute bağlantı hattı ayrıntılarını alın.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. "Klasik işlemleri FALSE izin ver" olarak ayarlayın.

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Bağlantı hattı güncelleştirin. Bu işlemi başarıyla tamamlandıktan sonra Klasik dağıtım modelinde bağlantı hattına görmeye olmaz.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturma ve expressroute bağlantı hattı için yönlendirmeyi değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağ, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)
