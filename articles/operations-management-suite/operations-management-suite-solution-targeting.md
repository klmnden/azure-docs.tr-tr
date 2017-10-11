---
title: "Çözüm OMS hedefleme | Microsoft Docs"
description: "Çözüm hedefleme bulunan bir özelliktir Operations Management Suite (OMS) aracıları belirli bir dizi yönetim çözümleri sınırlamanıza olanak sağlar.  Bu makalede, bir kapsam yapılandırması oluşturma ve bir çözüm uygulama açıklar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: cb73a2d7ae57a5a11869259dbe913ae83ffb2b01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-solution-targeting-in-operations-management-suite-oms-to-scope-management-solutions-to-specific-agents-preview"></a>Kapsam yönetim çözümleri için belirli aracıları (Önizleme) için Operations Management Suite (OMS) çözümü hedefleme kullanın
Bir çözüm için OMS eklediğinizde, varsayılan olarak, günlük analizi çalışma alanına bağlı tüm Windows ve Linux aracıları için otomatik olarak dağıtılır.  Maliyetlerinizi yönetmek ve yönelik bir çözüm aracıların belirli bir dizi sınırlama tarafından toplanan veri miktarını sınırlamak isteyebilirsiniz.  Bu makalede nasıl kullanılacağını açıklar **çözüm hedefleme** kapsam çözümlerinizi uygulamanıza imkan sağlayan bir OMS özelliği olduğu.

## <a name="how-to-target-a-solution"></a>Hedef bir çözümü nasıl
Aşağıdaki bölümlerde açıklandığı gibi bir çözüm hedefleme için üç adım vardır.  OMS portalı ve Azure portal için farklı adımlar gerekeceğini unutmayın.


### <a name="1-create-a-computer-group"></a>1. Bir bilgisayar grubu oluşturun
Bir kapsamda oluşturarak dahil etmek istediğiniz bilgisayarları belirttiğiniz bir [bilgisayar grubu](../log-analytics/log-analytics-computer-groups.md) günlük analizi içinde.  Bilgisayar grubu günlük aramaya bağlı veya Active Directory veya WSUS grupları gibi diğer kaynaklardan alınamadı. Olarak [aşağıda açıklanan](#solutions-and-agents-that-cant-be-targeted), günlük analizi için doğrudan bağlanan bilgisayarlar kapsamda dahil edilir.

Bir kez bir veya daha fazla çözümleri uygulanabilir bir kapsam yapılandırmasında dahil sonra çalışma alanınızda, oluşturulan bilgisayar grubuna sahip.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Bir kapsam yapılandırması oluştur
 A **kapsam yapılandırması** bir veya daha fazla bilgisayar gruplarını içerir ve bir veya daha fazla çözümleri uygulanabilir. 
 
 Aşağıdaki işlemi kullanarak bir kapsam yapılandırmasını oluşturun.  

 1. Azure portalında gidin **günlük analizi** ve çalışma alanınızı seçin.
 2. Çalışma alanı altında özelliklerinde **çalışma veri kaynakları** seçin **kapsam yapılandırmaları**.
 3. Tıklatın **Ekle** yeni bir kapsam yapılandırması oluşturmak için.
 4. Tür a **adı** kapsam yapılandırması için.
 5. Tıklatın **bilgisayar gruplarını seçin**.
 6. Oluşturduğunuz bilgisayar grubu ve isteğe bağlı olarak diğer gruplar yapılandırmasına eklemek için seçin.  **Seç**'e tıklayın.  
 6. Tıklatın **Tamam** kapsam yapılandırması oluşturmak için. 


 ### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Kapsam yapılandırması bir çözüm için geçerlidir.
Daha sonra bir kapsam yapılandırması olduktan sonra onu bir veya daha fazla çözümleri uygulayabilirsiniz.  Tek bir kapsam yapılandırma ile birden çok çözümler kullanılabilse de, her bir çözüm için yalnızca bir kapsam yapılandırması kullanabilirsiniz.

Aşağıdaki işlemi kullanarak bir kapsam yapılandırması uygulanır.  

 1. Azure portalında gidin **günlük analizi** ve çalışma alanınızı seçin.
 2. Çalışma alanı özelliklerinde seçin **çözümleri**.
 3. Kapsama istediğiniz çözüm tıklayın.
 4. Çözümü özelliklerinde **çalışma veri kaynakları** seçin **çözüm hedefleme**.  Seçeneği kullanılabilir değilse, ardından [Bu çözüm hedefleyemez](#solutions-and-agents-that-cant-be-targeted).
 5. Tıklatın **Ekle kapsam yapılandırması**.  Bu çözüm için uygulanan bir yapılandırma zaten varsa bu seçenek kullanılamaz.  Başka bir tane eklemeden önce mevcut yapılandırmayı kaldırmanız gerekir.
 6. Kapsam yapılandırmasını, oluşturduğunuz'ı tıklatın.
 7. Gözcü **durum** gösterdiğine dikkat emin olmak için yapılandırmasının **başarılı**.  Bir hata durumu gösterir, seçin ve yapılandırma sağındaki elips'ı tıklatın **Düzen kapsam yapılandırması** değişiklik yapma.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Çözümler ve hedefleyemez aracıları
Aracılar ve çözüm hedeflemede kullanılamaz çözümleri ölçütlerini aşağıda verilmiştir.

- Çözüm hedefleme yalnızca aracılara dağıtmak çözümleri için geçerlidir.
- Çözüm hedefleme yalnızca Microsoft tarafından sağlanan çözümleri için geçerlidir.  Çözümleri uygulanmaz [kendiniz veya iş ortakları tarafından oluşturulan](operations-management-suite-solutions-creating.md).
- Yalnızca doğrudan günlük Analizi'ne bağlamak aracıları filtreleyebilirsiniz.  Çözümleri bir kapsam yapılandırmasında dahil edildiklerini olup olmadığına bağlı bir Operations Manager yönetim grubunun parçası olan tüm aracıları otomatik olarak dağıtır.

### <a name="exceptions"></a>Özel durumlar
Belirtilen ölçütlere uyan olsa da çözüm hedefleme aşağıdaki çözümleri ile kullanılamaz.

- Aracı sistem durumu değerlendirmesi

## <a name="next-steps"></a>Sonraki adımlar
- Ortamınıza yükleme kullanılabilir çözümleri dahil olmak üzere yönetim çözümleri hakkında daha fazla bilgi [çalışma alanınıza ekleyin Azure günlük analizi yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md).
- Bilgisayar gruplarının oluşturma hakkında daha fazla bilgi [günlük analizi bilgisayar gruplarında oturum aramaları](../log-analytics/log-analytics-computer-groups.md).