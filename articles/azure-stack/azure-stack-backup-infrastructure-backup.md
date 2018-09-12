---
title: Azure Stack altyapısını yedekleme hizmetiyle için yedekleme ve veri kurtarma | Microsoft Docs
description: Yedekleme ve yapılandırma ve hizmet veri altyapısını Yedekleme hizmetini kullanarak geri yükleyebilirsiniz.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9B51A3FB-EEFC-4CD8-84A8-38C52CFAD2E4
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/10/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 9f2668ff84ade4ba99b7aa7dcd67feafadc1c6c4
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377845"
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Azure Stack altyapısını yedekleme hizmetiyle için yedekleme ve veri kurtarma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Yedekleme ve yapılandırma ve hizmet veri altyapısını Yedekleme hizmetini kullanarak geri yükleyebilirsiniz. Her Azure Stack yükleme Hizmeti'nin bir örneğini içerir. Azure Stack Bulutu, yeniden dağıtım için hizmeti tarafından oluşturulan yedekleri, kimlik, güvenlik ve Azure Resource Manager verileri geri yüklemek için kullanabilirsiniz.

Bulut üretime koymak hazır olduğunuzda yedeklemeyi etkinleştirebilirsiniz. Test gerçekleştirmek planlıyorsanız, yedekleme ve uzun bir süre için doğrulama etkinleştirmeyin.

Yedekleme hizmetinizi etkinleştirmeden önce olduğundan emin olun [gereksinimleri yerinde](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> Yedekleme hizmet olarak altyapı, kullanıcı verileri ve uygulamaları içermez. Yedekleme yönergeleri için aşağıdaki makalelere bakın ve geri yükleme [uygulama hizmetleri](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql), ve [MySQL](https://aka.ms/azure-stack-mysql) kaynak sağlayıcıları ve ilişkili kullanıcı verileri...

## <a name="the-infrastructure-backup-service"></a>Altyapı yedekleme hizmeti

Hizmetler aşağıdaki özellikleri içerir.

| Özellik                                            | Açıklama                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme Altyapısı Hizmetleri                     | Backup, Azure Stack altyapı hizmetlerinde bir alt kümesi arasında koordine edin. Olağanüstü bir durum varsa, verileri yeniden dağıtım işleminin bir parçası olarak geri yüklenebilir. |
| Sıkıştırma ve dışarı aktarılan yedekleme verileri şifreleme | Yedekleme verilerini sıkıştırılır ve yönetici tarafından sağlanan dış depolama konumuna aktarılır önce sistem tarafından şifrelenir.                |
| Yedekleme işini izleme                              | Yedekleme işleri başarısız olur ve düzeltme adımları, sistem size bildirir.                                                                                                |
| Yedekleme yönetimi deneyimi                       | Yedekleme RP yedeklemeyi etkinleştirme olanağı destekler.                                                                                                                         |
| Bulut kurtarma                                     | Geri dönülemez veri kaybı varsa, yedekleme, temel Azure Stack bilgi dağıtımının parçası olarak geri yüklemek için kullanılabilir.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Yedekleme hizmet olarak Altyapı gereksinimlerini doğrulayın

- **Depolama konumu**  
  Bir dosya paylaşımı erişilebilir yedi yedek içeren bir Azure Stack gerekir. Her yedekleme yaklaşık 10 GB'dir. Paylaşımınızın 140 GB'lık yedeklemeleri depolamak gerekir. Azure Stack altyapısını yedekleme hizmeti için bir depolama konumu seçme hakkında daha fazla bilgi için bkz. [yedeği denetleyicisi gereksinimleri](azure-stack-backup-reference.md#backup-controller-requirements).
- **Kimlik Bilgileri**  
  Bir etki alanı kullanıcı hesabı ve kimlik bilgileri gerekir, örneğin, Azure Stack yönetici kimlik bilgilerini kullanabilir.
- **Şifreleme anahtarı**  
  Yedekleme dosyaları, bu anahtarı kullanılarak şifrelenir. Bu anahtarı güvenli bir konuma depoladığınızdan emin olun. Bu anahtar ilk kez ayarlayın veya anahtarı gelecekte Döndür sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla bilgi için komut dosyalarını izleyin [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Yönetim Portalı'ndan Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-console.md).
- Bilgi edinmek için nasıl [PowerShell ile Azure Stack için yedekleme etkinleştir](azure-stack-backup-enable-backup-powershell.md).
- Bilgi edinmek için nasıl [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md )
- Bilgi edinmek için nasıl [geri dönülemez veri kaybından kurtarma](azure-stack-backup-recover-data.md)
