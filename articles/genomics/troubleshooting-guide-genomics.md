---
title: Microsoft Genomics sorun giderme kılavuzu
titleSuffix: Azure
description: Sorun giderme stratejileri hakkında daha fazla bilgi edinin
keywords: sorun giderme, hata, hata ayıklama
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 07/18/2018
ms.openlocfilehash: bd946f84023345c68a01a48a4dc310b7afb68397
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735395"
---
# <a name="troubleshooting-guide-for-microsoft-genomics"></a>Microsoft Genomics için sorun giderme kılavuzu
Bu genel bakış, Microsoft Genomics hizmeti kullanırken sık karşılaşılan sorunları gidermek üzere stratejilerini açıklar. Bkz: Genel SSS [sık sorulan sorular](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>İş durumumu nasıl kontrol edebilirim?
Çağırarak, akışınızın durumunu kontrol edebilirsiniz `msgen status` komut satırından gösterildiği gibi. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Üç gerekli bağımsız değişkenleri şunlardır:
* Adresa URL – API için ana URI
* ANAHTARI - Genomiks hesabı için erişim anahtarı'nı tıklatın. 
* Kimliği - iş akışı kimliği

URL'sini ve ANAHTARINI bulmak için Azure portalına gidin ve Genomiks hesabı sayfanızı açın. Altında **Yönetim** başlığı seçin **erişim anahtarları**. Burada, hem API URL'sine ve erişim tuşlarınızı bulun.

Alternatif olarak, doğrudan URL'sini ve ANAHTARINI girmek yerine yapılandırma dosyasının yolu içerebilir. Yapılandırma dosyası yanı sıra komut satırında bu bağımsız değişkenler dahil ederseniz, komut satırı bağımsız değişkenleri öncelik gerektiğine dikkat edin. 

Arama sonra `msgen status`, kullanıcı dostu bir ileti, iş akışı başarılı olup olmadığını açıklayan veya iş başarısızlık nedeni vererek görüntülenir. 


## <a name="get-more-information-about-my-workflow-status"></a>İş akışı durum hakkında daha fazla bilgi edinin

Neden bir işi başarılı değil hakkında daha fazla bilgi almak için iş akışı sırasında üretilen günlük dosyalarını keşfedebilirsiniz. Çıkış kapsayıcısında görmelisiniz bir `[youroutputfilename].logs.zip` klasör.  Bu klasör açma sırasında aşağıdaki öğeleri görürsünüz:

* outputFileList.txt - iş akışı sırasında oluşturulan çıktı dosyaları listesi
* StandardError.txt - Bu dosya boş olur.
* StandardOutput.txt - iş akışının en üst düzey günlük kaydı içerir. 
* GATK günlük dosyalarını - diğer tüm dosyalarda `logs` klasörü

`standardoutput.txt` Dosya neden akışınızı başarılı olmadı, iş akışının alt düzey daha fazla bilgi içeren belirlemek başlatmak için iyi bir yerdir. 

## <a name="common-issues-and-how-to-resolve-them"></a>Sık karşılaşılan sorunları ve bunların nasıl çözüleceğine
Bu bölümde kısaca sık karşılaşılan sorunları ve bunların nasıl çözüleceğine vurgular.

### <a name="fastq-files-are-unmatched"></a>Eşleşmeyen Fastq dosyası
Fastq dosyası yalnızca sondaki /1 veya örnek tanımlayıcıda /2 farklı olmalıdır. Eşleşmeyen FASTQ dosyası yanlışlıkla gönderdiyseniz çağrılırken aşağıdaki hata iletilerini görebilirsiniz `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

Bu sorunu çözmek için iş akışı gönderdiniz fastq dosyası gerçekten varsa gözden eşleşen kümesi. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>.Bam dosya karşıya yükleme hatası. Çıktı blobu zaten var ve üzerine yazma seçeneği False olarak ayarlanmış.
Aşağıdaki hata iletisini görürseniz `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, çıktı klasörü zaten aynı ada sahip bir çıktı dosyası içeriyor.  Mevcut çıkış dosyasını silmek ya da yapılandırma dosyasında üzerine yaz seçeneğini etkinleştirin. Daha sonra iş akışınızı yeniden gönderin.

### <a name="when-to-contact-microsoft-genomics-support"></a>Ne zaman Genomics Microsoft desteğine başvurun
Aşağıdaki hata iletilerinden görürseniz, bir iç hata oluştu. 

* `Error locating input files on worker machine`
* `Process management failure`

İş akışınızı yeniden göndermeyi deneyin. İş hataları devam ederseniz veya diğer herhangi bir sorunuz varsa, Azure portalında Microsoft Genomiks desteğe başvurun. Bir destek talebi göndermek nasıl hakkında daha fazla bilgi bulunabilir [burada](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, siz ve Microsoft Genomics hizmeti ile ilgili yaygın sorunları gidermek nasıl öğrendiniz. Daha fazla bilgi ve daha Genel SSS için bkz. [sık sorulan sorular](frequently-asked-questions-genomics.md). 
