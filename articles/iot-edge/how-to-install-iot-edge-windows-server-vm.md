---
title: Azure IOT Edge, Windows Server sanal makinelerinde çalıştırma | Microsoft Docs
description: Azure IOT Edge kurulum yönergelerini Windows Server Market sanal makineler
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: gregman
ms.openlocfilehash: be7479d3f042d6e64428a07e0509907b78595200
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159788"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>Azure IOT Edge, Windows Server sanal makinelerinde çalıştırma
Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz.

IOT Edge çalışma zamanı nasıl çalıştığını ve hangi bileşenler dahildir hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Azure IOT Edge çalışma zamanı, Windows Server 2019 kullanarak bir sanal makine üzerinde çalıştırmak için adımlar bu makalede listelenmektedir [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) Azure Marketi'nde teklif. Konumundaki yönergeleri [Azure IOT Edge çalışma zamanı yükleme](how-to-install-iot-edge-windows.md) diğer sürümleriyle birlikte kullanılmak üzere Windows üzerinde.

> [!NOTE]
> IOT Edge çalışma zamanı Windows Server'da yer [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="deploy-from-the-azure-marketplace"></a>Azure Market görüntüsünden dağıtım
1.  Gidin [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) Azure Marketi'nde teklif veya "Windows Server" arama [Azure Marketi](https://azuremarketplace.microsoft.com/)
2.  Seçin **alma şimdi** 
3.  İçinde **yazılım planı**, "Windows Server 2019 veri merkezi Sunucu Çekirdeği ile kapsayıcıları" bulun ve ardından **devam** sonraki iletişim kutusunda.
    * Kapsayıcılar ile Windows Server'ın diğer sürümleri için bu yönergeleri kullanabilirsiniz
4.  Azure portalında bir kez seçin **Oluştur** ve VM dağıtmak için sihirbazı izleyin. 
    *   İlk kez bir VM çalışırken ise, bunu bir parola kullanmak ve RDP ve SSH ortak gelen bağlantı noktası menüde etkinleştirmek için en kolay yoldur. 
    *   Bir kaynak kullanımı yoğun iş yükü varsa, daha fazla CPU ve/veya bellek ekleyerek sanal makine boyutu yükseltmeniz gerekir.
5.  Sanal makine dağıtıldıktan sonra IOT Hub'ınıza bağlanmak için yapılandırın:
    1.  IOT Edge Cihazınızı IOT hub'ına oluşturulan cihaz bağlantı dizesini kopyalayın (izleyebilirsiniz [Azure portalından yeni Azure IOT Edge cihaz kaydetme](how-to-register-device-portal.md) bu işlemle ilgili bilgi sahibi değilseniz, nasıl yapılır Kılavuzu)
    1.  Azure portal ve açık, yeni oluşturulan sanal makine kaynağı seçin **komutu Çalıştır** seçeneği
    1.  Seçin **RunPowerShellScript** seçeneği
    1.  Bu betik, cihaz bağlantısı dizeniz ile komut penceresine kopyalayın: 
        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```
    1.  Edge çalışma zamanı yükleme ve bağlantı dizenizi seçerek ayarlamak için bu betiği yürütün **çalıştırın**
    1.  Bir veya iki dakika sonra Edge çalışma zamanı yüklü ve başarıyla kaynak sağlandı, bir ileti görürsünüz.


## <a name="deploy-from-the-azure-portal"></a>Azure portalından dağıtma
1. Azure portalında, "Windows Server'de" arayın ve seçin **Windows Server 2019 Datacenter** VM oluşturma iş akışı başlatmak için. 
2. Gelen **yazılım planı seçin** "Windows Server 2019 veri merkezi Sunucu Çekirdeği ile kapsayıcıları" öğesini seçin ve ardından **oluştur**
3. Yukarıdaki 5. adım "Dağıtma gelen Azure Marketi'nde" yönergeleri tamamlayın.

## <a name="deploy-from-azure-cli"></a>Azure CLI üzerinden dağıtma
1. Sizin masaüstünüzde Azure CLI kullanıyorsanız, oturum açarak başlatın:

   ```azurecli-interactive
   az login
   ```
    
1. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:
   1. Aboneliklerinizi listeleyin:
    
      ```azurecli-interactive
      az account list --output table
      ```
    
   1. Kopyalama için kullanmak istediğiniz aboneliği Subscriptionıd alanı
   1. Kopyaladığınız Kimliğiyle şu komutu çalıştırın:
    
      ```azurecli-interactive 
      az account set -s {SubscriptionId}
      ```
    
1. Yeni bir kaynak grubu oluşturun (veya var olan bir sonraki adımlarda belirtin):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```
    
1. Yeni bir sanal makine oluşturun:

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```
   * Bu komut için bir parola ister, ancak seçeneğini ekleyebilirsiniz `--admin-password` daha kolay bir betikte ayarlamak için
   * Windows Server Core görüntü desteği yalnızca Uzak Masaüstü ile satır, tam masaüstü deneyimi isterseniz belirtin komut bulunuyor `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` görüntü

1. Cihaz bağlantı dizesini ayarlayalım (izleyebilirsiniz [Azure CLI ile yeni bir Azure IOT Edge cihazı kaydetme](how-to-register-device-cli.md) bu işlemle ilgili bilgi sahibi değilseniz, nasıl yapılır kılavuzunda):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

Düzgün bir şekilde yükleme Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).

Windows sanal makinelerde kullanma hakkında daha fazla bilgi edinin [Windows sanal makineler belgeleri](https://docs.microsoft.com/azure/virtual-machines/windows/).

Kurulumdan sonra bu VM'ye SSH istiyorsanız izleyin [yükleme OpenSSH için Windows Server'ın](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) Uzak Masaüstü veya uzak powershell kullanarak Kılavuzu.
