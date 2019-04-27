---
title: Bir hub'ı oluşturup azure'da Terraform ile karma ağ topolojisi
description: Terraform kullanarak Azure'da bir tüm hibrit ağ başvuru mimarisi oluşturma işlemini gösteren öğretici
services: terraform
ms.service: azure
keywords: terraform, hub ve bağlı bileşen, ağlar, hibrit ağlar, devops, sanal makine, azure, sanal ağ eşleme, ağ sanal Gereci
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 648369d89bd2b5b08171e1f6f5482c81bfba3c66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884732"
---
# <a name="tutorial-create-a-hub-and-spoke-hybrid-network-topology-with-terraform-in-azure"></a>Öğretici: Bir hub'ı oluşturup azure'da Terraform ile karma ağ topolojisi

Bu öğretici serisinde, Azure'da uygulama Terraform kullanmayı gösterir bir [merkez ve uç ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). 

Bir hub ve bağlı bileşen topolojisi ortak Hizmetleri paylaşırken iş yüklerini yalıtmak için bir yoldur. Bu hizmetler, kimlik ve güvenlik içerir. Hub'ı şirket içi ağ merkezi bağlantı noktası gibi davranan bir sanal ağ (VNet) ' dir. Uçlar, merkezle eşlenen sanal ağlardır. Bileşen ağlarını içinde ayrı iş yüklerini dağıtılırken paylaşılan hizmetler hub'ında dağıtılır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Merkez ve uç hibrit ağ başvuru mimarisi kaynakları düzenlemek için HCL (HashiCorp dili) kullanın
> * Hub'ı ağ Gereci kaynakları oluşturmak için Terraform'u kullanın
> * Tüm kaynaklar için ortak noktası olarak hareket edecek Azure hub'ı ağ oluşturmak için Terraform'u kullanın
> * Ayrı iş yüklerini uç azure'da sanal ağları olarak oluşturmak için Terraform'u kullanın
> * Ağ geçitleri ve şirket içi ve Azure ağları arasında bağlantı kurmak için Terraform'u kullanın
> * Uç ağlara VNet eşlemesi oluşturmak için Terraform'u kullanın

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

- **Terraform'u yükleme ve yapılandırma**: VM'ler ve diğer altyapı, azure'da sağlamak için [Terraform'u yükleme ve yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)

## <a name="hub-and-spoke-topology-architecture"></a>Hub ve bağlı bileşen topolojisi mimarisi

Hub ve bağlı bileşen topolojisinde merkez bir sanal ağ ' dir. Sanal ağ, merkezi bir şirket içi ağınıza bağlantı noktası olarak görev yapar. Uçlar ise merkezle eşleştirilmiş bir ağdır ve iş yüklerini yalıtmak için kullanılabilir. Şirket içi veri merkezi ile merkez arasındaki trafik akışı bir ExpressRoute veya VPN ağ geçidi bağlantısı üzerinden gerçekleşir. Aşağıdaki görüntüde, hub ve bağlı bileşen topolojisinde bileşenleri gösterilmektedir:

![Azure'da hub ve bağlı bileşen topolojisi mimarisi](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-architecture.png)

## <a name="benefits-of-the-hub-and-spoke-topology"></a>Hub ve bağlı bileşen topolojisi avantajları

Bir merkez ve uç ağ topolojisi ortak Hizmetleri paylaşırken iş yüklerini yalıtmak için bir yoldur. Bu hizmetler, kimlik ve güvenlik içerir. Hub'ı şirket içi ağ merkezi bağlantı noktası gibi davranan bir sanal ağ ' dir. Uçlar, merkezle eşlenen sanal ağlardır. Bileşen ağlarını içinde ayrı iş yüklerini dağıtılırken paylaşılan hizmetler hub'ında dağıtılır. Merkez ve uç ağ topolojisi bazı avantajları şunlardır:

- **Maliyet tasarrufu** tarafından birden çok iş yükleri tarafından paylaşılan tek bir konumda Hizmetleri merkezileştirme. Bu iş yükleri, ağ sanal Gereçleri ve DNS sunucularını içerir.
- **Abonelik sınırlarını aşma**: Farklı aboneliklere ait sanal ağlar merkeze eşlenerek sınırlar aşılabilir.
- **İşlerin ayrılması**: Merkezi BT (SecOps, InfraOps) ve iş yüklerini (DevOps) ilgilendiren konular ayrılır.

## <a name="typical-uses-for-the-hub-and-spoke-architecture"></a>Merkez ve uç mimarinin tipik kullanımları

Bir hub ve bağlı bileşen mimarinin tipik kullanımları bazıları şunlardır:

- Birçok müşteri, farklı ortamlarda dağıtılan iş yükleri vardır. Bu ortamların, geliştirme, test ve üretim içerir. Çoğu zaman, bu iş yükleri DNS, Kimlikleri, NTP veya AD DS gibi hizmetleri paylaşmanız gerekir. Bu paylaşılan hizmetler Merkezi sanal ağa yerleştirilebilir. Bu şekilde, her ortam, yalıtım sağlamak için bir uç dağıtılır.
- Birbirlerine gerektirmez, ancak paylaşılan hizmetlere erişim gerektiren iş yükleri.
- Güvenlik konuları üzerinde merkezi denetime gerektiren kuruluşlar.
- Her bir uçtaki iş yükleri için ayrı yönetim gerektiren kuruluşlar.

## <a name="preview-the-demo-components"></a>Tanıtım bileşenleri Önizleme

Bu serideki her öğretici ile çalışırken, çeşitli bileşenler ayrı Terraform betiklerde tanımlanır. Oluşturulan ve dağıtılan tanıtım mimari aşağıdaki bileşenlerden oluşur:

- **Şirket içi ağı**. Bir kuruluş ile çalışan bir özel yerel ağ. Merkez ve uç başvuru mimarisi için azure'da bir sanal ağ, şirket içi ağ benzetimini yapmak için kullanılır.

- **VPN cihazı**. Bir VPN cihazına veya hizmet şirket içi ağa dış bağlantı sağlar. VPN cihazı, bir donanım aygıtı veya bir yazılım çözümü olabilir. 

- **Merkezi sanal ağ**. Hub'ı şirket içi ağınıza bağlantı ve hizmetlerini barındırmak için bir yer merkezi noktadır. Bu hizmetler, uç sanal ağlarında barındırılan farklı iş yükleri tarafından kullanılabilecek.

- **Ağ geçidi alt ağı**. VNet ağ geçitleri aynı alt ağda tutulur.

- **Uç sanal ağları**. Uçlar, iş yüklerini diğer uçlardan ayrı olarak yönetilen kendi sanal ağlarında yalıtmak için kullanılabilir. Her iş yükü birden fazla katman içerebilir. Birden fazla alt ağ, Azure yük dengeleyicileri aracılığıyla birbirine bağlanır. 

- **Sanal ağ eşlemesi**. İki sanal ağ eşleme bağlantısı kullanarak bağlanabilir. Eşleme bağlantıları, sanal ağlar arasındaki geçişsiz ve gecikme süresi düşük bağlantılardır. Sanal ağlar eşlendikten sonra bir yönlendiriciye gerek kalmadan Azure omurgasını kullanarak trafiği exchange. Bir merkez ve uç ağ topolojisinde her ucu merkeze hub'a bağlanmak için VNet eşlemesi kullanılır. Aynı bölgede ya da farklı bölgelerdeki sanal ağlar eşlenebilir.

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma

Tanıtımı için Terraform yapılandırma dosyalarının bulunduğu dizin oluşturun.

1. [Azure portala](https://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `hub-spoke` adlı bir dizin oluşturun.

    ```bash
    mkdir hub-spoke
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-azure-provider"></a>Azure sağlayıcısını tanımlama

Azure sağlayıcısını tanımlayan Terraform yapılandırma dosyasını yapılandırın.

1. Cloud Shell'de adlı yeni bir dosya açın `main.tf`.

    ```bash
    code main.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```JSON
    provider "azurerm" {
        version = "~>1.22"
    }
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="create-the-variables-file"></a>Değişkenleri dosyası oluşturma

Farklı komut dosyaları arasında kullanılan genel değişkenleri Terraform yapılandırma dosyası oluşturun.

1. Cloud Shell'de adlı yeni bir dosya açın `variables.tf`.

    ```bash
    code variables.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```JSON
    variable "location" {
      description = "Location of the network"
      default     = "centralus"
    }
    
    variable "username" {
      description = "Username for Virtual Machines"
      default     = "testadmin"
    }
    
    variable "password" {
      description = "Password for Virtual Machines"
      default     = "Password1234!"
    }
    
    variable "vmsize" {
      description = "Size of the VMs"
      default     = "Standard_DS1_v2"
    }
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Terraform ile şirket içi sanal ağ oluşturma](./terraform-hub-spoke-on-prem.md)
