---
title: Azure Backup'ın kurtarma Hizmetleri kasasına yükseltme Backup Kasası '
description: Yükseltme Backup kasasının kurtarma Hizmetleri kasasına yedekleme Resource Manager Vm'leri, Gelişmiş güvenlik, VMware VM yedekleme ve Windows sunucuları için sistem durumu yedeklemesi gibi yeni özellikleri almak için
services: backup
author: trinadhk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/10/2017
ms.author: trinadhk
ms.openlocfilehash: 01aacaecba8c5a4adf1dab5483a2f921df9314c0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51252539"
---
# <a name="backup-vault-upgraded-to-recovery-services-vault"></a>Yedekleme kasası kurtarma Hizmetleri kasasına yükseltildi
Bu makalede, hangi kurtarma Hizmetleri kasası sağlar genel bir bakış sağlar, Kurtarma Hizmetleri kasası ve yükseltme sonrası adımlarını var olan yedekleme yükseltme hakkında sık sorulan sorular kasası. Kurtarma Hizmetleri kasası, yedekleme verilerinizi barındıran bir Backup kasasının Azure Resource Manager eşdeğerdir. Veriler genellikle veri ya da sanal makineleri (VM'ler), iş yükleri, sunucular ve iş istasyonları için yapılandırma bilgilerini kopyalarını olup şirket içi veya azure'de.

## <a name="what-is-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası nedir?
Kurtarma Hizmetleri kasası Azure'da yedek kopyalar, kurtarma noktaları ve yedekleme ilkeleri gibi verilerin tutulması için kullanılan bir çevrimiçi depolama varlığıdır. Kurtarma Hizmetleri kasaları, Iaas VM'ler (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure Hizmetleri için yedekleme verileri tutmak için kullanabilirsiniz. Kurtarma Hizmetleri kasaları destek System Center DPM, Windows Server, Azure Backup sunucusu ve daha fazlası. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Karşılaştırma kurtarma Hizmetleri kasaları ve yedekleme kasaları
Kurtarma Hizmetleri kasaları, yedekleme kasaları Azure Service Manager modeline dayanır ancak Azure, Azure Resource Manager modelinin temel alır. Bir Backup kasasının kurtarma Hizmetleri kasasına yükselttiğinizde, yedekleme verileri sırasında ve sonrasında yükseltme işlemi değişmeden kalır. Kurtarma Hizmetleri kasaları için yedekleme kasalarını, mevcut olmayan özellikler gibi sağlayın:

- **Gelişmiş Özellikler yedek verilerin korunmasına yardımcı olun**: ile kurtarma Hizmetleri kasaları, Azure Backup bulut yedeklemelerini korumak için güvenlik özellikleri sağlar. Bu güvenlik özellikleri, yedeklerinizi güvenli ve güvenli bir şekilde üretim ve yedekleme sunucuları tehlikede olsa bile bulut yedeklemelerden veri kurtarmak emin olun. [Daha fazla bilgi](backup-azure-security-feature.md)

- **Karma BT ortamındaki Merkezi İzleme**: ile kurtarma Hizmetleri kasaları, izleyebilmeniz için yalnızca, [Azure Iaas Vm'leri](backup-azure-manage-vms.md) aynı zamanda, [şirket varlıkları](backup-azure-manage-windows-server.md#manage-backup-items) merkezi bir portaldan. [Daha fazla bilgi](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rol tabanlı erişim denetimi (RBAC)**: RBAC, azure'da ayrıntılı erişim yönetimi denetim sağlar. [Azure, çeşitli yerleşik rol sağlar](../role-based-access-control/built-in-roles.md), ve Azure Backup sahip üç [kurtarma noktaları yönetmek için yerleşik roller](backup-rbac-rs-vault.md). Kurtarma Hizmetleri kasaları, yedekleme kısıtlayan RBAC ile uyumludur ve tanımlanan kullanıcı rollerini kümesine erişimi geri yüklemek. [Daha fazla bilgi](backup-rbac-rs-vault.md)

- **Tüm yapılandırmalar Azure sanal makinelerinin korunmasına**: Resource Manager tabanlı Vm'leri Premium diskler, yönetilen diskler ve şifrelenmiş VM'ler dahil olmak üzere Kurtarma Hizmetleri kasaları şunları korur. Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme, Resource Manager tabanlı sanal makinelere Service Manager tabanlı sanal makinelerinize yükseltme olanağı sunar. Kasa yükseltme yapılırken, Service Manager tabanlı VM kurtarma noktalarını Beklet ve yükseltilen (Resource Manager özellikli) VM'ler için korumayı yapılandırın. [Daha fazla bilgi](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Iaas Vm'leri için anında geri yükleme**: kullanarak bir kurtarma Hizmetleri kasaları, dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri sağlayan tüm VM'yi geri yüklemeden geri yükleyebilirsiniz. Iaas Vm'leri için anında geri yükleme, hem Windows hem de Linux Vm'leri için kullanılabilir. [Daha fazla bilgi](https://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

> [!NOTE]
> MARS Aracısı ile 2.0.9083.0,'den önceki bir Backup kasasına kayıtlı öğeler varsa [en yeni MARS Aracısı'nı indirme]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe) kurtarma Hizmetleri kasası tüm özelliklerinin avantajlarından yararlanmak için sürüm. 
> 

## <a name="managing-your-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarınızı yönetmek
Aşağıdaki ekranlarda yeni bir kurtarma Hizmetleri kasası, yedekleme kasası, Azure portalında uygulamasından yükseltilen gösterir. Yükseltilen kasa "Varsayılan RecoveryServices-ResourceGroup-coğrafi" adlı bir varsayılan kaynak grubunda mevcut olacaktır. Örnek: yedekleme kasanız Batı ABD'de bulunuyorsa, varsayılan olarak varsayılan RecoveryServices ResourceGroup westus adlı RG yerleştirilir.
> [!NOTE]
> Kaynak grubu, CPS standart müşteriler için kasa yükseltmeden sonra değiştirilmez ve yükseltmeden önce olduğu gibi aynı kalır.

Kasa için anahtar varlıklar görüntüler kasa panosunda ilk ekran gösterilmektedir.
![Kurtarma Hizmetleri kasasına yedekleme kasasından yükseltme örneği](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

İkinci ekranında Yardım bağlantıları yardımcı olması kullanılabilir kurtarma Hizmetleri kasası ile çalışmaya başlamak gösterir.

![Hızlı Başlangıç dikey penceresinde Yardım bağlantıları](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Yükseltme sonrası adımlar
Kurtarma Hizmetleri kasası, yedekleme ilkesinde belirten saat dilimi bilgilerini destekler. Kasa başarıyla yükseltildikten sonra yedekleme İlkeleri Kasa ayarları menüsünden gidin ve her bir kasa içinde yapılandırılmış ilkeler için saat dilimi bilgilerini güncelleştirin. Bu ekran zaten ilkesi oluşturduğunuzda kullanıldığında yerel saat dilimi olarak belirtilen yedekleme zamanlaması saati gösterir. 

## <a name="enhanced-security"></a>Geliştirilmiş güvenlik
Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltildiğinde bu kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Ne zaman yedeklemeler silme gibi belirli işlemleri üzerindeki güvenlik ayarlarını olan veya bir parolayı değiştirmeniz bir [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) PIN. Gelişmiş güvenlik hakkında daha fazla bilgi için bkz [karma yedeklemeler korumak için güvenlik özellikleri](backup-azure-security-feature.md). Gelişmiş Güvenlik etkinleştirildiğinde, veri yedekleme kurtarma noktası bilgilerini kasadan silindikten sonra 14 gün için saklanır. Müşteriler, bu güvenlik verilerinin depolanması için faturalandırılır. Güvenlik veri saklama, Azure Yedekleme aracısı, Azure Backup sunucusu ve System Center Data Protection Manager için gerçekleştirilen kurtarma noktaları için geçerlidir. 

## <a name="gather-data-on-your-vault"></a>Kasanızı üzerinde veri toplayın
Bir kez, bir kurtarma Hizmetleri kasasına yükseltme (Iaas Vm'leri ve Microsoft Azure kurtarma Hizmetleri aracısı için) için Azure Backup raporlarını yapılandırma ve raporlara erişmek için Power BI'ı kullanın. Veri toplamayı hakkında ek bilgi için bkz [yapılandırma Azure Backup raporları](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Planınızı yükseltin, devam eden yedekleme İşlemlerim etkiliyor mu?**</br>
Hayır. Devam eden yedeklemeler kesintisiz sırasında ve yükseltme sonrasında devam eder.

**Bu yükseltme ortalaması mevcut araçlarım için ne?**</br>
Yükseltmeden sonra çalışmaya devam etmesini sağlamak için Resource Manager dağıtım modeli için var olan Otomasyon veya araçları güncelleştirmeniz gerekir. PowerShell cmdlet'leri başvurusu başvurun [Resource Manager dağıtım modeli](backup-client-automation.md).

**Geri yükseltmeden sonra geri alabilir miyim?**</br>
Hayır. Kaynakları başarıyla güncellendikten sonra geri alma desteklenmiyor.

**Yükseltme sonrası my Klasik kasa görüntüleyebilirim?**</br>
Hayır. Görüntüleyemez veya Klasik kasanızı yükseltme sonrası yönetin. Yalnızca yeni Azure portalına kasasındaki tüm yönetim eylemleri için kullanmanız mümkün olacaktır.

**My yükseltilen kasadaki MARS aracısı tarafından korunan sunucular neden göremiyorum?**</br>
MARS Aracısı kasanızdaki tarafından korunan tüm sunucular görmek için en yeni MARS aracısı yüklemeniz gerekir. Aracısı'ndan en son sürümünü indirebilirsiniz [burada]( http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).

**Yükseltmeden sonra MARS aracısı tarafından korunan sunucular için yedekleme ilkesini göremiyorum**</br>
Kasanın yedekleme İlkesi, süresi dolmuş olabilir ve bu nedenle yükseltilen kasaya eşitlenemiyor. Lütfen ilkelerinizi yükseltilen kasadaki görmeye devam etmesini sağlamak üzere ilke güncelleştirin.
İlke güncelleştirmek için MARS Aracısı gidin ve yapılandırılan yedekleme ilkesini güncelleştirin.

**Yükseltmeden sonra Belgelerim yedekleme İlkesi neden güncelleştiremiyorum?**</br>
Eski bir yedekleme aracısı üzerinde ve izin verilen en düşük değerden daha küçük olması için en düşük Bekletme dönemi seçin kullandığınızda ortaya çıkar. Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltildiğinde bu kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Kullanılabilir olduğundan emin her zaman geçerli bir kurtarma noktası sayısı emin olmak için güvenlik özelliği göre tutulması gereken bazı en düşük Bekletme dönemi yoktur. Daha fazla ayrıntı için bkz [burada](backup-azure-security-feature.md).
Ayrıca, Azure Backup Aracısı Azure Backup'ın en son özelliklere avantajlarından yararlanmak için en son sürüme güncelleştirmek gerekir.

**Aracım güncelleştirdiniz mi, ancak herhangi bir nesne yükseltmeden sonra bile bir gün eşitlendiğinden göremiyorum**</br>
Aynı makineye birden fazla kasaya kayıtlı olmadığını denetleyin. MARS Aracısı kayıtlı olduğu aynı kasaya aradığınız emin olun. MARS aracınızı için kayıtlı bir kasa hangi Windows kayıt defterini açın ve HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config altındaki ServiceResourceName anahtarının değerini denetleyin bulmak için kasa MARS Aracısı görünür kayıtlı vardır. ServiceResourceName anahtar sisteminizde görünür değilse ResourceId ve Machineıd anahtarları HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config altında değeriyle bize ulaşın ve sorunu çözmenize yardımcı olacağız.

**Neden iş bilgileri Kaynaklarım için yükseltme sonrasında göremiyorum?**</br>
(MARS aracısı ve Iaas) yedeklemeler için izleme, yedekleme kasası kurtarma Hizmetleri kasasına yükseltme yaptığınızda, size yeni bir özelliktir. İzleme bilgileri, eşitleme hizmeti ile 12 saat sürer.

**Bir sorunu nasıl bildirebilirim?**</br>
Herhangi bir bölümünü kasa yükseltmesi başarısız olursa, hatayı Operationıd listelenen unutmayın. Microsoft Support, sorunu çözmek için proaktif olarak çalışır. Desteğine ulaşın veya adresinden bize e-posta rsvaultupgrade@service.microsoft.com , abonelik kimliği, kasa adınız ve Operationıd. Mümkün olan en kısa sürede sorunu dener. İşlem, bunu yapmak için Microsoft tarafından açıkça belirtilmediği sürece yeniden denemeyin.

## <a name="next-steps"></a>Sonraki adımlar
Yönelik bu makaleleri kullanın:</br>
[Bir Iaas VM'yi yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure Backup sunucusu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Bir Windows Server Yedekleme](backup-configure-vault.md)
