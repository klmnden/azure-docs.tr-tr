---
title: Yedekleme ve veri kurtarma altyapı Backup hizmeti ile Azure yığınının | Microsoft Docs
description: Yedekleme ve yapılandırma ve hizmet verileri altyapı Yedekleme hizmetini kullanarak geri yükleyin.
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
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 8c8037fe3936485082299250e603b2f3ea3859b9
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Azure yığın altyapı Backup hizmeti ile yedekleme ve veri kurtarma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Yedekleme ve yapılandırma ve hizmet verileri altyapı Yedekleme hizmetini kullanarak geri yükleyin. Her Azure yığın yükleme Hizmeti'nin bir örneğini içerir. Yeniden dağıtım Azure yığın bulut hizmeti tarafından oluşturulan yedekleri, kimlik, güvenlik ve Azure Resource Manager verileri geri yüklemek için kullanabilirsiniz.

Bulut üretime sokmak hazır olduğunuzda, yedekleme etkinleştirebilirsiniz. Testleri gerçekleştirme planlıyorsanız, yedekleme ve uzun bir süre için doğrulama etkinleştirmeyin.

Yedekleme hizmeti etkinleştirmeden önce olduğundan emin olun [yerinde gereksinimleri](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> Kullanıcı verileri ve uygulamaları altyapı Backup hizmeti dahil değildir. Yedekleme hakkında yönergeler için aşağıdaki makalelere bakın ve geri yükleme [uygulama hizmetleri](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql), ve [MySQL](https://aka.ms/azure-stack-mysql) kaynak sağlayıcıları ve ilişkili kullanıcı verilerini...

## <a name="the-infrastructure-backup-service"></a>Altyapı yedekleme hizmeti

Hizmetleri aşağıdaki özellikleri içerir.

| Özellik                                            | Açıklama                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme Altyapısı Hizmetleri                     | Bir alt kümesini Azure yığınında altyapı hizmetleri arasında yedekleme koordinatı. Bir olağanüstü durum varsa, veri dağıtımın bir parçası olarak geri yüklenebilir. |
| Sıkıştırma ve dışarı aktarılan yedekleme verileri şifreleme | Yedekleme verilerini sıkıştırılmış ve yönetici tarafından sağlanan harici depolama konumuna dışa önce sistem tarafından şifrelenir.                |
| Yedekleme işi izleme                              | Yedekleme işleri başarısız olur ve düzeltme adımları sistem bildirir.                                                                                                |
| Yedekleme yönetim deneyimi                       | Yedekleme RP etkinleştirme yedeklemeyi destekler.                                                                                                                         |
| Bulut kurtarma                                     | Geri dönülemez veri kaybı ise, yedeklemeler çekirdek Azure yığın bilgileri dağıtımının bir parçası olarak geri yüklemek için kullanılabilir.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Yedekleme hizmet Altyapı gereksinimlerini doğrulayın

- **Depolama konumu**  
  Bir dosya paylaşımı erişilebilir Azure yığından yedi yedeklemeleri içerebilir gerekir. Her yedekleme yaklaşık 10 GB'tır. Paylaşımınıza 140 GB yedeklemelerini depolamak gerekir. Azure yığın altyapı yedekleme hizmeti için bir depolama konumu seçme hakkında daha fazla bilgi için bkz: [yedekleme denetleyicisi gereksinimlerinin](azure-stack-backup-reference.md#backup-controller-requirements).
- **Kimlik Bilgileri**  
  Bir etki alanı kullanıcı hesabı ve kimlik bilgileri gerekir, örneğin, Azure yığın yönetici kimlik bilgilerini kullanabilir.
- **Şifreleme anahtarı**  
  Yedekleme dosyaları bu anahtar kullanılarak şifrelenmiş. Bu anahtarı güvenli bir yerde sakladığınızdan emin olun. Bu anahtar ilk kez ayarlama veya anahtarı gelecekte döndürme sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla yönerge için komut dosyaları izleyin [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](http://azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure yığınının Yönetim Portalı'ndan yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-console.md).
- Bilgi edinmek için nasıl [yedekleme etkinleştirmek için PowerShell ile Azure yığın](azure-stack-backup-enable-backup-powershell.md).
- Bilgi edinmek için nasıl [Azure yığın yedekleyin](azure-stack-backup-back-up-azure-stack.md )
- Bilgi edinmek için nasıl [geri dönülemez veri kaybına karşı Kurtar](azure-stack-backup-recover-data.md)
