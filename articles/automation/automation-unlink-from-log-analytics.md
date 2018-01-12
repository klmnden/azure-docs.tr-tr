---
title: "Azure Otomasyon hesabı günlük analizi bağlantısını | Microsoft Docs"
description: "Bu makalede, Azure Automation hesabınız bir OMS çalışma alanından bağlantısının nasıl genel bakış sağlar."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 415efd54759f4ff87c53f31308450590dc71466b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Günlük analizi çalışma alanı penceresinden Otomasyon hesabınızı bağlantısının nasıl

Azure Otomasyonu kalmaz, runbook işleri tüm Automation hesapları arasında izlemeyi desteklemek için günlük analizi ile tümleştirilir, ancak günlük analizi bağımlı olan aşağıdaki çözümleri içeri aktardığınızda de gereklidir:

* [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md)
* [Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md)
* [Yoğun olmayan saatlerde sırasında Başlat/Durdur VM'ler](automation-solution-vm-management.md)
 
Otomasyon hesabı günlük analizi ile tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı bağlantısını kaldırabilirsiniz.  Devam etmeden önce ilk daha önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlem devam etmesini engellenir.  Kaldırmak için gerekli olan adımları anlamak için içeri aktardığınız belirli çözüm konusunu gözden geçirin.  

Bu çözümlerin kaldırdıktan sonra Automation hesabınız bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

## <a name="unlink-workspace"></a>Çalışma alanı bağlantısını Kaldır

1. Azure portalından Automation hesabınızı açın ve sayfa seç üzerinde Otomasyon hesabı **çalışma bağlantısını** bölümünün altında **ilgili kaynaklar** soldaki.<br><br> ![Çalışma alanı seçeneği bağlantısını Kaldır](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Bağlantıyı kaldır çalışma sayfasında, tıklatın **çalışma bağlantısını**.<br><br> ![Çalışma alanı sayfası bağlantısını](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.<br><br>
3. Azure Otomasyon hesabı günlük analizi çalışma alanınız bağlantısını dener, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

İsteğe bağlı güncelleştirme yönetimi çözümü kullandıysanız, çözüm kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamaları güncelleştirin.  Her oluşturduğunuz güncelleştirme dağıtımları eşleşen adlara sahip olacaktır)

* Çözüm için oluşturulan karma çalışan grupları.  Her benzer şekilde machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8 için olarak adlandırılır).

İsteğe bağlı olarak Başlat/Durdur VM'ler yoğun olmayan saatlerde çözüm sırasında kullandıysanız, çözüm kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Başlatma ve durdurma VM runbook zamanlama 
* Başlatma ve VM runbook'ları durdurun
* Değişkenler   

## <a name="next-steps"></a>Sonraki adımlar

Otomasyon hesabınızı OMS günlük analizi ile tümleştirmek üzere yapılandırmak için bkz: [Otomasyon iş durumu ve iş akışları günlük analizi (OMS) iletmek](automation-manage-send-joblogs-log-analytics.md). 