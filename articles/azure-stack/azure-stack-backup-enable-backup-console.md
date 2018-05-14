---
title: Yönetim Portalı'ndan Azure yığını için yedeklemeyi etkinleştirme | Microsoft Docs
description: Böylece bir hata olduğunda Azure yığın geri altyapı Yedekleme Hizmet Yönetim Portalı aracılığıyla etkinleştirin.
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
ms.date: 05/11/2018
ms.author: jeffgilb
ms.openlocfilehash: 0ef8247eba4605d3c8e5ef0992ce97bce989002e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yedekleme Azure yığını için Yönetim Portalı'ndan etkinleştir
Azure yığın yedeklemeleri oluşturabilmesi altyapı Yedekleme Hizmet Yönetim Portalı aracılığıyla etkinleştirin. Bulut kurtarma durumunda kullanarak ortamınızı geri yüklemek için bu yedeklemeler kullanabilirsiniz [geri dönülemez bir hataya](.\azure-stack-backup-recover-data.md). Bulut kurtarma amacı, Kurtarma tamamlandıktan sonra işleçler ve kullanıcıları portalına geri oturum sağlamaktır. Kullanıcıların, rol tabanlı erişim izinleri ve rolleri, özgün planları, teklifleri ve önceden tanımlanmış işlem, depolama ve ağ kotalar dahil olmak üzere geri aboneliği gerekir.

Ancak, altyapı yedekleme hizmeti, Iaas Vm'leri, ağ yapılandırmaları ve depolama hesapları, BLOB'lar, tablolar gibi depolama kaynaklarını yedeklemez ve böylelikle kullanıcıların bulut kurtarma işleminden sonra oturum açmayı tamamlanır benzeri, daha önce mevcut birini görmez kaynaklar. Platform (PaaS) hizmet olarak kaynakları ve veri Ayrıca hizmet tarafından yedeklenmez. 

Yöneticiler ve kullanıcılar yedekleme ve Iaas ve PaaS kaynaklarına altyapı yedekleme işlemleri ayrı olarak geri yükleme sorumludur. Iaas ve PaaS kaynaklarına yedekleme hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Sanal Makineler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Konsolu aracılığıyla yedekleme etkinleştirmeden önce yedekleme hizmeti yapılandırmanız gerekir. Yedekleme hizmeti PowerShell kullanarak yapılandırabilirsiniz. Daha fazla bilgi için bkz: [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Yedeklemeyi etkinleştirme

1. Azure yığın Yönetim Portalı'ndaki açmak [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Seçin **daha fazla hizmet** > **altyapı yedekleme**. Seçin **yapılandırma** içinde **altyapı yedekleme** dikey.

    ![Azure yığın - yedekleme denetleyicisi ayarları](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihaz üzerinde barındırılan bir dosya paylaşımına yol için bir Evrensel Adlandırma Kuralı (UNC) dizesi kullanın. Bir UNC dize paylaşılan dosyaları veya aygıt konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Olağanüstü durum sonra yedek verilerin kullanılabilirliğini sağlamak için aygıt ayrı bir konumda olmalıdır.
    > [!Note]  
    > Ortamınızı Kurumsal ortamınıza ad çözümlemesi Azure yığın altyapı ağdan destekliyorsa, IP yerine bir FQDN kullanabilirsiniz.
4. Tür **kullanıcıadı** etki alanı ve kullanıcı adı'nı kullanarak. Örneğin, `Contoso\administrator`.
5. Tür **parola** kullanıcı için.
5. Parolayı yeniden yazın **parolayı onayla**.
6. Önceden paylaşılan bir anahtar sağlamak **şifreleme anahtarı** kutusu. Yedekleme dosyaları bu anahtar kullanılarak şifrelenmiş. Bu anahtarı güvenli bir yerde sakladığınızdan emin olun. Bu anahtar ilk kez ayarlama veya anahtarı gelecekte döndürme sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla yönerge için komut dosyaları izleyin [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Seçin **Tamam** yedek denetleyicisi ayarlarınızı kaydetmek için.

Bir yedekleme yürütmek için Azure yığın araçları yükleyin ve ardından PowerShell cmdlet'ini çalıştırın gereksinim **başlangıç AzSBackup** Azure yığın yönetim düğümü üzerinde. Daha fazla bilgi için bkz: [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Sonraki adımlar

- Bir yedekleme işinin öğrenin. Bkz: [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).
- Yedekleme çalıştığını doğrulamak öğrenin. Bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md).
