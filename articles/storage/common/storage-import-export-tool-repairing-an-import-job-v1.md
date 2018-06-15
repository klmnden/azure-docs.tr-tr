---
title: Bir Azure içeri/dışarı aktarma alma işi - v1 onarma | Microsoft Docs
description: Oluşturulmuş ve Azure içeri/dışarı aktarma hizmeti kullanarak çalışan bir alma işi onarmak öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c837713fd9e7d03287ae5a3644fd6bb47714c9d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23927470"
---
# <a name="repairing-an-import-job"></a>Bir içeri aktarma işini onarma
Microsoft Azure içeri/dışarı aktarma hizmeti, bazı dosyalar veya, bir dosyanın parçalarını Windows Azure Blob hizmeti kopyalamak başarısız olabilir. Hataları görmek için bazı nedenler şunlardır:  
  
-   Bozuk dosyaları  
  
-   Bozuk sürücüler  
  
-   Depolama hesabı anahtarı dosyası aktarılırken değiştirildi.  
  
Microsoft Azure içeri/dışarı aktarma aracı işin kopya günlük dosyalarını içeri aktarmaya çalıştırabilirsiniz ve aracı içeri aktarma işlemini tamamlamak için Windows Azure depolama hesabınıza eksik dosyaları (veya, bir dosyanın parçalarını) yükler.  
  
## <a name="repairimport-parameters"></a>RepairImport parametreleri

Aşağıdaki parametreleri ile belirtilen **RepairImport**: 
  
|||  
|-|-|  
|**/ r:**< RepairFile\>|**Gerekli.** Onarım ilerleyişini izler ve kesintiye uğramış bir onarım sürdürmek için veren onarım dosyasının yolu. Her bir sürücü bir ve yalnızca bir onarım dosyası olması gerekir. Belirli bir sürücü için onarım başlattığınızda yolunda henüz var olmayan bir onarım dosyaya geçirin. Kesintiye uğramış bir onarım sürdürmek için varolan bir onarma dosya adına geçirmelisiniz. Onarım dosya hedef sürücüye karşılık gelen her zaman belirtilmesi gerekir.|  
|**/ LOGDIR:**< LogDirectory\>|**İsteğe bağlı.** Günlük dosyası dizini. Ayrıntılı günlük dosyalarını bu dizine yazılır. Günlük dizini belirtilmezse, geçerli dizin günlük dizini olarak kullanılır.|  
|**/ d:**< TargetDirectories\>|**Gerekli.** İçe aktarılan özgün dosyaları içeren bir veya daha çok noktalı virgülle ayrılmış dizinleri. İçeri aktarma sürücü de kullanılabilir, ancak özgün dosyaları alternatif konumlara kullanılabilir, gerekli değildir.|  
|**/BK:**< BitLockerKey\>|**İsteğe bağlı.** Özgün dosya kullanılabildiği şifrelenmiş sürücünün kilidini açmak için aracın istiyorsanız BitLocker anahtarını belirtmeniz gerekir.|  
|**/sn:**< StorageAccountName\>|**Gerekli.** İçe aktarma işi için depolama hesabı adı.|  
|**/SK:**< StorageAccountKey\>|**Gerekli** bir kapsayıcı SAS varsa ve yalnızca belirtilmezse. İçe aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas:**< ContainerSas\>|**Gerekli** depolama hesabı anahtarı belirtilmezse varsa ve yalnızca. İçe aktarma işi ile ilişkili BLOB'ları erişmek için kapsayıcı SAS.|  
|**/ CopyLogFile:**< DriveCopyLogFile\>|**Gerekli.** Sürücüyü Kopyala günlük dosyası (ya da ayrıntılı günlük veya hata günlüğü) yolu. Dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve işle ilişkili blob depolama biriminden indirilebilir. Kopya günlük dosyası başarısız BLOB'lar ya da onarılması olan dosyalar hakkında bilgi içerir.|  
|**/ PathMapFile:**< DrivePathMapFile\>|**İsteğe bağlı.** Aldığınız aynı işlemde aynı ada sahip birden fazla dosya varsa belirsizlikleri çözümlemek için kullanılan bir metin dosyasının yolu. İlk kez aracı çalıştırdığınızda, bu dosya tüm belirsiz adları ile doldurabilirsiniz. Sonraki çalıştığında aracı bu dosyayı belirsizlikleri gidermek için kullanın.|  
  
## <a name="using-the-repairimport-command"></a>RepairImport komutunu kullanma  
İçeri aktarma verileri ağ üzerinden veri akış tarafından onarmak için alma kullanarak özgün dosyaları içeren dizinlerini belirt `/d` parametresi. Depolama hesabınızdan yüklediğiniz copy günlük dosyası da belirtmeniz gerekir. İçe aktarma işi kısmi hatalarıyla onarmak için tipik bir komut satırı şuna benzer:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Aşağıdaki örnekte, bir kopya günlük dosyası için içeri aktarma işi sevk edilen sürücüsünde bir dosya bir 64 K parçası bozulmuş. Belirtilen tek hatası olduğundan iş bloblar geri kalanı başarıyla içeri aktarıldı.  
  
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
  
Bu kopyalama günlük Azure içeri/dışarı aktarma aracı geçirildiğinde eksik içeriği ağ üzerinden kopyalayarak bu dosya için alma işlemini bitirmek aracı çalışır. Yukarıdaki örnek aracı görünmesi için özgün dosya `\animals\koala.jpg` iki dizin içinde `C:\Users\bob\Pictures` ve `X:\BobBackup\photos`. Varsa dosyayı `C:\Users\bob\Pictures\animals\koala.jpg` yoksa, Azure içeri/dışarı aktarma aracı karşılık gelen blob veri eksik aralığını kopyalar `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>RepairImport kullanırken çakışmalarını çözme  
Bazı durumlarda, aracı bulunamıyor veya gerekli dosya aşağıdaki nedenlerden birinden dolayı açmak mümkün olmayabilir: dosya bulunamadı, dosyanın erişilebilir değil, dosya adı belirsiz veya dosyanın içeriği artık doğru değil.  
  
Aracı bulmak çalışıyor belirsiz bir hata oluşabilir `\animals\koala.jpg` ve her ikisi de altında bu ada sahip bir dosya `C:\Users\bob\pictures` ve `X:\BobBackup\photos`. Diğer bir deyişle, her ikisi de `C:\Users\bob\pictures\animals\koala.jpg` ve `X:\BobBackup\photos\animals\koala.jpg` alma işi sürücülerinde mevcut.  
  
`/PathMapFile` Seçeneği, bu hataları çözün olanak sağlar. Aracı'nı doğru şekilde belirlemek mümkün değildi dosyaların listesini içeren dosyanın adını belirtebilirsiniz. Aşağıdaki komut satırı örnek doldurur `9WM35C2V_pathmap.txt`:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Araç ardından sorunlu dosya yolları yazacak `9WM35C2V_pathmap.txt`, her satırda bir tane. Örneğin, dosya, komutu çalıştırdıktan sonra aşağıdaki girdileri içerebilir:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Listedeki her dosya için araç kullanılabildiğinden emin olmak için dosyasını bulun ve açın denemelisiniz. Aracı açıkça bir dosyayı nerede bulacağını bildirir isterseniz, yol haritası değiştirebilir ve bir sekme karakteriyle ayrılmış aynı satırda her dosya yolunu ekleyin:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Gerekli dosyaları aracı için kullanılabilir hale getirme veya yol haritası güncelleştirme sonra içeri aktarma işlemini tamamlamak için aracını çalıştırabilirsiniz.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma aracı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
