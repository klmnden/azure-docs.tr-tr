---
title: Azure Site Kurtarma'yı kullanarak Azure için Azure olağanüstü durum kurtarma de ağ oluşturmayla ilgili | Microsoft Docs
description: Azure Site Recovery kullanarak Azure VM'ler, çoğaltma için ağ genel bir bakış sağlar.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/31/2018
ms.author: sujayt
ms.openlocfilehash: 7e717d06aaaef6031a0a3b26c5caf76f0c8c11df
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715947"
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Azure için Azure çoğaltma ağ oluşturma hakkında



Çoğaltma ve kullanarak Azure Vm'leri bir bölgesinden diğerine kurtarma Ağ Kılavuzu bu makalede sunar [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery olağanüstü durum kurtarma için nasıl sağladığını öğrenin [bu senaryo](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Tipik ağ altyapısı

Aşağıdaki diyagram, Azure Vm'lerinde çalışan uygulamalar için tipik bir Azure ortamı gösterir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute veya bir VPN bağlantısı şirket içi ağınızdan Azure'a kullanıyorsanız, ortam aşağıdaki gibidir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, ağları güvenlik duvarları ve ağ güvenlik gruplarını (Nsg'ler) kullanarak korunur. Güvenlik duvarları, ağ bağlantısını denetlemek için URL veya IP tabanlı uygulamaları güvenilir listeye almayı kullanın. Nsg'leri ağ bağlantısını denetlemek için IP adres aralıklarını kullanan kurallar sağlar.

>[!IMPORTANT]
> Doğrulanmış bir proxy denetim ağ bağlantısı için kullanarak Site Recovery tarafından desteklenmiyor ve çoğaltma etkinleştirilemez.


## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız, bu Site kurtarma URL'leri izin ver:


**URL** | **Ayrıntılar**  
--- | ---
*.blob.core.windows.net | Veri kaynağı bölgede önbellek depolama hesabı sanal makineden yazılabilir böylece gereklidir.
login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmeti URL'lerine için gereklidir.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti iletişimi sanal makineden gerçekleştirilmesi gerekir.
*.servicebus.windows.net | Böylece Site Recovery izleme ve tanılama verilerini sanal makineden yazılabilir gereklidir.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

Giden bağlantıyı denetlemek için bir IP tabanlı bir güvenlik duvarı proxy ya da NSG kuralları kullanıyorsanız, bu IP aralıkları izin verilmesi gerekir.

- Kaynak bölgede depolama hesaplarına karşılık gelen tüm IP adres aralıkları
    - Oluşturma bir [depolama hizmeti etiketi](../virtual-network/security-overview.md#service-tags) tabanlı kaynak bölge için NSG kuralının.
    - Böylece veri önbelleği depolama hesabına sanal makineden yazılabilir bu adresleri izin verin.
- Office 365'e karşılık gelen tüm IP adres aralıklarını [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
    - Yeni adresler için Office 365 aralıkları gelecekte eklenirse, yeni NSG kuralları oluşturmanız gerekir.
- Kurtarma Hizmeti uç noktası IP adresleri - kullanılabilir site bir [XML dosyası](https://aka.ms/site-recovery-public-ips) ve hedef konumuna bağlıdır.
-  Yapabilecekleriniz [indirin ve bu komut dosyası](https://aka.ms/nsg-rule-script)üzerinde NSG gerekli kuralları otomatik olarak oluşturmak için.
- Bir test NSG gerekli NSG kuralları oluşturmak ve bir üretim NSG kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.


Site kurtarma IP adres aralıklarını aşağıdaki gibidir:

   **Hedef** | **Site kurtarma IP** |  **Site Recovery IP izleme**
   --- | --- | ---
   Doğu Asya | 52.175.17.132 | 13.94.47.61
   Güneydoğu Asya | 52.187.58.193 | 13.76.179.223
   Orta Hindistan | 52.172.187.37 | 104.211.98.185
   Güney Hindistan | 52.172.46.220 | 104.211.224.190
   Orta Kuzey ABD | 23.96.195.247 | 168.62.249.226
   Kuzey Avrupa | 40.69.212.238 | 52.169.18.8
   Batı Avrupa | 52.166.13.64 | 40.68.93.145
   Doğu ABD | 13.82.88.226 | 104.45.147.24
   Batı ABD | 40.83.179.48 | 104.40.26.199
   Orta Güney ABD | 13.84.148.14 | 104.210.146.250
   Orta ABD | 40.69.144.231 | 52.165.34.144
   Doğu ABD 2 | 52.184.158.163 | 40.79.44.59
   Japonya Doğu | 52.185.150.140 | 138.91.1.105
   Japonya Batı | 52.175.146.69 | 138.91.17.38
   Güney Brezilya | 191.234.185.172 | 23.97.97.36
   Avustralya Doğu | 104.210.113.114 | 191.239.64.144
   Avustralya Güneydoğu | 13.70.159.158 | 191.239.160.45
   Orta Kanada | 52.228.36.192 | 40.85.226.62
   Doğu Kanada | 52.229.125.98 | 40.86.225.142
   Batı Orta ABD | 52.161.20.168 | 13.78.149.209
   Batı ABD 2 | 52.183.45.166 | 13.66.228.204
   Birleşik Krallık Batı | 51.141.3.203 | 51.141.14.113
   Birleşik Krallık Güney | 51.140.43.158 | 51.140.189.52
   UK Güney 2 | 13.87.37.4| 13.87.34.139
   UK Kuzey | 51.142.209.167 | 13.87.102.68
   Kore Orta | 52.231.28.253 | 52.231.32.85
   Kore Güney | 52.231.298.185 | 52.231.200.144
   Fransa Orta | 52.143.138.106 | 52.143.136.55
   Fransa Güney | 52.136.139.227 |52.136.136.62


## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırma

Bu örnek çoğaltmak bir VM NSG kurallarını yapılandırmak nasıl gösterir.

- Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, bağlantı noktası: 443 "HTTPS giden izin ver" kuralı tüm gerekli IP adresi aralıkları için kullanın.
- Örneğin, VM kaynak konumu "Doğu ABD" olduğunu ve hedef konumu "Orta ABD" olduğunu varsayar.

### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi NSG "Storage.EastUS" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

      ![Depolama etiketi](./media/azure-to-azure-about-networking/storage-tag.png)

2. Office 365'e karşılık gelen tüm IP adres aralıkları için giden HTTPS (443) kuralları oluşturma [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Hedef konuma karşılık gelen Site kurtarma IP'ler için giden HTTPS (443) kurallarını oluşturun:

   **Konum** | **Site kurtarma IP adresi** |  **Site kurtarma izleme IP adresi**
    --- | --- | ---
   Orta ABD | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD

Bu kurallar, böylece kaynak bölgesi yük devretme sonrasında için etkin hale getirilebilir çoğaltma hedef bölgesinden gereklidir:

1. NSG "Storage.CentralUS" için bir giden HTTPS (443) güvenlik kuralı oluşturun.

2. Office 365'e karşılık gelen tüm IP adres aralıkları için giden HTTPS (443) kuralları oluşturma [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

3. Kaynak konumuna karşılık gelen Site kurtarma IP'ler için giden HTTPS (443) kurallarını oluşturun:

   **Konum** | **Site kurtarma IP adresi** |  **Site kurtarma izleme IP adresi**
    --- | --- | ---
   Orta ABD | 13.82.88.226 | 104.45.147.24

## <a name="network-virtual-appliance-configuration"></a>Ağ sanal gereç yapılandırması

Sanal makineleri giden ağ trafiğini denetlemek için ağ sanal Gereçleri (NVAs) kullanıyorsanız, tüm çoğaltma trafiği NVA geçerse Gereci kısıtlanan. Çoğaltma trafiği için nva'nın geçmez böylece bir Ağ Hizmeti uç noktası "Depolama için" sanal ağınızda oluşturma öneririz.

### <a name="create-network-service-endpoint-for-storage"></a>Ağ Hizmeti uç noktası için depolama alanı oluşturma
Böylece çoğaltma trafiğini Azure sınır bırakmaz "Depolama için" sanal ağınızda Ağ Hizmeti uç noktası oluşturabilirsiniz.

- Azure sanal ağınıza seçip 'Hizmet uç noktaları üzerinde' seçeneğini tıklatın

    ![Depolama uç noktası](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- 'Ekle' seçeneğini tıklatın ve 'hizmet uç noktaları Ekle' sekmesinde açar
- 'Service' altında ' Microsoft.Storage' ve 'Alt' alanı altında gerekli alt ağları seçin ve 'Ekle'yi tıklatın

>[!NOTE]
>Depolama hesaplarınıza ASR için kullanılan sanal ağ erişimini kısıtlamaz. 'Tüm ağlar' erişimden sağlamalıdır

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

0.0.0.0/0 adres ön eki ile için Azure'un varsayılan sistem yolu geçersiz bir [özel rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ve bir şirket içi ağ sanal gereç (NVA) VM trafiğine yönlendir, ancak bu yapılandırma için Site Recovery önerilmez Çoğaltma. Özel yollar kullanıyorsanız yapmanız gerekenler [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ "Depolama için" böylece çoğaltma trafiğini Azure sınır bırakmaz.

## <a name="next-steps"></a>Sonraki adımlar
- İş yükleri tarafından korumaya başlamak [Azure sanal makineleri çoğaltmak](site-recovery-azure-to-azure.md).
- Daha fazla bilgi edinmek [IP adresi bekletme](site-recovery-retain-ip-azure-vm-failover.md) Azure sanal makinesi yük devretme için.
- Olağanüstü durum kurtarma hakkında daha fazla bilgi [ExpressRoute Azure sanal makinelerle ](azure-vm-disaster-recovery-with-expressroute.md).
