---
title: -V1 bir Azure içeri/dışarı aktarma içeri aktarma işini onarma | Microsoft Docs
description: Oluşturulmuş ve Azure içeri/dışarı aktarma hizmetini kullanarak çalışan bir içeri aktarma işini onarma hakkında bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: fda1d3d626c91ba984f08b96c79ab6a2fd2ec74b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61477595"
---
# <a name="repairing-an-import-job"></a>Bir içeri aktarma işini onarma
Microsoft Azure içeri/dışarı aktarma hizmeti Windows Azure Blob hizmetine dosyası ya da bir dosyanın bölümlerini kopyalamak başarısız olabilir. Hatalarının bazı nedenleri şunlardır:  
  
-   Bozuk dosya  
  
-   Bozuk sürücüleri  
  
-   Depolama hesabı anahtarını dosyası aktarılırken değiştirildi.  
  
Microsoft Azure içeri/dışarı aktarma aracı işin kopyalama günlük dosyalarını içeri aktarmaya çalıştırabilir ve araç eksik dosyaları (veya, bir dosyanın parçalarını) içeri aktarma işi tamamlamak için Windows Azure depolama hesabına yükler.  
  
## <a name="repairimport-parameters"></a>RepairImport parametreleri

Aşağıdaki parametreler ile belirtilen **RepairImport**: 
  
|||  
|-|-|  
|**/ r:**< RepairFile\>|**Gerekli.** Bu onarım ilerlemesini izler ve kesintiye uğramış bir onarım devam etmek için Onar dosyasının yolu. Her sürücü bir ve yalnızca bir onarım dosyası olması gerekir. Belirli bir sürücü için onarım başlattığınızda, yolda henüz var olmayan bir onarım dosyasına geçirin. Kesintiye uğramış bir onarım devam etmek için var olan bir onarım dosya adını geçmelidir. Hedef sürücüye karşılık gelen onarım dosyasını her zaman belirtilmesi gerekir.|  
|**/ LOGDIR:**< LogDirectory\>|**İsteğe bağlı.** Günlük dizini. Ayrıntılı günlük dosyası bu dizine yazılır. Hiçbir günlük dizini belirtilmezse, geçerli dizin günlük dizini kullanılır.|  
|**/ d:**< TargetDirectories\>|**Gerekli.** İçeri aktarılan özgün dosyaları içeren bir veya daha fazla noktalı virgülle ayrılmış dizinler. İçeri aktarma sürücü ayrıca kullanılabilir ancak özgün dosya alternatif konum kullanılabilir, gerekli değildir.|  
|**/BK:**< BitLockerKey\>|**İsteğe bağlı.** Özgün dosyalar nerede şifrelenmiş sürücünün kilidini açmak için araç istiyorsanız BitLocker anahtarı belirtmeniz gerekir.|  
|**/sn:**< StorageAccountName\>|**Gerekli.** İçeri aktarma işi için depolama hesabı adı.|  
|**/sk:**<StorageAccountKey\>|**Gerekli** kapsayıcı SAS belirtilmedi ve yalnızca. İçeri aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas:**<ContainerSas\>|**Gerekli** depolama hesap anahtarı belirtilmedi ve yalnızca. İçeri aktarma işi ile ilişkili bloblara erişmek için kapsayıcı SAS.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Gerekli.** Sürücüyü Kopyala günlük dosyası (ya da ayrıntılı günlüğü veya hata günlüğü) yolu. Dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve iş ile ilişkili blob depolamadan indirebilirsiniz. Kopyalama günlük dosyası başarısız bloblar ya da onarılması olan dosyalar hakkında bilgi içerir.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**İsteğe bağlı.** Belirsizlikler aldığınız aynı iş içinde aynı ada sahip birden çok dosyanız varsa çözümlemek için kullanılan bir metin dosyasının yolu. Aracı ilk çalıştırıldığında, bu dosya tüm belirsiz adları ile doldurabilirsiniz. Aracı'nın sonraki çalışmaları belirsizlikleri çözmek için bu dosyayı kullanın.|  
  
## <a name="using-the-repairimport-command"></a>RepairImport komutunu kullanma  
Ağ üzerinden veri akışı tarafından verileri içeri aktarma onarmak için alma kullanarak özgün dosyaları içeren dizinler belirtmelisiniz `/d` parametresi. Kopyalama günlük dosyası depolama hesabınızdan indirdiğiniz de belirtmeniz gerekir. İçeri aktarma işi ile kısmi hataları onarmak için tipik bir komut satırı şuna benzer:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Kopyalama günlük dosyasının aşağıdaki örnekte, içeri aktarma işine setleriyle sürücüsünde bir dosya bir 64 K parça bozuktu. Bu belirtilen yalnızca başarısız olduğundan, kalan işin BLOB'ları başarıyla içeri aktarıldı.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
Bu kopyalama günlük için Azure içeri/dışarı aktarma aracı geçirildiğinde, araç bu dosya için içeri aktarma eksik içeriği ağ üzerinden kopyalayarak son dener. Yukarıdaki örneği izleyerek, aracı için özgün dosyayı arar `\animals\koala.jpg` iki dizin içinde `C:\Users\bob\Pictures` ve `X:\BobBackup\photos`. Varsa dosyayı `C:\Users\bob\Pictures\animals\koala.jpg` yoksa, Azure içeri/dışarı aktarma aracı eksik veri aralığını karşılık gelen blob'a kopyalayan Azure `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>RepairImport kullanırken çakışmalarını çözümleme  
Bazı durumlarda, araç bulun veya aşağıdaki nedenlerden biri için gereken dosya açmak mümkün olmayabilir: dosya bulunamadı, dosya erişilebilir değil, dosya adı belirsiz veya dosyanın içeriği artık doğru değil.  
  
Aracı bulmaya çalışırken belirsiz bir hata ortaya çıkabilir `\animals\koala.jpg` ve her ikisi de altında bu ada sahip bir dosya var. `C:\Users\bob\pictures` ve `X:\BobBackup\photos`. Diğer bir deyişle, her ikisi de `C:\Users\bob\pictures\animals\koala.jpg` ve `X:\BobBackup\photos\animals\koala.jpg` içeri aktarma işi sürücülerinde mevcut.  
  
`/PathMapFile` Seçeneği bu hataları gidermek sağlar. Aracı'nı doğru şekilde belirlemek mümkün değildi dosyaların listesini içeren dosya adını belirtebilirsiniz. Aşağıdaki komut satırı örneği doldurur `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Araç ardından sorunlu dosya yolları yazacak `9WM35C2V_pathmap.txt`, her satırda bir tane. Örneğin, dosya, komutu çalıştırdıktan sonra aşağıdaki girdileri içerebilir:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Listedeki her dosya için bulun ve aracı için kullanılabilir olduğundan emin olmak için dosyayı açmak için denemeniz gerekir. Aracı bir dosyayı bulmak nereye açıkça söyleyin istiyorsanız, yol haritası değiştirebilir ve aynı satırda bir sekme karakteri tarafından ayrılmış her dosya yolunu ekleyin:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Gerekli dosyaları aracı için kullanılabilir hale getirme veya yol haritası güncelleştirme sonra içeri aktarma işlemini tamamlamak için aracı çalıştırabilirsiniz.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma Aracı'nı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
