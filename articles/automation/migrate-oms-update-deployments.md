---
title: OMS güncelleştirme dağıtımlarınızı Azure'a geçirme
description: Bu makalede, var olan bir OMS güncelleştirme dağıtımlarını Azure'a geçirme
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 07/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9469a5827765a9b82469ac1eedc66666231d82d6
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117451"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>OMS güncelleştirme dağıtımlarınızı Azure'a geçirme

Operations Management Suite (OMS) portalı olan [kullanım dışı](../log-analytics/log-analytics-oms-portal-transition.md). Azure portalında OMS portalında güncelleştirme yönetimi için kullanılabilir olan tüm işlevselliği kullanılabilir. Bu makalede, Azure portalına geçiş için ihtiyacınız olan bilgileri sağlar.

## <a name="key-information"></a>Anahtar bilgileri

* Var olan dağıtımlar çalışmaya devam eder. Azure'da dağıtımı yeniden oluşturulması sonra eski dağıtımınız OMS'den silebilirsiniz.
* OMS portalında olan tüm mevcut özellikleri, Azure'da bulunan, güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimine genel bakış](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Azure portalına erişim

OMS çalışma alanınızı tıklayın **azure'da açık**. Bu, kullanılan OMS Log Analytics çalışma alanına gider.

![Azure - OMS portalında Aç](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Azure portalında **Otomasyon hesabı**

![Log Analytics](media/migrate-oms-update-deployments/log-analytics.png)

Otomasyon hesabınızda tıklayın **güncelleştirme yönetimi** güncelleştirme yönetimini açın.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/azure-automation.png)

Gelecekte doğrudan Azure portalının altında gidebilirsiniz **tüm hizmetleri**seçin **Otomasyon hesapları** altında **Yönetim Araçları**, uygun Otomasyon seçin Hesap ve tıklayın **güncelleştirme yönetimi**.

## <a name="recreate-existing-deployments"></a>Var olan dağıtımları yeniden oluşturun

OMS portalında oluşturulan tüm güncelleştirme dağıtımları sahip bir [kayıtlı araması](../log-analytics/log-analytics-computer-groups.md) olarak da bilinen bir bilgisayar grubu, var olan güncelleştirme dağıtımının aynı ada sahip. Kayıtlı arama güncelleştirme dağıtımına dahil zamanlanan makinelerin listesini içerir.

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
|Güncelleştirilecek makineler |Güncelleştirilecek makineler için mevcut bir OMS dağıtım tarafından kullanılan kayıtlı arama seçin. |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin. CentOS desteklemiyor bu kullanıma hazır.|
|Hariç tutulacak güncelleştirmeler|Hariç tutulacak güncelleştirmeler girin. Windows için KB makalesi olmadan girin **KB** önek. Linux için paket adını girin veya bir joker karakterini kullanın.  |
|Zamanlama ayarları|Başlangıç saati seçin ve ardından ya da **kez** veya **yinelenen** yinelenme.|| Bakım penceresi |Güncelleştirmeler için dakika sayısı. Değer, 30 dakika ve 6 saatten az olamaz. |

Tıklayın **zamanlanan güncelleştirme dağıtımları** yeni oluşturulan güncelleştirme dağıtım durumunu görüntülemek için.

![yeni güncelleştirme dağıtımı](media/migrate-oms-update-deployments/new-update-deployment.png)

Azure portalı üzerinden yeni dağıtımlarınızı yapılandırıldıktan sonra daha önce belirtildiği gibi var olan dağıtımları OMS Portalı'ndan kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure ortamında güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimi](automation-update-management.md)