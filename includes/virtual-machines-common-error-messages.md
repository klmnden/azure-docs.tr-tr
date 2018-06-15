---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/25/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: db241c1a3c8bfd15e13ae0bd9f1cdf4c92c7081d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34013945"
---
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

|  Hata Kodu  |  Hata İletisi  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Diski oluşturulurken kira elde edemedi '{0}' URI'sini içeren blob kullanılarak {1}. BLOB zaten kullanılıyor.  |  
|  AllocationFailed  |  Ayırma başarısız oldu. Lütfen VM boyutunu ya da VM'lerin sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı kullanılabilirlik kümesi için bir veya farklı bir Azure konumuna dağıtmayı deneyin.  |  
|  AllocationFailed  |  VM ayırma bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtmayı deneyin.  |
|  ArtifactNotFound  |  Yayımcı VM uzantısıyla '{0}'ve türü'{1}'konumunda bulunamadı'{2}'.  |
|  ArtifactNotFound  |  Yayımcı uzantısıyla '{0}', türü '{1}', tür işleyicisi sürümü '{2}' uzantı deposunda bulunamadı.  |
|  ArtifactVersionNotFound  |  İstenen sürüm karşılayan yapıt deposunda hiçbir sürüm bulunamadı '{0}'.  |
|  ArtifactVersionNotFound  |  İstenen sürüm karşılayan yapıt deposunda hiçbir sürüm bulunamadı '{0}'için VM uzantısı yayımcısı'{1}'ve türü'{2}'.  |
|  AttachDiskWhileBeingDetached  |  Veri diski bağlanamaz '{0}'için VM'{1}' disk şu anda ayrılmakta olduğundan. Disk tamamen ayrılana kadar bekleyin ve yeniden deneyin.  |
|  BadRequest  |  Hizalı ' kullanılabilirlik kümeleri henüz bu bölgede desteklenmiyor.  |
|  BadRequest  |  Yönetilen diskleri yönetilmeyen kullanılabilirlik kümesi için bir VM eklenmesi veya blob tabanlı disklerin yönetilen kullanılabilirlik kümesi için bir VM eklenmesi desteklenmiyor. Lütfen 'yönetilen' özelliği için bir VM yönetilen disklerle eklemek için ayarlanmış bir kullanılabilirlik kümesi oluşturun.  |
|  BadRequest  |  Yönetilen Diskler bu bölgede desteklenmiyor.  |
|  BadRequest  |  İşletim sistemi türü için desteklenmeyen işleyici başına birden çok Vmextension '{0}'. VMExtension '{1}'işleyicisiile'{2}' zaten eklenmiş veya girişinde belirtildi.  |
|  BadRequest  |  İşlem '{0}'Kaynağında desteklenmiyor'{1}' yönetilen diske sahip.  |
|  CertificateImproperlyFormatted  |  Kaynağından alınan JSON gösterimi parolanın {0} düzgün şekilde biçimlendirilmemiş bir PFX dosyası olmayan bir veri alanı içeriyor veya belirtilen parola PFX dosyasını doğru şekilde çözmüyor.  |
|  CertificateImproperlyFormatted  |  Kaynağından alınan veriler {0} JSON'a serisi değil.  |
|  Çakışma  |  Yalnızca bir VM oluşturulurken veya VM serbest bırakılırken diski yeniden boyutlandırmaya izin verilir.  |
|  ConflictingUserInput  |  Disk '{0}'eklenemiyor, disk VM tarafından zaten sahip olduğu gibi'{1}'.  |
|  ConflictingUserInput  |  Kaynak ve hedef kaynak grupları aynı.  |
|  ConflictingUserInput  |  Kaynak ve hedef depolama hesapları için disk {0} farklıdır.  |
|  ContainerAlreadyOnLease  |  Zaten var. bir kira URI'sini içeren blobu barındıran depolama kapsayıcısı üzerinde {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları taşıma isteği, bir veya daha fazla tarafından başvurulan KeyVault kaynakları içeriyor. {0}istekte s. Bu çapraz abonelik taşıma şu anda desteklenmiyor. KeyVault kaynak kimlikleri için hata ayrıntılarını gözden geçirin.  |
|  DiagnosticsOperationInternalError  |  VM tanılama profili işlenirken bir iç hata oluştu {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} VM'ine ait başka bir disk tarafından zaten kullanılıyor '{1}'. Disk başvuru bilgisi için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  URI'sini içeren VHD blobu bulunamıyor {0} disk için '{1}'.  |
|  DiskBlobNotFound  |  URI'sini içeren VHD blobu bulunamıyor {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} gizli anahtarı yoksa {1} etiketler. Lütfen parolanın sürümünü güncelleştirin, gerekli etiketleri ekleyip yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Parola değerini kaydırma {0} anahtarı kullanarak değer {1} başarısız oldu.  |
|  DiskImageNotReady  |  Disk görüntüsü {0} yer {1} durumu. Görüntü hazır olduğunda lütfen yeniden deneyin.  |
|  DiskPreparationError  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için disk örneği görünümüne bakın.  |
|  DiskProcessingError  |  VM'in başarısız disklerde başka diskleri olduğu için disk işleme durduruldu.  |
|  ImageBlobNotFound  |  URI'sini içeren VHD blobu bulunamıyor {0} disk için '{1}'.  |
|  ImageBlobNotFound  |  URI'sini içeren VHD blobu bulunamıyor {0}.  |
|  IncorrectDiskBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} disk için '{1}' ise blok blobu türünde değil.  |
|  IncorrectDiskBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} türü '{1}'.  |
|  IncorrectImageBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} disk için '{1}' ise blok blobu türünde değil.  |
|  IncorrectImageBlobType  |  Disk blobları yalnızca türü sayfa blobu olabilir. BLOB {0} türü '{1}'.  |
|  InternalOperationError  |  Depolama hesabı çözümlenemedi {0}. Lütfen işlem kaynağıyla aynı konumda bulunan depolama kaynağı sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  InternalOperationError  |  {0} Hedef arama görevi başarısız oldu.  |
|  InternalOperationError  |  VM'nin ağ profili doğrulanırken hata oluştu '{0}'.  |
|  InvalidAccountType  |  AccountType {0} geçersiz.  |
|  InvalidParameter  |  Parametresinin değeri {0} geçersiz.  |
|  InvalidParameter  |  Belirtilen Yönetici parolasına izin verilmiyor.  |
|  InvalidParameter  |  "Sağlanan parola arasında olmalıdır {0}-{1} karakter uzunluğunda ve en az getirmelidir {2} aşağıdaki parola karmaşıklık gereksinimleri: <ol><li> Büyük bir karakter içeriyor</li><li>Küçük bir karakter içeriyor</li><li>Sayısal hane içerir</li><li>Özel bir karakter içeriyor.</li></ol>  |
|  InvalidParameter  |  Belirtilen Yönetici Kullanıcı Adı'na izin verilmiyor.  |
|  InvalidParameter  |  VM bir platform veya kullanıcı görüntüsünden oluşturulduysa var olan bir İşletim Sistemi diski VM'ye bağlanamaz.  |
|  InvalidParameter  |  Kapsayıcı adı {0} geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire başında olmalıdır ve bir alfasayısal karakter.  |
|  InvalidParameter  |  Kapsayıcı adı {0} URL'de {1} geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire başında olmalıdır ve bir alfasayısal karakter.  |
|  InvalidParameter  |  URL'sindeki blob adı {0} bir eğik çizgi içerir. Bu diskleri için şu anda desteklenmiyor.  |
|  InvalidParameter  |  URI {0} doğru blob URI'si gibi görünmüyor.  |
|  InvalidParameter  |  Adlı bir disk '{0}' zaten aynı LUN'u kullanan: {1}.  |
|  InvalidParameter  |  Adlı bir disk '{0}' zaten mevcut.  |
|  InvalidParameter  |  Belirtilen görüntü başvurusunda zaten tanımlı olan bir disk için kullanıcı görüntüsü geçersiz kılmaları belirtilemez.  |
|  InvalidParameter  |  Adlı bir disk '{0}' VHD URL'si zaten kullanıyor {1}.  |
|  InvalidParameter  |  Belirtilen hata etki alanı sayısı {0} aralığında olmalıdır {1} için {2}.  |
|  InvalidParameter  |  Lisans türü {0} geçersiz. Geçerli lisans türleri: Windows_Client ve Windows_Server, büyük küçük harfe duyarlı.  |
|  InvalidParameter  |  Linux ana bilgisayar adı en fazla {0} karakter uzunluğunda veya şu karakterleri içeremez: {1}.  |
|  InvalidParameter  |  Hedef yolu Ssh ortak anahtarları için varsayılan değer olarak şu anda sınırlı {0} Linux sağlama aracısındaki bilinen bir sorun nedeniyle.  |
|  InvalidParameter  |  LUN bir disk {0} zaten mevcut.  |
|  InvalidParameter  |  Abonelik {0} isteği aboneliğiyle eşleşmelidir {1} yönetilen disk kimliğindeki.  |
|  InvalidParameter  |  Osprofile özel verileri Base64 kodlaması ve en büyük uzunluğa sahip olmalıdır {0} karakter.  |
|  InvalidParameter  |  URL'sindeki BLOB adı {0} ile bitmelidir '{1}' uzantısı.  |
|  InvalidParameter  |  {0}' geçerli bir yakalanmış VHD blob adı öneki değil. Geçerli bir önek regex eşleşen '{1}'.  |
|  InvalidParameter  |  VM Aracısı sağlanmadan VM'nize sertifika eklenemez.  |
|  InvalidParameter  |  LUN bir disk {0} zaten mevcut.  |
|  InvalidParameter  |  VM için oluşturulamıyor istenen boyuta {0} burada kullanılabilirlik kümesi şu anda ayrılır kümedeki kullanılabilir değil. Kullanılabilir boyutları: {1}. Strateji, yeniden boyutlandırma üzerinde daha fazla VM okuma https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Geçerli bölgede boyutları: {1}. Her bir bölge içinde kullanılabilir VM boyutları hakkında daha fazla bilgi bulmak https://aka.ms/azure-regions.  |
|  InvalidParameter  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Her bir bölge içinde kullanılabilir VM boyutları hakkında daha fazla bilgi bulmak https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Windows yönetici kullanıcı adı olamaz birden fazla {0} karakter uzunluğunda, ile bitemez veya şu karakterleri içeremez: {1}.  |
|  InvalidParameter  |  Windows bilgisayar adı birden fazla {0} karakterler uzun, tamamen rakamlardan oluşamaz veya şu karakterleri içeremez: {1}.  |
|  MissingMoveDependentResources  |  Kaynakları taşıma isteği tüm bağımlı kaynakları içermiyor. Eksik kaynak kimlikleri için hata ayrıntılarını gözden geçirin.  |
|  MoveResourcesHaveInvalidState  |  Kaynakları taşıma isteği geçersiz depolama hesaplarıyla ilişkili VM'ler içerir. Bu kaynak kimlikleri ve başvurulan depolama hesabı adları ayrıntılarını gözden geçirin.  |
|  MoveResourcesHavePendingOperations  |  Kaynakları taşıma isteği, bir işlemi bekliyor kaynaklar içeriyor. Bu kaynak kimliklerinin ayrıntılarını gözden geçirin. Bekleyen işlemler tamamlandıktan sonra işlemi yeniden deneyin.  |
|  MoveResourcesNotFound  |  Kaynakları taşıma isteği bulunamayan kaynaklar içeriyor. Bu kaynak kimliklerinin ayrıntılarını gözden geçirin.  |
|  NetworkingInternalOperationError  |  Bilinmeyen ağ ayırma hatası.  |
|  NetworkingInternalOperationError  |  Bilinmeyen ağ ayırma hatası  |
|  NetworkingInternalOperationError  |  VM'nin ağ profili işlenirken bir iç hata oluştu.  |
|  Bulunamadı  |  Kullanılabilirlik kümesi {0} bulunamıyor.  |
|  Bulunamadı  |  Kaynak sanal makine '{0}' içinde belirtilen istek Azure bu konumda mevcut değil.  |
|  Bulunamadı  |  Kimliğine sahip Kiracı {0} bulunamadı.  |
|  Bulunamadı  |  Görüntü {0} bulunamıyor.  |
|  NotSupported  |  Lisans türü {0}, ancak görüntü blob'u {1} şirket içinden değil.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi {0} silinemez. Bir kullanılabilirlik kümesi silmeden önce lütfen tüm VM içermediğinden emin olun.  |
|  OperationNotAllowed  |  'Hizalı' SKU 'Klasik' için kullanılabilirlik kümesi değiştirilmesine izin verilmez.  |
|  OperationNotAllowed  |  VM çalışır durumda değilken VM uzantıları değiştirilemez.  |
|  OperationNotAllowed  |  Yakalama eylem yalnızca blob tabanlı disklerin bir sanal makinede desteklenmiyor. Yönetilen bir sanal makineden bir görüntü oluşturmak için lütfen 'Image' kaynak API'leri kullanın.  |
|  OperationNotAllowed  |  Kaynak {0} görüntüden oluşturulamıyor {1} kadar görüntüsü başarıyla oluşturuldu.  |
|  OperationNotAllowed  |  VM ayrılır, VM serbest bırakıldıktan sonra lütfen yeniden deneyin encryptionSettings güncelleştirmelerine izin verilmiyor  |
|  OperationNotAllowed  |  Blob tabanlı diskleri olan bir VM'ye yönetilen disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Bu boyutta bir VM'ye eklenmiş izin veri diski maksimum sayısı {0}.  |
|  OperationNotAllowed  |  Yönetilen diskleri olan bir VM'ye blob tabanlı disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  İşlem '{0}'Görüntüye izin verilmez'{1}' görüntünün silinmek için işaretlendiğinden. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' VM genelleştirilmiş olduğundan.  |
|  OperationNotAllowed  |  İşlem '{0}'geri yükleme noktası koleksiyonu olarak izin verilmez'{1}' silinmek üzere işaretlenmiş.  |
|  OperationNotAllowed  |  İşlem '{0}'VM uzantısı üzerinde izin verilmiyor'{1}' silinmek üzere işaretlendiğinden. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  İşlem '{0}'Sanal makineleri bu yana izin verilmez'{1}' görüntü kullanılarak sağlanır '{2}'.  |
|  OperationNotAllowed  |  İşlem '{0}'Sanal makine ScaleSet bu yana izin verilmez'{1}'görüntünün kullanmakta olduğu'{2}'.  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' VM silinmek için işaretlendiğinden. Yalnızca silme işlemini yeniden deneyin (veya devam eden bir tamamlanmasını bekleyin).  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' VM serbest veya serbest bırakılmak üzere işaretlendiği beri.  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' VM çalıştığından. VM'yi konuk işletim sisteminin içinden kapatmanız durumunda güç kapalı açıkça Lütfen.  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' VM değil serbest beri.  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}'VM uzantısı olduğundan'{2}' durumunda.  |
|  OperationNotAllowed  |  İşlem '{0}'VM izin verilmez'{1}' başka bir işlem sürmekte olduğundan.  |
|  OperationNotAllowed  |  İşlemi '{0}'Sanal makine gerektiriyor'{1}' genelleştirilmiş için.  |
|  OperationNotAllowed  |  Bu işlem, VM'nin çalışıyor (veya çalışmak üzere ayarlanmış) olmasını gerektirir.  |
|  OperationNotAllowed  |  Disk boyutu ile {0}boyutundan küçük GB {1}görüntüde, karşılık gelen disk GB'lik izin verilmez.  |
|  OperationNotAllowed  |  İşleyicisinin VM ölçek kümesi Uzantıları '{0}' yalnızca VM ölçek kümesi oluşturma sırasında eklenebilir.  |
|  OperationNotAllowed  |  İşleyicisinin VM ölçek kümesi Uzantıları '{0}' yalnızca VM ölçek kümesi silme işlemi sırasında silinebilir.  |
|  OperationNotAllowed  |  VM '{0}' yönetilen diskleri zaten kullanıyor.  |
|  OperationNotAllowed  |  VM '{0}'' Klasik' kullanılabilirlik kümesine ait'{1}'. Lütfen 'Hizalı' SKU kullanın ve dönüştürme yeniden denemek için kullanılabilirlik kümesinde güncelleştirin.  |
|  OperationNotAllowed  |  Görüntüden oluşturulan VM blob tabanlı disklerin sahip olamaz. Tüm diskleri yönetilen diskleri olması gerekir.  |
|  OperationNotAllowed  |  VM genelleştirilmiş olmadığından yakalama işlemi tamamlanamıyor.  |
|  OperationNotAllowed  |  Yönetim işlemlerini VM üzerinde '{0}' VM diskleri yönetilen disklere dönüştürüldüğünden izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden bir işlem sanal makinenin güç durumunu değiştirme {0} için {1}. Lütfen işlemi {2} bir süre sonra.  |
|  OperationNotAllowed  |  Eklenemiyor veya VM güncelleştirin. İstenen VM boyutu {0} varolan ayırma biriminde kullanılamayabilir. Strateji, yeniden boyutlandırma üzerinde daha fazla VM okuma https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  VM olduğundan yeniden boyutlandırmak için istenen boyuta {0} burada kullanılabilirlik kümesi şu anda ayrılır kümedeki kullanılabilir değil. Kullanılabilir boyutları: {1}. Strateji, yeniden boyutlandırma üzerinde daha fazla VM okuma https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  VM olduğundan yeniden boyutlandırmak için istenen boyuta {0} burada VM şu anda ayrılır kümedeki kullanılabilir değil. VM'nize yeniden boyutlandırmak için {1} Lütfen serbest bırakma (durdurma işlemi Azure portalında budur) ve yeniden boyutlandırma işlemi yeniden deneyin. Strateji, yeniden boyutlandırma üzerinde daha fazla VM okuma https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  İşletim sistemi sağlama başarısız oldu VM için '{0}' şu anda Konuk işletim sistemi sağlanmakta olduğundan.  |
|  OSProvisioningClientError  |  VM için işletim sistemi sağlama '{0}' başarısız oldu. Hata ayrıntıları: {1} görüntü düzgün hazırlanmış emin olun (genelleştirildiğinden). <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana anahtar oluşturma işlemi başarısız oldu. Hata ayrıntıları: {0}. Gidermek için bu sorunu doğrulayın Linux Aracısı düzgün bir şekilde ayarlanırsa. <ul><li>Kısmındaki yönergeleri denetleyebilirsiniz: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen kullanıcı adı bu Linux dağıtım için geçersiz. Hata ayrıntıları: {0}.  |
|  OSProvisioningInternalError  |  İşletim sistemi sağlama başarısız oldu VM için '{0}' iç hata nedeniyle.  |
|  OSProvisioningTimedOut  |  VM için işletim sistemi sağlama '{0}' ayrılan sürede tamamlanmadı. VM, hala başarıyla sağlama işlemi olabilir. Lütfen sağlama durumunu daha sonra denetleyin.  |
|  OSProvisioningTimedOut  |  VM için işletim sistemi sağlama '{0}' ayrılan sürede tamamlanmadı. VM, hala başarıyla sağlama işlemi olabilir. Lütfen sağlama durumunu daha sonra denetleyin. Ayrıca, görüntünün doğru hazırlanmış emin olun (genelleştirildiğinden).   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  VM için işletim sistemi sağlama '{0}' ayrılan sürede tamamlanmadı. Ancak, VM Konuk aracısının çalıştıran algılandı. Bunu konuk işletim sistemini olmayan bir VM görüntüsü olarak kullanılacak doğru hazırlanmış öneren (CreateOption ile FromImage =). Bu sorunu gidermek için kullanın ya da CreateOption ile olduğu gibi VHD Attach = ya da bir görüntü olarak kullanmak için düzgün şekilde hazırlayın:   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu, seçilen konumda şu an için kullanılamıyor.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Kaynak, devam eden platform güncelleştirmesi nedeniyle şu anda güncelleştirilemiyor. Lütfen daha sonra tekrar deneyin.  |
|  StorageAccountLimitation  |  Depolama hesabı '{0}' diskler oluşturmak için gereken sayfa bloblarını desteklemez.  |
|  StorageAccountLimitation  |  Depolama hesabı '{0}', ayrılmış kotasını aştı.  |
|  StorageAccountLocationMismatch  |  Depolama hesabı çözümlenemedi {0}. Lütfen işlem kaynağıyla aynı konumda bulunan depolama kaynağı sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  StorageAccountNotFound  |  Depolama hesabı {0} bulunamadı. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait emin olun.  |
|  StorageAccountNotRecognized  |  Lütfen depolama kaynağı sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. Kullanımı {0} desteklenmiyor.  |
|  StorageAccountOperationInternalError  |  İç hata oluştu. depolama hesabına erişilirken {0}.  |
|  StorageAccountSubscriptionMismatch  |  Depolama hesabı {0} aboneliğine ait değil {1}.  |
|  StorageAccountTooBusy  |  Depolama hesabı '{0}' şu anda çok meşgul. Başka bir hesabı kullanmayı deneyin.  |
|  StorageAccountTypeNotSupported  |  Disk {0} kullanan {1} Blob storage hesabı olduğu. Lütfen genel amaçlı depolama hesabıyla yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama hesabı {0} değil {1} türü. Tanılama destekleyen önyükleme {2} depolama hesap türü.  <ul><li>Premium depolama hesabı için önyükleme tanılaması kullanırsanız, bu hata oluşur. Daha fazla bilgi için bkz: [önyükleme tanılaması kullanmayı](../articles/virtual-machines/windows/boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Abonelik yetkilendirilmemiş.  |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} zaten mevcut. Farklı bir blob URI'si yeni bir boş veri diskini oluşturmak için lütfen '{1}'.  |
|  TargetDiskBlobAlreadyExists  |  Yakalama işlemi hedef görüntü blobu için devam edemiyor {0} zaten var ve VHD bloblarının üzerine yazmayı sağlayan bayrağı ayarlanmamış. Blob silin veya VHD bloblarının üzerine yazmayı ve yeniden deneyin bayrağını ayarlayın.  |
|  TargetDiskBlobAlreadyExists  |  Yakalama işlemi hedef görüntü blobu için devam edemiyor {0} üzerinde etkin bir kira sahiptir.   |
|  TargetDiskBlobAlreadyExists  |  BLOB {0} zaten mevcut. Lütfen farklı bir blob URI'si diskin hedefi olarak sağlarlar '{1}'.  |
|  TooManyVMRedeploymentRequests  |  VM için çok fazla yeniden dağıtım isteği alındı '{0}' veya bu VM ile aynı kullanılabilirlik kümesinde bulunan VM'ler. Lütfen daha sonra yeniden deneyin.  |
|  VHDSizeInvalid  |  Belirtilen disk boyutu değeri {0} disk için '{1}' blob ile {2} geçersiz. Disk boyutu arasında olmalıdır {3} ve {4}.  |
|  VMAgentStatusCommunicationError  |  VM '{0}' VM aracısının veya uzantılarının durumunu bildirmedi. Lütfen VM çalışan bir VM Aracısı olduğunu ve Azure depolamaya giden bağlantı kurabilir doğrulayın.  |
|  VMArtifactRepositoryInternalError  |  VM yapıt ayrıntılarını almak için yapıt deposuyla iletişim kurulurken bir hata oluştu.  |
|  VMArtifactRepositoryInternalError  |  Yapıt deposundan VM yapıt verileri alınırken bir iç hata oluştu.  |
|  VMExtensionHandlerNonTransientError  |  İşleyici '{0}'VM uzantısı için hata bildirdi'{1}'terminal hata koduyla'{2}' ve hata iletisi: '{3}'  |
|  VMExtensionManagementInternalError  |  İç hata oluştu. VM uzantısı işlenirken '{0}'.  |
|  VMExtensionManagementInternalError  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningError  |  VM uzantısı işlenirken bir hata bildirildi '{0}'. Hata iletisi: "{1}".  |
|  VMExtensionProvisioningError  |  Birden çok VM uzantıları VM sağlanması başarısız oldu. Lütfen ayrıntılar için VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  VM uzantısının sağlanması '{0}' zaman aşımına uğradı. Genişletme yüklemesinin çok uzun sürüyor veya uzantı durumu sağlanamadı.  |
|  VMMarketplaceInvalidInput  |  Olmayan Market görüntüsünden sanal makine oluşturma gerekmez bilgi planlama, lütfen Plan bilgileri istekte kaldırın. İşletim sistemi disk adı {0}.  |
|  VMMarketplaceInvalidInput  |  Satın alma bilgileri eşleşmiyor. Market görüntüsünden dağıtım yapılamıyor. İşletim sistemi disk adı {0}.  |
|  VMMarketplaceInvalidInput  |  Market görüntüsünden sanal makine oluşturulurken Plan bilgisi isteği gerektirir. İşletim sistemi disk adı {0}.  |
|  VMNotFound  |  VM '{0}' bulunamıyor.  |
|  VMRedeploymentFailed  |  VM '{0}' vm'inin yeniden dağıtımı bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin.  |
|  VMRedeploymentTimedOut  |  VM'nin yeniden '{0}' ayrılan sürede bitmedi. Başarıyla içinde süre bitirmek. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  VMStartTimedOut  |  VM '{0}' ayrılan sürede başlatılmadı. VM hala başarıyla başlatılabilir. Lütfen daha sonra güç durumunu denetleyin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.