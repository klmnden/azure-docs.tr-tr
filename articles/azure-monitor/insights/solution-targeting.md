---
title: Azure yönetim çözümlerine hedefleme | Microsoft Docs
description: Yönetim çözümlerini hedefleyen yönetim çözümleri belirli bir aracılar kümesi sınırlamanıza olanak sağlar.  Bu makalede bir kapsam yapılandırması oluşturma ve bunu bir çözüm uygulayabilirsiniz.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 53f28d29b9667bb885a5c3d0da8d926f756f3427
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682087"
---
# <a name="targeting-management-solutions-in-azure-preview"></a>(Önizleme) Azure yönetim çözümlerine hedefleme
Aboneliğiniz için bir yönetim çözümü eklediğinizde, Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracıları için varsayılan olarak otomatik olarak dağıtılır.  Maliyetlerinizi yönetin ve belirli bir aracılar kümesi için sınırlayarak bir çözüm için toplanan veri miktarını sınırlamak isteyebilirsiniz.  Bu makalede nasıl kullanılacağını **çözüm hedefleme** çözümlerinize bir kapsam uygulamanıza imkan sağlayan bir özelliği olan.

## <a name="how-to-target-a-solution"></a>Nasıl bir çözümü hedeflemek için
Aşağıdaki bölümlerde açıklandığı gibi bir çözüm hedefleme için üç adım vardır. 


### <a name="1-create-a-computer-group"></a>1. Bir bilgisayar grubu oluşturun
Bir kapsamda oluşturarak dahil etmek istediğiniz bilgisayarları belirttiğiniz bir [bilgisayar grubu](../../azure-monitor/platform/computer-groups.md) Log analytics'te.  Bilgisayar grubunu bir günlük arama tabanlı veya Active Directory veya WSUS grupları gibi diğer kaynaklardan içeri aktarılabilir. Olarak [aşağıda açıklanan](#solutions-and-agents-that-cant-be-targeted), doğrudan Log Analytics'e bağlı olan bilgisayarları kapsamda dahil edilir.

Bir veya daha fazla çözüm için uygulanabilir bir kapsam yapılandırmasında dahil sonra çalışma alanınızda oluşturduğunuz bilgisayar grubu olduğunda.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Kapsam yapılandırması oluşturma
 A **kapsam yapılandırması** bir veya daha fazla bilgisayar grupları içerir ve bir veya daha fazla çözüm için uygulanabilir. 
 
 Aşağıdaki işlemi kullanarak bir kapsam yapılandırması oluşturun.  

 1. Azure portalında gidin **Log Analytics** ve çalışma alanınızı seçin.
 2. Çalışma alanı altında özelliklerinde **çalışma alanı veri kaynakları** seçin **kapsam yapılandırmaları**.
 3. Tıklayın **Ekle** yeni bir kapsam yapılandırması oluşturmak için.
 4. Tür a **adı** kapsam yapılandırması için.
 5. Tıklayın **bilgisayar grupları Seç**.
 6. Oluşturduğunuz bilgisayar grubu ve isteğe bağlı olarak diğer gruplar yapılandırmasına eklemek için seçin.  **Seç**'e tıklayın.  
 6. Tıklayın **Tamam** kapsam yapılandırması oluşturmak için. 


 ### <a name="3-apply-the-scope-configuration-to-a-solution"></a>3. Kapsam yapılandırması bir çözüm için geçerlidir.
Ardından kapsam yapılandırması aldıktan sonra bunu için bir veya daha fazla çözüm uygulayabilirsiniz.  Sahip birden çok çözümü tek bir kapsam yapılandırma kullanılabilse de, her bir çözüm için yalnızca bir kapsam yapılandırması kullanabileceğinizi unutmayın.

Aşağıdaki işlemi kullanarak bir kapsam yapılandırması uygulanır.  

 1. Azure portalında gidin **Log Analytics** ve çalışma alanınızı seçin.
 2. Çalışma alanı özelliklerini seçin **çözümleri**.
 3. Kapsama istediğiniz çözümü tıklayın.
 4. Çözüm için özelliklerde **çalışma alanı veri kaynakları** seçin **çözüm hedefleme**.  Seçenek kullanılabilir değilse, ardından [Bu çözüm hedeflenemez](#solutions-and-agents-that-cant-be-targeted).
 5. Tıklayın **kapsam yapılandırması Ekle**.  Bu çözüm için uygulanan bir yapılandırma zaten varsa bu seçenek kullanılamaz.  Başka bir tane eklemeden önce mevcut yapılandırmayı kaldırmanız gerekir.
 6. Kapsam yapılandırmasına göre oluşturduğunuz tıklayın.
 7. İzleme **durumu** gösterildiğinden emin olmak için yapılandırmayı, **başarılı**.  Seçin ve yapılandırmayı sağındaki elips durumu hata gösterir, ardından **düzenleme kapsam yapılandırması** değişiklik yapma.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Çözümler ve hedeflenemez aracıları
Aracıları ve çözüm hedefleme ile kullanılamaz çözümleri ölçütlerini aşağıda verilmiştir.

- Çözüm hedefleme yalnızca aracılar için dağıtım çözümleri için geçerlidir.
- Çözüm hedefleme yalnızca Microsoft tarafından sağlanan çözümleri için geçerlidir.  Çözümleri uygulanmaz [kendiniz veya iş ortakları tarafından oluşturulan](solutions-creating.md).
- Yalnızca doğrudan Log Analytics'e bağlama aracıları filtreleyebilirsiniz.  Çözümler, bunlar bir kapsam yapılandırmasına dahil edilip edilmeyeceğini bağlı bir Operations Manager yönetim grubunun parçası olan tüm aracılara otomatik olarak dağıtır.

### <a name="exceptions"></a>Özel durumlar
Bunlar belirtilen ölçütlere uyan olsa bile çözüm hedefleme aşağıdaki çözümleri ile kullanılamaz.

- Aracı sistem durumu değerlendirmesi

## <a name="next-steps"></a>Sonraki adımlar
- Ortamınızda yükleme kullanılabilir çözümleri dahil olmak üzere yönetim çözümleri hakkında daha fazla bilgi [çalışma alanınıza eklemek Azure Log Analytics yönetim çözümleri](solutions.md).
- Bilgisayar grupları oluşturma hakkında daha fazla bilgi [bilgisayar grupları Log analytics'te günlük aramaları](../../azure-monitor/platform/computer-groups.md).
