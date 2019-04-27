---
title: OMS güncelleştirme dağıtımlarınızı Azure'a geçirme
description: Bu makalede, var olan bir OMS güncelleştirme dağıtımlarını Azure'a geçirme
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 07/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4d11dfcb66a545cbecc80b6bdad558ca6d328ed2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499265"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>OMS güncelleştirme dağıtımlarınızı Azure'a geçirme

Operations Management Suite (OMS) portalı olan [kullanım dışı](../azure-monitor/platform/oms-portal-transition.md). Azure portalında OMS portalında güncelleştirme yönetimi için kullanılabilir olan tüm işlevselliği kullanılabilir. Bu makalede, Azure portalına geçiş için ihtiyacınız olan bilgileri sağlar.

## <a name="key-information"></a>Anahtar bilgileri

* Var olan dağıtımlar çalışmaya devam eder. Azure'da dağıtımı yeniden oluşturulması sonra eski dağıtımınız OMS'den silebilirsiniz.
* OMS portalında olan tüm mevcut özellikleri, Azure'da bulunan, güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimine genel bakış](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Azure portalına erişim

OMS çalışma alanınızı tıklayın **azure'da açık**. Bu, kullanılan OMS Log Analytics çalışma alanına gider.

![Azure - OMS portalında Aç](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Azure portalında **Otomasyon hesabı**

![Azure İzleyici günlükleri](media/migrate-oms-update-deployments/log-analytics.png)

Otomasyon hesabınızda tıklayın **güncelleştirme yönetimi** güncelleştirme yönetimini açın.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/azure-automation.png)

Gelecekte doğrudan Azure portalının altında gidebilirsiniz **tüm hizmetleri**seçin **Otomasyon hesapları** altında **Yönetim Araçları**, uygun Otomasyon seçin Hesap ve tıklayın **güncelleştirme yönetimi**.

## <a name="recreate-existing-deployments"></a>Var olan dağıtımları yeniden oluşturun

OMS portalında oluşturulan tüm güncelleştirme dağıtımları sahip bir [kayıtlı araması](../azure-monitor/platform/computer-groups.md) olarak da bilinen bir bilgisayar grubu, var olan güncelleştirme dağıtımının aynı ada sahip. Kayıtlı arama güncelleştirme dağıtımına dahil zamanlanan makinelerin listesini içerir.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/oms-deployment.png)

Var olan bu kayıtlı arama kullanmak için aşağıdaki adımları izleyin:

Dağıtım, Azure portalına gidin yeni bir güncelleştirme oluşturmak için kullanılır ve Otomasyon hesabı seçin **güncelleştirme yönetimi**. Tıklayın **güncelleştirme dağıtımı zamanla**.

![Güncelleştirme dağıtımını zamanla](media/migrate-oms-update-deployments/schedule-update-deployment.png)

**Yeni güncelleştirme dağıtımı** bölmesi açılır. Aşağıdaki tabloda açıklanan özellikler için değerleri girin ve ardından **Oluştur**:

Güncelleştirilecek makineler için mevcut bir OMS dağıtım tarafından kullanılan kayıtlı arama seçin.

| Özellik | Açıklama |
| --- | --- |
|Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| Seçin **Linux** veya **Windows**.|
|Güncelleştirilecek makineler |İçeri aktarılan grubu, kayıtlı bir aramayı seçin veya makine açılan listeden seçin ve tek bir makine seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure İzleyici günlüklerine bilgisayar grupları oluşturma farklı yöntemleri hakkında bilgi edinmek için bkz: [Azure İzleyici günlüklerine bilgisayar grupları](../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin. CentOS desteklemiyor bu kullanıma hazır.|
|Hariç tutulacak güncelleştirmeler|Hariç tutulacak güncelleştirmeler girin. Windows için KB makalesi olmadan girin **KB** önek. Linux için paket adını girin veya bir joker karakterini kullanın.  |
|Zamanlama ayarları|Başlangıç saati seçin ve ardından ya da **kez** veya **yinelenen** yinelenme. | 
| Bakım penceresi |Güncelleştirmeler için dakika sayısı. Değer, 30 dakika ve 6 saatten az olamaz. |
| Denetim yeniden başlatma| Yeniden başlatma işlemlerini nasıl işleneceğini belirler.</br>Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Tıklayın **zamanlanan güncelleştirme dağıtımları** yeni oluşturulan güncelleştirme dağıtım durumunu görüntülemek için.

![yeni güncelleştirme dağıtımı](media/migrate-oms-update-deployments/new-update-deployment.png)

Azure portalı üzerinden yeni dağıtımlarınızı yapılandırıldıktan sonra daha önce belirtildiği gibi var olan dağıtımları OMS Portalı'ndan kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure ortamında güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimi](automation-update-management.md)
