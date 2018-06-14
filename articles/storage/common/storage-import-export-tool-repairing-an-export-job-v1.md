---
title: Bir Azure içeri/dışarı aktarma dışarı aktarma işinin - v1 onarma | Microsoft Docs
description: Oluşturulmuş ve Azure içeri/dışarı aktarma hizmeti kullanarak çalışan bir dışarı aktarma işinin onarmak öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 57ab58fa1fd8371d0b6f019f94bb162bcc1e0e43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873880"
---
# <a name="repairing-an-export-job"></a>Bir dışarı aktarma işini onarma
Bir dışarı aktarma işi tamamlandıktan sonra Microsoft Azure içeri/dışarı aktarma aracı şirket içi çalıştırabilirsiniz:  
  
1.  Azure içeri/dışarı aktarma hizmeti veremedi dosyalarını indirin.  
  
2.  Sürücüdeki dosyalar doğru aktarılmış doğrulayın.  
  
Bu işlevselliği kullanmak için Azure Storage bağlantısı olması gerekir.  
  
İçe aktarma işi onarmak için komut **RepairExport**.

## <a name="repairexport-parameters"></a>RepairExport parametreleri

Aşağıdaki parametreleri ile belirtilen **RepairExport**:  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|**/ r: < RepairFile\>**|Gereklidir. Onarım ilerleyişini izler ve kesintiye uğramış bir onarım sürdürmek için veren onarım dosyasının yolu. Her bir sürücü bir ve yalnızca bir onarım dosyası olması gerekir. Belirli bir sürücü için onarım başlattığınızda, henüz var olmayan bir onarım dosya yolunda geçer. Kesintiye uğramış bir onarım sürdürmek için varolan bir onarma dosya adına geçirmelisiniz. Onarım dosya hedef sürücüye karşılık gelen her zaman belirtilmesi gerekir.|  
|**/ LOGDIR: < LogDirectory\>**|İsteğe bağlı. Günlük dosyası dizini. Bu dizin için ayrıntılı günlük dosyalarına yazılır. Günlük dizini belirtilmezse, geçerli dizin günlük dizini olarak kullanılır.|  
|**/ d: < TargetDirectory\>**|Gereklidir. Doğrulama ve onarmak için dizin. Bu genellikle verme sürücüsünün kök dizinindeki olmakla birlikte, bir ağ dosya paylaşımı dışa aktarılan dosyaların bir kopyasını da içeren.|  
|**/BK: < BitLockerKey\>**|İsteğe bağlı. Bir şifrelenmiş dışa aktarılan dosyaların depolandığı kilidini açmak için aracın istiyorsanız BitLocker anahtarını belirtmeniz gerekir.|  
|**/sn: < StorageAccountName\>**|Gereklidir. Dışa aktarma işi için depolama hesabı adı.|  
|**/SK: < StorageAccountKey\>**|**Gerekli** bir kapsayıcı SAS varsa ve yalnızca belirtilmezse. Dışa aktarma işi için depolama hesabı için hesap anahtarı.|  
|**/csas: < ContainerSas\>**|**Gerekli** depolama hesabı anahtarı belirtilmezse varsa ve yalnızca. Dışa aktarma işi ile ilişkili BLOB'ları erişmek için kapsayıcı SAS.|  
|**/ CopyLogFile: < DriveCopyLogFile\>**|Gereklidir. Sürücüyü Kopyala günlük dosyası yolu. Dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve işle ilişkili blob depolama biriminden indirilebilir. Kopya günlük dosyası başarısız BLOB veya onarılması dosyaları hakkında bilgi içerir.|  
|**/ Manıfestfıle: < DriveManifestFile\>**|İsteğe bağlı. Dışarı aktarma sürücünün bildirim dosyasının yolu. Bu dosya Windows Azure içeri/dışarı aktarma hizmeti tarafından oluşturulan ve dışarı aktarma sürücü ve isteğe bağlı olarak işle ilişkili depolama hesabındaki blob depolanır.<br /><br /> Dosyaları dışarı aktarma sürücüde içeriğini bu dosyada bulunan MD5 karma ile doğrulanır. Bozuk olduğu belirlenir herhangi bir dosya indirilir ve hedef dizinleri yeniden yazılmıştır.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>Başarısız dışarı aktarmaları düzeltmek için RepairExport modunu kullanma  
Dışarı aktarılamadı dosyalarını indirmek için Azure içeri/dışarı aktarma aracını kullanabilirsiniz. Dosya Kopyala günlük dışarı aktarılamadı dosyaların bir listesini içerir.  
  
Dışarı aktarma hatalarının nedenlerini aşağıdaki özellikleri içerir:  
  
-   Bozuk sürücüler  
  
-   Aktarma işlemi sırasında değiştirilmiş depolama hesabı anahtarı  
  
Aracı çalıştırmak için **RepairExport** modu, ilk ihtiyacınız bilgisayarınıza dışa aktarılan dosyaları içeren sürücü bağlanmak. Ardından, bu sürücüyle yolunu belirtme Azure içeri/dışarı aktarma aracı çalıştırın `/d` parametresi. Ayrıca indirdiğiniz sürücünün kopyalama günlük dosyası yolunu belirtmeniz gerekir. Aşağıdaki komut satırı örnek dışarı aktarılamadı dosyalarını onarmak için aracını çalıştırır:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Blob içinde bir bloğu gösterir kopyalama günlük dosyası örneği dışarı aktarılamadı verilmiştir:  
  
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
  
Windows Azure içeri/dışarı aktarma hizmeti blob'un blokları birini verme sürücüsündeki dosyanın indirilmesi edildi uygulanırken bir hata oluştu kopyalama günlük dosyasında belirtilir. Dosyanın diğer bileşenleri başarıyla indirildi ve dosya uzunluğunu doğru şekilde ayarlandı. Bu durumda, araç sürücüde dosyasını açın, blok depolama hesabından karşıdan yükle ve uzaklık 65536 uzunluğu 65536 ile başlayarak dosya aralığına yazma.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Sürücü içeriklerini doğrulamak için RepairExport kullanma  
Azure içeri/dışarı aktarma ile de kullanabileceğiniz **RepairExport** sürücüde içeriklerini doğrulamak için seçeneği doğru. Bildirim dosyası her dışarı aktarma sürücüde sürücü içeriğini için MD5s içerir.  
  
Azure içeri/dışarı aktarma hizmeti de bildirim dosyaları bir depolama hesabı için dışa aktarma işlemi sırasında kaydedebilirsiniz. Bildirim dosyalarının konumunu aracılığıyla kullanılabilir [alma işi](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) iş tamamlandığında işlemi. Bkz: [içeri/dışarı aktarma hizmeti bildirim dosyası biçimi](storage-import-export-file-format-metadata-and-properties.md) sürücü bildirim dosyası biçimi hakkında daha fazla bilgi için.  
  
Aşağıdaki örnek, Azure içeri/dışarı aktarma aracı ile çalıştırmak gösterilmiştir **/ManifestFile** ve **/CopyLogFile** Parametreler:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Bildirim dosyası örneği verilmiştir:  
  
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
  
Onarım işlemi bittikten sonra aracı bildirim dosyasında başvurulan her bir dosya aracılığıyla okuma ve MD5 karma ile dosya bütünlüğünü doğrulayın. Listesi için aşağıdaki bileşenleri geçer.  

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

Doğrulama başarısız olan herhangi bir bileşeni aracı tarafından indirilir ve sürücüde aynı dosyaya yeniden yazılmıştır.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure içeri/dışarı aktarma aracı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
