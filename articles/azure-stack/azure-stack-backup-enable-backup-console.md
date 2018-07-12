---
title: Yönetim Portalı'ndan Azure Stack için yedeklemeyi etkinleştirme | Microsoft Docs
description: Yönetim Portalı aracılığıyla hizmet altyapı yedeklemeyi etkinleştirin; böylelikle bir hata varsa, Azure Stack geri yüklenebilir.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeffgilb
ms.openlocfilehash: fba04490aca4c7123ca478ae07a5f0c865d9a826
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968706"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yönetim Portalı'ndan Azure Stack için yedeklemeyi etkinleştirme
Azure Stack yedekleme oluşturabilmesi altyapı Backup Hizmeti Yönetim Portalı aracılığıyla etkinleştirin. Bu yedeklemeler bulut kurtarma durumunda kullanarak ortamınızda geri yüklemek için kullanabileceğiniz [geri dönülemez bir arıza](.\azure-stack-backup-recover-data.md). Bulut kurtarma amacı, Kurtarma tamamlandıktan sonra operatörler ve kullanıcılar portalına geri dönüp oturum açabildiğinizden emin sağlamaktır. Kullanıcılar, rol tabanlı erişim izinleri ve rolleri, özgün planları, teklifleri ve önceden tanımlı bilgi işlem, depolama ve ağ kotaları dahil olmak üzere geri aboneliklerini sahip olacaktır.

Ancak, altyapı yedekleme hizmeti, Iaas Vm'leri, ağ yapılandırmaları ve depolama hesapları, BLOB'ları, tabloları gibi depolama kaynaklarını yedeklemez ve daha önce mevcut birini bulut kurtarma işleminden sonra oturum açan kullanıcıların tamamlandıktan şekilde vb. görmezsiniz kaynaklar. Platform (PaaS) hizmet olarak kaynakları ve veri Ayrıca hizmet tarafından yedeklenmez. 

Yöneticiler ve kullanıcılar için yedekleme ve Iaas ve PaaS kaynakları ayrı ayrı altyapısını yedekleme işlemleri geri sorumludur. Iaas ve PaaS kaynaklarına yedekleme hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Sanal Makineler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Yedekleme Konsolu aracılığıyla etkinleştirmeden önce yedekleme hizmetini yapılandırmanız gerekir. PowerShell kullanarak backup hizmeti yapılandırabilirsiniz. Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Yedeklemeyi etkinleştir

1. Azure Stack Yönetim Portalı'ndaki açın [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Seçin **diğer hizmetler** > **altyapı yedeklemesine**. Seçin **yapılandırma** içinde **altyapı yedeklemesine** dikey penceresi.

    ![Azure Stack - yedekleme denetleyicisi ayarları](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihazda barındırılan bir dosya paylaşımı yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanın. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Yedekleme verilerinin kullanılabilirlik, olağanüstü bir durumla karşılaştığınızda emin olmak için cihaz ayrı bir konumda olmalıdır.
    > [!Note]  
    > Ortamınızı Azure Stack altyapısını ağdan ad çözümlemesi Kurumsal ortamınıza destekliyorsa, IP yerine bir FQDN kullanabilirsiniz.
4. Tür **kullanıcıadı** kullanıcı adı ve etki alanı dosyalarını okuma ve yazma için yeterli erişim ile kullanma. Örneğin, `Contoso\backupshareuser`.
5. Tür **parola** kullanıcı.
5. Parolayı yeniden yazın **parolayı onayla**.
6. Önceden paylaşılan bir anahtarı sağlayan **şifreleme anahtarı** kutusu. Yedekleme dosyaları, bu anahtarı kullanılarak şifrelenir. Bu anahtarı güvenli bir konuma depoladığınızdan emin olun. Bu anahtar ilk kez ayarlayın veya anahtarı gelecekte Döndür sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla bilgi için komut dosyalarını izleyin [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).
7. Seçin **Tamam** yedekleme denetleyicisi ayarlarınızı kaydetmek için.

Bir yedekleme yürütmek için Azure Stack Araçları'nı indirin ve ardından PowerShell cmdlet'ini çalıştırmak gereken **başlangıç AzSBackup** , Azure Stack yönetim düğümünde. Daha fazla bilgi için [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Sonraki adımlar

- Bir yedekleme işlemi yapılmasını öğrenin. Bkz: [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md ).
- Yedekleme çalıştırdığını doğrulamak öğrenin. Bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md).
