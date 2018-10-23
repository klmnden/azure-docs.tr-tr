---
title: Azure Stack için hizmet olarak doğrulama genel bakış | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulama genel bakış.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: fa2e85bb2b201fa8e83a03d24883a66a72910f11
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650188"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Azure Stack için hizmet olarak doğrulama nedir?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama (VaaS) hizmet olarak Azure Stack'te teklifleri Microsoft ile ortak mühendislik çözüm iş ortakları için tasarlanan yerel bir Azure hizmetidir. Çözüm ortakları hizmeti çözümleri Microsoft'un gereksinimlerini ve Azure Stack ile beklendiği gibi çalışmayabilir denetlemek için kullanabilirsiniz.

VaaS birincil kullanımları şunlardır:

- Yeni Azure Stack çözümleri doğrulanıyor
- Azure Stack yazılım değişiklikleri doğrulama
- Çözüm iş ortağı paketleri dağıtımı sırasında kullanılan dijital olarak imzalama
- Önizleme VaaS malzemeleri test

## <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözüm doğrula

İş ortakları, kullanım **çözüm doğrulama** yeni Azure Stack çözümleri doğrulamak için iş akışı. Çözüm, gerekli donanım Laboratuvar Seti (HLK) Azure Stack bileşen testleri geçmelidir. Donanım yapılandırmalarını bir dizi onaylamak için iş akışı her yeni çözüm için iki kez çalıştırılmalıdır: minimum ve maksimum yapılandırmaları için her bir kez.

Daha fazla bilgi için [yeni bir Azure Stack çözüm doğrulama](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Azure Stack yazılım değişiklikleri doğrulama

İş ortakları, kullanım **paket doğrulama** çözüm en son Azure Stack yazılım güncelleştirmeleriyle çalışıp çalışmadığını denetlemek için iş akışı. Paket doğrulama iş akışı burada düzeltme eki ve güncelleştirme (P & U) güncelleştirmeyi uygulamak için kullanılan bir Microsoft tarafından önerilen donanım ortamında çalıştırılmalıdır. Ayrıca iş akışı ana hat derlemesi üzerinde çalıştırmanız önerilir.

Daha fazla bilgi için [yazılım güncelleştirmelerini Microsoft gelen doğrulama](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Dijital olarak imzalanmış çözüm iş ortağı paketleri al

Azure Stack güncelleştirmeleri doğrulanıyor ek olarak, ortak kullanım **paket doğrulama** Azure Stack iş ortağı özel sürücüler, bellenim ve diğer yazılımları OEM özelleştirme paketleri için güncelleştirmeleri doğrulamak için iş akışı Azure Stack yazılım dağıtımı sırasında kullanılır. Azure Stack yazılımı desteklenecektir en küçük ölçekli çözüm en az kullanarak geçerli sürümünde doğrulamakta olduğunuz paket dağıtın. Paket, testleri çalıştırmadan önce VaaS için gönderilir. Testler başarıyla tamamlanırsa bildir [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) paketi test tamamlandı ve dijital olarak olmalıdır Azure Stack dijital imza ile imzalanmış. Microsoft, paketi imzalar ve Azure Stack iş ortağı paketi VaaS portalında indirilebilir olduğunu bildirir.

Daha fazla bilgi için [doğrulama OEM paketleri](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Önizleme VaaS malzemeleri test

Microsoft yeni özellikleri düzenli olarak Azure Stack'te kullanılabilir yapar. Bu özellikler pazara sunmaya yönelik geliştirme sürecinin bir parçası olarak yeni test malzemeleri içinde kullanılabilir hale getirileceğini **Test geçiş** iş akışı. Test geçiş iş akışı için resmi olmayan test yürütmeye olanak tanımak için diğer iş akışlarını gelen test malzemeleri içerir. Test geçiş iş akışı, onay için sonuçları göndermek için kullanmayın. Çözümünüz için resmi onay almak için çözüm doğrulama ve paket doğrulama iş akışları kullanın.

Daha fazla bilgi için [hızlı başlangıç: ilk test zamanlamak için bir servis portalı doğrulama kullanmak](azure-stack-vaas-schedule-test-pass.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet kaynakları olarak doğrulama ayarlayın](azure-stack-vaas-set-up-resources.md)
- Hakkında bilgi edinin [doğrulama olarak hizmet temel kavramları](azure-stack-vaas-key-concepts.md)
