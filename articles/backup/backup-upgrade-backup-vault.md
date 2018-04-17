---
title: Kurtarma Hizmetleri kasasına Azure Yedekleme'nin yükseltme yedekleme kasası | Microsoft Docs
description: Kaynak Yöneticisi Vm'leri, Gelişmiş güvenlik, VMware VM yedekleme ve sistem durumu yedeklemesi Windows sunucuları için yedekleme gibi yeni özellikler almak için kurtarma Hizmetleri kasasına yükseltme yedekleme kasası
services: backup
documentationcenter: ''
author: trinadhk
manager: vijayts
editor: ''
keyword: backup vault; upgrade vault; recovery services vault
ms.assetid: d037a8bf-49f2-4578-974a-3471d87ca278
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: trinadhk, sogup
ms.openlocfilehash: 7c340f60bc648909d073821f1987036da9633458
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Yedekleme kasasının kurtarma Hizmetleri Kasası'na yükseltme
Kurtarma Hizmetleri kasası ve yükseltme sonrası adımlarını var olan bir yedek yükseltme hakkında sık sorulan sorular kasa, bu makalede hangi kurtarma Hizmetleri kasası sağlar genel bir bakış sağlar. Kurtarma Hizmetleri kasası, yedekleme verilerinizi barındıran bir yedekleme kasası Azure Resource Manager eşdeğerdir. Verileri genellikle veri ya da sanal makineleri (VM'ler), iş yükleri, sunucular ve iş istasyonları için yapılandırma bilgilerini kopyalarını olup şirket içi veya azure'de.

## <a name="what-is-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası nedir?
Kurtarma Hizmetleri kasası Azure'da yedek kopyalar, kurtarma noktaları ve yedekleme ilkeleri gibi verilerin tutulması için kullanılan bir çevrimiçi depolama varlığıdır. Kurtarma Hizmetleri kasalarının Iaas VM'ler (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure Hizmetleri için yedek verileri saklamak için kullanabilirsiniz. Kurtarma Hizmetleri kasaları destek System Center DPM, Windows Server, Azure yedekleme sunucusu ve daha fazlası. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Karşılaştırma kurtarma Hizmetleri kasaları ve yedekleme kasaları
Kurtarma Hizmetleri kasaları, yedekleme kasaları Azure Service Manager modelini temel alan ancak Azure, Azure Resource Manager modelini temel alır. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme yaparken, yedekleme verilerini sırasında ve yükseltme işlemi sonrasında değişmeden kalır. Kurtarma Hizmetleri kasaları gibi yedekleme kasaları için kullanılamayan özellikleri sağlar:

- **Gelişmiş Güvenlik yedekleme verileri yardımcı olmak için özellikleri**: ile kurtarma Hizmetleri kasaları, Azure Backup bulut yedekleme korumak için güvenlik özellikleri sağlar. Bu güvenlik özellikleri Yedeklemelerinizin güvenli ve güvenli bir şekilde üretim ve yedekleme sunucuları tehlikeye olsa bile bulut yedeklemelerden veri kurtarmak emin olun. [Daha fazla bilgi](backup-azure-security-feature.md)

- **Karma BT ortamında Merkezi İzleme**: ile kurtarma Hizmetleri kasaları, izleyebilirsiniz yalnızca, [Azure Iaas Vm'leri](backup-azure-manage-vms.md) aynı zamanda, [içi varlıklar](backup-azure-manage-windows-server.md#manage-backup-items) merkezi portalından. [Daha fazla bilgi](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rol tabanlı erişim denetimi (RBAC)**: RBAC Azure ayrıntılı erişim yönetimi denetim sağlar. [Azure sağlayan çeşitli yerleşik roller](../role-based-access-control/built-in-roles.md), ve Azure Backup sahip üç [kurtarma noktaları yönetmek için yerleşik roller](backup-rbac-rs-vault.md). Kurtarma Hizmetleri kasaları yedekleme kısıtlayan RBAC ile uyumludur ve kullanıcı rolleri tanımlı kümesine erişim geri yükleyin. [Daha fazla bilgi](backup-rbac-rs-vault.md)

- **Tüm yapılandırmaları Azure sanal makinelerin korunmasına**: Kurtarma Hizmetleri kasaları şunları korur Resource Manager tabanlı VM'ler Premium diskler, yönetilen diskleri ve şifrelenmiş VM'ler dahil olmak üzere. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme Resource Manager tabanlı VM'ler için Service Manager tabanlı VM'ler yükseltme olanağı sağlar. Kasa yükseltirken, Service Manager tabanlı VM kurtarma noktalarını korumak ve yükseltilen (Resource Manager etkin) VM'ler için korumayı yapılandırın. [Daha fazla bilgi](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Iaas VM'ler için anlık geri**: kullanarak kurtarma Hizmetleri kasaları, dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri sağlar tüm VM geri yüklemeden geri yükleyebilirsiniz. Iaas VM'ler için anlık geri yükleme, Windows ve Linux VM'ler için kullanılabilir. [Daha fazla bilgi](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> MARS Aracısı ile 2.0.9083.0,'den önceki bir yedekleme Kasası'na kayıtlı öğeler varsa [son MARS Aracısı'nı indirme]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) fayda sağlar, Kurtarma Hizmetleri kasası, tüm özelliklerin sürümü. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarını yönetme
Aşağıdaki ekranlarını Azure portalında yedekleme Kasası'ndan yükseltme yeni bir kurtarma Hizmetleri kasası göster. Yükseltilen kasa "Varsayılan RecoveryServices-ResourceGroup-coğrafi" adlı bir varsayılan kaynak grubunda mevcut olacaktır. Örnek: yedekleme kasası Batı ABD bulunuyorsa, onu varsayılan olarak varsayılan RecoveryServices ResourceGroup westus adlı RG yerleştirilecek.
> [!NOTE]
> CPS standart müşteriler için kaynak grubu kasası yükseltmeden sonra değiştirilmez ve yükseltme işleminden önce olduğu gibi aynı kalır.

İlk ekranda kasa için anahtar varlıkları görüntüler kasa Panosu gösterilir.
![Kurtarma Hizmetleri kasasına yedekleme Kasası'nı yükseltme örneği](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

İkinci ekranı bağlantılar yardımcı olmak kullanılabilir kurtarma Hizmetleri kasası kullanmaya başlamanıza yardım gösterir.

![Hızlı Başlangıç dikey penceresinde Yardım bağlantıları](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Yükseltme sonrası adımlar
Kurtarma Hizmetleri kasasına yedekleme ilkesine belirten saat dilimi bilgilerini destekler. Kasa başarıyla yükseltildikten sonra kasa ayarları menüsünden yedekleme ilkeleri gidin ve her bir kasaya yapılandırılmış ilkeler için saat dilimi bilgilerini güncelleştirin. Bu ekran zaten kullanıldığında yerel saat dilimi ilke oluşturuldu olarak belirtilen yedekleme zamanlaması saati gösterir. 

## <a name="enhanced-security"></a>Geliştirilmiş güvenlik
Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltildiğinde, kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Ne zaman güvenlik ayarlarını, yedeklemeler silme gibi bazı işlemleri bulunan veya bir parola değiştirilmesini gerektiren bir [Azure çok faktörlü kimlik doğrulaması](../multi-factor-authentication/multi-factor-authentication.md) PIN. Gelişmiş Güvenlik ile ilgili daha fazla bilgi için bkz: [karma yedekleri korumak için güvenlik özellikleri](backup-azure-security-feature.md). Gelişmiş Güvenlik etkinleştirildiğinde, veri yedekleme kurtarma noktası bilgilerini kasadan silindikten sonra 14 gün için tutulur. Müşteriler bu güvenlik veri depolama için faturalandırılır. Güvenlik veri saklama Azure Yedekleme aracısı, Azure yedekleme sunucusu ve System Center Data Protection Manager için alınan kurtarma noktaları için geçerlidir. 

## <a name="gather-data-on-your-vault"></a>Kasanızda verileri toplayın
Bir kez, bir kurtarma Hizmetleri Kasası'na yükseltme, raporlar için Azure Backup (Iaas Vm'leri ve Microsoft Azure kurtarma Hizmetleri aracısı için), yapılandırıp Power BI raporlarına erişmek için kullanın. Makale veri toplamayı hakkında ek bilgi için bkz: [Azure Yedekleme'yi yapılandırma raporları](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Yükseltme planı devam eden yedeklerim etkiliyor mu?**</br>
Hayır. Devam eden Yedeklemelerinizin kesintisiz sırasında ve yükseltmeden sonra devam eder.

**Bu yükseltme ortalaması my varolan araçları için nedir?**</br>
Yükseltmeden sonra çalışmaya devam ettiğinden emin olmak için Resource Manager dağıtım modeli için var olan Otomasyon veya araç güncelleştirmeniz gerekir. PowerShell cmdlet'leri başvurularını başvurun [Resource Manager dağıtım modeli](backup-client-automation.md).

**Yükseltmeden sonra geri geri alabilirsiniz?**</br>
Hayır. Kaynakları başarılı bir şekilde yükselttikten sonra geri alma desteklenmiyor.

**Yükseltme sonrası my Klasik kasası görüntüleyebilir miyim?**</br>
Hayır. Görüntüleyemez veya Klasik kasanızı yükseltme sonrası yönetin. Yalnızca yeni Azure portalına kasa tüm yönetim eylemlerini kullanmak mümkün olacaktır.

**My yükseltilmiş kasasında MARS aracısı tarafından korunan sunucuları neden göremiyorum?**</br>
MARS Aracısı kasanızdaki tarafından korunan tüm sunucuları görmek için son MARS aracısı yüklemeniz gerekir. Aracısı'ndan en son sürümünü indirebilirsiniz [burada]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).

**Yükseltmeden sonra MARS aracısı tarafından korunan sunucular için yedekleme İlkesi göremiyorum**</br>
Yedekleme Kasası'nın İlkesi güncel olabilir ve bu nedenle yükseltilen kasaya eşitlenemiyor. Lütfen ilkelerinizi yükseltilmiş kasadaki görmeye devam emin olmak için ilke güncelleştirin.
İlke güncelleştirmek için MARS Aracısı'na gidin ve yapılandırılmış yedekleme ilkesini güncelleştirin.

**Neden ı yükseltmeden sonra my yedekleme İlkesi güncelleştirilemiyor?**</br>
Eski bir yedekleme aracısını olduğunda ve izin verilen minimum değerden daha küçük olacak şekilde minimum bekletme aralığını seçin olduğunda meydana gelir. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltildiğinde, kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Her zaman geçerli bir kurtarma noktası sayısı kullanılabilir olduğundan emin olmak için güvenlik özelliği uygun şekilde saklanması gereken bazı minimum saklama dönemi yoktur. Daha fazla ayrıntı için bkz [burada](backup-azure-security-feature.md).
Ayrıca, Azure Yedekleme aracısı en son Azure Backup özelliklerini yararları olabilmesi için en son sürüme güncelleştirmeniz gerekir.

**My aracı güncelleştirilmiş olan, ancak yükseltmeden sonra bile gün eşitlenmiş tüm nesnelerin görmeye devam edemiyor**</br>
Birden çok kasa aynı makineye kayıtlı olmadığını denetleyin. MARS Aracısı kayıtlı olduğu aynı kasayı baktığınızdan emin olun. MARS aracısı için kayıtlı hangi kasası Windows kayıt defterini açın ve HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config altında ServiceResourceName anahtar için değeri denetleyin bulmak için kasa MARS Aracısı görünür kayıtlı vardır. ServiceResourceName anahtar sisteminizde görünür durumda değilse, ResourceId ve ResourceId anahtarları HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config altında değeriyle bize ulaşın ve biz sorunu çözmenize yardımcı olur.

**Neden işleri bilgi Kaynaklarım için yükseltme sonrasında göremiyorum?**</br>
(MARS aracısı ve Iaas) yedeklemeler için izleme kurtarma Hizmetleri kasasına yedekleme kasası yükselttiğinizde aldığınız yeni bir özelliktir. İzleme bilgilerini hizmetiyle eşitleme 12 saat sürer.

**Bir sorunu nasıl bildirebilirim?**</br>
Herhangi bir kısmının kasası yükseltme başarısız olursa hata Operationıd listelenen not edin. Microsoft Support sorunu çözmek için proaktif olarak çalışır. Desteği'ne ulaşmak ya da adresinden bize e-posta rsvaultupgrade@service.microsoft.com abonelik kimliği, kasa adı ve Operationıd. Sorunu çözmek mümkün olan en kısa sürede dener. Açıkça Bunu yapmak için Microsoft tarafından belirtilmedikçe işlemi yeniden denemeyin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler için kullanın:</br>
[Bir Iaas VM'yi yedeklemek](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure yedekleme sunucuyu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Bir Windows Server'ı Yedekle](backup-configure-vault.md)
