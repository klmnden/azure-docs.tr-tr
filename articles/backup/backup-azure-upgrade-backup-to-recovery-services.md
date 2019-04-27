---
title: Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme
description: Azure yedekleme kasanıza bir kurtarma Hizmetleri kasasına yükseltme yönergeleri ve destek bilgileri.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 1/4/2018
ms.author: sogup
ms.openlocfilehash: b4ecebc6bef7f49a23455c7a85f25680df087a95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60848376"
---
# <a name="upgrade-a-backup-vault-to-a-recovery-services-vault"></a>Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme

Bu makalede, bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme açıklanmaktadır. Yükseltme işlemi, tüm çalışan yedekleme işleri etkilemez ve yedekleme veri kaybedilmez. Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme birincil nedeni:
- Tüm özellikleri bir Backup kasasının kurtarma Hizmetleri kasasında korunur.
- Kurtarma Hizmetleri kasaları, yedekleme kasaları, dahil olmak üzere daha fazla özelliğe sahip: izleme, tümleşik daha iyi güvenlik, daha hızlı geri yüklemeler ve öğe düzeyinde geri yüklemeler.
- Yedekleme öğeleri, geliştirilmiş ve Basitleştirilmiş portalından yönetin.
- Yeni özellikler yalnızca kurtarma Hizmetleri kasaları için geçerlidir.

## <a name="impact-to-operations-during-upgrade"></a>İşlem yükseltme sırasında bir etkisi

Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltirken, veri düzlemi işlemleri için herhangi bir etkisi yoktur. Tüm yedekleme işleri normal olarak devam etmek ve kesinti olmadan herhangi bir etkin geri yükleme işi devam. Yükseltme sırasında yönetim işlemlerini kısa bir kapalı kalma süresi uygulanır ve yeni öğelerinizi koruyun veya geçici yedekleme işleri oluşturun. Iaas Vm'leri için geri yükleme işleri, yükseltme sırasında çalışmaz. Kasa yükseltmesi tamamlanması altında bir saat sürer. İşleminizi tamamladıktan sonra bir kurtarma Hizmetleri kasasına yedekleme kasasına değiştirir.

## <a name="changes-to-your-automation-and-tool-after-upgrading"></a>Otomasyon ve aracı yükselttikten sonra yapılan değişiklikler

Kasa yükseltmesi için altyapınızı hazırlanırken, var olan Otomasyon veya yükseltmeden sonra çalışmaya devam etmesini sağlamak için araçları güncelleştirmeniz gerekir.
PowerShell cmdlet'leri başvurusu başvurun [Resource Manager dağıtım modeli](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Yükseltmeden önce

Backup kasalarınızı kurtarma Hizmetleri kasalarına yükseltmeden önce aşağıdaki sorunları inceleyin.

- **En düşük aracı sürümü**: Kasanızı yükseltme için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı en az olduğundan emin olun 2.0.9083.0 sürümü. MARS Aracısı 2.0.9083.0 eskiyse, yükseltme işlemini başlatmadan önce aracıyı güncelleştirin.
- **Örnek tabanlı faturalandırma modeli**: Kurtarma hizmeti kasası yalnızca örnek tabanlı faturalandırma modeli destekler. Faturalandırma modeli, eski depolama tabanlı faturalandırma modeli kullanarak bir yedekleme kasası varsa, yükseltme sırasında dönüştürün.
- **Yedekleme yapılandırması devam eden bir işlem**: Yükseltme sırasında yönetim düzlemi için erişim kısıtlıdır. Tüm yönetim düzlemi işlemleri tamamlayın ve ardından yükseltmeyi başlatın.

## <a name="using-powershell-scripts-to-upgrade-your-vaults"></a>Kasalarınızı yükseltmek için PowerShell betiklerini kullanma

Backup kasalarınızı kurtarma Hizmetleri kasalarına yükseltmenizi PowerShell komut dosyalarını kullanabilirsiniz. Kasa yükseltmesi tetiklemek için gerekli PowerShell bileşenlerine sahip olduğunu denetleyin. PowerShell betikleri için Backup kasaları kurtarma Hizmetleri kasaları için çalışmaz. Kasa yükseltme için ortamınızı hazırlama:

1. Yüklemeniz veya yükseltmeniz [Windows Management Framework (WMF) 5 sürümüne](https://www.microsoft.com/download/details.aspx?id=50395) veya üzeri.
2. [Azure PowerShell'i MSI yükleme](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. İndirme [PowerShell Betiği](https://aka.ms/vaultupgradescript2) kasalarınızı yükseltmek için.

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma

Kasalarınızı yükseltmek için aşağıdaki betiği kullanın. Aşağıdaki örnek betik, parametre açıklamalarını sahiptir.

RecoveryServicesVaultUpgrade-1.0.2.ps1 **-SubscriptionID** `<subscriptionID>` **-VaultName** `<vaultname>` **-Location** `<location>` **-ResourceType** `BackupVault` **-TargetResourceGroupName** `<rgname>`

**Subscriptionıd** -yükseltiliyor kasa abonelik kimliği sayısı.<br/>
**VaultName** -yükseltiliyor yedekleme kasasının adını.<br/>
**Konum** -yükseltilmekte olan kasasının konumu.<br/>
**ResourceType** -BackupVault kullanın.<br/>
**TargetResourceGroupName** - kasası Resource Manager tabanlı bir dağıtım için bir kaynak grubu belirtin yükselttiğiniz olduğundan. Mevcut bir kaynak grubunu kullanın veya yeni bir ad girerek oluşturabilirsiniz. Bir kaynak grubu adını yazarsanız, yeni bir kaynak grubu oluşturabilir. Kaynak grupları hakkında daha fazla bilgi için bu okuma [kaynak grupları hakkında genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Kaynak grubu adları kısıtlamalar vardır. Kılavuzu takip ettiğinizden emin olun; Bunun yapılmaması, kasa yükseltme başarısız olmasına neden olabilir.
>
>**Azure ABD kamu** "AzureUSGovernment" ortamı ayarlamak gereken betik çalıştırılırken müşteriler.
>**Azure Çin** "AzureChinaCloud" ortamı ayarlamak gereken betik çalıştırılırken müşteriler.

Aşağıdaki kod parçacığı, PowerShell komut şöyle görünmelidir, bir örnek verilmiştir:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

Betik parametre olmadan da çalıştırabilirsiniz ve girişleri için gerekli tüm parametreleri sağlamanız istenir.

PowerShell Betiği, kimlik bilgilerinizi girmenizi ister. Kimlik bilgilerinizi iki kez girin: Resource Manager hesabı için ikinci bir kez ve Service Manager hesabı için bir kez.

### <a name="pre-requisites-checking"></a>Önkoşullar denetleniyor
Azure kimlik bilgilerinizi girdikten sonra Azure ortamınızın aşağıdaki önkoşulları karşıladığını denetler:

- **En düşük aracı sürümü** -Backup kasaları kurtarma Hizmetleri kasalarına yükseltme gerektirir MARS aracısının en az sürüm 2.0.9083.0. Bir aracıyla, 2.0.9083.0'den önceki bir Backup kasasına kayıtlı öğeler varsa, önkoşul denetimi başarısız olur. Önkoşul denetimi başarısız olursa aracıyı güncelleştirin ve kasa yükseltme işlemini yeniden deneyin. Aracısı'ndan en son sürümünü indirebilirsiniz [ https://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe ](https://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Devam eden yapılandırma işleri**: Birisi iş yükseltilmek üzere ayarlanmış bir yedekleme kasası için yapılandırma veya bir öğe kayıt, önkoşul denetimi başarısız olur. Yapılandırmayı tamamlamak veya öğesi kaydetme tamamlayın ve sonra kasa yükseltme işlemini başlatın.
- **Depolama tabanlı faturalandırma modeli**: Kurtarma Hizmetleri kasaları örnek tabanlı faturalandırma modeli destekler. Depolama tabanlı faturalandırma modeli kullanan bir Backup kasasının kasa yükseltmesi çalıştırırsanız, faturalandırma modelinizin Kasayla birlikte yükseltme istenir. Faturalandırma modelinizin ilk olarak, aksi takdirde, güncelleştirebilir ve kasa yükseltmesini çalıştırın.
- Kurtarma Hizmetleri kasası için bir kaynak grubu belirleyin. Resource Manager dağıtım özelliklerden yararlanmak için bir kaynak grubunda bir kurtarma Hizmetleri kasası yerleştirmeniz gerekir. Varsa tanımadığınız kaynak grubunu kullanmak için bir ad sağlayın ve yükseltme işlemi, kaynak grubu oluşturulur. Yükseltme işlemi, yeni kaynak grubu ile kasayı de ilişkilendirir.

Önkoşullar denetleniyor yükseltme işlemi tamamlandıktan sonra işlem kasa yükseltmesini başlatmak isteyip istemediğinizi sorar. Siz onayladıktan sonra yükseltme işlemi genellikle, kasanız boyutuna bağlı olarak tamamlanması yaklaşık 15-20 dakika sürer. Büyük bir kasası varsa, yükseltme 90 dakikaya kadar sürebilir.

## <a name="managing-your-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarınızı yönetmek

Aşağıdaki ekranlarda yeni bir kurtarma Hizmetleri kasası, yedekleme kasası, Azure portalında uygulamasından yükseltilen gösterir. Kasa için anahtar varlıklar görüntüler kasa panosunda ilk ekran gösterilmektedir.

![Kurtarma Hizmetleri kasasına yedekleme kasasından yükseltme örneği](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

İkinci ekranında Yardım bağlantıları yardımcı olması kullanılabilir kurtarma Hizmetleri kasası ile çalışmaya başlamak gösterir.

![Hızlı Başlangıç dikey penceresinde Yardım bağlantıları](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Yükseltme sonrası adımlar
Kurtarma Hizmetleri kasası, yedekleme ilkesinde belirten saat dilimi bilgilerini destekler. Kasa başarıyla yükseltildikten sonra yedekleme İlkeleri Kasa ayarları menüsünden gidin ve her bir kasa içinde yapılandırılmış ilkeler için saat dilimi bilgilerini güncelleştirin. Bu ekran zaten ilkesi oluşturduğunuzda kullanıldığında yerel saat dilimi olarak belirtilen yedekleme zamanlaması saati gösterir.

## <a name="enhanced-security"></a>Gelişmiş güvenlik

Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltildiğinde bu kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Ne zaman yedeklemeler silme gibi belirli işlemleri üzerindeki güvenlik ayarlarını olan veya bir parolayı değiştirmeniz bir [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) PIN. Gelişmiş güvenlik hakkında daha fazla bilgi için bkz [karma yedeklemeler korumak için güvenlik özellikleri](backup-azure-security-feature.md).

Gelişmiş Güvenlik etkinleştirildiğinde, veri yedekleme kurtarma noktası bilgilerini kasadan silindikten sonra 14 gün için saklanır. Müşteriler, bu güvenlik verilerinin depolanması için faturalandırılır. Güvenlik veri saklama, Azure Yedekleme aracısı, Azure Backup sunucusu ve System Center Data Protection Manager için gerçekleştirilen kurtarma noktaları için geçerlidir.

## <a name="gather-data-on-your-vault"></a>Kasanızı üzerinde veri toplayın

Bir kurtarma Hizmetleri kasasına yükseltme sonra (Iaas Vm'leri ve Microsoft Azure kurtarma Hizmetleri (MARS)) için Azure Backup raporlarını yapılandırma ve raporlara erişmek için Power BI'ı kullanın. Veri toplamayı hakkında ek bilgi için bkz [yapılandırma Azure Backup raporları](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-the-upgrade-plan-affect-my-ongoing-backups"></a>Planınızı yükseltin, devam eden yedekleme İşlemlerim etkiliyor mu?
Hayır. Devam eden yedeklemeler kesintisiz sırasında ve yükseltme sonrasında devam eder.

### <a name="if-i-dont-plan-on-upgrading-soon-what-happens-to-my-vaults"></a>En kısa sürede yükseltme planlama yoktur, my kasalarına ne olacak?
Tüm yeni özellikler yalnızca kurtarma Hizmetleri kasaları için geçerli olduğundan, biz kasalarınızı yükseltmek rica. 1 Eylül 2017 tarihinden itibaren Microsoft, yedekleme kasaları kurtarma Hizmetleri kasalarına otomatik olarak yükseltme başlar. Kasım sonra 30,2017, PowerShell kullanarak Backup kasalarını artık oluşturabilirsiniz. Kasanız arasında istediğiniz zaman otomatik olarak yükseltilebilir. Microsoft, mümkün olan en kısa sürede kasanızı yükseltme önerir.

### <a name="what-does-this-upgrade-mean-for-my-existing-tooling"></a>Bu yükseltme ortalaması mevcut araçlarım için ne?
Resource Manager dağıtım modeli için araçlarınızı güncelleştirin. Kurtarma Hizmetleri kasaları için oluşturulan Resource Manager dağıtım modelinde kullanın. Resource Manager dağıtım modeli için planlama ve kasalarınızı fark hesap büyük/küçük harf önemlidir.

### <a name="during-the-upgrade-is-there-much-downtime"></a>Yükseltme sırasında kadar kapalı kalma süresi var mı?
Bu, yükseltilmekte olan kaynakların sayısına bağlıdır. (Korumalı örnekler birkaç düzine) daha küçük dağıtımlar için 20 dakikadan kısa bir sürede tam yükseltme almalıdır. Daha büyük dağıtımlar için bu en fazla bir saat sürer.

### <a name="can-i-roll-back-after-upgrading"></a>Geri yükselttikten sonra geri alabilir miyim?
Hayır. Kaynakları başarıyla güncellendikten sonra geri alma desteklenmiyor.

### <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-upgrade"></a>Aboneliğimi ya yükseltmesini özelliğine sahip olup olmadıklarını görmek için kaynakları doğrulayabilir miyim?
Evet. Yükseltme ilk adımı kaynakları yükseltmesini yeteneğine sahip olduğunu doğrular. Önkoşulları doğrulama başarısız olursa, yükseltme işleminin neden tamamlanamadığına dair tüm nedenlerle mesajlar alır.

### <a name="can-i-upgrade-my-csp-based-backup-vault"></a>My CSP tabanlı bir yedekleme kasası yükseltebilirim?
Hayır. Şu anda, CSP tabanlı Yedekleme kasaları yükseltemezsiniz. Sonraki sürümlerde CSP tabanlı Yedekleme kasaları yükseltme için destek ekleyeceğiz.

### <a name="can-i-view-my-classic-vault-post-upgrade"></a>Yükseltme sonrası my Klasik kasa görüntüleyebilirim?
Hayır. Görüntüleyemez veya Klasik kasanızı yükseltme sonrası yönetin. Yalnızca yeni Azure portalına kasasındaki tüm yönetim eylemleri için kullanmanız mümkün olacaktır.

### <a name="my-upgrade-failed-but-the-machine-that-held-the-agent-requiring-updating-doesnt-exist-anymore-what-do-i-do-in-such-a-case"></a>My yükseltme başarısız oldu, ancak aracının gerektiren tutulan makine güncelleştiriliyor, artık mevcut değil. Böyle bir durumda ne yapmalıyım?
Deponun kullanmanız gerekiyorsa, uzun süreli saklama ve ardından bu makine yedeklerini kasa yükseltmek mümkün olmayacaktır. Gelecek sürümlerde bu tür bir kasa yükseltme için destek ekleyeceğiz.
Artık bu makine yedeklerini depolamak gerekmez, lütfen bu makineden kasa kaydı ve yükseltmeyi yeniden deneyin.

### <a name="why-cant-i-see-the-jobs-information-for-my-resources-after-upgrade"></a>Neden iş bilgileri Kaynaklarım için yükseltme sonrasında göremiyorum?
(MARS aracısı ve Iaas) yedeklemeler için izleme, yedekleme kasası kurtarma Hizmetleri kasasına yükseltme yaptığınızda, size yeni bir özelliktir. İzleme bilgileri, eşitleme hizmeti ile 12 saat sürer.

### <a name="how-do-i-report-an-issue"></a>Bir sorunu nasıl bildirebilirim?
Herhangi bir bölümünü kasa yükseltmesi başarısız olursa, hatayı Operationıd listelenen unutmayın. Microsoft Support, sorunu çözmek için proaktif olarak çalışır. Desteğine ulaşın veya adresinden bize e-posta rsvaultupgrade@service.microsoft.com , abonelik kimliği, kasa adınız ve Operationıd. Mümkün olan en kısa sürede sorunu dener. İşlem, bunu yapmak için Microsoft tarafından açıkça belirtilmediği sürece yeniden denemeyin.


## <a name="next-steps"></a>Sonraki adımlar
Şu makaleyi kullanın:</br>
[Bir Iaas VM'yi yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure Backup sunucusu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server Yedekleme](backup-configure-vault.md).
