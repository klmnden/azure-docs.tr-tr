---
title: -V1 bir Azure içeri/dışarı aktarma dışarı aktarma işini onarma | Microsoft Docs
description: Oluşturulmuş ve Azure içeri/dışarı aktarma hizmetini kullanarak çalışan bir dışarı aktarma işini onarma hakkında bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 915cf1e66ec400e0d2461873d9fb3d66be9883fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61477952"
---
# <a name="repairing-an-export-job"></a>Bir dışarı aktarma işini onarma
Dışarı aktarma işi tamamlandıktan sonra Microsoft Azure içeri/dışarı aktarma aracı şirket içine çalıştırabilirsiniz:  
  
1.  Azure içeri/dışarı aktarma hizmet veremedi tüm dosyaları indirin.  
  
2.  Sürücüdeki dosyalar doğru verildiğini doğrulayın.  
  
Bu işlevselliği kullanmak için Azure depolama bağlantısı olması gerekir.  
  
Bu komut bir içeri aktarma işini onarma **RepairExport**.

## <a name="repairexport-parameters"></a>RepairExport parametreleri

Aşağıdaki parametreler ile belirtilen **RepairExport**:  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|**/ r: < RepairFile\>**|Gereklidir. Bu onarım ilerlemesini izler ve kesintiye uğramış bir onarım devam etmek için Onar dosyasının yolu. Her sürücü bir ve yalnızca bir onarım dosyası olması gerekir. Belirli bir sürücü için onarım başlattığınızda, henüz mevcut olmayan bir onarım dosya yolunda geçer. Kesintiye uğramış bir onarım devam etmek için var olan bir onarım dosya adını geçmelidir. Hedef sürücüye karşılık gelen onarım dosyasını her zaman belirtilmesi gerekir.|  
|**/ LOGDIR: < LogDirectory\>**|İsteğe bağlı. Günlük dizini. Ayrıntılı günlük dosyası bu dizine yazılır. Hiçbir günlük dizini belirtilmezse, geçerli dizin günlük dizini kullanılır.|  
|**/ d: < TargetDirectory\>**|Gereklidir. Doğrulama ve onarma dizin. Dışarı aktarma sürücünün kök dizininde genellikle budur ancak bir ağ dosya paylaşımına dışa aktarılan dosyaların bir kopyasını da içeren.|  
|**/bk:<BitLockerKey\>**|İsteğe bağlı. Bir şifrelenmiş dışa aktarılan dosyaların depolandığı kilidini açmak için araç istiyorsanız BitLocker anahtarı belirtmeniz gerekir.|  
|**/sn: < StorageAccountName\>**|Gereklidir. Dışarı aktarma işi için depolama hesabı adı.|  
|**/sk:<StorageAccountKey\>**|**Gerekli** kapsayıcı SAS belirtilmedi ve yalnızca. Dışarı aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas:<ContainerSas\>**|**Gerekli** depolama hesap anahtarı belirtilmedi ve yalnızca. Dışarı aktarma işi ile ilişkili bloblara erişmek için kapsayıcı SAS.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Gereklidir. Sürücüyü Kopyala günlük dosyası yolu. Dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve iş ile ilişkili blob depolamadan indirebilirsiniz. Kopyalama günlük dosyası başarısız bloblar ya da onarılması dosyaları hakkında bilgi içerir.|  
|**/ Manıfestfıle: < DriveManifestFile\>**|İsteğe bağlı. Dışarı aktarma sürücünün bildirim dosyasının yolu. Bu dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve dışarı aktarma sürücüsünde ve isteğe bağlı olarak işle ilişkili depolama hesabındaki bir blob içinde depolanan.<br /><br /> Dosyaları dışarı aktarma sürücüde içeriğini, bu dosyada bulunan MD5 karmaları ile doğrulanır. Bozulmuş belirlenen dosyaları indirilir ve hedef dizinler yeniden.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>Başarısız dışarı aktarmalar düzeltmek için RepairExport modunu kullanma  
Dışarı aktarılamadı dosyaları indirmek için Azure içeri/dışarı aktarma Aracı'nı kullanabilirsiniz. Kopyalama günlük dosyasını dışarı aktarmak için başarısız olan dosyaların listesini içerir.  
  
Dışarı Aktarma hataları aşağıdaki olasılıkları nedenler:  
  
-   Bozuk sürücüleri  
  
-   Aktarım işlemi sırasında değiştirilen depolama hesabı anahtarı  
  
Aracı çalıştırmak için **RepairExport** modu, ilk gerekir bilgisayarınıza dışa aktarılan dosyaları içeren sürücüde bağlanmak. Ardından, o sürücüye ile yolunu belirtme Azure içeri/dışarı aktarma aracı çalıştırın `/d` parametresi. İndirdiğiniz sürücünün kopyalama günlük dosyası yolunu belirtmeniz gerekir. Aşağıdaki komut satırı örneği dışarı aktarılamadı, hiçbir dosya onarmak için Aracı'nı çalıştırır:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Dışarı aktarılamadı, bir blok blob'una gösteren bir kopya günlük dosyası örneği verilmiştir:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Kopyalama günlük dosyası, Windows Azure içeri/dışarı aktarma hizmeti bir blobun blok dosyasına dışarı aktarma sürücüdeki yükleme olan uygulanırken bir hata oluştu, gösterir. Dosyanın diğer bileşenleri başarıyla indirildi ve dosya uzunluğu doğru ayarlandı. Bu durumda, aracı sürücüsünde dosyasını açın, blok depolama hesabından indirin ve uzunluğu 65536 olan uzaklığı 65536'dan başlayan dosya aralığını yazın.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Sürücü içeriğini doğrulamak için RepairExport kullanma  
Azure içeri/dışarı aktarma ile de kullanabileceğiniz **RepairExport** sürücüsündeki içeriği doğrulamak için seçeneği doğru. Bildirim dosyası her dışarı aktarma sürücüde sürücü içeriğini için MD5s içerir.  
  
Azure içeri/dışarı aktarma hizmeti Ayrıca bildirim dosyalarını bir depolama hesabına dışarı aktarma işlemi sırasında kaydedebilirsiniz. Bildirim dosyalarının konumunu aracılığıyla kullanılabilir [alma işi](/rest/api/storageimportexport/jobs) işi tamamlandığında işlemi. Bkz: [içeri/dışarı aktarma hizmet bildirimi dosyası biçimi](storage-import-export-file-format-metadata-and-properties.md) sürücü bildirim dosyasının biçimi hakkında daha fazla bilgi için.  
  
Aşağıdaki örnek, Azure içeri/dışarı aktarma aracı ile çalıştırmak gösterilmektedir **/ManifestFile** ve **/CopyLogFile** parametreleri:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Bir bildirim dosyası örneği verilmiştir:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Onarım işlemi bittikten sonra aracı her dosya bildirim dosyasında başvurulan inceleyin ve MD5 karmaları ile dosya bütünlüğünü doğrulayın. Listesi için aşağıdaki bileşenleri geçer.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Doğrulama başarısız olan herhangi bir bileşeni aracı tarafından indirilir ve sürücüsüne aynı dosyaya yazılan.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma Aracı'nı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
