>[!NOTE]
> Bu sayfadaki geri bildirim veya aracılığıyla açıklamaları bırakabilirsiniz [Azure geri bildirim](https://feedback.azure.com/forums/216843-virtual-machines) #azerrormessage etiketi.

## <a name="error-response-format"></a>Hata yanıt biçimi 
Azure VM'ler için hata yanıtı aşağıdaki JSON biçimi kullanın:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Bir hata yanıtı her zaman bir durum kodu ve hata nesnesi içerir. Her hata nesnesi her zaman bir hata kodu ve bir ileti içerir. VM sahip bir şablon oluşturduysanız, hata nesnesi iç düzeyde hata kodları ve iletisi içeren bir ayrıntıları bölümü de içerir. Normalde, hata iletisi en iç düzeyi kök hatasıdır.


## <a name="common-virtual-machine-management-errors"></a>Genel sanal makine Yönetimi hataları

Bu bölümde, sanal makineleri yönetirken karşılaşabileceğiniz genel hata iletileri listelenir:

|  Hata kodu  |  Hata iletisi  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  URI {1} blob kullanılarak ' {0}' diski oluşturulurken kira elde edemedi. BLOB zaten kullanılıyor.  |  
|  AllocationFailed  |  Ayırma başarısız oldu. Lütfen VM boyutunu ya da VM'lerin sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı kullanılabilirlik kümesi için bir veya farklı bir Azure konumuna dağıtmayı deneyin.  |  
|  AllocationFailed  |  VM ayırma bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtmayı deneyin.  |
|  ArtifactNotFound  |  VM uzantısı Yayımcı '{0}' ve türü '{1}' ile '{2}' konumunda bulunamadı.  |
|  ArtifactNotFound  |  Uzantısı ile Yayımcı '{0}', '{1}' yazın ve tür işleyicisi sürümü '{2}' uzantı deposunda bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, istenen '{0}' sürümünü karşılayan hiçbir sürüm bulunamadı.  |
|  ArtifactVersionNotFound  |  Hiçbir istenen Sürüm '{0}' karşılayan yapıt deposunda VM uzantısı için '{1}' yayımcıyla bulundu ve '{2}' yazın.  |
|  AttachDiskWhileBeingDetached  |  Disk şu anda ayrılmakta olduğundan veri diski '{0}' '{1}' VM'ye bağlanamaz. Disk tamamen ayrılana kadar bekleyin ve yeniden deneyin.  |
|  BadRequest  |  Hizalı ' kullanılabilirlik kümeleri henüz bu bölgede desteklenmiyor.  |
|  BadRequest  |  Yönetilen diskleri yönetilmeyen kullanılabilirlik kümesi için bir VM eklenmesi veya blob tabanlı disklerin yönetilen kullanılabilirlik kümesi için bir VM eklenmesi desteklenmiyor. Lütfen 'yönetilen' özelliği için bir VM yönetilen disklerle eklemek için ayarlanmış bir kullanılabilirlik kümesi oluşturun.  |
|  BadRequest  |  Yönetilen diskleri bu bölgede desteklenmiyor.  |
|  BadRequest  |  İşletim sistemi için desteklenmeyen işleyici başına birden çok Vmextension türü: '{0}'. VMExtension '{1}' işleyicisi '{2}' ile zaten eklendi veya girişinde belirtildi.  |
|  BadRequest  |  Kaynak yönetilen disklerle ' {1}' {0}' işlemi desteklenmiyor.  |
|  CertificateImproperlyFormatted  |  Parolanın {0} kaynağından alınan JSON gösterimi, düzgün şekilde biçimlendirilmemiş bir PFX dosyası olan veri alanı içeriyor veya belirtilen parola PFX dosyasını doğru şekilde çözmüyor.  |
|  CertificateImproperlyFormatted  |  {0} kaynağından alınan veriler JSON biçiminde seri hale getirilemez.  |
|  Çakışma  |  Yalnızca bir VM oluşturulurken veya VM serbest bırakılırken diski yeniden boyutlandırmaya izin verilir.  |
|  ConflictingUserInput  |  Disk '{0}' diski zaten VM '{1}' tarafından sahiplenilmiş olarak eklenemiyor.  |
|  ConflictingUserInput  |  Kaynak ve hedef kaynak grupları aynı.  |
|  ConflictingUserInput  |  {0} diskinin kaynak ve hedef depolama hesapları farklı.  |
|  ContainerAlreadyOnLease  |  {0} URI'sini içeren blobu barındıran depolama kapsayıcısı üzerinde zaten bir kira var.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları taşıma isteği, istekteki bir veya daha fazla {0} s tarafından başvurulan KeyVault kaynakları içeriyor. Bu çapraz abonelik taşıma şu anda desteklenmiyor. KeyVault kaynak kimlikleri için hata ayrıntılarını gözden geçirin.  |
|  DiagnosticsOperationInternalError  |  VM {0} tanılama profili işlenirken bir iç hata oluştu.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} '{1}' VM'ine ait başka bir disk tarafından zaten kullanılıyor. Disk başvuru bilgisi için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  '{1}' disk için URI {0} içeren VHD blobu bulunamıyor.  |
|  DiskBlobNotFound  |  {0} URI'sini içeren VHD blobu bulunamıyor.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} gizli {1} etiketleri sahip değil. Lütfen parolanın sürümünü güncelleştirin, gerekli etiketleri ekleyip yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Başarısız anahtar {1} kullanarak gizli {0} değerini açılacak.  |
|  DiskImageNotReady  |  Disk görüntüsü {0} {1} durumda. Görüntü hazır olduğunda lütfen yeniden deneyin.  |
|  DiskPreparationError  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için disk örneği görünümüne bakın.  |
|  DiskProcessingError  |  VM başarısız olan disklerle diğer diskler içerdiğinden disk işleme işlemi durduruldu.  |
|  ImageBlobNotFound  |  '{1}' disk için URI {0} içeren VHD blobu bulunamıyor.  |
|  ImageBlobNotFound  |  {0} URI'sini içeren VHD blobu bulunamıyor.  |
|  IncorrectDiskBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. Disk '{1}' için BLOB {0} ise blok blobu türünde değil.  |
|  IncorrectDiskBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} '{1}' türünde.  |
|  IncorrectImageBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. Disk '{1}' için BLOB {0} ise blok blobu türünde değil.  |
|  IncorrectImageBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} '{1}' türünde.  |
|  InternalOperationError  |  Depolama hesabı {0} çözümlenemedi. Lütfen işlem kaynağıyla aynı konumda bulunan depolama kaynağı sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  InternalOperationError  |  {0} hedef arama görevi başarısız oldu.  |
|  InternalOperationError  |  '{0}' adlı VM'nin ağ profili doğrulanırken hata oluştu.  |
|  InvalidAccountType  |  AccountType {0} geçersiz.  |
|  InvalidParameter  |  {0} parametre değeri geçersiz.  |
|  InvalidParameter  |  Belirtilen Yönetici parolasına izin verilmiyor.  |
|  InvalidParameter  |  "Sağlanan parola {0} arasında olmalıdır-\ {1\} karakter uzunluğunda ve en az getirmelidir aşağıdaki parola karmaşıklık gereksinimlerinden {2}: <ol><li> Büyük bir karakter içeriyor</li><li>Küçük bir karakter içeriyor</li><li>Sayısal hane içerir</li><li>Özel bir karakter içeriyor.</li></ol>  |
|  InvalidParameter  |  Belirtilen Yönetici Kullanıcı Adı'na izin verilmiyor.  |
|  InvalidParameter  |  VM bir platform veya kullanıcı görüntüsünden oluşturulduysa var olan bir İşletim Sistemi diski VM'ye bağlanamaz.  |
|  InvalidParameter  |  {0} kapsayıcı adı geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire başında olmalıdır ve bir alfasayısal karakter.  |
|  InvalidParameter  |  Kapsayıcı {0} URL {1} adı geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire başında olmalıdır ve bir alfasayısal karakter.  |
|  InvalidParameter  |  URL {0} blob adı bir eğik çizgi içerir. Bu diskleri için şu anda desteklenmiyor.  |
|  InvalidParameter  |  URI {0} doğru blob URI'si gibi görünmüyor.  |
|  InvalidParameter  |  '{0}' adlı bir disk aynı LUN'u kullanan: {1}.  |
|  InvalidParameter  |  '{0}' adlı bir disk yok.  |
|  InvalidParameter  |  Belirtilen görüntü başvurusunda zaten tanımlı olan bir disk için kullanıcı görüntüsü geçersiz kılmaları belirtilemez.  |
|  InvalidParameter  |  '{0}' adlı bir disk aynı VHD URL'si {1} kullanır.  |
|  InvalidParameter  |  Belirtilen hata etki alanı sayısı {0} aralığı {1}, {2} için olması gerekir.  |
|  InvalidParameter  |  Lisans türü {0} geçersiz. Geçerli lisans türleri: Windows_Client ve Windows_Server, büyük küçük harfe duyarlı.  |
|  InvalidParameter  |  Linux ana bilgisayar adı en fazla {0} karakter uzunluğunda veya şu karakterleri içeremez: {1}.  |
|  InvalidParameter  |  Linux sağlama aracısındaki bilinen bir sorun nedeniyle Ssh ortak anahtarları için hedef yol şu anda varsayılan değeri olan {0} ile sınırlı.  |
|  InvalidParameter  |  {0} adlı LUN'da zaten bir disk var.  |
|  InvalidParameter  |  İstek {0} aboneliği, yönetilen disk kimliğindeki abonelik {1} eşleşmesi gerekir.  |
|  InvalidParameter  |  OSProfile özel verileri Base64 ile kodlanmalı ve en fazla {0} karakter uzunluğunda olmalıdır.  |
|  InvalidParameter  |  URL {0} BLOB adı '{1}' uzantısıyla bitmelidir.  |
|  InvalidParameter  |  {0}' geçerli bir yakalanmış VHD blob adı öneki değil. Geçerli bir önek regex '{1}' ile eşleşir.  |
|  InvalidParameter  |  VM aracısı sağlanmadan VM'nize sertifika eklenemez.  |
|  InvalidParameter  |  {0} adlı LUN'da zaten bir disk var.  |
|  InvalidParameter  |  İstenen boyuta {0} kullanılabilirlik belirlendiği kümedeki kullanılabilir olmadığından oluşturulamıyor VM şu anda ayrılmış. Kullanılabilir boyutları: {1}. Daha fazla üzerinde VM stratejisi https://aka.ms/azure-resizevm adresindeki yeniden boyutlandırma okuyun.  |
|  InvalidParameter  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Geçerli bölgede boyutları: {1}. Kullanılabilir VM boyutları hakkında daha fazla bilgi https://aka.ms/azure-regions adresindeki her bölgede öğrenin.  |
|  InvalidParameter  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Kullanılabilir VM boyutları hakkında daha fazla bilgi https://aka.ms/azure-regions adresindeki her bölgede öğrenin.  |
|  InvalidParameter  |  Windows yönetici kullanıcı adı olamaz fazlasını {0} karakterden uzun, ile bitemez veya şu karakterleri içeremez: {1}.  |
|  InvalidParameter  |  Windows bilgisayar adı olamaz fazlasını {0} karakterden uzun, tamamen rakamlardan oluşamaz veya şu karakterleri içeremez: {1}.  |
|  MissingMoveDependentResources  |  Kaynakları taşıma isteği tüm bağımlı kaynakları içermiyor. Eksik kaynak kimlikleri için hata ayrıntılarını gözden geçirin.  |
|  MoveResourcesHaveInvalidState  |  Kaynakları taşıma isteği geçersiz depolama hesaplarıyla ilişkili VM'ler içerir. Bu kaynak kimlikleri ve başvurulan depolama hesabı adları ayrıntılarını gözden geçirin.  |
|  MoveResourcesHavePendingOperations  |  Kaynakları taşıma isteği, bir işlemi bekliyor kaynaklar içeriyor. Bu kaynak kimliklerinin ayrıntılarını gözden geçirin. Bekleyen işlemler tamamlandıktan sonra işlemi yeniden deneyin.  |
|  MoveResourcesNotFound  |  Kaynakları taşıma isteği bulunamayan kaynaklar içeriyor. Bu kaynak kimliklerinin ayrıntılarını gözden geçirin.  |
|  NetworkingInternalOperationError  |  Bilinmeyen ağ ayırma hatası.  |
|  NetworkingInternalOperationError  |  Bilinmeyen ağ ayırma hatası  |
|  NetworkingInternalOperationError  |  VM'nin ağ profili işlenirken bir iç hata oluştu.  |
|  notFound  |  {0} Kullanılabilirlik Kümesi bulunamıyor.  |
|  notFound  |  Kaynak sanal makine '{0}' istekte belirtilen Azure bu konumda mevcut değil.  |
|  notFound  |  {0} kimlikli kiracı bulunamadı.  |
|  notFound  |  {0} Görüntüsü bulunamıyor.  |
|  NotSupported  |  Lisans türü {0}, ancak görüntü blob {1} şirket içinden değil.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi {0} silinemez. Bir kullanılabilirlik kümesi silmeden önce lütfen tüm VM içermediğinden emin olun.  |
|  OperationNotAllowed  |  'Hizalı' SKU 'Klasik' için kullanılabilirlik kümesi değiştirilmesine izin verilmez.  |
|  OperationNotAllowed  |  VM çalışır durumda değilken VM uzantıları değiştirilemez.  |
|  OperationNotAllowed  |  Yakalama eylem yalnızca blob tabanlı disklerin bir sanal makinede desteklenmiyor. Yönetilen bir sanal makineden bir görüntü oluşturmak için lütfen 'Image' kaynak API'leri kullanın.  |
|  OperationNotAllowed  |  Görüntüsü başarıyla oluşturuldu kadar kaynak {0} görüntü {1} oluşturulamıyor.  |
|  OperationNotAllowed  |  VM ayrıldığında encryptionSettings güncelleştirmelerine izin verilmiyor. VM serbest bırakıldıktan sonra lütfen yeniden deneyin  |
|  OperationNotAllowed  |  Blob tabanlı diskleri olan bir VM'ye yönetilen disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Bu boyutta bir VM'ye eklenmiş izin veri diski maksimum sayısı {0} ' dir.  |
|  OperationNotAllowed  |  Yönetilen diskleri olan bir VM'ye blob tabanlı disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Görüntü silinmek için işaretlendiğinden işlemi '{0}' görüntüsü '{1}' üzerinde izin verilmiyor. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  VM genelleştirilmiş olduğundan '{0}' işlemi '{1}' VM üzerinde izin verilmiyor.  |
|  OperationNotAllowed  |  Geri yükleme noktası koleksiyonu '{1}' silinmek üzere işaretlenmiş şekilde işlemi '{0}' izin verilmiyor.  |
|  OperationNotAllowed  |  '{0}' işlemi silinmek için işaretlendiğinden VM uzantısı '{1}' izin verilmiyor. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  '{0}' işlemi '{2}' görüntüsü kullanarak sanal makineleri '{1}' sağlanacak bu yana izin verilmiyor.  |
|  OperationNotAllowed  |  Sanal makine ScaleSet '{1}' '{2}' görüntüsü şu anda kullanıldığından işlemi '{0}' izin verilmiyor.  |
|  OperationNotAllowed  |  VM silinmek için işaretlendiğinden VM '{1}' işlemi '{0}' izin verilmiyor. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  VM serbest veya serbest bırakılmak üzere işaretlendiği beri '{0}' işlemi '{1}' VM üzerinde izin verilmiyor.  |
|  OperationNotAllowed  |  VM çalıştığından işlemi '{0}' VM '{1}' izin verilmiyor. VM'yi konuk işletim sisteminin içinden kapatmanız durumunda güç kapalı açıkça Lütfen.  |
|  OperationNotAllowed  |  VM değil serbest olduğundan '{0}' işlemi '{1}' VM izin verilmiyor.  |
|  OperationNotAllowed  |  VM uzantısı '{2}' başarısız durumda olduğundan '{0}' işlemi '{1}' VM üzerinde izin verilmiyor.  |
|  OperationNotAllowed  |  Başka bir işlem sürmekte olduğundan işlem '{0}' VM '{1}' izin verilmiyor.  |
|  OperationNotAllowed  |  '{0}' işlemi genelleştirilmiş sanal makine '{1}' gerektirir.  |
|  OperationNotAllowed  |  Bu işlem, VM'nin çalışıyor (veya çalışmak üzere ayarlanmış) olmasını gerektirir.  |
|  OperationNotAllowed  |  Boyutundan daha küçük olan disk boyutu {0} GB, görüntü, karşılık gelen disk {1}GB izin verilmez.  |
|  OperationNotAllowed  |  '{0}' işleyicisinin VM Ölçek Kümesi uzantıları yalnızca VM Ölçek Kümesi oluşturma işlemi sırasında eklenebilir.  |
|  OperationNotAllowed  |  '{0}' işleyicisinin VM Ölçek Kümesi uzantıları yalnızca VM Ölçek Kümesi silme işlemi sırasında eklenebilir.  |
|  OperationNotAllowed  |  VM '{0}', yönetilen diskleri zaten kullanıyor.  |
|  OperationNotAllowed  |  VM '{0}', 'Klasik' kullanılabilirlik kümesi '{1}' için aittir. Lütfen 'Hizalı' SKU kullanın ve dönüştürme yeniden denemek için kullanılabilirlik kümesinde güncelleştirin.  |
|  OperationNotAllowed  |  Görüntüden oluşturulan VM blob tabanlı disklerin sahip olamaz. Tüm diskleri yönetilen diskleri olması gerekir.  |
|  OperationNotAllowed  |  VM genelleştirilmiş olmadığından yakalama işlemi tamamlanamıyor.  |
|  OperationNotAllowed  |  VM diskleri yönetilen disklere dönüştürüldüğünden VM '{0}' yönetim işlemlerini izin verilmez.  |
|  OperationNotAllowed  |  Devam eden bir işlem sanal makine {0} güç durumunu {1} için değiştiriyor. Lütfen bir süre sonra işlemi {2} gerçekleştirin.  |
|  OperationNotAllowed  |  Eklenemiyor veya VM güncelleştirin. İstenen VM boyutu {0} varolan ayırma biriminde kullanılamayabilir. Daha fazla üzerinde VM stratejisi https://aka.ms/azure-resizevm adresindeki yeniden boyutlandırma okuyun.  |
|  OperationNotAllowed  |  İstenen boyuta {0} kullanılabilirlik belirlendiği kümedeki kullanılabilir olmadığından yeniden boyutlandırmak için VM şu anda ayrılmış. Kullanılabilir boyutları: {1}. Daha fazla üzerinde VM stratejisi https://aka.ms/azure-resizevm adresindeki yeniden boyutlandırma okuyun.  |
|  OperationNotAllowed  |  İstenen boyuta {0} burada VM şu anda ayrılmış küme içinde kullanılabilir olmadığından VM'yi yeniden boyutlandırın kurulamıyor. Lütfen VM'nize {1} ayırması yeniden boyutlandırma (durdurma işlemi Azure portalında budur) ve yeniden boyutlandırma işlemi yeniden deneyin. Daha fazla üzerinde VM stratejisi https://aka.ms/azure-resizevm adresindeki yeniden boyutlandırma okuyun.  |
|  OSProvisioningClientError  |  Şu anda konuk işletim sistemi sağlanmakta olduğundan, '{0}' adlı VM için İşletim Sistemi Sağlama başarısız oldu.  |
|  OSProvisioningClientError  |  VM için '{0}' işletim sistemi sağlama başarısız oldu. Hata ayrıntıları: {1} görüntü düzgün hazırlanmış emin olun (genelleştirildiğinden). <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana anahtar oluşturma işlemi başarısız oldu. Hata ayrıntıları: {0}. Gidermek için bu sorunu doğrulayın Linux Aracısı düzgün bir şekilde ayarlanırsa. <ul><li>Kısmındaki yönergeleri denetleyebilirsiniz: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen kullanıcı adı bu Linux dağıtım için geçersiz. Hata ayrıntıları: {0}.  |
|  OSProvisioningInternalError  |  '{0}' adlı VM için İşletim Sistemi Sağlama işlemi bir iç hata nedeniyle başarısız oldu.  |
|  OSProvisioningTimedOut  |  İşletim sistemi sağlama VM '{0}' için ayrılan sürede tamamlanmadı. VM, hala başarıyla sağlama işlemi olabilir. Lütfen sağlama durumunu daha sonra denetleyin.  |
|  OSProvisioningTimedOut  |  İşletim sistemi sağlama VM '{0}' için ayrılan sürede tamamlanmadı. VM, hala başarıyla sağlama işlemi olabilir. Lütfen sağlama durumunu daha sonra denetleyin. Ayrıca, görüntünün doğru hazırlanmış emin olun (genelleştirildiğinden).   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  İşletim sistemi sağlama VM '{0}' için ayrılan sürede tamamlanmadı. Ancak, VM Konuk aracısının çalıştıran algılandı. Bunu konuk işletim sistemini olmayan bir VM görüntüsü olarak kullanılacak doğru hazırlanmış öneren (CreateOption ile FromImage =). Bu sorunu gidermek için kullanın ya da CreateOption ile olduğu gibi VHD Attach = ya da bir görüntü olarak kullanmak için düzgün şekilde hazırlayın:   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu, seçilen konumda şu an için kullanılamıyor.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Kaynak, devam eden platform güncelleştirmesi nedeniyle şu anda güncelleştirilemiyor. Lütfen daha sonra tekrar deneyin.  |
|  StorageAccountLimitation  |  '{0}' adlı depolama hesabı, disk oluşturmak için gerekli olan sayfa bloblarını desteklemiyor.  |
|  StorageAccountLimitation  |  '{0}' adlı depolama hesabı, ayrılmış kotasını aştı.  |
|  StorageAccountLocationMismatch  |  Depolama hesabı {0} çözümlenemedi. Lütfen işlem kaynağıyla aynı konumda bulunan depolama kaynağı sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  StorageAccountNotFound  |  Bulunamadı depolama hesabı {0}. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait emin olun.  |
|  StorageAccountNotRecognized  |  Lütfen depolama kaynağı sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. {0} kullanımı desteklenmiyor.  |
|  StorageAccountOperationInternalError  |  {0} adlı depolama hesabına erişilirken bir iç hata oluştu.  |
|  StorageAccountSubscriptionMismatch  |  Depolama hesabı {0} abonelik {1} ait değil.  |
|  StorageAccountTooBusy  |  '{0}' depolama hesabı şu anda çok meşgul. Başka bir hesabı kullanmayı deneyin.  |
|  StorageAccountTypeNotSupported  |  Disk {0} bir Blob Depolama hesabı olan {1} kullanır. Lütfen genel amaçlı depolama hesabıyla yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama hesabı {0} {1} türü değil. Önyükleme tanılaması {2} depolama hesabı türlerini destekler.  |
|  SubscriptionNotAuthorizedForImage  |  Abonelik yetkilendirilmemiş.  |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} zaten var. Lütfen farklı bir blob URI'si yeni boş veri diski '{1}' oluşturulacağını belirtin.  |
|  TargetDiskBlobAlreadyExists  |  Yakalama işlemine devam edilemiyor çünkü hedef görüntü blobu {0} zaten var ve VHD bloblarının üzerine yazmayı sağlayan bayrağı ayarlanmamış. Blob silin veya VHD bloblarının üzerine yazmayı ve yeniden deneyin bayrağını ayarlayın.  |
|  TargetDiskBlobAlreadyExists  |  {0} hedef görüntü blobunun üzerinde etkin bir kira olduğundan yakalama işlemine devam edilemiyor.   |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} zaten var. Lütfen farklı bir blob URI'si '{1}' diski için hedef olarak belirtin.  |
|  TooManyVMRedeploymentRequests  |  VM '{0}' veya bu VM ile aynı kullanılabilirlik kümesinde bulunan VM'ler için çok fazla yeniden dağıtım isteği alınmadı. Lütfen daha sonra yeniden deneyin.  |
|  VHDSizeInvalid  |  {0} disk blob {2} ile ' {1}' için belirtilen disk boyutu değeri geçersiz. Disk boyutu {3} ve {4} arasında olmalıdır.  |
|  VMAgentStatusCommunicationError  |  VM '{0}', VM aracısının veya uzantılarının durumunu bildirmedi. Lütfen VM çalışan bir VM Aracısı olduğunu ve Azure depolamaya giden bağlantı kurabilir doğrulayın.  |
|  VMArtifactRepositoryInternalError  |  VM yapıt ayrıntılarını almak için yapıt deposuyla iletişim kurulurken bir hata oluştu.  |
|  VMArtifactRepositoryInternalError  |  Yapıt deposundan VM yapıt verileri alınırken bir iç hata oluştu.  |
|  VMExtensionHandlerNonTransientError  |  İşleyici '{0}' bildirdi hatası VM uzantısı '{1}' terminal hata kodu '{2}' ve hata iletisi: '{3}'  |
|  VMExtensionManagementInternalError  |  '{0}' VM uzantısı işlenirken iç hata oluştu.  |
|  VMExtensionManagementInternalError  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningError  |  VM uzantısı '{0}' işlenirken bir hata bildirdi. Hata iletisi: "{1}".  |
|  VMExtensionProvisioningError  |  Birden çok VM uzantıları VM sağlanması başarısız oldu. Lütfen ayrıntılar için VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  '{0}' VM uzantısının sağlanması zaman aşımına uğradı. Genişletme yüklemesinin çok uzun sürüyor veya uzantı durumu sağlanamadı.  |
|  VMMarketplaceInvalidInput  |  Olmayan Market görüntüsünden sanal makine oluşturma gerekmez bilgi planlama, lütfen Plan bilgileri istekte kaldırın. İşletim sistemi disk adı {0} ' dir.  |
|  VMMarketplaceInvalidInput  |  Satın alma bilgileri eşleşmiyor. Market görüntüsünden dağıtım yapılamıyor. İşletim sistemi disk adı {0} ' dir.  |
|  VMMarketplaceInvalidInput  |  Market görüntüsünden sanal makine oluşturulurken Plan bilgisi isteği gerektirir. İşletim sistemi disk adı {0} ' dir.  |
|  VMNotFound  |  '{0}' VM bulunamıyor.  |
|  VMRedeploymentFailed  |  VM '{0}' vm'inin yeniden dağıtımı bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin.  |
|  VMRedeploymentTimedOut  |  VM '{0}' vm'inin yeniden dağıtımı ayrılan sürede bitmedi. Başarıyla içinde süre bitirmek. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  VMStartTimedOut  |  VM '{0}', ayrılan sürede başlatılmadı. VM hala başarıyla başlatılabilir. Lütfen daha sonra güç durumunu denetleyin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.