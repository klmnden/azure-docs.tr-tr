---
title: Azure depolama BLOB'ları için sabit depolama | Microsoft Docs
description: Azure depolama, kullanıcıların verileri silinebilir olmayan ve değiştirilebilir olmayan bir durumda, belirtilen bir zaman aralığı için depolamak olanak sağlayan Blob (nesne) depolama SOLUCAN (bir kez yazma, okuma çok) desteği sunar.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 09/18/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 38e34391294e1a070d506583fbc30dcdb703bea0
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156910"
---
# <a name="store-business-critical-data-in-azure-blob-storage"></a>İş açısından kritik verilerin Azure Blob Depolama alanında Store

Sabit depolama (nesne) Azure Blob Depolama için kullanıcıların bir SOLUCAN (bir kez yazma, okuma çok) durumda iş açısından kritik verileri depolamak sağlar. Bu durum verileri silinebilir olmayan ve değiştirilebilir olmayan bir kullanıcı tarafından belirtilen aralığı için yapar. Blobları oluşturulabilir ve okuma, ancak değil değiştirildi veya silindi, saklama aralığı süresince.

## <a name="overview"></a>Genel Bakış

Sabit depolama Finans kuruluşları ve dağıtılmasından--verileri güvenli bir şekilde depolamak için özellikle Aracısı dağıtıcı kuruluşların--yardımcı olur. Ayrıca, hiçbir senaryoda silinmeye karşı kritik verileri korumak için de yararlanılabilir.  

Tipik kullanım alanları şunlardır:

- **Yasal Uyumluluk**: Azure Blob Depolama için sabit depolama kuruluşların adresi sn 17a-4(f), CFTC 1.31(d), STANDARTLAR ve diğer düzenlemelere yardımcı olur.

- **Belge bekletme güvenli**: Blob Depolama, veri değiştirilemez veya hesabı yönetici ayrıcalıklarına sahip kullanıcılar dahil olmak üzere herhangi bir kullanıcı tarafından silinmiş olduğunu sağlar.

- **Yasal tutma**: Azure Blob Depolama için sabit depolama veya bir suç artıklığının durumda istenen süre için kritik önem taşır hassas bilgileri depolamak kullanıcıların sağlar.

Sabit depolama sağlar:

- **Zamana bağlı Bekletme İlkesi desteği**: kullanıcılar için belirli bir aralıkta verileri depolamak için ilkeler ayarlama.

- **Yasal tutma ilkesi desteği**: elde tutma aralığı bilinmiyor, kullanıcıların yasal tutma temizlenene kadar immutably verileri depolamak için yasal tutma kuralı ayarlayabilirsiniz.  Yasal tutma söz konusu olduğunda bloblar oluşturulabilir ve okunabilir ancak değiştirilemez veya silinemez. Her yasal tutma süreci, tanımlayıcı dize olarak kullanılan ve kullanıcı tarafından belirlenen bir alfasayısal etiketle (olay kimliği gibi) ilişkilendirilir.

- **Tüm katmanları blob desteği**: SOLUCAN ilkeleri, Azure Blob Depolama katmanını bağımsızdır ve katmanları için geçerlidir: sık erişimli, seyrek erişimli ve Arşiv. Kullanıcılar, veri değiştirilemezlik korurken veri iş yüklerini en maliyet açısından iyileştirilmiş katmanına geçiş yapabilirsiniz.

- **Kapsayıcı düzeyi yapılandırma**: kullanıcılar, zamana bağlı bekletme ilkeleri yapılandırabilir ve kapsayıcı düzeyinde yasal tutma etiketler. Basit kapsayıcı düzeyi ayarlarını kullanarak, kullanıcılar oluşturabilir ve kilitleme zamana bağlı bekletme ilkeleri, bekletme aralıkları genişletme, ayarlayın ve yasal tutma kuralı ve daha fazlasını temizleyin. Bu ilkeleri, mevcut ve yeni kapsayıcıdaki tüm blobları için geçerlidir.

- **Oturum açma desteği denetim**: denetim günlüğü her kapsayıcı içerir. Bu üç günlük bekletme aralığı uzantıları için en fazla kilitli zamana bağlı bekletme ilkeleri için en fazla beş zamana bağlı bekletme komutları gösterir. Zamana bağlı bekletme için kullanıcı kimliği, komut türü, zaman damgaları ve bekletme aralığı günlük içerir. Yasal tutma kuralı günlüğü, kullanıcı kimliği, komut türü, zaman damgalarını içerir ve yasal tutma etiketler. Bu günlük, yaşam süresi, sn 17a-4(f) yasal yönergelerine uygun olarak kapsayıcı sağlamak için tutulur. [Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) daha kapsamlı bir günlük denetim düzlemi etkinlikleri gösterir. Bu günlük dosyaları sınıflandırmanıza, yasal veya diğer amaçlar için gerekli olduğu gibi depolamak kullanıcının sorumluluğundadır.

Azure tüm ortak bölgelerde sabit depolama etkin.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Blob Depolama için sabit depolama SOLUCAN veya sabit ilkeleri iki tür destekler: zamana bağlı bekletme ve yasal tutma kuralı. Bu sabit ilkelerinin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [Başlarken](#Getting-started) bölümü.

Zamana bağlı bekletme ilkesi veya yasal tutma bir kapsayıcı uygulandığında, tüm mevcut blobları değişmez Taşı (yazma ve silme korumalı) durumu. Kapsayıcıya yüklenir ve yeni tüm bloblar da değişmez durumuna taşınır.

> [!IMPORTANT]
> Zamana bağlı bekletme ilkesi olmalıdır *kilitli* bir sabit olarak blob için (yazma ve silme korumalı) sn 17a-4(f) ve diğer yasal uyumluluk durumunda. Bir makul süre içinde genellikle 24 saat içindeki ilke kilitleme öneririz. Kullanımı önerilmemektedir *kilidi* kısa süreli özellik denemeler dışındaki herhangi bir amaçla durum.

Bir kapsayıcı zamana bağlı Bekletme İlkesi uygulandığında, kapsayıcıdaki tüm blob'lara süresi boyunca sabit durumda kalır *etkili* saklama süresi. Mevcut bloblar için geçerli olan saklama süresi, blob oluşturma zamanı ile kullanıcı tarafından belirtilen saklama aralığı arasındaki farka eşittir.

Yeni bloblar için geçerli olan saklama süresi, kullanıcı tarafından belirtilen saklama aralığına eşittir. Kullanıcıları elde tutma aralığı genişletebilirsiniz çünkü sabit depolama en son değeri kullanıcı tarafından belirtilen bekletme aralığının geçerli saklama dönemi hesaplamak için kullanır.

> [!TIP]
> Örnek:
>
> Bir kullanıcı bir zamana bağlı bekletme ilkesi ile beş yıllık bir bekletme aralığı oluşturur.
>
> Mevcut blob testblob1, bu kapsayıcıdaki bir yıl önce oluşturuldu. Testblob1 etkili saklama süresini dört yıldır.
>
> Kapsayıcıya testblob2 adlı yeni bir blob yüklenmiştir. Bu yeni blob için geçerli saklama süresi, beş yıl ' dir.

### <a name="legal-holds"></a>Yasal tutma

Yasal tutma ayarlandığında, tüm mevcut ve yeni BLOB'lar yasal tutma temizlenene kadar değişmez durumda kalır. Ayarlama ve Temizle yasal tutma kuralı hakkında daha fazla bilgi için bkz. [Başlarken](#Getting-started) bölümü.

Bir kapsayıcı, aynı anda hem yasal tutma hem de zamana bağlı bekletme ilkesi olabilir. Tüm yasal tutma kuralı temizlenir kadar bu kapsayıcıdaki tüm blob'lara, etkin tutma süresi dolmuş olsa bile değişmez durumda kalır. Buna karşılık, etkin tutma süresi dolana kadar tüm yasal tutma kuralı temizlenmiş olsa bile bir blob değişmez bir durumda kalır.

Aşağıdaki tabloda farklı bir sabit senaryolar için devre dışı bırakılmış blob işlemlerin türlerini gösterir. Daha fazla bilgi için [Azure Blob hizmeti API'si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) belgeleri.

|Senaryo  |BLOB durumu  |İzin blob işlemleri  |
|---------|---------|---------|
|Blobdaki geçerli saklama süresi dolmadı ve/veya yasal tutma ayarlandı     |Sabit: hem silme hem de yazma korumalı         |Kapsayıcı, Blob, Put Blob Sil<sup>1</sup>, blok yerleştirme<sup>1</sup>, engelleme listesine koymak<sup>1</sup>, Blob meta verileri ayarlama sayfası yerleştirmek, Blob özelliklerini, anlık görüntü Blob artımlı kopya Blob ayarlayın Blok ekleme         |
|Blob üzerindeki geçerli saklama süresi doldu     |Yalnızca yazma korumalı (silme işlemlerine izin verilir)         |İkili büyük nesne koyma<sup>1</sup>, blok yerleştirme<sup>1</sup>, engelleme listesine koymak<sup>1</sup>, Blob meta verileri ayarlama, sayfa yerleştirme, Blob özelliklerini, anlık görüntü Blob artımlı kopya blob'u ayarlayın, blok ekleme         |
|Tüm yasal temizlenmiş tutar ve zamana bağlı bekletme ilkesi yok, kapsayıcıdaki ayarlanır     |Değiştirilebilir         |None         |
|Hiçbir SOLUCAN İlkesi (zamana bağlı bekletme veya yasal tutma) oluşturulur.     |Değiştirilebilir         |None         |

<sup>1</sup> kez bir blob oluşturmak için bu işlem uygulamayı çağırabilir. Blob üzerindeki tüm sonraki işlemleri izin verilmiyor.

> [!NOTE]
>
> Sabit depolama, yalnızca genel amaçlı V2 ve Blob Depolama hesapları kullanılabilir. Hesap aracılığıyla oluşturulmalıdır [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="pricing"></a>Fiyatlandırma

Bu özelliği kullanmak için ek ücret yoktur. Sabit veri aynı şekilde normal, kesilebilir veri olarak fiyatlandırılır. Azure Blob Depolama fiyatlandırma ayrıntıları için bkz: [Azure depolama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).


## <a name="getting-started"></a>Başlarken

En son sürümleri [Azure portalında](http://portal.azure.com) ve [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) önizleme sürümünü yanı sıra [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018) sabit depolama Azure Blob Depolama için destek.

### <a name="azure-portal"></a>Azure portal

1. Sabit durumda tutulması gereken blobların depolanması için yeni bir kapsayıcı oluşturun veya mevcut bir kapsayıcıyı seçin.
 Kapsayıcı bir GPv2 veya blob depolama hesabında olmalıdır.
2. Seçin **erişim ilkesi** kapsayıcı ayarları. Ardından **+ ilke Ekle** altında **sabit blob depolaması**.

    ![Portalda kapsayıcı ayarları](media/storage-blob-immutable-storage/portal-image-1.png)

3. Zamana bağlı bekletme etkinleştirmek için seçin **zamana bağlı bekletme** aşağı açılan menüden.

    !["Zamana bağlı bekletme"İlke türü altında"Seçili"](media/storage-blob-immutable-storage/portal-image-2.png)

4. Bekletme aralığı gün içinde (en az bir gündür) girin.

    !["Güncelleştirme saklama dönemini" kutusu](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Ekran görüntüsünde görüldüğü gibi ilkeyi Başlangıç durumu kilitli değil. Özelliği daha küçük bir bekletme aralığı ile test etmek ve bunu kilitlediğinizde, ilkeye değişiklikleri yapın. Kilitleme sn 17a-4 gibi düzenlemelerle uyumluluğu için önemlidir.

5. İlke kilitleyin. Üç nokta simgesine sağ tıklayın (**...** ), ve aşağıdaki menü görünür:

    !["İlkesi menüsünde kilitleme"](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Seçin **kilitleme ilkesi**, ve ilkenin durumunu kilitli olarak görünür. İlke kilitlendikten sonra silinemez ve yalnızca uzantıları bekletme aralığının izin verilir.

6. Yasal tutma Kuralı etkinleştirmek için seçin **+ ilke Ekle**. Seçin **yasal tutma** aşağı açılan menüden.

    !["Bir yasal tutma" altında "İlke türü" menüsünde](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Yasal tutma, bir veya daha fazla etiket ile oluşturun.

    ![İlke türü altında "Etiket adı" kutusu](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

8. Yasal tutma temizlemek için etiketi kaldırmak yeterlidir.

### <a name="azure-cli"></a>Azure CLI

Bu özellik aşağıdaki komut gruplarını dahildir: `az storage container immutability-policy` ve `az storage container legal-hold`. Çalıştırma `-h` bunlara komutları görmek için.

### <a name="powershell"></a>PowerShell

[PowerShell sürüm 4.4.0-preview](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) değişmez depolamayı destekler.
Özelliği etkinleştirmek için aşağıdaki adımları izleyin:

1. PowerShellGet yüklü en son sürümüne sahip olduğunuzdan emin olun: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Azure PowerShell'in önceki tüm yüklemesini kaldırın.
3. Yükleme AzureRM: `Install-Module AzureRM –Repository PSGallery –AllowClobber`. Azure, bu depodan benzer şekilde yüklenebilir.
4. Depolama yönetim düzlemi cmdlet'leri önizleme sürümünü yükleyin: `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`.

[Örnek PowerShell kodu](#sample-powershell-code) bu makalenin devamındaki bölümüne özellik kullanımı gösterilmektedir.

## <a name="client-libraries"></a>İstemci kitaplıkları

Aşağıdaki istemci kitaplıkları, Azure Blob Depolama için sabit depolamayı destekler:

- [.NET istemci kitaplığı sürümü 7.2.0-preview ve üzeri](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [Node.js istemci kitaplığı sürüm 4.0.0 ve üzeri](https://www.npmjs.com/package/azure-arm-storage)
- [Python istemci kitaplığı sürüm 2.0.0 Sürüm Adayı 2 ve üzeri](https://pypi.org/project/azure-mgmt-storage/2.0.0rc2/)
- [Java istemci kitaplığı](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/storage/resource-manager/Microsoft.Storage/preview/2018-03-01-preview)

## <a name="supported-values"></a>Desteklenen değerler

- En düşük bekletme aralığı, bir gündür. Maksimum 400 yıldır.
- Bir depolama hesabı için en fazla kapsayıcı kilitli sabit ilkeleriyle 1.000 sayısıdır.
- Bir depolama hesabı için kapsayıcılar yasal tutma ayarı sayısı 1000 ' dir.
- Bir kapsayıcı için yasal tutma etiket sayısı 10'dur.
- Bir yasal etiket uzunluğu en fazla 23 alfasayısal karakter olabilir. En az üç karakter uzunluğundadır.
- Bir kapsayıcı için kilitli sabit ilkeleri için izin verilen bekletme aralığı uzantıları sayısı üçüncü bölümüdür.
- Kilitli bir sabit ilkesiyle bir kapsayıcı için en fazla beş zamana bağlı bekletme ilkesi günlükleri ve en fazla 10 yasal günlükleri kapsayıcı süresince korunur. ilke tutun.

## <a name="faq"></a>SSS

**Bu özellik, yalnızca blok blobları, veya sayfa ve ekleme blobları da için geçerli midir?**

Sabit depolama ile herhangi bir blob türü kullanılabilir, ancak çoğunlukla blok blobları için kullanmanızı öneririz. Aksine blok blobları, sayfa blobları ve ekleme blobları dışında bir SOLUCAN container oluşturulması gerekir ve ardından içinde kopyalanır. Bu bloblar bir SOLUCAN kapsayıcıya Hayır daha da kopyaladığınız sonra *ekler* için bir ekleme blobu veya bir sayfa blobu değişiklikleri izin verilir.

**Bu özelliği kullanabilmek için her seferinde yeni bir depolama hesabı mı oluşturmam gerekiyor?**

Tüm mevcut veya yeni oluşturulan genel amaçlı V2 veya Blob Depolama hesapları ile sabit depolamayı kullanabilirsiniz. Bu özellik yalnızca Blob Depolama için kullanılabilir.

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir kapsayıcıyı silmeye çalışırsam ne olur?**

Kilitli zamana bağlı bekletme ilkesi veya yasal tutma ile en az bir blob mevcut kapsayıcı silme işlemi başarısız olur. Kapsayıcı silme işlemi yalnızca bir etkin tutma aralığı ile hiçbir blob mevcut olduğundan ve hiçbir yasal tutma kuralı vardır başarılı olur. Kapsayıcıyı silmeden önce BLOB'ları silmeniz gerekir.

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir WORM kapsayıcısı bulunan depolama hesabını silmeye çalışırsam ne olur?**

Yasal tutma ilkesine veya zaman tabanlı saklama aralığına sahip en az bir WORM kapsayıcısı olması halinde depolama hesabı silme işlemi başarısız olur.  Depolama hesabını silmeden önce tüm SOLUCAN kapsayıcıları silmeniz gerekir. Önceki soruyu kapsayıcı silme hakkında daha fazla bilgi için bkz.

**Blob sabit durumda olduğunda farklı blob katmanları (sık erişimli, seyrek erişimli, soğuk depolama) arasında veri aktarımı gerçekleştirebilir miyim?**

Evet, Set Blob Tier komutunu kullanarak verileri katmanlar arasında aktarabilir, verileri sabit durumda tutabilirsiniz. Sabit depolama desteklenir sık, seyrek ve Arşiv katmanları blob.

**Ödemeyi yapamadıysam ve saklama süresi dolmadıysa ne olur?**

Ödeme söz konusu olduğunda, normal veri bekletme ilkeleri görünürlüğe hüküm ve koşullar sözleşmenizin Microsoft ile uygulanır.

**Özelliği deneyebilmek için deneme veya yetkisiz kullanım süresi sunuyor musunuz?**

Evet. Zamana bağlı bekletme ilkesini ilk oluşturulduğunda bulunduğu bir *kilidi* durumu. Bu durumda, bekletme aralığı, artış gibi istediğiniz herhangi bir değişiklik yapmak veya azaltma ve bile ilkeyi silin. İlke kilitlendikten sonra sonsuza kadar silinemediğini kilitli kalır. Ayrıca ilke kilitlendikten sonra saklama süresi kısaltılamaz. Kullanmanızı öneririz *kilidi* durum yalnızca deneme amacıyla ve ilkeyi bir 24 saatlik süre içinde kilitleyin. Bu yöntemler sn 17a-4(f) ve diğer düzenlemelere uygun yardımcı olur.

**Bu özellik ulusal ve kamu bulutlarında mevcut mu?**

Yalnızca Azure ortak bölgelerde sabit depolama şu anda kullanılabilir. Belirli bir Ulusal bulutta ilgileniyorsanız, e-posta azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>Örnek PowerShell kodu

Aşağıdaki örnek PowerShell Betiği için bir başvurudur. Bu betik, yeni depolama hesabı ve kapsayıcı oluşturur. Bunu ardından, ayarlayın ve yasal tutma kuralı temizleyin, oluşturma ve bir zamana bağlı bekletme ilkesi (değiştirilemezlik ilkesi olarak da bilinir) kilitleyin ve elde tutma aralığı genişletmek gösterilmektedir.

Ayarlama ve Azure depolama hesabına test:

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzureRMAccount
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzureRmResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind StorageV2

# Create a new container
New-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzureRmStorageContainer -InputObject $containerObject2
```

Ayarlayın ve yasal tutma kuralı temizleyin:

```powershell
# Set a legal hold
Add-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag1>,<tag2>,...

# with an account object
Add-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>

# with a container object
Add-AzureRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>,<tag5>,...

# Clear a legal hold
Remove-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag <tag2>

# with an account object
Remove-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag <tag3>,<tag5>

# with a container object
Remove-AzureRmStorageContainerLegalHold -Container $containerObject -Tag <tag4>
```

Oluşturma veya güncelleştirme değiştirilemezlik İlkesi:
```powershell
# with an account name or container name
Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

# with an account object
Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

# with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

# with an immutability policy object
Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5
```

Değiştirilemezlik İlkesi alın:
```powershell
# Get an immutability policy
Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# with an account object
Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# with a container object
Get-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject
```

Kilitleme değiştirilemezlik İlkesi (add - isteği kapatmak için zorla):
```powershell
# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

# with an account name or container name
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

# with a container object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force
```

Değiştirilemezlik İlkesi genişletin:
```powershell

# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

# with an account name or container name
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

# with an account object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

# with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy
```

Değiştirilemezlik ilkesini kaldırmak (add - isteği kapatmak için zorla):
```powershell
# with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

# with an account name or container name
Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

# with an account object
Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

# with a container object
Remove-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag

```
