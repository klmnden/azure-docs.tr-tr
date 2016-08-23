<properties
    pageTitle="Azure Portal’da depolama hesabı oluşturma, yönetme veya silme | Microsoft Azure"
    description="Azure Portal’da yeni bir depolama hesabı oluşturun, hesap erişim tuşlarınızı yönetin veya bir depolama hesabını silin. Standart ve premium depolama hesapları hakkında bilgi edinin."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# Azure Storage hesapları hakkında

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## Genel Bakış

Azure Storage hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## Depolama hesabı faturalama

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Bir Azure Virtual Machine oluşturduğunuzda, ilgili konumda bir depolama hesabınız yoksa, depolama konumunda otomatik olarak bir depolama hesabı oluşturulur. Bu nedenle, sanal makine diskleriniz için bir depolama hesabı oluşturmak üzere aşağıdaki adımları izlemeniz gerekli değildir. Depolama hesabı adı sanal makine adına dayalı olarak belirlenir. Daha fazla ayrıntı için bkz. [Azure Virtual Machines belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

## Depolama hesabı uç noktaları

Azure Storage’da depoladığınız her nesnenin benzersiz bir URL adresi vardır. Depolama hesabı adı o adresin alt etki alanı adını oluşturur. Her hizmete özel alt etki alanı ve etki alanı birleşimi depolama hesabınız için bir *uç nokta* oluşturur.

Örneğin depolama hesabınızın adı *mystorageaccount* ise, depolama hesabınız için varsayılan uç noktalar şunlardır:

- Blob hizmeti: http://*mystorageaccount*.blob.core.windows.net

- Tablo hizmeti: http://*mystorageaccount*.table.core.windows.net

- Kuyruk hizmeti: http://*mystorageaccount*.queue.core.windows.net

- Dosya hizmeti: http://*mystorageaccount*.file.core.windows.net

> [AZURE.NOTE] Blob Storage hizmeti yalnızca Blob hizmeti uç noktasını verir.

Bir depolama hesabındaki bir nesneye erişmek için gerekli URL, nesnenin depolama hesabındaki konumunun uç noktaya eklenmesiyle oluşturulur. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca depolama hesabınız ile birlikte kullanmak üzere özel bir etki alanı adı yapılandırabilirsiniz. Klasik depolama hesapları için ayrıntıları öğrenmek üzere [Blob Depolama Uç Noktanız için özel bir etki alanı Adı yapılandırma](storage-custom-domain-name.md) sayfasına bakın. Bu özellik, Resource Manager depolama hesapları için henüz [Azure portalına](https://portal.azure.com) eklenmemiştir, ancak bu özelliği PowerShell ile yapılandırabilirsiniz. Daha fazla bilgi için [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) cmdlet’ine bakın.  

## Depolama hesabı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Hub menüsünde, **Yeni** -> **Veri + Depolama** -> **Depolama hesabı**’nı seçin.

3. Depolama hesabınız için bir ad girin. Depolama hesabınızın adının Azure Storage’da nesnelerinizin ele alınması için nasıl kullanılacağı hakkında ayrıntılar için bkz. [Depolama hesabı uç noktaları](#storage-account-endpoints).

    > [AZURE.NOTE] Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
    >  
    > Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. Seçtiğiniz depolama hesabı adı alınmışsa Azure portal bunun zaten kullanımda olduğunu bildirecektir.

4. Kullanılacak dağıtım modelini belirtin: **Resource Manager** veya **Klasik**. Önerilen dağıtım modeli **Resource Manager**’dır. Daha fazla bilgi için bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Blob Storage hesapları yalnızca Resource Manager dağıtım modeli kullanılarak oluşturulabilir.

5. Depolama hesabı türünü seçin: **Genel amaçlı** veya **Blob Storage**. Varsayılan seçenek **Genel amaçlı**’dır.

    **Genel amaçlı** seçeneği belirlenirse **Standart** veya **Premium** performans katmanlarından birini belirtin. Varsayılan seçenek **Standart**’tır. Standart ve premium depolama hesapları hakkında daha fazla bilgi için bkz. [Microsoft Azure Storage’a Giriş](storage-introduction.md) ve [Premium Storage: Azure Virtual Machineİş Yükleri için Yüksek Performanslı Depolama](storage-premium-storage.md).

    **Blob Storage** seçeneği belirlendiyse, erişim katmanını belirtin: **Sık Erişimli** veya **Seyrek Erişimli**. Varsayılan seçenek **Sık Erişimli**’dir. Daha fazla ayrıntı için bkz. [Azure Blob Storage: Seyrek erişimli ve Sık erişimli katmanlar](storage-blob-storage-tiers.md).

6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS**, **RA-GRS** veya **ZRS**. Varsayılan seçenek **RA-GRS**’dir. Azure Storage çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).

7. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.

8. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../resource-group-overview.md).

9. Depolama hesabınız için coğrafi konumu seçin. Hangi bölgede hangi hizmetin sağlandığına dair daha fazla bilgi için bkz.[Azure Bölgeleri](https://azure.microsoft.com/regions/#services).

10. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

## Depolama hesabınızı yönetme

### Hesap yapılandırmanızı değiştirme

Depolama hesabınızı oluşturduktan sonra hesap için kullanılan çoğaltma seçeneğini veya Blob Storage hesabının erişim katmanını değiştirme gibi hesabın yapılandırmasını değiştirebilirsiniz. [Azure portal](https://portal.azure.com)’da depolama hesabınıza gidin, **Tüm ayarlar**’a tıklayın ve ardından hesap yapılandırmasını görüntülemek ve/veya yapılandırmak için **Yapılandırma**’ya tıklayın.

> [AZURE.NOTE] Depolama hesabı oluştururken seçtiğiniz performans katmanına bağlı olarak bazı çoğaltma seçenekleri kullanılamayabilir.

Çoğaltma seçeneğinin değiştirilmesi, fiyatlandırmanızı da değiştirir. Daha fazla ayrıntı için [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfasına bakın.

Blob Storage hesapları için erişim katmanını değiştirdiğinizde, fiyatlandırmanızın değiştirilmesinin yanı sıra bu değişim için ücret alınabilir. Daha fazla ayrıntı için lütfen [Blob Storage hesapları - Fiyatlandırma ve Faturalama](storage-blob-storage-tiers.md#pricing-and-billing) sayfasına bakın.

### Depolama erişim tuşlarınızı yönetme

Bir depolama hesabı oluşturduğunuzda Azure, depolama hesabına erişim sağlandığında kimlik doğrulama için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sağlayarak AAzure Storage izmetinizde herhangi bir kesinti olmadan veya ilgili hizmete erişim sağlamaya gerek kalmadan anahtarları yeniden oluşturmanızı sağlar.

> [AZURE.NOTE] Depolama erişim tuşlarınızı başkalarıyla paylaşmaktan kaçınmanızı öneririz. Erişim tuşlarınızı vermeden depolama kaynaklarına erişim izni vermek için bir *paylaşılan erişim imzası* kullanabilirsiniz. Paylaşılan erişim imzası, hesabınızdaki bir kaynağa belirlediğiniz zaman diliminde belirlediğiniz izinlerle erişilmesini sağlar. Daha fazla için bkz. [Paylaşılan Erişim İmzaları: SAS modelini anlama](storage-dotnet-shared-access-signature-part-1.md).

#### Depolama erişim tuşlarını görüntüleme ve kopyalama

[Azure portal](https://portal.azure.com)’da depolama hesabınıza gidin, **Tüm ayarlar**’a tıklayın ve ardından hesap erişim tuşlarınızı görüntülemek, kopyalamak ve yeniden oluşturmak için **Erişim tuşları**’na tıklayın. **Erişim Tuşları** dikey penceresi, uygulamanızda kullanmak üzere kopyalayabileceğiniz birincil ve ikincil anahtarları kullanan önceden yapılandırılmış bağlantı dizeleri içerir.

#### Depolama erişim tuşlarını yeniden oluşturma

Depolama bağlantılarınızı güvenli tutmaya yardımcı olmak üzere depolama hesabınıza erişim tuşlarını düzenli aralıklarla değiştirmenizi öneririz. Bir erişim tuşunu kullanarak depolama hesabına bağlantıları sağlamak ve diğer anahtarı yeniden oluşturmak üzere kullanmanız için iki erişim tuşu atanır.

> [AZURE.WARNING] Erişim tuşlarınızı yeniden oluşturmak Azure’daki hizmetleri ve depolama hesabına bağlı kendi uygulamalarınızı etkileyebilir. Depolama hesabına erişmek için erişim tuşunu kullanan tüm istemciler yeni anahtarı kullanmak üzere güncelleştirilmelidir.

**Media Services** - Depolama hesabınıza bağlı medya hizmetleri varsa, anahtarları yeniden oluşturduktan sonra erişim tuşlarını medya hizmetlerinizle yeniden eşitlemeniz gerekir.

**Uygulamalar** - Depolama hesabını kullanan web uygulamalarınız veya bulut hizmetleriniz varsa, yeniden anahtar oluşturmanız durumunda, anahtarları toplamazsanız bağlantıları kaybedeceksiniz.

**Depolama Gezginleri** - Herhangi bir [depolama gezgin uygulaması](storage-explorers.md) kullanıyorsanız büyük olasılıkla söz konusu uygulamaların kullandığı depolama anahtarını güncelleştirmeniz gerekir.

Depolama erişim tuşlarınızı şu şekilde döndürebilirsiniz:

1. Depolama hesabının ikinci erişim tuşunu referans olarak göstermek için uygulama kodunuzdaki bağlantı dizinlerini güncelleştirin.

2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. **Erişim Tuşları** dikey penceresinde **Anahtarı Yeniden Oluştur1**’u seçin ve yeni bir anahtar oluşturmak istediğinizi onaylamak için **Evet**’e tıklayın.

3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.

4. İkincil erişim tuşunu da aynı şekilde yeniden oluşturun.

## Bir depolama hesabını silme

Artık kullanmadığınız bir depolama hesabını kaldırmak için [Azure portal](https://portal.azure.com)’da depolama hesabına gidin ve **Sil**’e tıklayın. Depolama hesabı silindiğinde, hesaptaki tüm veriler dahil olmak üzere tüm hesap silinir.

> [AZURE.WARNING] Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.

Bir Azure Virtual Machine ile ilişkili bir depolama hesabını silmek için, ilk olarak tüm sanal makine disklerinin silindiğinden emin olmanız gerekir. Öncelikle sanal makine disklerini silmezseniz, depolama hesabını silmeye çalıştığınızda şuna benzer bir hata iletisi ile karşılaşırsınız:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Depolama hesabınızda Klasik dağıtım modeli kullanılıyorsa, [Azure portal](https://manage.windowsazure.com)’da şu adımları uygulayarak sanal makineyi kaldırabilirsiniz:

1. [Klasik Azure portalı](https://manage.windowsazure.com)’na gidin.
2. Virtual Machines sekmesine gidin.
3. Diskler sekmesine tıklayın.
4. Veri diskinizi seçin ve Diski Sil’e tıklayın.
5. Disk görüntülerini silmek için Görüntüler sekmesine gidin ve hesapta yer alan tüm görüntüleri silin.

Daha fazla bilgi edinmek için bkz. [Azure Virtual Machines belgeleri](http://azure.microsoft.com/documentation/services/virtual-machines/).

## Sonraki adımlar

- [Azure Blob Storage: Seyrek erişimli ve Sık erişimli katmanlar](storage-blob-storage-tiers.md)
- [Azure Storage çoğaltma](storage-redundancy.md)
- [Azure Storage Bağlantı Dizelerini Yapılandırma](storage-configure-connection-string.md)
- [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
- [Azure Storage ekip blogunu](http://blogs.msdn.com/b/windowsazurestorage/) ziyaret edin.



<!--HONumber=Aug16_HO1-->


