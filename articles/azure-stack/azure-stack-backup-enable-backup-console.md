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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 1585eb460cc5f8ae437ee59a596dc7a854a108e7
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995739"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yönetim Portalı'ndan Azure Stack için yedeklemeyi etkinleştirme
Azure Stack altyapısını yedekleme oluşturabilmesi altyapı Backup Hizmeti Yönetim Portalı aracılığıyla etkinleştirin. Donanım iş ortağı bulut kurtarma durumunda kullanarak ortamınızda geri yüklemek için bu yedeklemeler kullanabilirsiniz [geri dönülemez bir arıza](./azure-stack-backup-recover-data.md). Bulut kurtarma amacı, Kurtarma tamamlandıktan sonra operatörler ve kullanıcılar portalına geri dönüp oturum açabildiğinizden emin sağlamaktır. Kullanıcılar, rol tabanlı erişim izinleri ve rolleri, özgün planları, teklifleri ve önceden tanımlı bilgi işlem, depolama, ağ kotalarının ve Key Vault gizli dizileri de dahil olmak üzere geri aboneliklerini sahip olacaktır.

Ancak, Yedekleme hizmet olarak altyapı değil Iaas Vm'lerini yedekleme, ağ yapılandırmalarını ve depolama hesapları, BLOB'ları gibi depolama kaynaklarını tablolar vb., bulut Kurtarma tamamlandıktan sonra oturum açan kullanıcıların daha önce mevcut birini görmezsiniz şekilde kaynaklar. Platform (PaaS) hizmet olarak kaynakları ve veri Ayrıca hizmet tarafından yedeklenmez. 

Yöneticiler ve kullanıcılar için yedekleme ve Iaas ve PaaS kaynakları ayrı ayrı altyapısını yedekleme işlemleri geri sorumludur. Iaas ve PaaS kaynaklarına yedekleme hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Sanal Makineler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [App Service](https://docs.microsoft.com/azure/app-service/manage-backup)
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

9. Şifreleme ayarları, sertifika .cer dosyasını box'taki bir sertifika sağlayın. Yedekleme dosyalarının bu ortak anahtar sertifika kullanılarak şifrelenir. Yedekleme ayarlarını yapılandırdığınızda, yalnızca ortak anahtar kısmını içeren bir sertifika sağlamanız gerekir. Bu sertifika için ilk kez ayarladığınız veya gelecekte sertifikayı döndürme sonra sertifikanın parmak izini yalnızca görüntüleyebilir. İndirin veya karşıya yüklenen sertifika dosyasını görüntüleyin. Sertifika dosyası oluşturmak için otomatik olarak imzalanan bir sertifika ile genel ve özel anahtarları oluşturmak ve yalnızca ortak anahtar kısmını sahip bir sertifika vermek için aşağıdaki PowerShell komutunu çalıştırın.

    ```powershell

        $cert = New-SelfSignedCertificate `
            -DnsName "www.contoso.com" `
            -CertStoreLocation "cert:\LocalMachine\My"

        New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 
        Export-Certificate `
            -Cert $cert `
            -FilePath c:\certs\AzSIBCCert.cer 
    ```

    > [!Note]  
    > **1901 ve yukarıdaki**: Azure Stack altyapısını yedekleme verilerini şifrelemek için bir sertifika kabul eder. Sertifika ortak ve özel anahtarı güvenli bir yerde depoladığınızdan emin olun. Güvenlik nedenleriyle, Yedekleme ayarlarını yapılandırmak için genel ve özel anahtarları sertifika kullanmanız önerilmez. Bu sertifika yaşam döngüsü yönetme hakkında daha fazla bilgi için bkz. [altyapı Backup hizmeti en iyi](azure-stack-backup-best-practices.md).

10. Seçin **Tamam** yedekleme denetleyicisi ayarlarınızı kaydetmek için.

![Azure Stack - yedekleme denetleyicisi ayarları](media/azure-stack-backup/backup-controller-settings-certificate.png)

## <a name="start-backup"></a>Yedeklemeyi Başlat
Bir yedekleme başlatmak için tıklayın **Şimdi Yedekle** isteğe bağlı yedekleme başlatmak için. İsteğe bağlı yedekleme sonraki zamanlanmış yedekleme için saat değiştirmez. Görev tamamlandıktan sonra ayarları onaylayın **Essentials**:

![Azure Stack - isteğe bağlı yedekleme](media/azure-stack-backup/scheduled-backup.png)

PowerShell cmdlet'ini de çalıştırabilirsiniz **başlangıç AzsBackup** Azure Stack yönetim bilgisayarınızda. Daha fazla bilgi için [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Etkinleştirmek veya devre dışı otomatik yedekleme
Yedeklemeler, otomatik yedeklemeyi etkinleştirdiğinizde zamanlanır. Sonraki zamanlama yedekleme zamanını kontrol edebilirsiniz **Essentials**. 

![Azure Stack - isteğe bağlı yedekleme](media/azure-stack-backup/on-demand-backup.png)

Sonraki zamanlanmış yedeklemeleri devre dışı bırakmanız gerekirse, tıklayarak **Otomatik yedeklemeleri devre dışı**. Devre dışı bırakma otomatik yedeklemeler, yapılandırılan yedekleme ayarları tutar ve yedekleme zamanlaması korur. Bu eylem, gelecekteki yedeklemeler atlamak için Zamanlayıcı yalnızca söyler. 

![Azure Stack - devre dışı bırakma zamanlanmış yedeklemeler](media/azure-stack-backup/disable-auto-backup.png)

Sonraki zamanlanmış yedeklemelerin içinde devre dışı olduğunu onaylayın **Essentials**:

![Azure Stack - yedeklemeleri devre dışı bırakıldı onaylayın](media/azure-stack-backup/confirm-disable.png)

Tıklayarak **etkinleştirmek otomatik yedeklemeler** zamanlanan saatte gelecekte yedeklemeleri başlatmak için Zamanlayıcı bildirmek için. 

![Azure Stack - enable zamanlanmış yedeklemeler](media/azure-stack-backup/enable-auto-backup.png)


> [!Note]  
> Altyapı yedekleme için 1807 güncelleştirmeden önce yapılandırdıysanız, otomatik yedeklemeler devre dışı bırakılır. Bu şekilde, Azure Stack tarafından başlatılan yedeklemeleri dış bir görev zamanlama altyapısı tarafından başlatılan bir yedekleme ile çakışmaz. Tüm dış Görev Zamanlayıcı'yı devre dışı bıraktıktan sonra tıklayarak **etkinleştirmek otomatik yedeklemeler**.

## <a name="update-backup-settings"></a>Yedekleme ayarlarını güncelleştirme
Şifreleme anahtarı kullanım dışıdır 1901 itibarıyla desteği. Yedekleme 1901 ilk kez yapılandırıyorsanız, bir sertifika kullanmanız gerekir. Azure Stack, yalnızca anahtar için 1901 güncelleştirmeden önce yapılandırılmışsa şifreleme anahtarını destekler. Geriye dönük uyumluluk modu için üç yayınları devam eder. Bundan sonra artık şifreleme anahtarları desteklenecektir. 

### <a name="default-mode"></a>Varsayılan mod
Şifreleme ayarları, altyapı yedeklemesine yüklerken veya güncellerken 1901 için sonra ilk kez yapılandırırken bir sertifika ile yedekleme yapılandırmalısınız. Bir şifreleme anahtarı kullanarak artık desteklenmiyor. 

Yedekleme verilerini şifrelemek için kullanılan bir sertifikayı güncelleştirmek için yeni bir karşıya yükleyebilirsiniz. CER ortak anahtar kısmını ile dosya ve ayarları kaydetmek için Tamam'ı seçin. 

Yeni yedeklemeleri yeni sertifikanın ortak anahtarı kullanmaya başlar. Önceki sertifikayla oluşturulan tüm mevcut yedeklemeler için herhangi bir etkisi yoktur. Bulut kurtarma için gerektiğinde geçici olarak eski sertifikayı güvenli bir yerde sakladığınızdan emin olun.

![Azure Stack - görünüm sertifika parmak izi](media/azure-stack-backup/encryption-settings-thumbprint.png)

### <a name="backwards-compatibility-mode"></a>Geriye dönük uyumluluk modu
Yedekleme için 1901 güncelleştirmeden önce yapılandırdıysanız, ayarları davranışında değişiklik birlikte taşınır. Bu durumda, şifreleme anahtarını geri desteklenen uyumluluk. Bir sertifika kullanmak için şifreleme anahtarını güncelleştirmek veya değiştirme seçeneğiniz vardır. Şifreleme anahtarını güncelleştirmeye devam etmek için üç sürümleri gerekir. Bu zaman geçiş için bir sertifika kullanın. 

![Azure Stack - geriye dönük uyumluluk modunda şifreleme anahtarı kullanma](media/azure-stack-backup/encryption-settings-backcompat-encryption-key.png)

> [!Note]  
> Şifreleme anahtarı için sertifika güncelleştirilirken bir tek yönlü bir işlemdir. Bu değişikliği yaptıktan sonra şifreleme anahtarına geçiş yapamazsınız. Tüm mevcut yedeklemeler önceki şifreleme anahtarıyla şifrelenmiş olarak kalır. 

![Azure Stack - geriye dönük uyumluluk modunda şifreleme sertifikasını kullan](media/azure-stack-backup/encryption-settings-backcompat-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir yedekleme işlemi yapılmasını öğrenin. Bkz: [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md)

Yedekleme çalıştırdığını doğrulamak öğrenin. Bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md)