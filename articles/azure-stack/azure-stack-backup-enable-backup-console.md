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
ms.date: 09/05/2018
ms.author: jeffgilb
ms.openlocfilehash: 1373e98b8edac81ebdb15aaf36d8bbfc910029fe
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44026194"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yönetim Portalı'ndan Azure Stack için yedeklemeyi etkinleştirme
Azure Stack yedekleme oluşturabilmesi altyapı Backup Hizmeti Yönetim Portalı aracılığıyla etkinleştirin. Bu yedeklemeler bulut kurtarma durumunda kullanarak ortamınızda geri yüklemek için kullanabileceğiniz [geri dönülemez bir arıza](.\azure-stack-backup-recover-data.md). Bulut kurtarma amacı, Kurtarma tamamlandıktan sonra operatörler ve kullanıcılar portalına geri dönüp oturum açabildiğinizden emin sağlamaktır. Kullanıcılar, rol tabanlı erişim izinleri ve rolleri, özgün planları, teklifleri ve önceden tanımlı bilgi işlem, depolama ve ağ kotaları dahil olmak üzere geri aboneliklerini sahip olacaktır.

Ancak, altyapı yedekleme hizmeti, Iaas Vm'leri, ağ yapılandırmaları ve depolama hesapları, BLOB'ları, tabloları gibi depolama kaynaklarını yedeklemez ve daha önce mevcut birini bulut kurtarma işleminden sonra oturum açan kullanıcıların tamamlandıktan şekilde vb. görmezsiniz kaynaklar. Platform (PaaS) hizmet olarak kaynakları ve veri Ayrıca hizmet tarafından yedeklenmez. 

Yöneticiler ve kullanıcılar için yedekleme ve Iaas ve PaaS kaynakları ayrı ayrı altyapısını yedekleme işlemleri geri sorumludur. Iaas ve PaaS kaynaklarına yedekleme hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Sanal Makineler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>Etkinleştirmek veya yedekleme yeniden yapılandırın

1. Açık [Azure Stack Yönetim Portalı](azure-stack-manage-portals.md).
2. Seçin **tüm hizmetleri**ve ardından altındaki **Yönetim** kategorisi seçin **altyapı yedeklemesine**. Seçin **yapılandırma** içinde **altyapı yedeklemesine** dikey penceresi.
3. Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihazda barındırılan bir dosya paylaşımı yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanın. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Yedekleme verilerinin kullanılabilirlik, olağanüstü bir durumla karşılaştığınızda emin olmak için cihaz ayrı bir konumda olmalıdır.

    > [!Note]  
    > Ortamınızı Azure Stack altyapısını ağdan ad çözümlemesi Kurumsal ortamınıza destekliyorsa, IP yerine bir FQDN kullanabilirsiniz.
    
4. Tür **kullanıcıadı** kullanıcı adı ve etki alanı dosyalarını okuma ve yazma için yeterli erişim ile kullanma. Örneğin, `Contoso\backupshareuser`.
5. Tür **parola** kullanıcı.
6. Parolayı yeniden yazın **parolayı onayla**.
7. **Sıklık saat içindeki** ne sıklıkta belirleyen yedekleme oluşturulur. 12 varsayılan değerdir. Zamanlayıcı, en fazla 12 ve en az 4 destekler. 
8. **Bekletme süresi (gün)** kaç güne kadar yedek bir dış konuma göre korunur belirler. Varsayılan değer 7'dir. Zamanlayıcı, en fazla 14 ve en az 2 destekler. Yedekleri saklama süresinden daha eski bir dış konumdan otomatik olarak silinir.

    > [!Note]  
    > Yedekleri saklama süresinden daha eski arşivlemek istiyorsanız, Zamanlayıcı yedeklerin de sileceğini önce dosyaları yedekleme emin olun. Yedekleme bekletme süresi azaltılırsa (örneğin 7 gün-5 gün), Zamanlayıcı yeni saklama süresinden daha eski olan tüm yedeklemeler silin. Bu değer güncelleştirmeden önce silinmiş yedeklemeleri Tamam olduğundan emin olun. 

9. Önceden paylaşılan bir anahtarı sağlayan **şifreleme anahtarı** kutusu. Yedekleme dosyaları, bu anahtarı kullanılarak şifrelenir. Bu anahtarı güvenli bir konuma depoladığınızdan emin olun. Bu anahtar ilk kez ayarlayın veya anahtarı gelecekte Döndür sonra anahtarı bu arabirimden görüntüleyemezsiniz. Anahtar oluşturmak için aşağıdaki Azure Stack PowerShell komutlarını çalıştırın:
    ```powershell
    New-AzsEncryptionKeyBase64
    ```
10. Seçin **Tamam** yedekleme denetleyicisi ayarlarınızı kaydetmek için.

    ![Azure Stack - yedekleme denetleyicisi ayarları](media\azure-stack-backup\backup-controller-settings.png)

## <a name="start-backup"></a>Yedeklemeyi Başlat
Bir yedekleme başlatmak için tıklayın **Şimdi Yedekle** isteğe bağlı yedekleme başlatmak için. İsteğe bağlı yedekleme sonraki zamanlanmış yedekleme için saat değiştirmez. Görev tamamlandıktan sonra ayarları onaylayın **Essentials**:

![Azure Stack - isteğe bağlı yedekleme](media\azure-stack-backup\scheduled-backup.png).

PowerShell cmdlet'ini de çalıştırabilirsiniz **başlangıç AzsBackup** Azure Stack yönetim bilgisayarınızda. Daha fazla bilgi için [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Etkinleştirmek veya devre dışı otomatik yedekleme
Yedeklemeler, otomatik yedeklemeyi etkinleştirdiğinizde zamanlanır. Sonraki zamanlama yedekleme zamanını kontrol edebilirsiniz **Essentials**. 

![Azure Stack - isteğe bağlı yedekleme](media\azure-stack-backup\on-demand-backup.png)

Sonraki zamanlanmış yedeklemeleri devre dışı bırakmanız gerekirse, tıklayarak **Otomatik yedeklemeleri devre dışı**. Devre dışı bırakma otomatik yedeklemeler, yapılandırılan yedekleme ayarları tutar ve yedekleme zamanlaması korur. Bu eylem, gelecekteki yedeklemeler atlamak için Zamanlayıcı yalnızca söyler. 

![Azure Stack - devre dışı bırakma zamanlanmış yedeklemeler](media\azure-stack-backup\disable-auto-backup.png)

Sonraki zamanlanmış yedeklemelerin içinde devre dışı olduğunu onaylayın **Essentials**:

![Azure Stack - yedeklemeleri devre dışı bırakıldı onaylayın](media\azure-stack-backup\confirm-disable.png)

Tıklayarak **etkinleştirmek otomatik yedeklemeler** zamanlanan saatte gelecekte yedeklemeleri başlatmak için Zamanlayıcı bildirmek için. 

![Azure Stack - enable zamanlanmış yedeklemeler](media\azure-stack-backup\enable-auto-backup.png)


> [!Note]  
> Altyapı yedekleme için 1807 güncelleştirmeden önce yapılandırdıysanız, otomatik yedeklemeler devre dışı bırakılır. Bu şekilde, Azure Stack tarafından başlatılan yedeklemeleri dış bir görev zamanlama altyapısı tarafından başlatılan bir yedekleme ile çakışmaz. Tüm dış Görev Zamanlayıcı'yı devre dışı bıraktıktan sonra tıklayarak **etkinleştirmek otomatik yedeklemeler**.


## <a name="next-steps"></a>Sonraki adımlar

- Bir yedekleme işlemi yapılmasını öğrenin. Bkz: [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md ).
- Yedekleme çalıştırdığını doğrulamak öğrenin. Bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md).
