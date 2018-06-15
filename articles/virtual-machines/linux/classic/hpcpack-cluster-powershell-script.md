---
title: Linux HPC küme dağıtmak için PowerShell komut dosyası | Microsoft Docs
description: Azure sanal makinelerde Linux HPC Pack 2012 R2 küme dağıtmak için bir PowerShell betiğini çalıştırın
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 66affb47190ba0c6fccaae8e8267b310682aee46
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841852"
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Yüksek performanslı bilgi işlem (HPC) küme Linux ile HPC Pack Iaas dağıtım komut dosyası oluşturma
HPC Pack Iaas dağıtımı Azure sanal makinelerde Linux iş yükleri için tam bir HPC Pack 2012 R2 küme dağıtmak için PowerShell betiğini çalıştırın. Küme Windows Server ve Microsoft HPC Pack çalıştıran bir Active Directory katılmış baş düğüm ve HPC paketi tarafından desteklenen Linux dağıtımları birini çalıştırın işlem düğümleri oluşur. Windows Azure iş yükleri HPC Pack kümesinde dağıtmak istiyorsanız, bkz: [bir Windows HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
> [!IMPORTANT] 
> Bu makalede açıklanan PowerShell Betiği Azure'da Klasik dağıtım modeli kullanarak bir Microsoft HPC Pack 2012 R2 kümesi oluşturur. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> Ayrıca, bu makalede açıklanan betik HPC Pack 2016 desteklemez. HPC Pack 2012 R2 ve HPC Pack 2016 için Resource Manager şablonları hakkında bilgi için bkz: [HPC paketi küme dağıtım seçenekleri azure'da](../hpcpack-cluster-options.md).


[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Örnek yapılandırma dosyası
Aşağıdaki yapılandırma dosyası, bir etki alanı denetleyicisi ve etki alanı ormanı oluşturur ve yerel veritabanlarını tek bir baş düğüm ve 10 Linux işlem düğümlerini içeren bir HPC Pack kümesi dağıtır. Tüm bulut hizmetlerine doğrudan Doğu Asya konumda oluşturulur. Linux işlem düğümlerini iki bulut Hizmetleri ve iki depolama hesabı oluşturulduğunda (diğer bir deyişle, *MyLnxCN 0001* için *MyLnxCN 0005* içinde *MyLnxCNService01* ve *mylnxstorage01*, ve *MyLnxCN-0006* için *MyLnxCN 0010* içinde *MyLnxCNService02* ve *mylnxstorage02*). İşlem düğümleri OpenLogic CentOS sürüm 7.0 Linux görüntüden oluşturulur. 

Abonelik adı ve hesap ve hizmet adları için kendi değerlerinizi yerleştirin.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Sorun giderme
* **"Sanal ağ yok" hatası**. Bir abonelik altında eşzamanlı olarak birden çok küme azure'da dağıtmak için HPC Pack Iaas dağıtım betik çalıştırırsanız, bir veya daha fazla dağıtım hatasıyla başarısız olabilir "VNet *VNet\_adı* yok".
  Bu hata oluşursa, başarısız dağıtım için komut dosyası yeniden çalıştırın.
* **Azure sanal ağdan Internet erişirken sorun**. Dağıtım komut dosyası kullanarak yeni bir etki alanı denetleyicisi ile bir HPC paketi küme oluşturma veya etki alanı denetleyicisine bir üstbilgi düğüm VM'ine el ile yükseltmek, Azure sanal ağ içindeki VM'ler Internet'e bağlanmasını sorunlarla karşılaşabilirsiniz. İletici DNS sunucusu etki alanı denetleyicisinde otomatik olarak yapılandırılır ve bu iletici DNS sunucusu düzgün sorunu çözmezse, bu durum ortaya çıkabilir.
  
    Etki alanı denetleyicisi ve kaldırın veya iletici yapılandırma ayarı oturumunu bu soruna geçici bir çözüm veya geçerli iletici DNS sunucusunu yapılandırmak için. Bu, Sunucu Yöneticisi'ni tıklatın yapmak için **Araçları** > **DNS** DNS Yöneticisi'ni açın ve ardından **İleticiler**.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md) desteklenen Linux dağıtımları hakkında daha fazla bilgi için veri taşıma ve Linux HPC Pack kümeyle işlerini gönderme işlem düğümleri.
* Bir küme oluşturmak ve Linux HPC iş yükü çalıştırmak için komut dosyası kullanma öğreticileri için bkz:
  * [Linux işlem düğümlerinde Azure ile birlikte Microsoft HPC Pack NAMD çalıştırın](hpcpack-cluster-namd.md)
  * [Linux işlem düğümlerinde Azure ile birlikte Microsoft HPC Pack OpenFOAM çalıştırın](hpcpack-cluster-openfoam.md)
  * [Yıldız Çalıştır-CCM + Microsoft HPC Pack Linux işlem düğümlerini Azure](hpcpack-cluster-starccm.md)

