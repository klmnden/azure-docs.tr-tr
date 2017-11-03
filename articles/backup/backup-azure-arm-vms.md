---
title: Azure Vm'leri yedekleme | Microsoft Docs
description: "Bul, kaydetme ve Azure sanal makineleri bir kurtarma Hizmetleri kasasına yedekleme."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "sanal makine yedeklemesi; sanal makineyi geri; Yedekleme ve olağanüstü durum kurtarma; ARM vm yedekleme"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40983a3de104238d09b976b5fcf2419da42c1bba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a>Azure sanal makinelerini bir Kurtarma Hizmetleri kasasına yedekleme
> [!div class="op_single_selector"]
> * [Vm'leri kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md)
> * [Yedekleme Kasası'na Vm'leri yedekleme](backup-azure-vms.md)
>
>

Bu makalede Azure Vm'leri (Resource Manager tarafından dağıtılan ve klasik dağıtılan) bir kurtarma Hizmetleri kasasına yedekleme konusunda ayrıntılı olarak açıklanmaktadır. İşlerin çoğunu VM'lerin yedeklenmesi için hazırlık olur. Yedeklemek veya VM korumak için önce tamamlamanız gereken [Önkoşullar](backup-azure-arm-vms-prepare.md) Vm'lerinizi koruma için ortamınızı hazırlamak için. Sonra önkoşulları tamamladıktan sonra VM anlık görüntülerini almak için yedekleme işlemi başlatabilirsiniz.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Daha fazla bilgi için üzerinde makalelerine bakın [azure'da VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md) ve [Azure sanal makineleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Yedekleme işini tetiklemeden
Kurtarma Hizmetleri kasası ile ilişkili yedekleme İlkesi, Yedekleme işleminin ne sıklıkta ve ne zaman çalışması tanımlar. Varsayılan olarak, ilk zamanlanmış yedekleme ilk yedeklemedir. İlk yedekleme gerçekleştirilene kadar, **Yedekleme İşleri** dikey penceresindeki Son Yedekleme Durumu **Uyarı (ilk yedekleme bekleme)** olarak gösterilir.

![Yedekleme beklemede](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

İlk yedeklemeniz kısa süre içinde başlamazsa **Şimdi Yedekle** seçeneğini çalıştırmanız önerilir. Aşağıdaki yordam kasa panodan başlatır. Bu yordam, tüm önkoşullar tamamladıktan sonra ilk yedekleme işini çalıştırmak için görev yapar. İlk yedekleme işini zaten çalıştırıldıysa, bu yordam kullanılabilir değil. İlişkili yedekleme İlkesi bir sonraki yedekleme işi belirler.  

İlk yedekleme işini çalıştırmak için:

1. Kasa panosunda **Yedekleme Öğeleri** altındaki sayıya veya **Yedekleme Öğeleri** kutucuğuna tıklayın. <br/>
  ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  **Yedekleme Öğeleri** dikey penceresi açılır.

  ![Öğeleri yedekleme](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. **Yedekleme Öğeleri** dikey penceresinde öğeyi seçin.

  ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  **Yedekleme Öğeleri** listesi açılır. <br/>

  ![Tetiklenmiş yedekleme işi](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. **Yedekleme Öğeleri** listesinde üç noktaya **...** tıklayarak Bağlam menüsünü açın.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Bağlam menüsü görüntülenir.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Bağlam menüsünde **Şimdi yedekle**’ye tıklayın.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Şimdi Yedekle dikey penceresi açılır.

  ![Şimdi Yedekle dikey penceresi](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Şimdi Yedekle dikey penceresinde, takvim simgesine tıklayın, bu kurtarma noktasının korunduğu son günü seçmek için Takvim denetimlerini kullanın ve **Yedekle**’ye tıklayın.

  ![Şimdi Yedekle kurtarma noktasının korunduğu son günü ayarlayın](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

6. İlk yedekleme durumunu izlemek için kasa panosunda **Yedekleme İşleri** kutucuğunda **Devam eden**’e tıklayın.

  ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Yedekleme İşleri dikey penceresi açılır.

  ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  **Yedekleme işleri** dikey penceresinde tüm işlerin durumunu görebilirsiniz. VM’niz için yedekleme işinin devam edip etmediğini veya bitip bitmediğini kontrol edin. Yedekleme işi tamamlandığında, durum *Tamamlandı* olur.

  > [!NOTE]
  > Azure Backup hizmeti, yedekleme işleminin parçası olarak, her VM'deki yedekleme uzantısına tüm yazma işlemlerini boşaltmaya ve tutarlı bir anlık görüntü almaya yönelik bir komut verir.
  >
  >

## <a name="troubleshooting-errors"></a>Sorun giderme
Sanal makineniz yedekleme sırasında sorunları içine çalıştırırsanız bkz [VM sorun giderme makalesi](backup-azure-vms-troubleshoot.md) Yardım.

## <a name="next-steps"></a>Sonraki adımlar
VM korumalı, VM yönetim görevleri ve sanal makineleri geri yükleme hakkında bilgi edinmek için aşağıdaki makalelere bakın.

* [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
