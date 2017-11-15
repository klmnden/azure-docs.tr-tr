---
title: "Bir Backup kasasının kurtarma Hizmetleri kasası (Önizleme) yükseltme | Microsoft Docs"
description: "Yönergeler ve destek bilgileri için bir kurtarma Hizmetleri kasası, Azure yedekleme kasası yükseltmek için."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/09/2017
ms.author: sogup;markgal;arunak
ms.openlocfilehash: 4867a43aab1357cb8e01c2ddcef74cdebb41a84a
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="upgrade-a-backup-vault-to-a-recovery-services-vault"></a>Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme

Bu makalede, bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme açıklanmaktadır. Yükseltme işlemi herhangi bir çalışan yedekleme işi etkilemez ve hiçbir yedekleme verileri kaybolur. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltmek için en önemli nedenlerinden:
- Tüm özellikleri bir Backup kasasının kurtarma Hizmetleri kasasına korunur.
- Kurtarma Hizmetleri kasaları dahil olmak üzere, yedekleme kasaları daha fazla özelliğe sahip: izleme, tümleşik daha iyi güvenlik, daha hızlı geri yükler ve öğe düzeyinde geri yüklemeler.
- Yedekleme öğeleri geliştirilmiş, Basitleştirilmiş portalından yönetin.
- Yeni özellikler yalnızca kurtarma Hizmetleri kasaları için geçerlidir.

## <a name="impact-to-operations-during-upgrade"></a>Yükseltme sırasında operations etkisi

Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltirken veri düzlemi işlemlerinizin hiçbir etkisi yoktur. Tüm yedekleme işleri normal olarak devam etmek ve kesinti olmadan herhangi bir etkin geri yükleme işi devam. Yükseltme sırasında kısa bir kapalı kalma süresi yönetim işlemlerini uygulanır ve yeni öğelerinizi koruyun veya geçici yedekleme işleri oluşturun. Iaas VM'ler için geri yükleme işleri, yükseltme sırasında çalıştırmayın. Kasa yükseltme tamamlamak için altında bir saat sürer. İşlemi tamamladıktan sonra kurtarma Hizmetleri kasasına yedekleme kasası yerini alır.

## <a name="changes-to-your-automation-and-tool-after-upgrading"></a>Otomasyon ve aracı yükseltme yaptıktan sonra yapılan değişiklikler

Altyapınızı kasası yükseltme için hazırlık yaparken, var olan Otomasyon veya yükseltmeden sonra çalışmaya devam ettiğinden emin olmak için araç güncelleştirmeniz gerekir.
PowerShell cmdlet'leri başvurularını başvurun [Service Manager dağıtım modeli](backup-client-automation-classic.md) ve [Resource Manager dağıtım modeli](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Yükseltmeden önce

Kurtarma hizmeti kasaları, yedekleme kasaları yükseltmeden önce aşağıdaki sorunları kontrol edin.

- **En düşük aracı sürümü**: kasanızı yükseltmek için Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı en az olduğundan emin olun sürüm 2.0.9083.0. MARS Aracısı'nı 2.0.9083.0 eskiyse, aracı yükseltme işlemini başlatmadan önce güncelleştirin.
- **Örnek tabanlı faturalama modeli**: kurtarma hizmeti kasalarını yalnızca örnek tabanlı fatura modelini destekler. Eski depolama tabanlı fatura modelini kullanarak bir yedekleme kasası varsa, fatura modelini yükseltme sırasında dönüştürün.
- **Yedekleme yapılandırması devam eden işlem yok**: yükseltme sırasında yönetim düzeyi erişimi sınırlıdır. Tüm yönetim düzlemi eylemleri tamamlayın ve ardından yükseltme işlemini başlatmak.

## <a name="using-powershell-scripts-to-upgrade-your-vaults"></a>Kasa yükseltmek için PowerShell betiklerini kullanma

Kurtarma Hizmetleri kasalarının, yedekleme kasaları yükseltmek için PowerShell komut dosyalarını kullanabilirsiniz. Kasa yükseltilmesini sağlamak için gerekli PowerShell bileşenleri olup olmadığını denetleyin. Yedekleme kasaları için PowerShell komut dosyaları için kurtarma Hizmetleri kasalarını çalışmaz. Kasa yükseltmek için ortamınızı hazırlayın:

1. Yükleme veya yükseltme [Windows Management Framework (WMF) sürüm 5](https://www.microsoft.com/download/details.aspx?id=50395) veya üstü.
2. [Azure PowerShell MSI yükleme](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Karşıdan [PowerShell Betiği](https://aka.ms/vaultupgradescript2) , kasa yükseltmek için.

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma

Kasa yükseltmek için aşağıdaki komut dosyasını kullanın. Aşağıdaki örnek komut dosyası parametrelerinin açıklamaları vardır.

RecoveryServicesVaultUpgrade 1.0.2.ps1 **- Subscriptionıd** `<subscriptionID>` **- VaultName** `<vaultname>` **-konum** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName**`<rgname>`

**Subscriptionıd** -yükseltiliyor kasa abonelik kimliği sayısı.<br/>
**VaultName** -yükseltiliyor yedekleme kasasının adını.<br/>
**Konum** -yükseltilen kasasının konumu.<br/>
**ResourceType** -BackupVault kullanın.<br/>
**TargetResourceGroupName** - Resource Manager tabanlı bir dağıtım için bir kaynak grubu belirtin kasa yükseltiyorsanız bu yana. Varolan bir kaynak grubunu kullanın veya yeni bir ad sağlayarak oluşturabilirsiniz. Bir kaynak grubu adını yazarsanız, yeni bir kaynak grubu oluşturabilir. Kaynak grupları hakkında daha fazla bilgi için bu okuma [kaynak grupları hakkında genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Kaynak grubu adları kısıtlamaları vardır. Yönergeleri izlediğinizden emin olun; Bunun Sağlanamaması kasası yükseltme başarısız olmasına neden olabilir.
>
>**Azure ABD devlet kurumları** "AzureUSGovernment" ortamına ayarlamak gereken betik çalıştırılırken müşteriler.
>**Azure Çin** "AzureChinaCloud" ortamına ayarlamak gereken betik çalıştırılırken müşteriler.

Aşağıdaki kod parçacığını hangi PowerShell komutu gibi görünmelidir bir örnektir:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

Komut dosyası hiçbir parametre olmadan da çalıştırabilirsiniz ve girişleri için gerekli tüm parametreleri vermeniz istenir.

PowerShell Betiği kimlik bilgilerinizi girmenizi ister. Kimlik bilgilerinizi iki kez girin: Service Manager hesabı ve kaynak yöneticisi hesabı için ikinci kez için bir kez.

### <a name="pre-requisites-checking"></a>Ön koşul denetimi
Azure kimlik bilgilerinizi girdikten sonra Azure ortamınıza aşağıdaki önkoşulları karşıladığını denetler:

- **En düşük aracı sürümü** -yedekleme kasaları kurtarma Hizmetleri kasalarının yükseltme gerektirir en az olacak şekilde MARS Aracısı sürümü 2.0.9083.0. Bir aracı ile 2.0.9083.0'den önceki bir yedekleme Kasası'na kayıtlı öğeler varsa, önkoşul denetimi başarısız olur. Önkoşul denetimi başarısız olursa aracıyı güncelleştirin ve kasa yükseltmeyi yeniden deneyin. Aracısı'ndan en son sürümünü indirebilirsiniz [http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe](http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Devam eden yapılandırma işleri**: birisi işi yükseltilmesi için ayarlanmış bir yedekleme kasası için yapılandırma ya da bir öğe kayıt, önkoşul denetimi başarısız olur. Yapılandırmayı tamamlamak veya öğeyi kaydolma işlemini tamamlamak ve kasa yükseltme işlemi başlatın.
- **Depolama tabanlı faturalama modeli**: Kurtarma Hizmetleri kasalarının örnek tabanlı fatura modelini destekler. Depolama tabanlı fatura modelini kullanan bir yedekleme kasası kasası yükseltme çalıştırırsanız, faturalama modelinizi Kasayla birlikte yükseltmeniz istenir. Faturalama modelinizi ilk olarak, aksi takdirde, güncelleştirebilir ve kasa yükseltme çalıştırın.
- Kurtarma Hizmetleri kasası için bir kaynak grubu tanımlayın. Resource Manager dağıtım özelliklerden yararlanmak için bir kaynak grubu bir kurtarma Hizmetleri kasası konulmalıdır. Varsa kullanın, bir ad sağlamak için hangi kaynak grubu tanımadığınız ve yükseltme işlemi kaynak grubu oluşturur. Yükseltme işlemi kasası ayrıca yeni kaynak grubu ile ilişkilendirir.

Ön koşul denetimi yükseltme işlemi tamamlandıktan sonra işlemi kasası yükseltmeyi başlatmak ister. Siz onayladıktan sonra yükseltme işlemi genellikle, tamamlanması kasanızı boyutuna bağlı olarak yaklaşık 15-20 dakika sürer. Büyük bir kasa varsa, yükseltme 90 dakikaya kadar sürebilir.

## <a name="managing-your-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarını yönetme

Aşağıdaki ekranlarını Azure portalında yedekleme Kasası'ndan yükseltme yeni bir kurtarma Hizmetleri kasası göster. İlk ekranda kasa için anahtar varlıkları görüntüler kasa Panosu gösterilir.

![Kurtarma Hizmetleri kasasına yedekleme Kasası'nı yükseltme örneği](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

İkinci ekranı bağlantılar yardımcı olmak kullanılabilir kurtarma Hizmetleri kasası kullanmaya başlamanıza yardım gösterir.

![Hızlı Başlangıç dikey penceresinde Yardım bağlantıları](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Yükseltme sonrası adımlar
Kurtarma Hizmetleri kasasına yedekleme ilkesine belirten saat dilimi bilgilerini destekler. Kasa başarıyla yükseltildikten sonra kasa ayarları menüsünden yedekleme ilkeleri gidin ve her bir kasaya yapılandırılmış ilkeler için saat dilimi bilgilerini güncelleştirin. Bu ekran zaten kullanıldığında yerel saat dilimi ilke oluşturuldu olarak belirtilen yedekleme zamanlaması saati gösterir. 

## <a name="enhanced-security"></a>Geliştirilmiş güvenlik

Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltildiğinde, kasa için güvenlik ayarlarını otomatik olarak etkinleştirilir. Ne zaman güvenlik ayarlarını, yedeklemeler silme gibi bazı işlemleri bulunan veya bir parola değiştirilmesini gerektiren bir [Azure çok faktörlü kimlik doğrulaması](../multi-factor-authentication/multi-factor-authentication.md) PIN. Gelişmiş Güvenlik ile ilgili daha fazla bilgi için bkz: [karma yedekleri korumak için güvenlik özellikleri](backup-azure-security-feature.md). 

Gelişmiş Güvenlik etkinleştirildiğinde, veri yedekleme kurtarma noktası bilgilerini kasadan silindikten sonra 14 gün için tutulur. Müşteriler bu güvenlik veri depolama için faturalandırılır. Güvenlik veri saklama Azure Yedekleme aracısı, Azure yedekleme sunucusu ve System Center Data Protection Manager için alınan kurtarma noktaları için geçerlidir. 

## <a name="gather-data-on-your-vault"></a>Kasanızda verileri toplayın

Bir kurtarma Hizmetleri Kasası'na yükseltme yaptıktan sonra (Iaas Vm'leri ve Microsoft Azure kurtarma Hizmetleri (MARS)) raporları için Azure yedeklemeyi yapılandırma ve Power BI raporlarına erişmek için kullanın. Makale veri toplamayı hakkında ek bilgi için bkz: [Azure Yedekleme'yi yapılandırma raporları](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Yükseltme planı devam eden yedeklerim etkiliyor mu?**</br>
Hayır. Devam eden Yedeklemelerinizin kesintisiz sırasında ve yükseltmeden sonra devam eder.

**En kısa sürede yükseltmeyi düşünüyorsanız yok, my kasalarını ne olur?**</br>
Tüm yeni özellikler yalnızca kurtarma Hizmetleri kasaları için geçerli olduğundan, biz, kasa yükseltmenizi yönlendirmeye. Microsoft, sonunda Klasik Portalı'nı Kaldır. 1 Eylül 2017 başlangıç Microsoft Kurtarma Hizmetleri kasaları için yedekleme kasalarını otomatik yükseltme işlemini başlatacak. Kasım sonra 30,2017, PowerShell kullanarak yedekleme kasaları artık oluşturabilirsiniz. Kasanızı dilediğiniz zaman arasında otomatik olarak yükseltilebilir. Microsoft, mümkün olan en kısa sürede kasanızı yükseltme önerir.

**Bu yükseltme ortalaması my varolan araçları için nedir?**</br>
Resource Manager dağıtım modeli için araç güncelleştirin. Kurtarma Hizmetleri kasaları için oluşturulan Resource Manager dağıtım modelinde kullanın. Resource Manager dağıtım modeli için planlama ve, kasa fark Hesap önemlidir. 

**Yükseltme sırasında kadar kapalı kalma süresi var mı?**</br>
Yükseltilen kaynakları sayısına bağlıdır. (Korumalı örnekler birkaç onlarca) daha küçük dağıtımlar için tüm yükseltme 20 dakikadan kısa sürer. Büyük ölçekli dağıtımlarda, en çok bir saat sürer.

**I yükselttikten sonra geri alabilirsiniz?**</br>
Hayır. Kaynakları başarılı bir şekilde yükselttikten sonra geri alma desteklenmiyor.

**My abonelik ya da bunlar yükseltmesini yeteneğine sahip olup olmadığınızı görmek için kaynakları doğrulamak için?**</br>
Evet. Yükseltme ilk adımda kaynakları yükseltmesini yeteneğine sahip olduğunu doğrular. Ön koşul doğrulama başarısız olursa, yükseltme tamamlanamıyor tüm nedenlerle iletilerini alır.

**Kasa yükseltilmesini sağlamak hangi izinlerin kullanmalı mı?**</br>
Kasa yükseltme gerçekleştirmek için Azure Klasik portalında abonelik için ortak yönetici olarak eklenmelidir. Zaten Azure portalında sahibi olarak listelenen olsa bile, bu gereklidir. Abonelik için ortak yönetici olup olmadığını öğrenmek için Klasik Azure portalındaki abonelik için bir ortak yönetici eklemeyi deneyin. Bir ortak yönetici eklemek mümkün değilse, bir ortak yönetici ekleyebilirsiniz abonelik, Hizmet Yöneticisi veya ortak yönetici başvurun.

**My CSP tabanlı bir yedekleme kasası yükseltebilir miyim?**</br>
Hayır. Şu anda, CSP tabanlı Yedekleme kasalarını yükseltemezsiniz. Sonraki sürümlerde CSP tabanlı Yedekleme kasaları yükseltmek için destek ekleyeceğiz.

**Yükseltme sonrası my Klasik kasası görüntüleyebilir miyim?**</br>
Hayır. Görüntüleyemez veya Klasik kasanızı yükseltme sonrası yönetin. Yalnızca yeni Azure portalına kasa tüm yönetim eylemlerini kullanmak mümkün olacaktır.

**My yükseltmesi başarısız oldu, ancak gerektiren Aracısı tutulan makine güncelleştiriliyor, artık mevcut değil. Böyle bir durumda ne yapmalıyım?**</br>
Mağaza kullanmanız gerekiyorsa, bu makine için uzun vadeli bekletme olduktan sonra yedekleme kasası yükseltmeniz mümkün olmaz. Bu tür bir kasa yükseltmek için destek gelecek sürümlerde ekleyeceğiz.
Artık bu makine yedeklemelerini depolamak gerekmez, lütfen bu makineden kasa kaydı ve yükseltmeyi yeniden deneyin.

**Neden işleri bilgi Kaynaklarım için yükseltme sonrasında göremiyorum?**</br>
(MARS aracısı ve Iaas) yedeklemeler için izleme kurtarma Hizmetleri kasasına yedekleme kasası yükselttiğinizde aldığınız yeni bir özelliktir. İzleme bilgilerini hizmetiyle eşitleme 12 saat sürer.

**Bir sorunu nasıl bildirebilirim?**</br>
Herhangi bir kısmının kasası yükseltme başarısız olursa hata Operationıd listelenen not edin. Microsoft Support sorunu çözmek için proaktif olarak çalışır. Desteği'ne ulaşmak ya da adresinden bize e-posta rsvaultupgrade@service.microsoft.com abonelik kimliği, kasa adı ve Operationıd. Sorunu çözmek mümkün olan en kısa sürede dener. Açıkça Bunu yapmak için Microsoft tarafından belirtilmedikçe işlemi yeniden denemeyin.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleyi kullanın:</br>
[Bir Iaas VM'yi yedeklemek](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure yedekleme sunucuyu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server Yedekleme](backup-configure-vault.md).
