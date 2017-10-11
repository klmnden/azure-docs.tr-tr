# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Artımlı anlık görüntüleri ile Azure yönetilmeyen VM diskleri yedekleyin
## <a name="overview"></a>Genel Bakış
Azure Storage blobları anlık görüntüsünü yeteneği sağlar. Anlık görüntüler zamandaki o noktada blob durumu yakalayın. Bu makalede, anlık görüntülerini kullanarak sanal makine disklerinin yedeklerini bulundurabilirsiniz bir senaryo açıklanmaktadır. Azure yedekleme ve kurtarma hizmeti kullanmamayı ve sanal makine diskleriniz için özel bir yedekleme stratejisi oluşturmak isterseniz bu yöntemi kullanabilirsiniz.

Azure virtual machine diskleri sayfa blobları Azure storage'da olarak depolanır. Sanal makine disklerini bu makalede bir yedekleme stratejisi tanımlamakta olduğunuz olduğundan, biz anlık görüntüleri sayfa bloblarını bağlamında bakın. Anlık görüntüler hakkında daha fazla bilgi için bkz [Blob anlık görüntüsünün oluşturulmasına](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Bir anlık görüntü nedir?
Bir blob anlık bir noktada zamanında yakalanan bir blob, salt okunur bir sürümüdür. Bir anlık görüntü oluşturulduktan sonra okumak, kopyalanan, veya silinmiş, ancak değişiklik. Anlık görüntüler bir anda göründüğü gibi bir blob yedeklemek için bir yol sağlar. REST sürüm 2015-04-05 kadar tam anlık görüntü kopyalama becerisini vardı. Ayrıca sürüm 2015-07-08 REST ve üstü ile, artımlı anlık görüntüleri kopyalayabilirsiniz.

## <a name="full-snapshot-copy"></a>Tam anlık görüntü kopyalama
Anlık görüntüler için başka bir depolama hesabı temel blob yedeklerini tutmak için blob olarak kopyalanabilir. Blob bir önceki sürüme geri yüklemeyi gibi kendi temel blob üzerinden bir anlık görüntü de kopyalayabilirsiniz. Bir anlık görüntü bir depolama hesabından başka bir kopyalandığında temel sayfa blobu olarak aynı yer kaplar. Bu nedenle, tüm anlık görüntüler bir depolama hesabından kopyalama yavaş ve hedef depolama hesabı kadar alan kullanır.

> [!NOTE]
> Başka bir hedefe temel blob kopyalarsanız, blob anlık görüntüleri onunla birlikte kopyalanmaz. Benzer şekilde, bir kopya ile temel bir blob üzerine, temel blob ile ilişkili anlık görüntüleri etkilenmez ve temel blob adı altında olduğu gibi kalır.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Diskleri anlık görüntülerini kullanarak yedekleme
Sanal makine diskleriniz için bir yedekleme stratejisi disk ya da sayfa blob düzenli anlık görüntüleri alabilir ve bunları başka bir depolama hesabı kullanarak kopyalama araçları gibi [kopyalama Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) işlemi veya [AzCopy](../articles/storage/common/storage-use-azcopy.md). Farklı bir ada sahip bir hedef sayfa blob'u için bir anlık görüntü kopyalayabilirsiniz. Sonuçta elde edilen hedef sayfa blobu yazılabilir sayfa blobu ve anlık görüntü olmayan ' dir. Bu makalenin sonraki bölümlerinde anlık görüntülerini kullanarak sanal makine disklerinin yedeklerini uygulanacak adımlar açıklanmaktadır.

### <a name="restore-disks-using-snapshots"></a>Anlık görüntülerini kullanarak disklerini geri yükle
Yedekleme anlık görüntülerinin birini daha önce yakalanan kararlı bir sürüm diskinizde geri zamanı geldiğinde, bir anlık görüntü üzerinde temel sayfa blobu kopyalayabilirsiniz. Taban sayfası için anlık görüntü yükseltildikten sonra anlık görüntü kalır blob ancak kaynağı, hem okuma ve yazılabilir bir kopya ile üzerine yazılır. Bu makalenin sonraki bölümlerinde, disk önceki bir sürümü, anlık görüntüden geri yüklemek için adımları açıklar.

### <a name="implementing-full-snapshot-copy"></a>Tam anlık görüntü kopyalama uygulama
Aşağıdakileri yaparak tam anlık görüntü kopyalama uygulayabilirsiniz.

* İlk olarak, temel blob kullanmanın bir anlık görüntüsünü [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) işlemi.
* Ardından, hedef depolama hesabı kullanarak bir anlık görüntü kopyalama [kopyalama Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Temel blob yedek kopyalarını korumak için bu işlemi yineleyin.

## <a name="incremental-snapshot-copy"></a>Artımlı anlık görüntü kopyalama
Yeni özelliği [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API sayfa BLOB'ları veya diskleri anlık görüntüleri yedeklemek için daha iyi bir yöntem sunar. API değişikliklerin listesini temel blob ve anlık görüntüleri arasında yedekleme hesabında kullanılan depolama alanı miktarını azaltır döndürür. API, standart depolama yanı sıra Premium depolama üzerinde sayfa bloblarını destekler. Bu API kullanarak Azure VM'ler için daha hızlı ve daha verimli yedekleme çözümleri oluşturabilirsiniz. Bu API REST sürüm 2015-07-08 ile kullanılabilir ve daha yüksek olur.

Artımlı anlık görüntü kopyalama, bir depolama hesabından başka bir arasındaki farkı kopyalamanıza olanak sağlar,

* Temel blob ve onun anlık görüntü veya
* Temel BLOB iki tüm anlık görüntüleri

Aşağıdaki koşulların karşılandığından sağlanan,

* Blob Oca 1 2016 veya sonraki bir sürümünde oluşturulmuş.
* Blob ile yazıldı değil [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) veya [kopyalama Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) arasında iki anlık görüntüler.

**Not**: Bu özellik Premium ve standart Azure sayfa BLOB'ları için kullanılabilir.

Anlık görüntülerini kullanarak özel bir yedekleme stratejisi sahip olduğunuzda, anlık görüntüler bir depolama hesabından kopyalama başka bir yavaş olabilir ve kadar depolama alanı kullanabilir. Tüm anlık görüntü bir yedekleme depolama hesabına kopyalamak yerine art arda gelen anlık görüntüleri yedekleme sayfa blobu için arasındaki farkı yazabilirsiniz. Bu şekilde, kopyalama ve yedekleri depolamak için alanı süresini önemli ölçüde azalır.

### <a name="implementing-incremental-snapshot-copy"></a>Uygulama artımlı anlık görüntü kopyalama
Aşağıdakileri yaparak artımlı anlık görüntü kopyalama uygulayabilirsiniz.

* Temel blob kullanmanın bir anlık görüntüsünü [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Anlık görüntü kopyalama hedefi yedekleme depolama kullanarak hesabı [kopyalama Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Yedekleme sayfa blobu budur. Bir yedekleme sayfa blobu alın ve yedekleme hesabında saklayın.
* Anlık görüntü Blob kullanarak temel BLOB başka bir anlık görüntü alın.
* Temel blob kullanmanın ilk ve ikinci anlık görüntüleri arasındaki farkı alma [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Yeni bir parametre kullanmak **prevsnapshot**farkı almak için anlık görüntüsünü DateTime değeri belirtmek için. Bu parametre mevcut olduğunda, REST yanıt hedef anlık görüntüsü ile Temizle sayfalar dahil olmak üzere önceki anlık arasında değiştirilen sayfalar içerir.
* Kullanım [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) yedekleme sayfa blobu bu değişiklikleri uygulamak için.
* Son olarak, bir yedekleme sayfa blobu alın ve yedekleme depolama hesabında saklayın.

Sonraki bölümde, biz daha ayrıntılı olarak artımlı anlık görüntü kopyalama kullanarak diskleri yedeklerini nasıl koruyabilirsiniz anlatmaktadır

## <a name="scenario"></a>Senaryo
Bu bölümde, sanal makine disklerini anlık görüntülerini kullanarak özel bir yedekleme stratejisi içeren bir senaryo açıklanmaktadır.

DS serisi Azure VM'ye bağlı premium depolama P30 diskle göz önünde bulundurun. P30 disk adı *mypremiumdisk* adlı bir premium depolama hesabında depolanan *mypremiumaccount*. Standart depolama hesabı olarak adlandırılan *mybackupstdaccount* yedeğini depolamak için kullanılan *mypremiumdisk*. Biz anlık görüntüsünü almak istediğiniz *mypremiumdisk* 12 saatte bir.

Depolama hesabı ve diskleri oluşturma hakkında bilgi edinmek için bkz [Azure storage hesapları hakkında](../articles/storage/storage-create-storage-account.md).

Azure VM'lerin yedeklenmesi hakkında bilgi edinmek için bkz [planlama Azure VM yedeklemeleri](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Artımlı anlık görüntülerini kullanarak bir disk yedeklerini korumak için adımlar
Aşağıdaki adımlar anlık görüntülerini almak nasıl açıklamaktadır *mypremiumdisk* ve yedeklemelerin sürdürmek *mybackupstdaccount*. Adlı bir standart sayfa blob'u yedeğidir *mybackupstdpageblob*. Yedekleme sayfa blobu her zaman son anlık görüntüsü ile aynı duruma yansıtır *mypremiumdisk*.

1. Bir anlık görüntüsü tarafından yedekleme sayfa blobu, premium depolama diski oluşturma *mypremiumdisk* adlı *mypremiumdisk_ss1*.
2. Bu anlık görüntü için mybackupstdaccount adlı bir sayfa blob'u olarak kopyalamak *mybackupstdpageblob*.
3. Bir anlık görüntüsünü *mybackupstdpageblob* adlı *mybackupstdpageblob_ss1*kullanarak [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) içinde depolamak ve *mybackupstdaccount*.
4. Yedekleme penceresi sırasında başka bir anlık görüntüsünü oluşturmak *mypremiumdisk*, deyin *mypremiumdisk_ss2*, içinde depolamak ve *mypremiumaccount*.
5. Artımlı değişiklikler arasında iki anlık görüntüleri alma *mypremiumdisk_ss2* ve *mypremiumdisk_ss1*kullanarak [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) üzerinde *mypremiumdisk_ ss2* ile **prevsnapshot** parametre kümesi için zaman damgasını *mypremiumdisk_ss1*. Bu artımlı değişiklikler için yedekleme sayfa blobu yazma *mybackupstdpageblob* içinde *mybackupstdaccount*. Artımlı değişiklikler silinen aralık varsa yedekleme sayfa BLOB'dan temizlenmelidir. Kullanım [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) artımlı değişiklikler için yedekleme sayfa blobu yazma.
6. Yedekleme sayfa blobu bir anlık görüntüsünü *mybackupstdpageblob*adlı *mybackupstdpageblob_ss2*. Önceki anlık görüntüyü silmek *mypremiumdisk_ss1* premium depolama hesabından.
7. Her yedekleme penceresi 4-6 adımlarını yineleyin. Bu şekilde, yedeklerini bulundurabilirsiniz *mypremiumdisk* bir standart depolama hesabı.

![Artımlı anlık görüntülerini kullanarak diski yedeklemeniz](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Bir disk anlık görüntülerden geri yüklemek için adımları
Aşağıdaki adımlar, premium disk geri yükleme açıklamaktadır *mypremiumdisk* önceki bir anlık görüntüye yedekleme depolama hesabından *mybackupstdaccount*.

1. Premium diske geri yüklemek istediğiniz zaman noktası belirleyin. Anlık görüntü olduğunu düşünelim *mybackupstdpageblob_ss2*, yedekleme depolama hesabında depolanan *mybackupstdaccount*.
2. Anlık görüntü mybackupstdaccount içinde Yükselt *mybackupstdpageblob_ss2* yeni yedekleme temel sayfa blobu olarak *mybackupstdpageblobrestored*.
3. Adlı bu geri yüklenen yedekleme sayfa blobu bir anlık görüntüsünü *mybackupstdpageblobrestored_ss1*.
4. Geri yüklenen sayfa blob kopyalama *mybackupstdpageblobrestored* gelen *mybackupstdaccount* için *mypremiumaccount* yeni premium disk olarak  *mypremiumdiskrestored*.
5. Bir anlık görüntüsünü *mypremiumdiskrestored*adlı *mypremiumdiskrestored_ss1* gelecekteki artımlı yedekleme yapmak için.
6. Geri yüklenen diske DS serisi VM'ın üzerine *mypremiumdiskrestored* ve eski ayırma *mypremiumdisk* VM'den.
7. Geri yüklenen disk için önceki bölümde açıklanan yedekleme işlemine başlamak *mypremiumdiskrestored*kullanarak *mybackupstdpageblobrestored* yedekleme sayfa blobu olarak.

![Disk anlık görüntülerden geri yükleme](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bir blob anlık görüntülerini oluşturma ve VM yedekleme altyapınızı planlama hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanın.

* [Bir BLOB bir anlık görüntü oluşturma](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [VM yedekleme altyapınızı planlama](../articles/backup/backup-azure-vms-introduction.md)

