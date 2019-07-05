---
title: Kıyaslama toplu ölçümleri kullanarak Azure NetApp dosyaları için test etme | Microsoft Docs
description: Karşılaştırmalı test toplu performans ve ölçümleri kullanarak Azure NetApp dosyaları için öneriler sağlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: b-juche
ms.openlocfilehash: 12ae9e313655924f11799152b5e58b77776c135c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478816"
---
# <a name="benchmark-testing-for-volume-performance-and-metrics-using-azure-netapp-files"></a>Azure NetApp Files kullanarak birim performansı ve ölçümler için karşılaştırma testi

Bu makalede, toplu performans ve ölçümleri kullanarak Azure NetApp dosyaları için öneriler test Kıyaslama sağlar.

## <a name="overview"></a>Genel Bakış

Bir Azure NetApp dosyaları toplu performans özelliklerini anlamak için açık kaynak aracını kullanabilirsiniz [FIO](https://github.com/axboe/fio) Kıyaslama çeşitli iş yükleri benzetimi yapmak için bir dizi. Hem Linux üzerinde FIO yüklenebilir ve Windows tabanlı işletim sistemleri.  Bir birim için hem IOPS ve aktarım hızı hızlı bir anlık görüntüsünü almak için mükemmel bir araçtır.

### <a name="vm-instance-sizing"></a>VM örneği boyutlandırma

En iyi sonuçlar için testleri gerçekleştirmek için uygun şekilde boyutlandırıldığından bir sanal makine (VM) örneği kullandığınızdan emin olun. Aşağıdaki örnekler bir Standard_D32s_v3 örneği kullanın. Sanal makine örneği boyutlarını hakkında daha fazla bilgi için bkz: [boyutları için Windows azure'da sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) Windows tabanlı VM'ler için ve [azure'da Linux sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Linux tabanlı sanal makineler için.

### <a name="azure-netapp-files-volume-sizing"></a>Azure NetApp dosya birimi boyutlandırma

Beklenen performans düzeyi için doğru hizmet düzeyi ve birim kota boyutu seçtiğinizden emin olun. Bkz: [Azure NetApp dosyaları için hizmet düzeyleri](azure-netapp-files-service-levels.md) daha fazla bilgi için.

### <a name="virtual-network-vnet-recommendations"></a>Sanal ağ (VNet) önerileri

Azure NetApp dosyaları aynı sanal ağa test Kıyaslama gerçekleştirmeniz gerekir. Aşağıdaki örnekte önerinin gösterir:

![Sanal ağ önerileri](../media/azure-netapp-files/azure-netapp-files-benchmark-testing-vnet.png)

## <a name="installation-of-fio"></a>FIO yüklemesi

FIO ikili biçimi hem Linux hem de Windows için kullanıma sunulmuştur. İkili paketleri bölümünde izleyin [FIO](https://github.com/axboe/fio) tercih ettiğiniz platform için yüklenecek.

## <a name="fio-examples-for-iops"></a>IOPS için FIO örnekleri 

Bu bölümde yer alan FIO örnekler aşağıdaki Kurulum kullanın:
* Sanal makine örneğinin boyutu: D32s_v3
* Kapasitesi havuzu hizmet düzeyi ve boyutu: Premium / 50 TiB
* Birim kota boyutu: 48 TiB

Aşağıdaki örnekler, rastgele FIO okuyan ve yazan gösterir.

### <a name="fio-8k-block-size-100-random-reads"></a>FIO: % 100 Rastgele Okuma boyutu 8k engelle

`fio --name=8krandomreads --rw=randread --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128 --size=4G --runtime=600 --group_reporting`

### <a name="output-68k-read-iops-displayed"></a>Çıktı: Görüntülenen IOPS 68k okuyun

`Starting 4 processes`  
`Jobs: 4 (f=4): [r(4)][84.4%][r=537MiB/s,w=0KiB/s][r=68.8k,w=0 IOPS][eta 00m:05s]`

### <a name="fio-8k-block-size-100-random-writes"></a>FIO: 8k boyutu % 100 rastgele yazma işlemlerini engelle

`fio --name=8krandomwrites --rw=randwrite --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-73k-write-iops-displayed"></a>Çıktı: Görüntülenen IOPS 73k yazma

`Starting 4 processes`  
`Jobs: 4 (f=4): [w(4)][26.7%][r=0KiB/s,w=571MiB/s][r=0,w=73.0k IOPS][eta 00m:22s]`

## <a name="fio-examples-for-bandwidth"></a>Bant genişliği için FIO örnekleri

Bu bölümde show sıralı FIO örneklerde okur ve yazar.

### <a name="fio-64k-block-size-100-sequential-reads"></a>FIO: 64k boyutu % 100 sıralı okumalar engelle

`fio --name=64kseqreads --rw=read --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-118-gbits-throughput-displayed"></a>Çıktı: 11.8 görüntülenen Gbit/sn aktarım hızı

`Starting 4 processes`  
`Jobs: 4 (f=4): [R(4)][40.0%][r=1313MiB/s,w=0KiB/s][r=21.0k,w=0 IOPS][eta 00m:09s]`

### <a name="fio-64k-block-size-100-sequential-writes"></a>FIO: 64k boyutu % 100 sıralı yazma işlemleri engelle

`fio --name=64kseqwrites --rw=write --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-122-gbits-throughput-displayed"></a>Çıktı: 12.2 görüntülenen Gbit/sn aktarım hızı

`Starting 4 processes`  
`Jobs: 4 (f=4): [W(4)][85.7%][r=0KiB/s,w=1356MiB/s][r=0,w=21.7k IOPS][eta 00m:02s]`

## <a name="volume-metrics"></a>Toplu ölçümleri

Azure dosyaları NetApp performans verilerini Azure İzleyicisi sayaçları üzerinden kullanılabilir. Sayaçlar, Azure portalı ve REST API GET istekleri kullanılabilir. 

Geçmiş verileri için aşağıdaki bilgileri görüntüleyebilirsiniz:
* Ortalama okuma gecikme süresi 
* Ortalama yazma gecikme süresi 
* Okuma IOPS (ortalama)
* Yazma IOPS (ortalama)
* Mantıksal birim boyutu (ortalama)
* Birim anlık görüntü boyutu (ortalama)

### <a name="using-azure-monitor"></a>Azure İzleyici’yi kullanma 

Aşağıda gösterildiği gibi birim başına temelinde Azure NetApp dosyaları sayaçları ölçümleri sayfasından erişebilirsiniz:

![Azure İzleyici ölçümleri](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-metrics.png)

Ayrıca bir Pano Azure İzleyici'de Azure NetApp dosyaları için ölçümleri sayfaya gitme, NetApp için filtreleme ve ilgili birim sayaçlar belirterek oluşturabilirsiniz: 

![Azure İzleyici panosu](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-dashboard.png)

### <a name="azure-monitor-api-access"></a>Azure İzleyici API erişimi

REST API çağrıları kullanarak Azure NetApp dosyaları sayaçları erişebilirsiniz. Bkz: [Azure İzleyici ile desteklenen ölçümler: Microsoft.NetApp/netAppAccounts/capacityPools/Volumes](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftnetappnetappaccountscapacitypoolsvolumes) kapasitesi havuzu ve birimler için sayaçlar için.

Aşağıdaki örnek, mantıksal birim boyutunu görüntülemek için bir Al URL gösterir:

`#get ANF volume usage`  
`curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/ANFACCOUNTGOESHERE/capacityPools/ANFPOOLGOESHERE/Volumes/ANFVOLUMEGOESHERE/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=VolumeLogicalSize`


## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp dosyaları için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp dosyaları için performans kıyaslamalarını](azure-netapp-files-performance-benchmarks.md)