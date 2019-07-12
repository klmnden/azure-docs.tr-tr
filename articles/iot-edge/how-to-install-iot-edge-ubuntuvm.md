---
title: Azure IOT Edge Ubuntu sanal makinelerde çalışan | Microsoft Docs
description: Azure IOT Edge kurulum yönergelerini Ubuntu 16.04 Azure Market sanal makineler üzerinde
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: gregman
ms.openlocfilehash: 8275bceca1a18f49eb7eeece66a3866d77c47635
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67796170"
---
# <a name="run-azure-iot-edge-on-ubuntu-virtual-machines"></a>Azure IOT Edge Ubuntu sanal makineler üzerinde çalıştırın

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz.

IOT Edge çalışma zamanı nasıl çalıştığını ve hangi bileşenler dahildir hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Azure IOT Edge çalışma zamanı bir Ubuntu 16.04 önceden yapılandırılmış kullanarak sanal makine üzerinde çalıştırmak için adımlar bu makalede listelenmektedir [Ubuntu Azure Market teklifi Azure IOT Edge'de](https://aka.ms/azure-iot-edge-ubuntuvm). 

İlk önyüklemede Azure IOT Edge çalışma zamanı en son sürümünü Ubuntu sanal makinesi Azure IOT Edge'de çalışmasını engeller. Ayrıca bağlantı dizesini ayarlayalım ve Azure VM portal ya da kolayca yapılandırın ve bir SSH veya uzaktan başlatmadan IOT Edge cihazı bağlamayı olanak tanıyan Azure komut satırı aracılığıyla uzaktan tetiklenebilir çalışma zamanı yeniden için bir betik içerir Masaüstü oturumu. Bu betik, uygulamanızın automation'a oluşturmanıza gerek kalmaz, IOT Edge istemci tam olarak yüklenene kadar bağlantı dizesini ayarlamak için bekler.

## <a name="deploy-from-the-azure-marketplace"></a>Azure Market görüntüsünden dağıtım
1.  Gidin [Azure IOT Edge üzerinde Ubuntu'da](https://aka.ms/azure-iot-edge-ubuntuvm) Market teklifi veya "Azure IOT Edge üzerinde Ubuntu" arama [Azure Marketi](https://azuremarketplace.microsoft.com/)
2.  Seçin **almak artık BT** ardından **devam** sonraki iletişim kutusunda.
3.  Azure portalında bir kez seçin **Oluştur** ve VM dağıtmak için sihirbazı izleyin. 
    *   Bir VM'i çalışırken ilk kez etkinleştirilmişse, parola ve SSH ortak gelen bağlantı noktası menüsünde etkinleştirmek için kolay. 
    *   Bir kaynak kullanımı yoğun iş yükü varsa, daha fazla CPU ve/veya bellek ekleyerek sanal makine boyutu yükseltmeniz gerekir.
4.  Sanal makine dağıtıldıktan sonra IOT Hub'ınıza bağlanmak için yapılandırın:
    1.  IOT Edge Cihazınızı IOT hub'ına oluşturulan cihaz bağlantı dizesini kopyalayın (izleyebilirsiniz [Azure portalından yeni Azure IOT Edge cihaz kaydetme](how-to-register-device-portal.md) bu işlemle ilgili bilgi sahibi değilseniz, nasıl yapılır Kılavuzu)
    1.  Azure portal ve açık, yeni oluşturulan sanal makine kaynağı seçin **komutu Çalıştır** seçeneği
    1.  Seçin **RunShellScript** seçeneği
    1.  Komut penceresinde cihaz bağlantı dizesiyle aracılığıyla aşağıdaki betiği çalıştırın: `/etc/iotedge/configedge.sh “{device_connection_string}”`
    1.  Seçin **çalıştırın**
    1.  Birkaç dakika bekleyin ve bağlantı dizesini belirten bir başarı iletisi başarıyla ayarlandı ekran sağlamalıdır.


## <a name="deploy-from-the-azure-portal"></a>Azure portalından dağıtma
"Azure IOT Edge için" Azure portalından arayın ve seçin **Ubuntu Server 16.04 LTS + Azure IOT Edge çalışma zamanı** VM oluşturma iş akışı başlatmak için. Buradan, adım 3 ve 4. Yukarıdaki "Dağıtma gelen Azure Market" yönergeleri tamamlayın.

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
    
   1. Kullanmak istediğiniz aboneliği Subscriptionıd alanını kopyalayın.

   1. Çalışma aboneliğinizle kopyaladığınız kimliği ayarlayın:
    
      ```azurecli-interactive 
      az account set -s {SubscriptionId}
      ```
    
1. Yeni bir kaynak grubu oluşturun (veya var olan bir sonraki adımlarda belirtin):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. Sanal makine için kullanım koşullarını kabul edin. Koşulları önce gözden geçirmek istiyorsanız, adımları [dağıtma Azure Market](#deploy-from-the-azure-marketplace).

   ```azurecli-interactive
   az vm image accept-terms --urn microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest
   ```

1. Yeni bir sanal makine oluşturun:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys
   ```

1. Cihaz bağlantı dizesini ayarlayalım (izleyebilirsiniz [Azure CLI ile yeni bir Azure IOT Edge cihazı kaydetme](how-to-register-device-cli.md) bu işlemle ilgili bilgi sahibi değilseniz, nasıl yapılır kılavuzunda):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script "/etc/iotedge/configedge.sh '{device_connection_string}'"
   ```

Kurulumdan sonra bu VM'ye SSH istiyorsanız, Publicıpaddress komutu kullanın: `ssh azureuser@{publicIpAddress}`


## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

Düzgün bir şekilde yükleme IOT Edge çalışma zamanı ile ilgili sorunlar yaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
