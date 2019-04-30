---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: db1af4f046bd8849fddee299e949d6edbdaae86a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61448410"
---
## <a name="access-the-virtual-machine"></a>Sanal makineye erişim

Aşağıdaki adımları kullanın `az` Azure Cloud shell'de komutu. İsterseniz, aşağıdakileri yapabilirsiniz [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) geliştirme makinesi ve komutları yerel olarak çalıştırın.

İzin vermek için Azure sanal makineyi yapılandırmak nasıl bir aşağıdaki adımlarda **SSH** erişim. Gösterilen adımlar çözüm Hızlandırıcı için seçtiğiniz adı varsayar **contoso-simülasyon** --bu değer, dağıtımınızın adıyla değiştirin:

1. Çözüm Hızlandırıcı kaynakları içeren kaynak grubunun içeriğini listeleyin:

    ```azurecli
    az resource list -g contoso-simulation -o table
    ```

    Sanal makine, genel IP adresi ve ağ güvenlik grubu adını not edin; bu değerler daha sonra ihtiyacınız.

1. SSH erişimine izin vermek için ağ güvenlik grubu güncelleştirin. Ağ güvenlik grubu adı olduğu aşağıdaki komutu varsayar **contoso benzetimi nsg** --bu değer, ağ güvenlik grubunun adıyla değiştirin:

    ```azurecli
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimini etkinleştirin. SSH, etkinleştirirseniz [yeniden olabildiğince çabuk devre dışı](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices#disable-rdpssh-access-to-virtual-machines).

1. Parolasını güncelleştirin **azureuser** bildiğiniz bir parola sanal makine hesabında. Aşağıdaki komutu çalıştırdığınızda, kendi parola seçin:

    ```azurecli
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Sanal makinenizin genel IP adresini bulun. Aşağıdaki komut sanal makinenin adı varsayar **vm vikxv** --bu değeri daha önce Not yapılan sanal makinenin adıyla değiştirin:

    ```azurecli
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Sanal makinenizin genel IP adresini not edin.
