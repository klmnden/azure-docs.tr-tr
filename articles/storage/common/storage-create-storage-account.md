---
title: "Azure portalda depolama hesabı oluşturma, yönetme veya silme | Microsoft Docs"
description: "Azure portalda yeni bir depolama hesabı oluşturun, hesap erişim tuşlarınızı yönetin veya bir depolama hesabını silin. Standart ve premium depolama hesapları hakkında bilgi edinin."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
f1_keywords: sql13.swb.windowsazurestorage.connect.f1
ms.date: 10/11/2017
ms.author: tamram
ms.openlocfilehash: c9630e575de15b404bd91cbeefc60d698c5da667
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="about-azure-storage-accounts"></a>Azure Storage hesapları hakkında

[!INCLUDE [storage-selector-portal-create-storage-account](../../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Azure Storage hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir.

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Depolama hesabı faturalama

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

> [!NOTE]
> Bir Azure Virtual Machine oluşturduğunuzda, ilgili konumda bir depolama hesabınız yoksa, depolama konumunda otomatik olarak bir depolama hesabı oluşturulur. Bu nedenle, sanal makine diskleriniz için bir depolama hesabı oluşturmak üzere aşağıdaki adımları izlemeniz gerekli değildir. Depolama hesabı adı sanal makine adına dayalı olarak belirlenir. Daha fazla ayrıntı için bkz. [Azure Virtual Machines belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/).
> 
> 

## <a name="storage-account-endpoints"></a>Depolama hesabı uç noktaları
Azure Storage’da depoladığınız her nesnenin benzersiz bir URL adresi vardır. Depolama hesabı adı o adresin alt etki alanı adını oluşturur. Her hizmete özel alt etki alanı ve etki alanı birleşimi depolama hesabınız için bir *uç nokta* oluşturur.

Örneğin depolama hesabınızın adı *mystorageaccount* ise, depolama hesabınız için varsayılan uç noktalar şunlardır:

* Blob hizmeti: http://*mystorageaccount*.blob.core.windows.net
* Tablo hizmeti: http://*mystorageaccount*.table.core.windows.net
* Kuyruk hizmeti: http://*mystorageaccount*.queue.core.windows.net
* Dosya hizmeti: http://*mystorageaccount*.file.core.windows.net

> [!NOTE]
> Blob Storage hizmeti yalnızca Blob hizmeti uç noktasını verir.
> 
> 

Bir depolama hesabındaki bir nesneye erişmek için gerekli URL, nesnenin depolama hesabındaki konumunun uç noktaya eklenmesiyle oluşturulur. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca depolama hesabınız ile birlikte kullanmak üzere özel bir etki alanı adı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama Uç Noktanız için özel bir etki alanı adı yapılandırma](../blobs/storage-custom-domain-name.md). PowerShell ile de yapılandırabilirsiniz. Daha fazla bilgi için [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) cmdlet’ine bakın.  


## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Diğer Hizmetler**'i seçin. Ardından **Depolama** bölümüne inin ve **Depolama hesapları**'nı seçin. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
3. Depolama hesabınız için bir ad girin. Depolama hesabınızın adının Azure Storage’da nesnelerinizin ele alınması için nasıl kullanılacağı hakkında ayrıntılar için bkz. [Depolama hesabı uç noktaları](#storage-account-endpoints).
   
   > [!NOTE]
   > Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
   > 
   > Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. Seçtiğiniz depolama hesabı adı alınmışsa Azure portal bunun zaten kullanımda olduğunu bildirecektir.
   > 
   > 
4. Kullanılacak dağıtım modelini belirtin: **Resource Manager** veya **Klasik**. Önerilen dağıtım modeli **Resource Manager**’dır. Daha fazla bilgi için bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](../../azure-resource-manager/resource-manager-deployment-model.md).
   
   > [!NOTE]
   > Blob Storage hesapları yalnızca Resource Manager dağıtım modeli kullanılarak oluşturulabilir.

5. Depolama hesabı türünü seçin: **Genel amaçlı** veya **Blob Storage**. Varsayılan seçenek **Genel amaçlı**’dır.
   
    **Genel amaçlı** seçeneği belirlenirse **Standart** veya **Premium** performans katmanlarından birini belirtin. Varsayılan seçenek **Standart**’tır. Standart ve premium depolama hesapları hakkında daha fazla bilgi için bkz. [Microsoft Azure Storage’a Giriş](storage-introduction.md) ve [Premium Storage: Azure Virtual Machineİş Yükleri için Yüksek Performanslı Depolama](../../virtual-machines/windows/premium-storage.md).
   
    **Blob Storage** seçeneği belirlendiyse, erişim katmanını belirtin: **Sık Erişimli** veya **Seyrek Erişimli**. Varsayılan seçenek **Sık Erişimli**’dir. Daha fazla ayrıntı için bkz. [Azure Blob Storage: Seyrek erişimli ve Sık erişimli katmanlar](../blobs/storage-blob-storage-tiers.md).
6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS**, **RA-GRS** veya **ZRS**. Varsayılan seçenek **RA-GRS**’dir. Azure Storage çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).
7. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
8. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).
9. Depolama hesabınız için coğrafi konumu seçin. Hangi bölgede hangi hizmetin sağlandığına dair daha fazla bilgi için bkz.[Azure Bölgeleri](https://azure.microsoft.com/regions/#services).
10. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

## <a name="manage-your-storage-account"></a>Depolama hesabınızı yönetme
### <a name="change-your-account-configuration"></a>Hesap yapılandırmanızı değiştirme
Depolama hesabınızı oluşturduktan sonra hesap için kullanılan çoğaltma seçeneğini veya Blob Storage hesabının erişim katmanını değiştirme gibi hesabın yapılandırmasını değiştirebilirsiniz. Hesap yapılandırmanızı görüntülemek ve/veya değiştirmek için [Azure portalında](https://portal.azure.com) depolama hesabınıza gidin ve **AYARLAR** altında yer alan **Yapılandırma** seçeneğine tıklayın.

> [!NOTE]
> Depolama hesabı oluştururken seçtiğiniz performans katmanına bağlı olarak bazı çoğaltma seçenekleri kullanılamayabilir.
> 
> 

Çoğaltma seçeneğinin değiştirilmesi, fiyatlandırmanızı da değiştirir. Daha fazla ayrıntı için [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfasına bakın.

Blob Storage hesapları için erişim katmanını değiştirdiğinizde, fiyatlandırmanızın değiştirilmesinin yanı sıra bu değişim için ücret alınabilir. Daha fazla ayrıntı için lütfen [Blob Storage hesapları - Fiyatlandırma ve Faturalama](../blobs/storage-blob-storage-tiers.md#pricing-and-billing) sayfasına bakın.

### <a name="manage-your-storage-access-keys"></a>Depolama erişim tuşlarınızı yönetme
Bir depolama hesabı oluşturduğunuzda Azure, depolama hesabına erişim sağlandığında kimlik doğrulama için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sağlayarak AAzure Storage izmetinizde herhangi bir kesinti olmadan veya ilgili hizmete erişim sağlamaya gerek kalmadan anahtarları yeniden oluşturmanızı sağlar.

> [!NOTE]
> Depolama erişim tuşlarınızı başkalarıyla paylaşmaktan kaçınmanızı öneririz. Erişim tuşlarınızı vermeden depolama kaynaklarına erişim izni vermek için bir *paylaşılan erişim imzası* kullanabilirsiniz. Paylaşılan erişim imzası, hesabınızdaki bir kaynağa belirlediğiniz zaman diliminde belirlediğiniz izinlerle erişilmesini sağlar. Daha fazla bilgi edinmek için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).
> 
> 
<a id="view-and-copy-storage-access-keys"/></a>
#### <a name="view-and-copy-storage-access-keys"></a>Depolama erişim tuşlarını görüntüleme ve kopyalama
[Azure portal](https://portal.azure.com)’da depolama hesabınıza gidin, **Tüm ayarlar**’a tıklayın ve ardından hesap erişim tuşlarınızı görüntülemek, kopyalamak ve yeniden oluşturmak için **Erişim tuşları**’na tıklayın. **Erişim Tuşları** dikey penceresi, uygulamanızda kullanmak üzere kopyalayabileceğiniz birincil ve ikincil anahtarları kullanan önceden yapılandırılmış bağlantı dizeleri içerir.

#### <a name="regenerate-storage-access-keys"></a>Depolama erişim tuşlarını yeniden oluşturma
Depolama bağlantılarınızı güvenli tutmaya yardımcı olmak üzere depolama hesabınıza erişim tuşlarını düzenli aralıklarla değiştirmenizi öneririz. Bir erişim tuşunu kullanarak depolama hesabına bağlantıları sağlamak ve diğer anahtarı yeniden oluşturmak üzere kullanmanız için iki erişim tuşu atanır.

> [!WARNING]
> Erişim tuşlarınızı yeniden oluşturmak Azure’daki hizmetleri ve depolama hesabına bağlı kendi uygulamalarınızı etkileyebilir. Depolama hesabına erişmek için erişim tuşunu kullanan tüm istemciler yeni anahtarı kullanmak üzere güncelleştirilmelidir.
> 
> 

**Media Services** - Depolama hesabınıza bağlı medya hizmetleri varsa, anahtarları yeniden oluşturduktan sonra erişim tuşlarını medya hizmetlerinizle yeniden eşitlemeniz gerekir.

**Uygulamalar** - Depolama hesabını kullanan web uygulamalarınız veya bulut hizmetleriniz varsa, yeniden anahtar oluşturmanız durumunda, anahtarları toplamazsanız bağlantıları kaybedeceksiniz.

**Depolama Gezginleri** - Herhangi bir [depolama gezgin uygulaması](storage-explorers.md) kullanıyorsanız büyük olasılıkla söz konusu uygulamaların kullandığı depolama anahtarını güncelleştirmeniz gerekir.

Depolama erişim tuşlarınızı şu şekilde döndürebilirsiniz:

1. Depolama hesabının ikinci erişim tuşunu referans olarak göstermek için uygulama kodunuzdaki bağlantı dizinlerini güncelleştirin.
2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. **Erişim Tuşları** dikey penceresinde **Anahtarı Yeniden Oluştur1**’u seçin ve yeni bir anahtar oluşturmak istediğinizi onaylamak için **Evet**’e tıklayın.
3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.
4. İkincil erişim tuşunu da aynı şekilde yeniden oluşturun.

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme
Artık kullanmadığınız bir depolama hesabını kaldırmak için [Azure portal](https://portal.azure.com)’da depolama hesabına gidin ve **Sil**’e tıklayın. Depolama hesabı silindiğinde, hesaptaki tüm veriler dahil olmak üzere tüm hesap silinir.

> [!WARNING]
> Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.
> 

Bir Azure sanal makinesiyle ilişkili bir depolama hesabını silmeye çalışırsanız, depolama hesabının kullanımda olduğu hakkında bir hata alabilirsiniz. Bu hatayı giderme hakkında yardım için lütfen bkz. [Depolama hesaplarını sildiğinizde sorun giderme](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Azure Blob Depolama: Seyrek Erişimli ve Sık Erişimli katmanlar](../blobs/storage-blob-storage-tiers.md)
* [Azure Depolama çoğaltması](storage-redundancy.md)
* [Azure Depolama Bağlantı Dizelerini yapılandırma](../storage-configure-connection-string.md)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure Storage ekip blogunu](http://blogs.msdn.com/b/windowsazurestorage/) ziyaret edin.

