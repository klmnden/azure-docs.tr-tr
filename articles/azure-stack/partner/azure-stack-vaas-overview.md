---
title: Azure Stack için hizmet olarak doğrulama genel bakış | Microsoft Docs
description: Bilinen sorunlar hizmet olarak Azure Stack doğrulama genel bakış.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 56251245a23df031f3bc8fe3d36de43e194fbcc7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159682"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Azure Stack için hizmet olarak doğrulama nedir?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama (VaaS) hizmet olarak Azure Stack'te teklifleri Microsoft ile ortak mühendislik çözüm iş ortakları için tasarlanan yerel bir Azure hizmetidir. Çözüm ortakları hizmeti çözümleri Microsoft'un gereksinimlerini ve Azure Stack ile beklendiği gibi çalışmayabilir denetlemek için kullanabilirsiniz.

Birincil VaaS kullanılır:

- Yeni Azure Stack çözümleri doğrula
- Azure Stack yazılım değişiklikleri doğrulama
- Dijital olarak imzalanmış çözüm iş ortağı dağıtımı sırasında kullanılan paketleri al
- Azure Stack doğrulama malzemeleri Önizleme

## <a name="validate-new-azure-stack-solution"></a>Yeni Azure Stack çözüm doğrula

İş ortakları, yeni Azure Stack çözümleri denetlemek için çözüm doğrulama iş akışı kullanın. Çözüm, gerekli donanım Laboratuvar Seti (HKL) Azure Stack testleri bileşen testleri geçmelidir. Yalnızca iş akışı her yeni bir çözüm için iki kez çalıştırmanız gerekir: minimum ve maksimum yapılandırma için bir kez.

Daha fazla bilgi için [yeni bir Azure Stack çözüm doğrulama](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Azure Stack yazılım değişiklikleri doğrulama

İş ortakları, çözüm en son Azure Stack yazılım güncelleştirmesiyle çalışıp çalışmadığını denetlemek için paket doğrulama iş akışı kullanın. En azından, paket doğrulama iş akışı Microsoft tarafından önerilen donanım ortamı ve bir çözüm üzerinde nereye (P & U) güncelleştirme ve düzeltme eki güncelleştirmeyi uygulamak için kullanılan çalıştırmanız gerekir. Paket doğrulaması ana hat derlemesi üzerinde çalıştırmanız gerekir.

Daha fazla bilgi için [yazılım güncelleştirmelerini Microsoft gelen doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Dijital olarak imzalanmış çözüm iş ortağı paketleri al

Azure Stack güncelleştirmeleri doğrulanıyor ek olarak, Azure Stack iş ortağı özel sürücüler, bellenim ve Azure Stack dağıtımı sırasında kullanılan diğer yazılım OEM özelleştirme paketleri için güncelleştirmeleri denetlemek için paket doğrulama iş akışı kullanabilirsiniz yazılım. Azure Stack yazılım, desteklenecek bir en az en düşük ölçekli çözüm kullanarak geçerli sürüm denetimi paket dağıtın. Güncelleştirilmiş paket test başlatılıyor hizmetine önce karşıya yüklenmelidir. Testler başarıyla tamamlanırsa bildir vaashelp@microsoft.com. Azure Stack iş ortağı paketi test tamamlandı ve Azure Stack dijital imza ile dijital olarak imzalanması gerektiğini anlatın. Microsoft, paketi imzalar ve Azure Stack iş ortağı paketi portalında indirilebilir olduğunu bildirir.

Daha fazla bilgi için [doğrulama OEM paketleri](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Azure Stack doğrulama malzemeleri Önizleme

Microsoft yeni özellikleri düzenli olarak Azure Stack'te kullanılabilir yapar. Bu özellikler pazara sunmaya yönelik geliştirme sürecinin bir parçası olarak yeni test malzemeleri test geçiş iş akışı içinde kullanılabilir hale getirilir. Test geçiş iş akışı için resmi olmayan test yürütmeye olanak tanımak için diğer iş akışlarını gelen test malzemeleri içerir. Test geçiş iş akışı, onay için sonuçları göndermek için kullanmayın. Çözümü doğrulama ve paket doğrulama iş akışı, çözümünüz için resmi onay almak için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanmaya başlayın ve [doğrulama bir hizmet hesabı olarak ayarlama](azure-stack-vaas-validate-solution-new.md)
