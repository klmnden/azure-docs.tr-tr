---
title: Windows HPC Kümesi dağıtmak için PowerShell komut dosyası | Microsoft Docs
description: Azure sanal makinelerde Windows HPC Pack 2012 R2 küme dağıtmak için bir PowerShell betiğini çalıştırın
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: e05562aeac0ea89ec1c3d80d2967c8f59c68d332
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30914855"
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Yüksek performanslı bilgi işlem (HPC) küme Windows HPC Pack Iaas dağıtım komut dosyası ile oluşturma
HPC Pack Iaas dağıtımı Azure sanal makinelerde Windows iş yükleri için tam bir HPC Pack 2012 R2 küme dağıtmak için PowerShell betiğini çalıştırın. Küme Windows Server ve Microsoft HPC Pack çalıştıran bir Active Directory katılmış baş düğümü içerir ve ek Windows işlem kaynaklarını, belirtin. Azure Linux iş yükleri için bir HPC paketi küme dağıtmak istiyorsanız, bkz: [Linux HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](../../linux/classic/hpcpack-cluster-powershell-script.md). 

> [!IMPORTANT] 
> Bu makalede açıklanan PowerShell Betiği Azure'da Klasik dağıtım modeli kullanarak bir Microsoft HPC Pack 2012 R2 kümesi oluşturur. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> Ayrıca, bu makalede açıklanan betik HPC Pack 2016 desteklemez. HPC Pack 2012 R2 ve HPC Pack 2016 için Resource Manager şablonları hakkında bilgi için bkz: [HPC paketi küme dağıtım seçenekleri azure'da](../hpcpack-cluster-options.md).

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Örnek yapılandırma dosyaları
Aşağıdaki örneklerde, abonelik kimliği için kendi değerlerinizi veya adını ve hesap ve hizmet adlarını yerleştirin.

### <a name="example-1"></a>Örnek 1
Aşağıdaki yapılandırma dosyası bir baş düğüm yerel veritabanlarıyla bir HPC Pack kümeye dağıtır ve beş işlem düğümleri Windows Server 2012 R2 işletim sistemini çalıştıran. Tüm bulut hizmetlerine doğrudan Batı ABD konumunda oluşturulur. Baş düğüm etki alanı ormanı etki alanı denetleyicisi olarak işlev görür.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Örnek 2
Aşağıdaki yapılandırma dosyası var olan bir etki alanı ormanı HPC Pack kümede dağıtır. Küme yerel veritabanlarıyla 1 baş düğüm vardır ve işlem düğümleri uygulanan Bgınfo VM uzantısı ile 12.
Windows güncelleştirmeleri otomatik yüklemesini etki alanı ormanındaki tüm sanal makineleri devre dışı bırakılmıştır. Tüm bulut hizmetlerine doğrudan Doğu Asya konumda oluşturulur. İşlem düğümlerine oluşturulduğunu üç bulut Hizmetleri ve üç depolama hesabı: *MyHPCCN 0001* için *MyHPCCN 0005* içinde *MyHPCCNService01* ve *mycnstorage01*; *MyHPCCN-0006* için *MyHPCCN0010* içinde *MyHPCCNService02* ve *mycnstorage02*; ve *MyHPCCN 0011* için *MyHPCCN 0012* içinde *MyHPCCNService03* ve *mycnstorage03*). İşlem düğümleri, bir işlem düğümünden yakalanan bir görüntüden varolan özel oluşturulur. Otomatik büyüme ve küçültme hizmeti varsayılan olarak etkin büyür ve küçültme aralıkları.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Örnek 3
Aşağıdaki yapılandırma dosyası var olan bir etki alanı ormanı HPC Pack kümede dağıtır. Küme bir baş düğüm, 500 GB veri diski olan bir veritabanı sunucusu, iki Aracısı düğümleri Windows Server 2012 R2 işletim sistemini çalıştıran ve Windows Server 2012 R2 işletim sistemi çalıştıran beş işlem düğümleri içerir. Bulut hizmeti, MyHPCCNService benzeşim grubunda oluşturuldu *MyIBAffinityGroup*, ve diğer bulut hizmetlerine benzeşim grubunda oluşturulan *MyAffinityGroup*. HPC web portalı ve HPC iş Scheduler REST API baş düğümünde etkinleştirilir.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Örnek 4
Aşağıdaki yapılandırma dosyası var olan bir etki alanı ormanı HPC Pack kümede dağıtır. Yerel veritabanlarıyla iki baş düğüm kümesi olduğundan, iki Azure düğümü şablonları oluşturulur ve üç boyut Orta Azure düğümleri için Azure düğüm şablonu oluşturulur *AzureTemplate1*. Baş düğüm yapılandırıldıktan sonra bir komut dosyası baş düğüm üzerinde çalışır.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Sorun giderme
* **"Sanal ağ yok" hatası** -eşzamanlı olarak bir abonelik altında Azure birden çok kümeler dağıtmak için betik çalıştırırsanız, bir veya daha fazla dağıtım hatasıyla başarısız olabilir "VNet *VNet\_adı* yok ".
  Bu hata oluşursa, komut dosyası başarısız dağıtım için yeniden çalıştırın.
* **Azure sanal ağdan Internet erişirken sorun** - dağıtım komut dosyası kullanarak yeni bir etki alanı denetleyicisi ile bir küme oluşturma veya etki alanı denetleyicisine bir üstbilgi düğüm VM'ine el ile yükseltmek, bağlanma sorunlarla karşılaşabilirsiniz Sanal makineleri internet. Bu sorun bir iletici DNS sunucusu etki alanı denetleyicisinde otomatik olarak yapılandırılır ve bu iletici DNS sunucusu düzgün sorunu çözmezse ortaya çıkabilir.
  
    Etki alanı denetleyicisi ve kaldırın veya iletici yapılandırma ayarı oturumunu bu soruna geçici bir çözüm veya geçerli iletici DNS sunucusunu yapılandırmak için. Bu ayar Sunucu Yöneticisi'ni yapılandırmak için **Araçları** >
    **DNS** DNS Yöneticisi'ni açın ve ardından **İleticiler**.
* **RDMA ağ yoğun Vm'lerden erişirken sorun** - Windows Server işlem eklemek veya Aracısı düğümü bir RDMA özellikli kullanarak VM'ler boyut A8 veya A9 gibi bu VM'lerin RDMA uygulama ağ bağlantısı sorunlarla karşılaşabilirsiniz. Bu sorun bir kümeye VM'ler eklendiğinde HpcVmDrivers uzantısı düzgün yüklenmemiş varsa nedenidir. Örneğin, uzantı yükleme durumda takılmış olabilir.
  
    Bu sorunu geçici olarak çözmek için önce sanal makineleri uzantı durumunu denetleyin. Uzantısı düzgün yüklü değilse, HPC küme düğümleri kaldırmayı deneyin ve ardından düğümler yeniden ekleyin. Örneğin, işlem düğümü VM'ler baş düğümünde Ekle HpcIaaSNode.ps1 komut dosyası çalıştırarak ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Küme üzerinde bir test iş yükü çalıştırmayı deneyin. Bir örnek için bkz: HPC paketi [Başlangıç Kılavuzu](https://technet.microsoft.com/library/jj884144).
* Küme dağıtımı komut dosyası ve bir HPC iş yükü çalıştırmak bir öğretici için bkz: [Excel ve SOA iş yüklerini çalıştırmak için Azure HPC Pack kümede kullanmaya başlama](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Başlatabilir, durdurabilir, eklemek için HPC paketi araçları deneyin ve işlem düğümlerini, oluşturduğunuz bir kümeden kaldırın. Bkz: [bir HPC Pack Yönet işlem düğümleri küme Azure'da](hpcpack-cluster-node-manage.md).
* Yerel bilgisayardan kümeye iş göndermek için ayarladığınız için bkz: [bir HPC Pack gönderme HPC işlerini bir şirket içi bilgisayardan küme Azure'da](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

