---
title: Özel IP adresleri VM'ler için (Klasik) - Azure CLI 1.0 yapılandırma | Microsoft Docs
description: Özel IP adresleri için Azure komut satırı arabirimi (CLI) 1.0 kullanarak sanal makineleri (Klasik) yapılandırma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a18877167d04fdb039070d5315390a846925fd29
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak sanal makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. Ayrıca [Resource Manager dağıtım modelinde statik özel IP adresi yönetmek](virtual-networks-static-private-ip-arm-cli.md).

Aşağıdaki örnek Azure CLI komutları önceden oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce açıklanan test ortamı oluşturmak [vnet oluşturma](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı yeni bir VM oluşturmak için *DNS01* adlı yeni bir bulut hizmetinde *TestService* yukarıdaki senaryoya bağlı olarak, aşağıdaki adımları izleyin:

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Çalıştırma **azure hizmet oluşturma** bulut hizmetini oluşturmak için komutu.
   
        azure service create TestService --location uscentral
   
    Beklenen çıktı:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Çalıştırma **azure vm oluşturma** VM oluşturmak için komutu. Özel bir statik IP adresi değeri dikkat edin. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Beklenen çıktı:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (veya --konum)**. VM oluşturulacağı azure bölgesi. Bizim senaryomuz için bu *centralus* ’tur.
   * **-n (veya--vm adı)**. Oluşturulacak VM adıdır.
   * **-w (veya--ağ adı sanal)**. VM oluşturulacağı Vnet'in adı. 
   * **-S (veya--statik IP)**. Özel için statik IP adresi VM.
   * **TestService**. VM oluşturulacağı bulut hizmeti adı.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. VM oluşturmak için kullanılan görüntü.
   * **AdminUser**. Windows VM için yerel yönetici.
   * **AdminP@ssw0rd**. Windows VM için yerel yönetici parolası.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için özel statik IP adresi bilgilerini alma
Komut dosyası yukarıdaki VM oluşturulan için statik özel IP adresi bilgilerini görüntülemek için Azure CLI ve aşağıdaki komutu çalıştırın değeri gözlemlemek *ağ StaticIP*:

    azure vm static-ip show DNS01

Beklenen çıktı:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Özel bir statik IP adresi bir VM'den kaldırma
Özel statik IP adresi kaldırmak için yukarıdaki komut VM'ye aşağıdaki Azure CLI komutu çalıştırın ekledi:

    azure vm static-ip remove DNS01

Beklenen çıktı:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Mevcut bir VM'yi statik özel IP ekleme
Statik eklemek için özel IP adresi çalışma yukarıdaki komut dosyası kullanılarak oluşturulan VM için komutu aşağıdaki:

    azure vm static-ip set DNS01 192.168.1.101

Beklenen çıktı:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Statik olarak bir VM işletim sistemi içinde Azure sanal makineye atanan özel IP sürece atadığınız değil, önerilen gerekli. İşletim sistemi içinde özel IP adresini el ile ayarlayın, Azure VM'ye atanan özel IP adresi aynı adresi olduğundan emin olun veya sanal makineye bağlantısını kaybedebilir. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

