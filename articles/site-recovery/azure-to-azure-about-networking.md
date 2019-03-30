---
title: İlgili Azure Site Recovery kullanarak Azure'a olağanüstü durum kurtarma | Microsoft Docs
description: Azure Site Recovery kullanarak bir Azure VM çoğaltma için ağı genel bir bakış sağlar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 3/29/2019
ms.author: sujayt
ms.openlocfilehash: 42db22d39a7c87363cf97f874c85955a09cbe653
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651552"
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Azure'dan Azure'a çoğaltma ağı hakkında



Çoğaltma ve da kullanarak Azure Vm'leri bir bölgeden diğerine kurtarma Bu makalede, ağ yönergeleri sağlanır. [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery olağanüstü durum kurtarma için nasıl sağladığını öğrenin [bu senaryo](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Tipik ağ altyapısını

Aşağıdaki diyagram, Azure Vm'lerinde çalışan uygulamalar için tipik bir Azure ortamına gösterir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute veya VPN bağlantısı kullanarak şirket içi ağınızdan Azure'a kullanıyorsanız, ortam aşağıdaki gibidir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, ağ güvenlik duvarları ve ağ güvenlik grupları (Nsg'ler) kullanarak korunur. Güvenlik duvarları, ağ bağlantısını denetlemek için URL veya IP tabanlı beyaz listeye ekleme özelliğini kullanın. Nsg'leri ağ bağlantısını denetlemek için IP adres aralıkları kullanan kurallar sağlar.

>[!IMPORTANT]
> Ağ bağlantısını denetlemek için kimliği doğrulanmış bir ara sunucu kullanarak Site Recovery tarafından desteklenmiyor ve çoğaltma etkinleştirilemez.


## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı güvenlik duvarı proxy'si kullanıyorsanız, bu Site Recovery hizmeti URL'lerine izin ver:


**URL** | **Ayrıntılar**  
--- | ---
*.blob.core.windows.net | Böylece veri kaynak bölgedeki önbellek depolama hesabına VM'den yazılabilir gereklidir. Önbellek depolama hesapları sanal makineleriniz için biliyorsanız gücüne özel depolama hesap URL'leri beyaz liste olabilir (örn: cache1.blob.core.windows.net ve cache2.blob.core.windows.net) yerine *. blob.core.windows.net
login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmet URL'leri için gereklidir.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmet iletişimi VM'den gerçekleştirilmesi gerekir. Güvenlik duvarı proxy IP'ler destekliyorsa, karşılık gelen 'Site Recovery IP' kullanabilirsiniz.
*.servicebus.windows.net | Böylece Site Recovery izleme ve Tanılama verileri VM'den yazılabilir gereklidir. Güvenlik duvarı proxy IP'ler destekliyorsa, karşılık gelen 'Site Recovery izleme IP' kullanabilirsiniz.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

Giden bağlantıyı denetlemek için IP tabanlı güvenlik duvarı proxy veya NSG kuralları kullanıyorsanız, bu IP aralıklarına izin verilmesi gerekir.

- Depolama hesabı kaynak bölgede karşılık gelen tüm IP adresi aralıkları
    - Oluşturma bir [depolama hizmet etiketi](../virtual-network/security-overview.md#service-tags) NSG kuralı kaynak bölge için bağlı.
    - Sanal makineden önbellek depolama hesabına veri yazılabilir böylece bu adresler sağlar.
- Oluşturma bir [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) erişimi için AAD karşılık gelen tüm IP adreslerine izin vermek için NSG kural tabanlı
    - Azure Active Directory (AAD) gelecekte yeni adresler eklenir, yeni NSG kuralları oluşturmak gerekir.
- Site Recovery Hizmeti uç noktası IP adresleri - bulunan bir [XML dosyası](https://aka.ms/site-recovery-public-ips) ve hedef konumunuz bağlıdır.
- Gerekli NSG kuralları NSG test oluşturma ve bir üretim NSG kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.


Site Recovery IP adresi aralıklarını aşağıdaki gibidir:

   **Hedef** | **Site Recovery IP** |  **Site Recovery IP izleme**
   --- | --- | ---
   Doğu Asya | 52.175.17.132 | 13.94.47.61
   Güneydoğu Asya | 52.187.58.193 | 13.76.179.223
   Merkez Hindistan | 52.172.187.37 | 104.211.98.185
   Güney Hindistan | 52.172.46.220 | 104.211.224.190
   Kuzey Orta ABD | 23.96.195.247 | 168.62.249.226
   Kuzey Avrupa | 40.69.212.238 | 52.169.18.8
   Batı Avrupa | 52.166.13.64 | 40.68.93.145
   Doğu ABD | 13.82.88.226 | 104.45.147.24
   Batı ABD | 40.83.179.48 | 104.40.26.199
   Güney Orta ABD | 13.84.148.14 | 104.210.146.250
   Orta ABD | 40.69.144.231 | 52.165.34.144
   Doğu ABD 2 | 52.184.158.163 | 40.79.44.59
   Doğu Japonya | 52.185.150.140 | 138.91.1.105
   Batı Japonya | 52.175.146.69 | 138.91.17.38
   Güney Brezilya | 191.234.185.172 | 23.97.97.36
   Doğu Avustralya | 104.210.113.114 | 191.239.64.144
   Güney Doğu Avustralya | 13.70.159.158 | 191.239.160.45
   Kanada Orta | 52.228.36.192 | 40.85.226.62
   Kanada Doğu | 52.229.125.98 | 40.86.225.142
   Batı Orta ABD | 52.161.20.168 | 13.78.149.209
   Batı ABD 2 | 52.183.45.166 | 13.66.228.204
   BK Batı | 51.141.3.203 | 51.141.14.113
   BK Güney | 51.140.43.158 | 51.140.189.52
   BK Güney 2 | 13.87.37.4| 13.87.34.139
   BK Kuzey | 51.142.209.167 | 13.87.102.68
   Kore Orta | 52.231.28.253 | 52.231.32.85
   Kore Güney | 52.231.298.185 | 52.231.200.144
   Fransa Orta | 52.143.138.106 | 52.143.136.55
   Fransa Güney | 52.136.139.227 |52.136.136.62
   Avustralya Orta| 20.36.34.70 | 20.36.46.142
   Avustralya Orta 2| 20.36.69.62 | 20.36.74.130
   Güney Afrika Batı | 102.133.72.51 | 102.133.26.128
   Güney Afrika Kuzey | 102.133.160.44 | 102.133.154.128
## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırma

Bu örnek, sanal Makineyi çoğaltmak için NSG kurallarını nasıl yapılandıracağınızı gösterir.

- Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, tüm gerekli IP adresi aralıkları için bağlantı noktası: 443 "Giden HTTPS izin ver" kuralları kullanın.
- Örnek, VM kaynak konumu "Doğu ABD" ve hedef konum "Orta ABD" olduğunu varsayar.

### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG "Storage.EastUS" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

      ![Depolama-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG "AzureActiveDirectory" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Hedef konuma karşılık gelen Site kurtarma IP'ler için giden HTTPS (443) kurallarını oluşturun:

   **Konum** | **Site Recovery IP adresi** |  **Site Recovery izleme IP adresi**
    --- | --- | ---
   Orta ABD | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD

Bu kurallar, çoğaltma hedefi bölgeden kaynak bölgeye yük devretme sonrası için etkinleştirilebilir. böylece gereklidir:

1. NSG "Storage.CentralUS" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

2. NSG "AzureActiveDirectory" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

3. Kaynak konuma karşılık gelen Site kurtarma IP'ler için giden HTTPS (443) kurallarını oluşturun:

   **Konum** | **Site Recovery IP adresi** |  **Site Recovery izleme IP adresi**
    --- | --- | ---
   Orta ABD | 13.82.88.226 | 104.45.147.24

## <a name="network-virtual-appliance-configuration"></a>Ağ sanal Gereci yapılandırması

Vm'lerden giden ağ trafiğinizi denetlemek için ağ sanal Gereçleri (Nva) kullanıyorsanız, tüm çoğaltma trafiğinin NVA üzerinden geçerse gereç kısıtlanan. Çoğaltma trafiği için NVA geçmez, bir ağ hizmet uç noktaları sanal ağınızda bulunan "Depolama için" oluşturmanızı öneririz.

### <a name="create-network-service-endpoint-for-storage"></a>Depolama için Ağ Hizmeti uç noktası oluşturma
Böylece çoğaltma trafiğini Azure sınırını terk değil, "Depolama için" sanal ağınızda bir ağ hizmet uç noktası oluşturabilirsiniz.

- Azure sanal ağınız seçin ve 'Hizmet uç noktaları üzerinde' tıklayın

    ![Depolama uç noktası](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- 'Ekle' seçeneğini tıklatın ve 'hizmet uç noktaları Ekle' sekmesinde açar
- 'Hizmet' altında ' Microsoft.Storage' ve 'Alt' alanı altında gerekli alt ağlar'ı seçin ve 'Ekle' seçeneğine tıklayın

>[!NOTE]
>ASR için kullanılan depolama hesaplarınız için sanal ağ erişimini kısıtlamaz. 'Tüm ağlar' dan erişime izin vermeniz gerekir

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Azure'nın varsayılan sistem yolu 0.0.0.0/0 adres ön eki ile geçersiz kılabilirsiniz bir [özel rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ve bir şirket içi ağ sanal Gereci (NVA) VM trafiğine yöneltmektir, ancak bu yapılandırma, Site Recovery için önerilmez Çoğaltma. Özel yollar kullanıyorsanız yapmanız gerekenler [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ için "depolama" böylece çoğaltma trafiğini Azure sınır bırakmaz.

## <a name="next-steps"></a>Sonraki adımlar
- Yüklerinizi korumaya başlayın [Azure sanal makinelerini çoğaltarak](site-recovery-azure-to-azure.md).
- Daha fazla bilgi edinin [IP adresi bekletme](site-recovery-retain-ip-azure-vm-failover.md) Azure sanal makine yük devretme için.
- Olağanüstü durum kurtarma hakkında daha fazla bilgi [ExpressRoute ile Azure sanal makineleri](azure-vm-disaster-recovery-with-expressroute.md).
