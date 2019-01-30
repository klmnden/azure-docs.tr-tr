---
title: Azure Stack için planlama depolama kapasitesi | Microsoft Docs
description: Depolama kapasitesi için Azure Stack dağıtımlarını planlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 09/18/2018
ms.openlocfilehash: 5d9d01a482483d030569a4dcad03c9ecef7cffc0
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245159"
---
# <a name="azure-stack-storage-capacity-planning"></a>Azure Stack depolama kapasitesi planlama
Aşağıdaki bölümler solutions depolama gereksinimlerini planlama stratejilerinde destek olmak için Azure Stack depolama kapasitesini planlama bilgileri sağlar.

## <a name="uses-and-organization-of-storage-capacity"></a>Kullanır ve kuruluş depolama kapasitesi
Azure Stack'in hiper yakınsanmış yapılandırması fiziksel depolama cihazlarının paylaşımına olanak sağlar. Kullanılabilir depolama alanı, üç ana bölümleri, altyapı, Kiracı sanal makinelerinin geçici depolama blobları, tablolar ve Kuyruklar Azure tutarlı depolama (ACS) Hizmetleri, yedekleme depolama arasında ve ' dir.

## <a name="spaces-direct-cache-and-capacity-tiers"></a>Önbellek ve kapasite katmanları alanları doğrudan
Depolama kapasitesi gereksinimlerini işletim sistemi, yerel günlüğe kaydetme, dökümleri ve diğer altyapı geçici depolama için kullanılan yoktur. Depolama alanları doğrudan Yapılandırması Yönetim altına duruma depolama aygıtlarını (cihazlar ve kapasite) ayırmak bu yerel depolama kapasitesidir. Depolama aygıtlarını kalanını tek bir ölçek birimindeki sunucularının sayısından bağımsız olarak depolama kapasitesi havuzu yerleştirilir. Bu cihazları iki türleri şunlardır: Önbellek ve kapasite.  Önbellek: önbellek cihazlardır. Alanları doğrudan geri yazma için bu cihazları kullanma ve önbelleğe alma okuyun. Bu önbellek cihazları kapasiteleri kullanılabilir ancak, biçimlendirilmiş, "visible" biçimlendirilmiş sanal-disklerin kapasitesi iletilmez. Kapasite cihazları bu amaçla kullanılır ve "Giriş" depolama alanları tarafından yönetilen veri sağlamalısınız.

Tüm depolama kapasitesi ayrılan ve doğrudan Azure Stack altyapısı tarafından yönetilir. İşleci hakkında yapılandırma, yükleme seçimleri yapmanıza veya kapasite genişletmesi geldiğinde seçtiklerinizle uğraşmanız gerekmez. Bu tasarım kararlarını çözüm gereksinimleri ile hizalamak için yapılan ve ilk ya da yükleme/dağıtım sırasında veya kapasite genişletmesi sırasında otomatik. Dayanıklılık, yeniden oluşturulması için ayrılmış kapasite ve diğer ayrıntıları ayrıntılarını tasarımının bir parçası düşünür. 

İşleçleri bir tüm flash veya karma depolama yapılandırma arasında seçim yapabilirsiniz:

![Azure depolama kapasitesi planlama](media/azure-stack-capacity-planning/storage.png)

Tüm flash yapılandırmasında NVMe kapasite SATA SSD veya NVMe seçeneğiyle bir önbellektir. Kapasite HDD olsa da karma Yapılandırması'nda önbellek bir SATA SSD ile NVMe arasındaki seçimdir.

Kısa bir özeti depolama alanları doğrudan ve Azure Stack depolama yapılandırması aşağıdaki gibidir:
- Bir depolama alanları havuzundan (tüm depolama cihazları içinde tek bir havuz yapılandırılır) ölçek birimi başına
- En iyi performans ve dayanıklılık için üç kopyalama yansıtma olarak oluşturulmuş sanal diskler
- Sanal disklerin bir ReFS dosya sistemi olarak biçimlendirilir.
- Sanal disk kapasitesi hesaplanır ve veri kapasitesi havuzu ayrılmamış bir kapasite cihazın miktarını bırakmak için farklı bir şekilde ayrılmış. Bu sunucu başına tek bir kapasite sürücü eşdeğerdir.
- Her ReFS dosya sistemi BitLocker için bekleyen veri şifrelemesi etkin olacaktır. 

Sanal-otomatik olarak oluşturulan diskler ve kapasitelerini aşağıdaki gibidir:




|Name|Kapasite hesaplama|Açıklama|
|-----|-----|-----|
|Yerel/önyükleme aygıtı|En az 340 GB<sup>1</sup>|İşletim sistemi görüntüleri ve "yerel" altyapı Vm'leri için ayrı ayrı sunucu depolama|
|Altyapı|3,5 TB|Tüm Azure Stack altyapısını kullanımı|
|VmTemp|Aşağıya bakın<sup>2</sup>|Kiracı sanal makinelerinizin bağlı olarak geçici bir diskle ve bu verileri bu sanal diskler depolanır|
|ACS|Aşağıya bakın <sup>3</sup>|Bloblar, tablolar ve Kuyruklar bakım için azure tutarlı depolama kapasitesi|

<sup>1</sup> Azure Stack çözüm iş ortağı gerekli en düşük depolama kapasitesi.

<sup>2</sup> Kiracı sanal makine geçici diskler için kullanılan sanal disk boyutu, sunucunun fiziksel bellek oranı hesaplanır. Tablolarda Azure Iaas VM boyutları için belirtildiği gibi geçici disk sanal makineye atanan fiziksel belleğin oranıdır. Azure stack'teki "geçici disk" depolama için yapılan ayırma çoğu kullanım örnekleri yakalamak için farklı bir yolla yapılır, ancak tüm geçici disk depolama gereksinimlerini karşılamak mümkün olmayabilir. Seçilen çözümün depolama kapasitesi için yalnızca temp disk kapasitesi çoğunu değil kullanırken geçici depolama kullanılabilir hale getirme arasında bir denge oranıdır. Bir geçici depolama diskini, Ölçek birimindeki sunucu başına oluşturulur. Geçici depolama kapasitesini %10 genel olarak kullanılabilir depolama kapasitesi depolama havuzundaki ölçek birimi ötesinde büyüyüp değil. Hesaplama aşağıdaki örnekte olduğu gibi bir şeydir:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> sanal-ACS tarafından kullanılmak üzere oluşturulan kalan kapasite basit bir bölme disklerdir. Belirtildiği gibi tüm diskler sanal üç yönlü yansıtma ve her sunucu için kapasite tutarında bir kapasite sürücünün ayrılmamış. Çeşitli sanal-yukarıda numaralandırılan diskleri ilk ayrılır ve kalan kapasite ACS-için sanal diskleri sonra kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack kapasite elektronik planlama hakkında bilgi edinin](capacity-planning-spreadsheet.md)
