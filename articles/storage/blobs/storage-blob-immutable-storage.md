---
title: Azure Blob Depolama (Önizleme) için değişmez depolama | Microsoft Docs
description: Azure depolama SOLUCAN (yazma, okuma çok kez) verileri silinebilir olmayan ve değiştirilebilir olmayan bir durumda bir kullanıcı tarafından belirtilen zaman aralığı için depolamak kullanıcıların sağlayan Blob (nesne) depolama desteği sunar. Birçok düzenlenen sektörlerde, sn 17a-4(f) ile uyumlu bir şekilde ve diğer düzenlemelere verileri depolamak için özellikle Aracısı dağıtıcı kuruluşlar, bir kuruluşta SOLUCAN Azure Blob Depolama için destek sağlar.
services: storage
author: sangsinh
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: sangsinh
ms.component: blobs
ms.openlocfilehash: a69d26b8c60f25b5710e48500cc727421d9e5c9a
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263336"
---
# <a name="store-business-critical-data-in-azure-blob-storage-preview"></a>İş açısından kritik verilerin Azure Blob Depolama (Önizleme) Store

Azure Blob (nesne) depolama için sabit depolama SOLUCAN (yazma, okuma çok kez) durumunda bir Azure blob depolama alanındaki iş açısından kritik verileri depolamak kullanıcıların sağlar. Bu durum, verileri, kullanıcı tarafından belirlenen bir süre boyunca silinemez ve değiştirilemez hale getirir. Blobları oluşturulabilir ve okuma, ancak değil değiştirildi veya silindi, saklama aralığı süresince.

## <a name="overview"></a>Genel Bakış

Birçok düzenlenen sektörlerde, sn 17a-4(f) ile uyumlu bir şekilde ve diğer düzenlemelere verileri depolamak için özellikle Aracısı dağıtıcı kuruluşlar, bir kuruluşta sabit depolama sağlar.

Tipik kullanım alanları şunlardır:

- **Yasal Uyumluluk**: Azure Blob Depolama için sabit depolama Finans kuruluşları yardımcı olmak için tasarlanmıştır ve dağıtılmasından sn 17a-4(f), CFTC 1.31©-(d) STANDARTLAR vb. adres.

- **Belge bekletme güvenli**: kullanıcılar alır en fazla veri koruması gibi veri değiştirilemez veya hesabı yönetici ayrıcalıklarına sahip olanlar da dahil olmak üzere herhangi bir kullanıcı tarafından silindi, Blob Depolama sağlar.

- **Yasal tutma**: Azure Blob Depolama için sabit depolama hassas bilgileri bir veya suç vb. için kritik bir artıklığının durumda istenen süreyle depolamak kullanıcıların sağlar.

Sabit depolama sağlar:

- **Zaman tabanlı saklama ilkesi desteği:** Kullanıcılar verilerin belirli bir süre boyunca depolanması için ilkeler oluşturabilir.

- **Yasal tutma ilkesi desteği:** Saklama süresi bilinmediğinde kullanıcılar yasal tutma gereksinimi ortadan kalkana kadar geçerli olacak yasal saklama süreleri belirleyebilir.  Yasal tutma söz konusu olduğunda bloblar oluşturulabilir ve okunabilir ancak değiştirilemez veya silinemez. Her yasal tutma süreci, tanımlayıcı dize olarak kullanılan ve kullanıcı tarafından belirlenen bir alfasayısal etiketle (olay kimliği gibi) ilişkilendirilir.

- **Tüm blob katmanları için destek:** WORM ilkeleri Azure blob depolama katmanından bağımsızdır ve sık erişimli, seyrek erişimli katmanlara ve arşiv katmanlarına uygulanır. Bu sayede müşteriler verileri iş yüküne göre en uygun maliyetli katmanda depolarken verilerin de sabit tutulmasını sağlayabilir

- **Kapsayıcı düzeyinde yapılandırma:** Sabit Depolama özelliği kullanıcıların kapsayıcı düzeyinde zaman tabanlı saklama ilkeleri ve yasal tutma etiketleri yapılandırmasını sağlar.  Kullanıcılar kapsayıcı düzeyindeki basit ayarlarla zaman tabanlı saklama ilkeleri oluşturup kilitleyebilir, saklama aralıklarını uzatabilir ve yasal tutma ayarlayıp silebilir.  Bu ilkeler kapsayıcıdaki mevcut ve yeni tüm bloblara uygulanır.

- **Denetim Günlüğü desteği:** Her kapsayıcıda saklama aralığı uzatmaları için en fazla üç günlük dosyası olmak üzere kilitli zaman tabanlı saklama ilkeleri için en fazla beş zaman tabanlı saklama komutunu gösteren bir denetim günlüğü bulunur.  Zaman tabanlı saklama olaylarında günlük girişinde kullanıcı kimliği, komut türü, zaman damgaları ve saklama aralığı yer alır. Yasal tutma olaylarında günlük girişinde kullanıcı kimliği, komut türü, zaman damgaları ve yasal tutma etiketleri yer alır. Bu günlük, SEC 17a-4(f) düzenleme şartları doğrultusunda kapsayıcının tutulduğu süre boyunca muhafaza edilir. Tüm denetim ortamı etkinliklerinin daha ayrıntılı günlük kaydına [Azure Etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) sayfasından ulaşabilirsiniz. Düzenlemeler veya diğer amaçlar doğrultusunda ihtiyaç duyulabilecek günlüklerin düzenli olarak depolanması kullanıcının sorumluluğundadır.

Azure tüm ortak bölgelerde sabit depolama etkin.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Blob Depolama için sabit depolama SOLUCAN veya sabit ilkeleri iki tür destekler: zamana bağlı bekletme ve yasal tutma kuralı. Bu sabit ilkeleri nasıl oluşturacağınızı öğrenmek için [Başlarken](#Getting-started) bölümüne bakın.
Bir kapsayıcıya zaman tabanlı saklama ilkesi veya yasal tutma uygulandığında mevcut blobların tümü sabit (yazma ve silme korumalı) duruma geçer. Kapsayıcıya yüklenen yeni bloblar da sabit duruma geçer.

> [!IMPORTANT]
> Blobun SEC 17a-4(f) ve diğer düzenlemelere uygun olması amacıyla sabit (yazma ve silme korumalı) duruma gelmesi için zaman tabanlı saklama ilkesinin *kilitli* olması gerekir. İlkenin genellikle 24 saat olmak üzere makul bir süre içinde kilitlenmesi gerekir. *Kilidi açık* durumun kısa vadeli özellik denemeleri dışındaki amaçlarla kullanılması önerilir.

 Bir kapsayıcıya zaman tabanlı saklama ilkesi uygulandığında kapsayıcı içindeki tüm bloblar saklama süresinin *geçerli* olduğu süre boyunca sabit durumda kalır. Mevcut bloblar için geçerli olan saklama süresi, blob oluşturma zamanı ile kullanıcı tarafından belirtilen saklama aralığı arasındaki farka eşittir. Yeni bloblar için geçerli olan saklama süresi, kullanıcı tarafından belirtilen saklama aralığına eşittir. Kullanıcılar saklama aralığını değiştirebileceğinden geçerli olan saklama süresinin hesaplanması için kullanıcı tarafından belirtilen en son saklama aralığı değeri kullanılır.

> [!TIP]
> Örnek: Kullanıcı beş yıllık saklama süresine sahip bir zaman tabanlı ilke oluşturur.
> Bir yıl önce oluşturulan bir kapsayıcının içinde testblob1 adlı bir blob mevcuttur. testblob1 için geçerli olan saklama süresi dört yıl olacaktır.
> Kapsayıcıya testblob2 adlı yeni bir blob yüklenmiştir. Bu yeni blob için geçerli olan saklama süresi beş yıl olacaktır.

### <a name="legal-holds"></a>Yasal tutma

Yasal tutma durumunda ilgili yasal tutma durumu kaldırılana kadar mevcut olan ve yeni yüklenen tüm bloblar sabit durumda kalır.
Yasal tutma ayarlama ve silme hakkında bilgi almak için lütfen [Başlarken](#Getting-started) bölümüne bakın.

Bir kapsayıcıda aynı anda hem yasal tutma hem de zaman tabanlı saklama ilkesi bulunabilir. Geçerli saklama süresi sona erse dahi tüm yasal tutma durumları kaldırılana kadar kapsayıcı içindeki tüm bloblar sabit durumda kalacaktır. Buna karşın tüm yasal tutma ilkeleri silinse dahi geçerli olan saklama süresi boyunca blob sabit durumda kalacaktır.
Aşağıdaki tabloda farklı sabit tutma senaryolarında devre dışı bırakılacak blob işlemi türleri gösterilmektedir.
Blob REST API hakkında ayrıntılı bilgi için [Azure Blob Hizmeti API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) belgelerine bakın.

|Senaryo  |Blob Durumu  |İzin verilmeyen Blob İşlemleri  |
|---------|---------|---------|
|Blobdaki geçerli saklama süresi dolmadı ve/veya yasal tutma ayarlandı     |Sabit: hem silme hem de yazma korumalı         |Delete Container, Delete Blob, Put Blob1, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Blob üzerindeki geçerli saklama süresi doldu     |Yalnızca yazma korumalı (silme işlemlerine izin verilir)         |Put Blob, Put Block, Put Block List, Set Blob Metadata, Put Page, Set Blob Properties,  Snapshot Blob, Incremental Copy Blob, Append Block         |
|Tüm yasal tutma ilkeleri silindi ve kapsayıcıda zaman tabanlı saklama ilkesi ayarlanmadı     |Değiştirilebilir         |None         |
|WORM ilkesi oluşturulmadı (zaman tabanlı saklama veya yasal tutma)     |Değiştirilebilir         |None         |

> [!NOTE]
> Yukarıdaki tabloda yer alan ilk iki senaryoda blob oluşturmak için gerekli ilk Put Blob, Put Block List ve Put Block işlemlerine izin verilir ve sonrasındaki tüm işlemler engellenir.
> Sabit depolama yalnızca GPv2 ve blob depolama hesaplarında kullanılabilir ve aracılığıyla oluşturulmalıdır [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="pricing"></a>Fiyatlandırma

Bu özellik ek ücrete tabi değildir ve Sabit veriler normal, değiştirilebilir verilerle aynı şekilde fiyatlandırılır. İlgili fiyatlandırma ayrıntıları için [Azure Depolama Fiyatlandırması sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/) bakın.

### <a name="restrictions"></a>Kısıtlamalar

Genel önizleme boyunca aşağıdaki kısıtlamalar geçerli olacaktır:

- **Üretim verilerini veya iş açısından kritik verileri depolamayın**
- Tüm önizleme/NDA kısıtlamaları geçerlidir

## <a name="getting-started"></a>Başlarken

Azure Blob Depolama için sabit Azure depolama üzerinde bulunan en son sürümlerini desteklenen [Azure portalı](http://portal.azure.com), Azure [CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ve Azure [PowerShell](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018)

### <a name="azure-portal"></a>Azure portal

1. Sabit durumda tutulması gereken blobların depolanması için yeni bir kapsayıcı oluşturun veya mevcut bir kapsayıcıyı seçin.
 Kapsayıcının GPv2 depolama hesabında olması gerekir.
2. Aşağıda gösterilen şekilde kapsayıcı ayarlarında Erişim İlkesi'ne ve ardından **Sabit blob depolaması** altında **+ İlke Ekle**'ye tıklayın.

    ![Portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Zaman tabanlı saklamayı etkinleştirmek için açılan menüden Zaman Tabanlı Saklama'yı seçin.

    ![Bekletme](media/storage-blob-immutable-storage/portal-image-2.png)

4. İstenen saklama aralığı için gün değerini girin (en az bir gün girebilirsiniz)

    ![Saklama Aralığı](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Yukarıda göreceğiniz gibi ilkenin ilk durumu kilidi açık şeklindedir. Bu durum özelliği daha küçük bir saklama aralığıyla test ederek kilitlemeden önce ilke üzerinde değişiklik yapmanızı sağlar. Kilitleme, SEC 17a-4 gibi düzenlemelere uyumluluk için gereklidir.

5. ... üzerine sağ tıklayarak ilkeyi kilitlediğinizde aşağıdaki menü görünür:

    ![İlkeyi Kilitle](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    İlkeyi Kilitle’ye tıkladığınızda ilke durumu kilitli olarak gösterilir. İlke kilitlendikten sonra silinemez ve yalnızca saklama aralığının uzatılmasına izin verilir.

6. Yasal tutma ilkelerini etkinleştirmek için + İlke Ekle’ye tıklayın ve açılan menüden Yasal Tutma’yı seçin

    ![Yasal Tutma](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Bir veya daha fazla etiketle bir yasal tutma oluşturma

    ![Yasal tutma etiketlerini ayarlama](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

### <a name="cli-20"></a>CLI 2.0

`az extension add -n storage-preview` komutuyla [CLI uzantısını](http://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yükleyin

Yüklü uzantı zaten varsa, sabit depolama etkinleştirmek için aşağıdaki komutu kullanın: `az extension update -n storage-preview`

Özellik şu komut gruplarında bulunur (komutları görmek için "-h" parametresini ekleyin): `az storage container immutability-policy` ve `az storage container legal-hold`.

### <a name="powershell"></a>PowerShell

Sabit Depolama özelliği [PowerShell sürüm 4.4.0-önizleme](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) sürümünde desteklenir.
Özelliği etkinleştirmek için aşağıdaki adımları izleyin:

1. `Install-Module PowerShellGet –Repository PSGallery –Force` komutunu kullanarak en güncel PowerShellGet sürümünü yüklediğinizden emin olun
2. Eski Azure PowerShell yüklemelerini kaldırın
3. AzureRM yükleyin (Azure da benzer şekilde bu depodan yüklenebilir) `Install-Module AzureRM –Repository PSGallery –AllowClobber`
4. Depolama yönetim ortamı cmdlet’lerinin `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber` önizleme sürümünü yükleyin

Özelliğin kullanımını gösteren örnek bir PowerShell kodu aşağıda verilmiştir.

## <a name="client-libraries"></a>İstemci kitaplıkları

Azure Blob Depolama için sabit depolama istemci kitaplığı sürümlerde desteklenir

- [.net İstemci Kitaplığı (sürüm 7.2.0-önizleme ve üzeri](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [node.js İstemci Kitaplığı (sürüm 4.0.0 ve üzeri)](https://www.npmjs.com/package/azure-arm-storage)
- [Python İstemci Kitaplığı (sürüm 2.0.0 Sürüm Adayı 2 ve üzeri)](https://pypi.org/project/azure-mgmt-storage/2.0.0rc1/)

## <a name="supported-values"></a>Desteklenen değerler

- Saklama aralığı en az bir gün, en fazla 400 yıldır
- Belirli bir depolama hesabı için depolama hesabı başına kilitli sabit ilkeye sahip kapsayıcı sayısı en fazla 1000 olabilir
- Belirli bir depolama hesabı için yasal tutma ayarına sahip kapsayıcı sayısı en fazla 1000 olabilir
- Belirli bir kapsayıcı için yasal tutma etiketi sayısı en fazla 10 olabilir
- Yasal tutma etiketi uzunluğu en fazla 23 alfasayısal karakter, en az üç karakter olabilir
- Belirli bir kapsayıcıda kilitli sabit ilkeler için saklama süresi en fazla üç kez uzatılabilir
- Kilitli sabit ilkeye sahip bir kapsayıcı için, kapsayıcının süresi boyunca en fazla beş zaman tabanlı bekletme ilkesi günlüğü ve en fazla 10 yasal tutma ilkesi günlüğü saklanabilir.

## <a name="faq"></a>SSS

**Bu özellik yalnızca blok blobları için mi geçerli, sayfa ve ekleme blobları için de geçerli mi?**

Sabit depolama BLOB'ları için herhangi bir blob türü ile kullanılabilir.  Ancak bu özelliğin çoğunlukla blok blobları ile kullanılması önerilir. Sayfa blobları ve ekleme blobları, blok bloblarından farklı olarak bir WORM kapsayıcısının dışında oluşturulup daha sonra içine kopyalanmalıdır.  WORM kapsayıcısına kopyalandıktan sonra ekleme blobuna *ekleme* veya sayfa blobunda değişiklik yapılamaz.

**Bu özelliği kullanabilmek için her seferinde yeni bir depolama hesabı mı oluşturmam gerekiyor?**

Hesap türü GPv2 ise sabit depolama mevcut tüm GPv2 hesaplarını veya daha yeni bir depolama hesabı kullanabilirsiniz. Bu özellik yalnızca blob depolama için kullanılabilir.

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir kapsayıcıyı silmeye çalışırsam ne olur?**

Kilitli zaman tabanlı saklama veya yasal tutma ilkesine sahip en az bir blob varsa Delete Container işlemi başarısız olur. Veriler [geçici olarak silinmiş](storage-blob-soft-delete.md) olsa bile bu durum geçerlidir. Etkin saklama aralığına ve yasal tutma ilkesine sahip bir blob yoksa Delete Container işlemi başarısız olur. Kapsayıcıyı silebilmek için önce blobları silmeniz gerekir. 

**Zaman tabanlı saklama veya yasal tutma ilkesi ile *kilitlenmiş* bir WORM kapsayıcısı bulunan depolama hesabını silmeye çalışırsam ne olur?**

Yasal tutma ilkesine veya zaman tabanlı saklama aralığına sahip en az bir WORM kapsayıcısı olması halinde depolama hesabı silme işlemi başarısız olur.  Depolama hesabının silinebilmesi için tüm WORM kapsayıcılarının silinmesi gerekir.  Kapsayıcı silme hakkında bilgi için önceki soruya bakın.

**Blob sabit durumda olduğunda farklı blob katmanları (sık erişimli, seyrek erişimli, soğuk depolama) arasında veri aktarımı gerçekleştirebilir miyim?**

Evet, Set Blob Tier komutunu kullanarak verileri katmanlar arasında aktarabilir, verileri sabit durumda tutabilirsiniz. Sabit bir depolama blobu sık erişimli, seyrek erişimli ve soğuk katmanlar üzerinde desteklenir.

**Ödemeyi yapamadıysam ve saklama süresi dolmadıysa ne olur?**

Ödeme yapılmaması durumunda normal veri saklama ilkeleri Microsoft ile aranızdaki sözleşmenin hüküm ve koşullarında belirtilen yetkisiz kullanım süresi boyunca uygulanmaya devam eder.

**Özelliği deneyebilmek için deneme veya yetkisiz kullanım süresi sunuyor musunuz?**

Evet, zaman tabanlı saklama ilkesi oluşturulduğunda *kilidi açık* durumda olur. Bu durumda saklama süresini artırabilir, azaltabilir ve hatta ilkeyi silebilirsiniz. İlke kilitlendikten sonra süresiz olarak kilitli durumda kalır ve silme işlemi gerçekleştirilemez. Ayrıca ilke kilitlendikten sonra saklama süresi kısaltılamaz. *Kilidi açık* durumunu yalnızca deneme amacıyla kullanmanızı ve SEC 17a-4(f) ile diğer düzenlemelerle uyumsuzluk riski oluşturmamak adına ilkeyi 24 saat içinde kilitlemenizi öneririz.

**Bu özellik ulusal ve kamu bulutlarında mevcut mu?**

Yalnızca Azure ortak bölgelerde sabit depolama şu anda kullanılabilir. Belirli bir ulusal bulutla ilgili talepleriniz için azurestoragefeedback@microsoft.com adresine e-posta gönderebilirsiniz.

## <a name="sample-code"></a>Örnek kod

Aşağıda referans olarak kullanabileceğiniz örnek bir PowerShell betiği verilmiştir.
Bu betik yeni bir depolama hesabı ve kapsayıcı oluşturup yasal tutma belirleme ve silme, zaman tabanlı saklama ilkesi (Sabitleme İlkesi olarak da bilinir) oluşturma ve kilitleme ile saklama süresini uzatma gibi adımları nasıl gerçekleştireceğinizi gösterir.

```powershell
\$ResourceGroup = "\<Enter your resource group\>”

\$StorageAccount = "\<Enter your storage account name\>"

\$container = "\<Enter your container name\>"

\$container2 = "\<Enter another container name\>”

\$location = "\<Enter the storage account location\>"

\# Login to the Azure Resource Manager Account

Login-AzureRMAccount

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

\# Create your Azure Resource Group

New-AzureRmResourceGroup -Name \$ResourceGroup -Location \$location

\# Create your Azure storage account

New-AzureRmStorageAccount -ResourceGroupName \$ResourceGroup -StorageAccountName
\$StorageAccount -SkuName Standard_LRS -Location \$location -Kind Storage

\# Create a new container

New-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container

\# Create Container 2 with Storage Account object

\$accountObject = Get-AzureRmStorageAccount -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount

New-AzureRmStorageContainer -StorageAccount \$accountObject -Name \$container2

\# Get container

Get-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container

\# Get Container with Account object

\$containerObject = Get-AzureRmStorageContainer -StorageAccount \$accountObject
-Name \$container

\#list container

Get-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount

\#remove container (Add -Force to dismiss prompt)

Remove-AzureRmStorageContainer -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container2

\#with Account object

Remove-AzureRmStorageContainer -StorageAccount \$accountObject -Name
\$container2

\#with Container object

\$containerObject2 = Get-AzureRmStorageContainer -StorageAccount \$accountObject
-Name \$container2

Remove-AzureRmStorageContainer -InputObject \$containerObject2

\#Set LegalHold

Add-AzureRmStorageContainerLegalHold -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container -Tag tag1,tag2

\#with Account object

Add-AzureRmStorageContainerLegalHold -StorageAccount \$accountObject -Name
\$container -Tag tag3

\#with Container object

Add-AzureRmStorageContainerLegalHold -Container \$containerObject -Tag tag4,tag5

\#Clear LegalHold

Remove-AzureRmStorageContainerLegalHold -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -Name \$container -Tag tag2

\#with Account object

Remove-AzureRmStorageContainerLegalHold -StorageAccount \$accountObject -Name
\$container -Tag tag3,tag5

\#with Container object

Remove-AzureRmStorageContainerLegalHold -Container \$containerObject -Tag tag4

\# create/update ImmutabilityPolicy

\#\# with account/container name

Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -ContainerName \$container
-ImmutabilityPeriod 10

\#with Account object

Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container -ImmutabilityPeriod 1 -Etag \$policy.Etag

\#with Container object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -ImmutabilityPeriod 7

\#\# with ImmutabilityPolicy object

Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy \$policy
-ImmutabilityPeriod 5

\#get ImmutabilityPolicy

Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName \$ResourceGroup
-StorageAccountName \$StorageAccount -ContainerName \$container

\#with Account object

Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container

\#with Container object

Get-AzureRmStorageContainerImmutabilityPolicy -Container \$containerObject

\#Lock ImmutabilityPolicy (Add -Force to dismiss prompt)

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy
\$policy -force

\#\# with account/container name

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-Etag \$policy.Etag

\#with Account object

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount
\$accountObject -ContainerName \$container -Etag \$policy.Etag

\#with Container object

\$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -Etag \$policy.Etag -force

\#Extend ImmutabilityPolicy

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy
\$policy -ImmutabilityPeriod 11 -ExtendPolicy

\#\# with account/container name

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-ImmutabilityPeriod 11 -Etag \$policy.Etag -ExtendPolicy

\#with Account object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount
\$accountObject -ContainerName \$container -ImmutabilityPeriod 12 -Etag
\$policy.Etag -ExtendPolicy

\#with Container object

\$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container
\$containerObject -ImmutabilityPeriod 13 -Etag \$policy.Etag -ExtendPolicy

\#Remove ImmutabilityPolicy (Add -Force to dismiss prompt)

\#\# with ImmutabilityPolicy object

\$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container

Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy \$policy

\#\# with account/container name

Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName
\$ResourceGroup -StorageAccountName \$StorageAccount -ContainerName \$container
-Etag \$policy.Etag

\#with Account object

Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount \$accountObject
-ContainerName \$container -Etag \$policy.Etag

\#with Container object

Remove-AzureRmStorageContainerImmutabilityPolicy -Container \$containerObject
-Etag \$policy.Etag
```
