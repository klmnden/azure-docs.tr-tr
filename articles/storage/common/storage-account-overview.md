---
title: Azure depolama hesabına genel bakış | Microsoft Docs
description: Oluşturma ve bir Azure depolama hesabı kullanma seçeneklerini anlama.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 52226d07595120395909dd5f47d5d896f5cdaa75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483658"
---
# <a name="azure-storage-account-overview"></a>Azure depolama hesabına genel bakış

Azure depolama hesabınız Azure Storage veri nesnelerinizi içerir: bloblar, dosyalar, kuyruklar, tablolar ve diskler. Azure depolama hesabınızdaki veriler, herhangi bir dünyada HTTP veya HTTPS üzerinden dayanıklı ve yüksek oranda kullanılabilir, güvenli, yüksek düzeyde ölçeklenebilir ve erişilebilir.

Bir Azure depolama hesabı oluşturma hakkında bilgi edinmek için [depolama hesabı oluşturma](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>Genel amaçlı v2 hesapları

Genel amaçlı v2 depolama hesabı için en son Azure depolama özelliklerini desteklemek ve tüm işlevleri, genel amaçlı v1 ve Blob Depolama hesapları dahil edilip derecelendirilir. Genel amaçlı v2 hesapları Azure depolama, ek olarak sektörde rekabetçi işlem fiyatları düşük gigabayt başına kapasite fiyatlar sunar. Genel amaçlı v2 depolama hesaplarının, bu Azure depolama hizmetleri destekler:

- BLOB'ları (tüm türleri: Engelleme, Sayfa Ekle)
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

> [!NOTE]
> Microsoft, çoğu senaryo için bir genel amaçlı v2 depolama hesabı kullanmanızı önerir. Genel amaçlı v2 hesabına kapalı kalma süresi olmadan ve verileri kopyalamak zorunda kalmadan kolayca bir genel amaçlı v1 veya Blob Depolama hesabına yükseltebilirsiniz.
>
> Genel amaçlı v2 hesabına yükseltme hakkında daha fazla bilgi için bkz. [yükseltmek için bir genel amaçlı v2 depolama hesabı](storage-account-upgrade.md).

Genel amaçlı v2 depolama hesaplarının, kullanım düzenlerini esas alarak verileri depolamak için birden çok erişim katmanı sunar. Daha fazla bilgi için [erişim katmanları için blok blob verilerine](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>Genel amaçlı v1 hesapları

Genel amaçlı v1 hesapları, tüm Azure depolama hizmetlerine erişim sağlar, ancak en son özelliklere veya gigabayt fiyatlandırma başına en düşük olamaz. Genel amaçlı v1 depolama hesaplarında, bu Azure depolama hizmetleri destekler:

- BLOB'ları (tüm türleri için)
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

Genel amaçlı v1 hesapları, genel amaçlı v2 hesapları çoğu durumda önerilir, ancak bu senaryolar için en uygun seçenektir:

* Uygulamalarınızı Azure Klasik dağıtım modelini gerektirir. Genel amaçlı v2 ve Blob Depolama hesapları yalnızca Azure Resource Manager dağıtım modelini destekler.

* Uygulamalarınızı yoğun işlem gücü kullanımlı veya önemli coğrafi çoğaltma bant genişliği kullanın, ancak büyük kapasite gerektirmez. Bu durumda, genel amaçlı v1 en ekonomik bir seçenek olabilir.

* 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

### <a name="block-blob-storage-accounts"></a>Blok blob depolama hesapları

Bir blok blob depolama hesabı, blok blobları olarak yapılandırılmamış nesne verilerini depolamak için bir özel depolama hesabı veya ekleme blobları. Blok blob depolama hesapları, kullanım düzenlerini esas alarak verileri depolamak için birden çok erişim katmanı sunar. Daha fazla bilgi için [erişim katmanları için blok blob verilerine](#access-tiers-for-block-blob-data).

### <a name="filestorage-preview-storage-accounts"></a>Dosya deposundan (Önizleme) depolama hesapları

Bir dosya deposundan depolama hesabı, premium dosya paylaşımları oluşturmak ve depolamak için kullanılan bir özel depolama hesabıdır. Dosya deposundan depolama hesaplarının IOPS Patlaması gibi ayrılmış Benzersiz performans özellikleri sunar. Bu özellikler hakkında daha fazla bilgi için bkz. [dosya paylaşımı performans katmanları](../files/storage-files-planning.md#file-share-performance-tiers) Planlama Kılavuzu dosyaları bölümü.

## <a name="naming-storage-accounts"></a>Depolama hesabı adlandırma

Depolama hesabınızı adlandırırken şu kuralları göz önünde bulundurun:

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
- Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. İki depolama hesabı aynı ada sahip olamaz.

## <a name="general-purpose-performance-tiers"></a>Genel amaçlı performans katmanları

Genel amaçlı depolama hesapları ya da aşağıdaki performans katmanları yapılandırılabilir:

* Bloblar, dosyalar, tabloları, kuyrukları ve Azure sanal makine disklerini depolamak için standart performans katmanı.
* Yalnızca yönetilmeyen sanal makine disklerini depolamak için bir premium performans katmanı.

## <a name="access-tiers-for-block-blob-data"></a>Blok blobu veri erişim katmanları

Azure depolama, kullanım düzenlerini esas alarak blok blob verilerine erişmek için farklı seçenekler sunar. Her Azure depolama erişim katmanında belirli bir desene veri kullanımı için optimize edilmiştir. Gereksinimleriniz için doğru erişim katmanı seçerek, blok blobu verileriniz en uygun maliyetli bir şekilde depolayabilirsiniz.

Kullanılabilir erişim katmanları şunlardır:

* **Etkin** erişim katmanı, sık sık depolama hesabındaki nesnelere erişimi için optimize edilmiştir. Depolama maliyetleri, daha yüksek olsa sık erişimli katmanı veri erişimi en uygun maliyetli. Yeni depolama hesaplarında sık erişimli oluşturulan varsayılan olarak katman.
* **Seyrek erişimli** erişim katmanı, büyük miktarlarda az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için optimize edilmiştir. Veri depolama seyrek erişim katmanında daha uygun maliyetlidir, ancak bu verilere sık erişimli katmandaki verilere göre daha pahalı olabilir.
* **Arşiv** katmanı, yalnızca tek bir blok bloblar için kullanılabilir. Arşiv katmanı, birkaç saatlik alma gecikmesinden etkilenmeyecek ve Arşiv katmanında en az 180 gün boyunca kalacak veriler için optimize edilmiştir. Arşiv katmanı verilerini depolamak için en uygun maliyetli bir seçenektir, ancak bu verilere erişmek sık erişimli veya seyrek erişimli katmanları veri erişimi değerinden daha pahalıdır.

Verilerinizin kullanım düzeninde bir değişiklik olursa herhangi bir zamanda bu erişim katmanları arasında geçiş yapabilirsiniz. Erişim katmanları hakkında daha fazla bilgi için bkz. [Azure Blob Depolama: sık erişimli, seyrek erişimli ve Arşiv erişim katmanları](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Mevcut bir depolama hesabı veya blob için erişim katmanının değiştirilmesi ek ücretlere neden olabilir. Daha fazla bilgi için [depolama hesabı bölümünde faturalama](#storage-account-billing).

## <a name="replication"></a>Çoğaltma

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Depolama çoğaltma hakkında daha fazla bilgi için bkz. [Azure depolama çoğaltma](storage-redundancy.md).

## <a name="encryption"></a>Şifreleme

Hizmet tarafında, depolama hesabınızdaki tüm veriler şifrelenir. Şifreleme hakkında daha fazla bilgi için bkz: [bekleyen veriler için Azure depolama hizmeti şifrelemesi](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Depolama hesabı uç noktaları

Bir depolama hesabı, Azure, verileriniz için benzersiz bir ad sağlar. Azure Storage'a depoladığınız her nesnenin benzersiz hesabınızın adını içeren bir adresi vardır. Hesap adı ve Azure Depolama Hizmeti uç noktası birleşimi depolama hesabınız için uç noktaları oluşturur.

Örneğin, genel amaçlı depolama hesabınızın adı *mystorageaccount*, bu hesap için varsayılan uç noktalar şunlardır:

* BLOB Depolama: http://*mystorageaccount*. blob.core.windows.net
* Tablo depolama: http://*mystorageaccount*. table.core.windows.net
* Kuyruk Depolama: http://*mystorageaccount*. queue.core.windows.net
* Azure dosyaları: http://*mystorageaccount*. file.core.windows.net

> [!NOTE]
> Blob Depolama hesabı yalnızca Blob Hizmeti uç noktasını kullanıma sunar.

Bir depolama hesabındaki bir nesneye erişim URL'si, depolama hesabındaki nesnenin konumu uç noktaya eklenmesiyle oluşturulur. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca, depolama hesabınızı özel bir etki alanı için BLOB'ları kullanacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için [Azure depolama hesabınız için bir özel etki alanı adı yapılandırma](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Hesap verilere erişimi denetleme

Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir. Denetiminiz kimlerin verilerinize erişebilir ve hangi izinlere sahiptirler.

Depolama hesabınıza karşı yapılan her isteği yetkili olması gerekir. Hizmet düzeyinde isteğin geçerli bir içermelidir *yetkilendirme* üst bilgi hizmeti isteği yürütmeden önce doğrulamak gereken bilgileri içerir.

Aşağıdaki yaklaşımlardan birini kullanarak depolama hesabınızdaki verilere erişim izni verebilirsiniz:

- **Azure Active Directory:** Bir kullanıcı, Grup veya diğer kimlik blob ve kuyruk verilere erişim için kimlik doğrulaması için Azure Active Directory (Azure AD) kimlik bilgilerini kullanın. Bir kimlik, kimlik doğrulaması başarılı olursa, Azure AD Azure Blob Depolama veya kuyruk depolama isteğine yetki verme içinde kullanmak için bir belirteç döndürür. Daha fazla bilgi için [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md).
- **Paylaşılan anahtar yetkilendirme:** Depolama hesabı erişim anahtarınız, uygulamanızın kullandığı çalışma zamanında Azure depolamaya erişmek için bir bağlantı dizesi oluşturmak için kullanın. Bağlantı dizesinde değerleri oluşturmak için kullanılan *yetkilendirme* Azure depolama alanına geçirilen başlığı. Daha fazla bilgi için [yapılandırma Azure Storage bağlantı dizelerini](storage-configure-connection-string.md).
- **Paylaşılan erişim imzası:** Azure AD kimlik doğrulamasını kullanmıyorsanız, depolama hesabınızdaki kaynaklara temsilci erişimi için paylaşılan erişim imzası'nı kullanın. Paylaşılan erişim imzası tüm URL noktasında Azure Depolama'ya yönelik bir isteği yetkilendirmek için gereken bilgileri yalıtan bir belirteçtir. Depolama kaynağı, verilen izinleri ve üzerinde izinleri geçerli aralık belirtebilirsiniz paylaşılan erişim imzasının bir parçası olarak. Daha fazla bilgi için [paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Kullanıcılar veya uygulamalar Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yetkilendirme başka bir yolla üstün güvenlik ve kullanım kolaylığı sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. 
>
> Microsoft Azure depolama blob ve kuyruk uygulamalarınız için mümkün olduğunda Azure AD kimlik doğrulaması kullanmanızı önerir.

## <a name="copying-data-into-a-storage-account"></a>Bir depolama hesabına veri kopyalama

Microsoft, şirket içi depolama cihazlar veya üçüncü taraf bulut depolama sağlayıcılarından verilerinizi almak için yardımcı programlar ve kitaplıkları sağlar. Kullandığınız çözüm, aktardığınız veri miktarı üzerinde bağlıdır. 

Bir genel amaçlı v2 hesabı genel amaçlı v1'den veya Blob Depolama hesabı yükselttiğinizde, verilerinizi otomatik olarak geçirilir. Microsoft hesabınızı yükseltmek için bu yol önerir. Bununla birlikte, verilerinizi el ile taşımanız gerekir verileri Blob Depolama hesabı için bir genel amaçlı v1 hesabı taşıma karar verirseniz, aşağıda açıklanan kitaplıkları ve araçları kullanarak. 

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy, verileri var olan bir genel amaçlı depolama hesabındaki bir Blob Depolama hesabına kopyalamak veya şirket içi depolama cihazlarından verileri karşıya yüklemek için kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Depolama veri taşıma kitaplığı, AzCopy’yi çalıştıran çekirdek veri taşıma altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bunu kullanarak AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanabilirsiniz. Daha fazla bilgi için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Azure istemci kitaplıklarından birini ya da Azure Storage hizmetleri REST API’sini kullanarak verilerinizi Blob Storage hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Azure depolama REST API'si hakkında daha fazla bilgi için bkz: [Azure depolama hizmetleri REST API Başvurusu](https://docs.microsoft.com/rest/api/storageservices/). 

> [!IMPORTANT]
> Bloblar, blobla birlikte istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. İstemci tarafı şifreleme ile şifrelenmiş bir blobu kopyalarsanız, kopyalama işleminin başta şifreleme ile ilgili meta veriler olmak üzere blob meta verilerini koruduğundan emin olun. Bir blobu şifreleme meta verileri olmadan kopyalarsanız blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="azure-importexport-service"></a>Azure İçeri/Dışarı Aktarma hizmeti

Büyük miktarda depolama hesabınıza alınacak verileri varsa, Azure içeri/dışarı aktarma hizmeti kullanmayı düşünün. İçeri/dışarı aktarma hizmeti, Azure veri merkezi dağıtımı disk sürücülerine büyük miktarda veriyi Azure Blob Depolama ve Azure dosyaları için güvenli bir şekilde içeri aktarmak için kullanılır. 

İçeri/dışarı aktarma hizmeti, verileri Azure Blob depolama alanından disk sürücülerine aktarmak ve şirket içi sitelerinize teslim etme için de kullanılabilir. Bir veya daha fazla disk sürücüsü verileri Azure Blob Depolama veya Azure dosyaları içeri aktarılabilir. Daha fazla bilgi için [Azure içeri/dışarı aktarma hizmeti nedir?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).

## <a name="storage-account-billing"></a>Depolama hesabı faturalama

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure depolama hesabı oluşturma hakkında bilgi edinmek için [depolama hesabı oluşturma](storage-quickstart-create-account.md).
* Yönetmek veya mevcut bir depolama hesabını silmek için bkz: [yönetme Azure depolama hesapları](storage-account-manage.md).
