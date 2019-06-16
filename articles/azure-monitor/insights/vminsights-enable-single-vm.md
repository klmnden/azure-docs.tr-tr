---
title: Azure İzleyici (Önizleme) VM'ler için değerlendirme için etkinleştirme | Microsoft Docs
description: Tek bir Azure sanal makine veya sanal makine ölçek kümesi sanal makineleri için Azure İzleyici değerlendirilecek öğrenin.
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
ms.openlocfilehash: ec909bcd16f923bbd7036f6a69df2bbb07e561b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122473"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Azure İzleyici (Önizleme) VM'ler için değerlendirme için etkinleştirin.

Azure İzleyici (Önizleme) VM'ler için az sayıda Azure sanal makineleri (VM) üzerinde değerlendirmek veya üzerinde tek bir sanal makine veya sanal makine ölçek kümesi. İzlemeyi etkinleştirmek için en kolay ve en doğrudan Azure portalından yoludur. Vm'lerinizi izleme ve herhangi bir performans veya kullanılabilirlik sorunları bulmak için amacınız anlamaktır. 

Başlamadan önce gözden [önkoşulları](vminsights-enable-overview.md) kaynak gereksinimlerini karşılamak ve aboneliğinizi emin olun.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Tek bir Azure VM için izlemeyi etkinleştir
Azure VM izlemeyi etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **sanal makineler**.

1. Listeden bir VM seçin.

1. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)** .

1. Üzerinde **Insights (Önizleme)** sayfasında **şimdi deneyin**.

    ![Bir VM için sanal makineler için Azure İzleyici etkinleştir](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Üzerinde **Azure İzleyici İçgörüler ekleme** sayfasında mevcut bir Log Analytics varsa, aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  

    Listenin varsayılan çalışma alanı ve VM aboneliğinde dağıtıldığı konum belirler. 

    >[!NOTE]
    >VM izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak için bkz [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). Log Analytics çalışma alanınızın birine ait olmalıdır [desteklenen bölgeler](vminsights-enable-overview.md#log-analytics).

İzleme etkinleştirdikten sonra sanal makine için sistem durumu ölçümleri görmeden önce yaklaşık 10 dakika beklemeniz gerekebilir.

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Tek sanal makine ölçek kümesi için izlemeyi etkinleştir

Azure sanal makine ölçek kümesini izlemeyi etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **sanal makine ölçek kümeleri**.

3. Listeden bir sanal makine ölçek kümesi'ni seçin.

4. Üzerine sanal makine ölçek kümesi sayfası, buna **izleme** bölümünden **Insights (Önizleme)** .

5. Üzerinde **Insights (Önizleme)** sayfasında mevcut bir Log Analytics çalışma alanı kullanmak istiyorsanız aşağı açılan listeden seçin.

    Listenin varsayılan çalışma alanı ve VM abonelikte dağıtılmış konumunu belirler. 

    ![Azure İzleyici VM'ler için bir sanal makine ölçek kümesi için etkinleştirin.](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >Sanal makine ölçek kümesi izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak için bkz [Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md). Log Analytics çalışma alanınızın birine ait olmalıdır [desteklenen bölgeler](vminsights-enable-overview.md#log-analytics).

İzleme etkinleştirdikten sonra ölçek kümesi için izleme verilerini görüntülemeden önce yaklaşık 10 dakika beklemeniz gerekebilir.

>[!NOTE]
>Ölçek kümeniz için el ile yükseltme modeli kullandığınız, Kurulumu tamamlamak için örneklerini yükseltin. Yükseltmelerinin başlayabilirsiniz **örnekleri** sayfasında **ayarları** bölümü.

![Dağıtım işlemi izlemeyi VM'ler için Azure İzleyicisi'ni etkinleştirme](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

Sanal makine veya sanal makine ölçek kümesi için izleme etkinleştirdikten sonra izleme bilgileri analiz VM'ler için Azure İzleyici'de kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Sistem durumu özelliği kullanmayı öğrenmek için bkz: [Vm'leri Azure izleme durumunu anlamak](vminsights-health.md). 
* Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [kullanımı Azure İzleyici Vm'leri harita](vminsights-maps.md). 
* Performans sorunlarını, genel kullanımı ve performansını belirlemek için bkz. [görünümü Azure VM performansını](vminsights-performance.md).