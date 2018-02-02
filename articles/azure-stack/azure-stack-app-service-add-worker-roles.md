---
title: "Uygulama Hizmetleri - Azure yığın çalışan rollerinde genişletme | Microsoft Docs"
description: "Azure yığın uygulama hizmetleri ölçeklendirme için ayrıntılı kılavuz"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: slinehan
editor: anwestg
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: anwestg
ms.openlocfilehash: a9be9011062f07d59842d417bf6761ec81c39275
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Azure yığın uygulama hizmeti: daha fazla altyapı veya çalışan rolleri Ekle
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*  

Bu belge, Azure yığın altyapı ve çalışan rolleri uygulama hizmeti ölçeklendirme hakkında yönergeler sağlar. Herhangi bir boyuttaki uygulamaları desteklemek için ek çalışan rolleri oluşturmak için adımları içerir.

> [!NOTE]
> Azure yığın ortamınıza en fazla 96 GB RAM yoksa ek kapasite ekleme ilgili sorunlar olabilir.

Varsayılan olarak, Azure yığın uygulama hizmeti ücretsiz ve paylaşılan çalışan katmanları destekler. Diğer çalışan katmanı eklemek için daha fazla çalışan rolleri eklemeniz gerekir.

Hangi Azure yığın yüklemede varsayılan uygulama hizmeti ile dağıtılan emin değilseniz, ek bilgileri gözden geçirebilirsiniz [genel bakış Azure yığın uygulama hizmeti](azure-stack-app-service-overview.md).

Azure uygulama hizmeti Azure yığında sanal makine ölçekleme kümeleri kullanarak tüm rolleri dağıtır ve bu nedenle bu iş yükünün ölçeklendirme özelliklerinden yararlanır. Bu nedenle, tüm ölçeklendirme çalışan katmanı uygulama Hizmet Yöneticisi yapılır

Uygulama hizmeti kaynak sağlayıcısı yönetici doğrudan içinde ek çalışanları ekleme

1. Azure yığın yönetim portalına Hizmet Yöneticisi olarak oturum açın.

2. Gözat **uygulama hizmetleri**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Tıklatın **rolleri**. Burada, dağıtılan tüm uygulama hizmeti rolleri dökümünü görürsünüz.

4. Ölçek ve ardından istediğiniz türü satırındaki sağ tıklayın **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Tıklatın **ölçeklendirme**, üzere ölçek ve ardından istediğiniz örneklerinin sayısını seçin **kaydetmek**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Azure yığın uygulama hizmeti şimdi ilave VM'ler eklemek, bunları yapılandırmanız, tüm gerekli yazılımları yüklemek ve bu işlem tamamlandıktan sonra bunları hazır olarak işaretlemek. Bu işlem yaklaşık 80 dakika sürebilir.

7. Çalışanlar görüntüleyerek yeni rolleri hazırlık ilerlemesini izleyebilirsiniz **rolleri** dikey.

Tam olarak dağıtılan ve hazır olduktan sonra çalışan kullanıcılar bunlara kendi iş yükü dağıtmak için kullanılabilir hale gelir. Aşağıdaki varsayılan olarak kullanılabilir birden çok fiyatlandırma katmanlarına örneği gösterir. Kullanılabilir hiçbir çalışanları belirli çalışan katmanı için varsa, karşılık gelen fiyatlandırma katmanını seçmesine izin seçeneği kullanılamaz.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Management'ı ölçeklendirmek için karşılık gelen VM ölçek kümesi ölçeklendirme ön uç veya yayımcı rolü ekleyin. Uygulama Hizmeti Yönetimi gelecek bir sürümde yoluyla bu rolleri ölçeği genişletme olanağı ekleyeceğiz.

Yönetim, ön uç veya yayımcı rollerini ölçeklendirmek için uygun rol türü seçme aynı adımları izleyin. Denetleyicileri ölçek kümeleri olarak dağıtılmaz ve bu nedenle iki tüm üretim dağıtımları için yükleme zamanında dağıtılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar

[Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)
