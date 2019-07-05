---
title: Azure NetApp dosyaları için performans kıyaslamalarını | Microsoft Docs
description: Azure NetApp dosyaları birim düzeyinde performans kıyaslama testlerinin sonuçlarını açıklar.
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
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: 14081daf1f45a84bc8ad19bf0239db1281d9e624
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449496"
---
# <a name="performance-benchmarks-for-azure-netapp-files"></a>Azure NetApp Files için performans kıyaslamaları

Bu makalede Azure NetApp dosyaları birim düzeyinde performans kıyaslama testlerinin sonuçlarını açıklar. 

## <a name="sample-application-used-for-the-tests"></a>Testler için kullanılan örnek uygulama

Performans testleri Azure NetApp dosyaları'nı kullanan bir örnek uygulaması ile çalıştırılamadı. Uygulama aşağıdaki özelliklere sahiptir: 

* Bulut için yapılandırılmış Linux tabanlı bir uygulama
* Eklenen sanal makinelerle (VM) gerektiği gibi bilgi işlem gücü artırmak için doğrusal olarak ölçeklendirebilirsiniz
* Data lake hızlı erişilebilirliğini gerektirir
* Bazen rastgele ve bazen sıralı g/ç düzenleri 
    * Rastgele bir desen, büyük miktarlarda g/ç için düşük gecikme süresi gerektirir. 
    * Bir ardışık düzeni, büyük miktarda bant genişliği gerektirir. 

## <a name="about-the-workload-generator"></a>İş yükü Oluşturucu hakkında

Sonuçları Vdbench Özet dosyalarından gelir. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) doğrulamak için depolama performansını, disk g/ç iş yükleri oluşturan bir komut satırı yardımcı programıdır. Kullanılan istemci-sunucu yapılandırmasını ölçeklenebilirdir.  Bu, tek bir karma master/istemci ve 14 ayrılmış istemci Vm'leri içerir.

## <a name="about-the-tests"></a>Testler hakkında

Testler, örnek uygulamayı olabilir sınırları ve yanıt süresi, eğriler sınırları belirlemek için tasarlanmıştır.  

Aşağıdaki test çalıştırılamadı: 

* % 100 8-rastgele KiB okuyun
* % 100 8 KiB rastgele yazma
* % 100 64-okuma KiB sıralı
* % 100 64 KiB sıralı yazma
* % 50 64-sıralı KiB okuma, % 50 64 KiB sıralı yazma
* % 50 8-rastgele KiB okuma, % 50 8 KiB rastgele yazma

## <a name="bandwidth"></a>Bant genişliği

Birden çok Azure NetApp dosyaları sunar [hizmet düzeyleri](azure-netapp-files-service-levels.md). Her hizmet düzeyi farklı miktarda bir bant genişliği başına sağlanan Kapasite (birim kota) TiB sunar. Bir birim için bant genişliği sınırı, hizmet düzeyi ve birim kota birleşimini temel sağlanır. Bant genişliği sınırını gerçekleşmiş aktarım hızı gerçek miktarı belirlemede yalnızca bir faktördür.  

Şu anda 4,500 MiB tek bir birim test karşı bir iş yükü tarafından elde en yüksek aktarım hızıdır.  Premium hizmet düzeyiyle 70.31 TiB birimi kotası, bu aktarım hızı aşağıdaki hesaplama gerçekleştirmek için yeterli bant genişliği sağlayacak: 

![Bant genişliği formülü](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Kota ve hizmet düzeyi](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Aktarım hızı açısından yoğun iş yükleri

Aktarım hızı testi Vdbench ve 12xD32s V3 depolama Vm'leri bir bileşimi kullanılır. Örnek birim testinde elde edilen aşağıdaki üretilen iş sayısı:

![Aktarım hızı testi](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>G/Ç açısından yoğun iş yükleri

G/ç test Vdbench ve 12xD32s V3 depolama Vm'leri bir bileşimi kullanılır. Örnek birim test aşağıdaki g/ç sayıları elde edebilirsiniz:

![G/ç test](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Gecikme süresi

Test sanal makineleri ve Azure NetApp dosyaları toplu arasındaki uzaklığı g/ç performansı üzerinde bir etkisi yoktur.  Aşağıdaki grafik, IOPS ve gecikme süresi yanıt eğrileri iki farklı sanal makine kümeleri için karşılaştırır.  Bir VM kümesi Azure NetApp dosyaları ve diğer küme hemen daha fazla.  Daha fazla VM kümesi artan gecikme sürelerini, IOPS paralellik derecesi belirli bir düzeyde elde edilen miktarı üzerinde bir etkisi yoktur.  Ne olursa olsun, bir birim yapılan okumalar aşağıda gösterildiği gibi 300.000 IOPS aşabilir: 

![Gecikme süresi incelemesi](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Özet

Bir milisaniye yanıt süresi, gecikmeye duyarlı iş yükleri (veritabanları) olabilir. İşlem performansı, üzerinde 300 k IOPS için tek bir birim olabilir.

Aktarım hızı duyarlı uygulamalar (için akış ve görüntüleme) 4.5GiB olabilir / s aktarım hızı.

## <a name="example-scripts"></a>Örnek betikler

Aşağıdaki örnek yalnızca gösterim amaçlı betikleridir.  Bunlar, üretim için kullanılamaz.  

    #
    #This script makes the following assumptions about the environment
    #VM Naming Convention:
    #   VM naming convention: vdbench-vnet1-1 .. vdbench-vnet1-x
    #
    #VM Count:
    #   The script is written for 24 VM's, 
    #   If you wish to test with a different number of vm's replace {1..24} with {1..Some Number}
    #
    #Volume mount point:
    #    The volumes in this script are mounted at the following mount points on all virtual machines
    #    /mnt/vdb1 ... /mnt/vdb6
    #   
    #Virtual machines must have
    #   make sure that vdbench is present on all vms in the environment and that java has been installed on each vm as well as nfs-utils
    #
    #The following tunables were configured on all virtual machines in the environment
    #    No special tunables were used to extract the results identified in this paper.
    #    Even rsize and wsize were left at the default (64K)
    #
    #Special Notes (thread counts) 
    #   You can hone in on the amount of I/O you hope to achieve in your testing using littles law,
    #   Run each test with a thread count of 1 to determnine the best possible latency.
    #   Thread count equals latency in seconds * desired I/O rate.  
    #   If you find a minimum latency of 1.14ms for example, divide that by 1000 to convert 1.3ms to .0013 seconds.
    #   
    #   If your goal from a single machine is for example 3,500 IOPS, a thread count of 4.0 is required.
    
    #   thread count 4.0 == .0013s * 5,000ops
    #
    #    
    #Special Notes: Increased window size:
    #   As round-trip time increases; corresponding increases are required in TCP Window Size – think Littles law as shown above. 
    #   Check the values of the following and adjust as needed: net.core.rmem_max, net.core.rmem_default, net.ipv4.tcp_rmem
    #
    #Run the utility like this:
    #    vdbench -f vnet1-SeqMix-workload
    #
    #When the utility is finished its run, grep 'avg' from the summary file to see overall run performance.
     
    
    
    #LUN CONFIG FILES
    for vnet in 1; do
        echo "sd=default,size=693g" > vnet${vnet}-luns-nfs
        for vm in {1..24}; do
            for vol in {1..6}; do
                echo "sd=sd-${vm}-${vol},host=vm${vm},lun=/mnt/vdb${vol}/file-${vm}-${vol},openflags=o_direct" >> vnet${vnet}-luns-nfs
            done
        done
    done
    
    
    #HOST CONFIG FILES
    for vnet in 1; do
        echo "hd=default,jvms=1,shell=ssh" > vnet${vnet}-hosts-nfs
        for vm in {1..24}; do
            echo "hd=vm${vm},system=vdbench-vnet${vnet}-${vm},user=root" >> vnet${vnet}-hosts-nfs
        done
    done
    
    #VDBENCH WORK SCRIPTS
    for vnet in 1; do
        for pattern in FillWrite SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "include=vnet${vnet}-hosts-nfs" > vnet${vnet}-${pattern}-workload
            echo "include=vnet${vnet}-luns-nfs" >> vnet${vnet}-${pattern}-workload
        done 
        #Fill Write File
        echo "wd=FillWrite,sd=sd*,rdpct=0,seekpct=eof,xfersize=64k" >> vnet${vnet}-FillWrite-workload
        echo "* Run Defaults" >> vnet${vnet}-FillWrite-workload
        echo 'rd=Initialize-LUNs-Full,wd=FillWrite,sd=("sd*"),iorate=max,interval=1,forthreads=1,elapsed=259200' >> vnet${vnet}-FillWrite-workload
    
        #The actual workload files - feel free to add more or use less.
        #   The thread counts shown below were used to generate the performance collateral, 
        #   feel free to change to suite your needs.
    
        echo "wd=SeqWrite,sd=sd*,rdpct=0,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqWrite-workload
        echo "wd=SeqRead,sd=sd*,rdpct=100,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqRead-workload
        echo "wd=RndRead,sd=sd*,rdpct=100,seekpct=100,xfersize=8k" >> vnet${vnet}-RndRead-workload
        echo "wd=RndWrite,sd=sd*,rdpct=0,seekpct=100,xfersize=8k" >> vnet${vnet}-RndWrite-workload
        echo "wd=SeqMix,sd=sd*,rdpct=50,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqMix-workload
        echo "wd=RndMix,sd=sd*,rdpct=50,seekpct=100,xfersize=8k" >> vnet${vnet}-RndMix-workload
        for pattern in SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "* Run Defaults" >> vnet${vnet}-${pattern}-workload
            #echo "rd=default,curve=(50,100),iorate=curve,warmup=60,elapsed=600,iorate=curve" >> vnet${vnet}-${pattern}-workload
            echo "rd=default,warmup=60,elapsed=600,iorate=max" >> vnet${vnet}-${pattern}-workload
            echo "* Run Definitions" >> vnet${vnet}-${pattern}-workload
        done
        echo 'rd=RndRead,wd=RndRead,sd=("sd*"),threads=25' >> vnet${vnet}-RndRead-workload
        echo 'rd=RndWrite,wd=RndWrite,sd=("sd*"),threads=20' >> vnet${vnet}-RndWrite-workload
        echo 'rd=SeqRead-4,wd=SeqRead,sd=("sd*"),threads=20' >> vnet${vnet}-SeqRead-workload
        echo 'rd=SeqWrite-26,wd=SeqWrite,sd=("sd*"),threads=26' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-32,wd=SeqWrite,sd=("sd*"),threads=32' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-40,wd=SeqWrite,sd=("sd*"),threads=40' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=RndMix,wd=RndMix,sd=("sd*"),threads=25' >> vnet${vnet}-RndMix-workload
        echo 'rd=SeqMix-6,wd=SeqMix,sd=("sd*"),threads=6' >> vnet${vnet}-SeqMix-workload
        echo 'rd=SeqMix-7,wd=SeqMix,sd=("sd*"),threads=7' >> vnet${vnet}-SeqMix-workload
    done
