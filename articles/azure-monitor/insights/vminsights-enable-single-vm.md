---
title: Azure İzleyici (Önizleme) VM'ler için değerlendirme için etkinleştirme | Microsoft Docs
description: Bu makalede, nasıl Azure İzleyici VM'ler için tek bir Azure sanal makine veya sanal makine ölçek kümesi Değerlendirme amacıyla olanak açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: 673f153b551e4b0c89a564c96d6bd9819ca26f5d
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524093"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Azure İzleyici (Önizleme) VM'ler için değerlendirme için etkinleştirin.

Azure İzleyici (Önizleme) Vm'lerinde Azure sanal makineleri veya tek bir sanal makine veya sanal makine ölçek kümesi az sayıda değerlendirmek için izlemeyi etkinleştirmek için kolay ve en doğrudan Azure portalından yaklaşımdır. Bu işlemin sonunda, başarıyla izlemesini bunlar başlamıştır ve performans veya kullanılabilirlik sorunları yaşıyorsanız öğrenin. 

Alma başlatıldı, gözden geçirdiğinizden emin olun önce [önkoşulları](vminsights-enable-overview.md) ve aboneliğiniz ve kaynak gereksinimlerini karşılamak doğrulayın.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Tek bir Azure VM için izlemeyi etkinleştir
Azure portalında Azure sanal makinenizin izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **sanal makineler**.

1. Listeden bir VM seçin.

1. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)**.

1. Üzerinde **Insights (Önizleme)** sayfasında **şimdi deneyin**.

    ![Bir VM için sanal makineler için Azure İzleyici etkinleştir](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Üzerinde **Azure İzleyici İçgörüler ekleme** sayfasında mevcut bir Log Analytics varsa, aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  

    Listenin varsayılan çalışma alanı ve sanal makine abonelikte dağıtılmış konumunu belirler. 

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md) desteklenen bölgelerden birinde listelenen [burada](vminsights-enable-overview.md#log-analytics).

İzleme etkinleştirdikten sonra sanal makine için sistem durumu ölçümleri görmeden önce yaklaşık 10 dakika sürebilir.

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Tek sanal makine ölçek kümesi için izlemeyi etkinleştir

Azure portalında Azure sanal makine ölçek kümenizi izleme etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **sanal makine ölçek kümeleri**.

3. Listeden bir sanal makine ölçek kümesi'ni seçin.

4. Üzerine sanal makine ölçek kümesi sayfası, buna **izleme** bölümünden **Insights (Önizleme)**.

5. Üzerinde **Insights (Önizleme)** sayfasında, aşağı açılan listeden seçin, kullanmak istediğiniz mevcut bir Log Analytics çalışma alanı varsa.

    Listenin varsayılan çalışma alanı ve sanal makine abonelikte dağıtılmış konumunu belirler. 

    ![Azure İzleyici VM'ler için bir sanal makine ölçek kümesi için etkinleştirin.](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md) desteklenen bölgelerden birinde listelenen [burada](vminsights-enable-overview.md#log-analytics).

İzleme etkinleştirdikten sonra bu ölçek kümesi için izleme verilerini görüntülemeden önce yaklaşık 10 dakika sürebilir.

>[!NOTE]
>Ölçek kümeniz için el ile yükseltme modeli kullanıyorsanız Kurulumu tamamlamak için örneklerini yükseltin gerekecektir.  Bu, altında örnekleri sayfasından yapılabilir **ayarları** bölümü.

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine veya sanal makine ölçek kümesi için izleme etkinleştirilir, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). Performans sorunlarını ve Vm'leri performansınızı ile genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).