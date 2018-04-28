---
title: 'Microsoft Genomics: sorun giderme kılavuzu | Microsoft Docs'
titleSuffix: Azure
description: Sorun giderme stratejileri hakkında daha fazla bilgi edinin
keywords: sorun giderme, hata, hata ayıklama
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/13/2018
ms.openlocfilehash: 18761c02cc423affe7b1050700e560b1f0b0594d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu
Bu genel bakışta, Microsoft Genomics hizmetini kullanırken sık karşılaşılan sorunları gidermek üzere stratejilerini açıklar. Genel SSS bkz [sık sorulan sorular](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>My işi durumunu denetleme nasıl?
Çağırarak, iş akışının durumunu kontrol edebilirsiniz `msgen status` komut satırından gösterildiği gibi. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Üç gerekli bağımsız değişkenleri şunlardır:
* URL - API için taban URI
* ANAHTAR - Genomics hesabınız için erişim anahtarı. 
* Kimliği - iş akışı kimliği

URL ve anahtarı bulmak için Azure Portalı'na gidin ve Genomics hesap sayfanıza açın. Altında **Yönetim** başlığını seçin **erişim anahtarları**. Burada, API URL'si ve erişim tuşlarınızı bulun.

Alternatif olarak, doğrudan anahtarı ve URL girmek yerine config dosya yolu içerebilir. Yapılandırma dosyası yanı sıra komut satırından bu bağımsız değişkenler dahil ederseniz, komut satırı bağımsız değişkenleri öncelik gerektiğine dikkat edin. 

Çağırdıktan sonra `msgen status`, kullanıcı dostu bir ileti, iş akışı başarılı olup olmadığını açıklayan veya iş başarısızlık nedeni vermiş görüntülenir. 


## <a name="get-more-information-about-my-workflow-status"></a>İş akışı Durumum hakkında daha fazla bilgi alın

Neden bir işi olmayan başarılı hakkında daha fazla bilgi almak için iş akışı sırasında oluşturulan günlük dosyalarını gözden geçirebilirsiniz. Çıktı kapsayıcısında görmelisiniz bir `[youroutputfilename].logs.zip` klasör.  Bu klasör unzipping, aşağıdaki öğeleri görürsünüz:

* outputFileList.txt - iş akışı sırasında oluşturulan çıkış dosyaları listesi
* StandardError.txt - Bu dosya boş olur.
* StandardOutput.txt - iş akışının en üst düzey günlük içerir. 
* GATK günlük dosyalarını - diğer tüm dosyalarda `logs` klasörü

`standardoutput.txt` Dosya neden akışınızı başarılı olmadı, iş akışının daha fazla alt düzey bilgiler içerir olarak belirlemek başlatmak için iyi bir yerdir. 

## <a name="common-issues-and-how-to-resolve-them"></a>Sık karşılaşılan sorunları ve bunların nasıl çözüleceği
Bu bölümde kısaca genel sorunları ve bunların nasıl çözüleceği vurgular.

### <a name="fastq-files-are-unmatched"></a>Fastq dosyaları eşleşmeyen
Fastq dosyaları yalnızca sondaki /1 veya örnek tanımlayıcıda /2 farklı olmalıdır. Eşleşmeyen FASTQ dosyaları yanlışlıkla gönderdiyseniz çağrılırken aşağıdaki hata iletileri görebilirsiniz `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

Bu sorunu çözmek için iş akışına gönderilen fastq dosyaları gerçekten olup olmadığını gözden eşlenmiş bir küme. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>.Bam dosya karşıya yükleme hatası oluştu. Çıktı blob zaten var ve üzerine yazma seçeneği False olarak ayarlayın.
Ayarlayamadı hata iletisini görürseniz, `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, çıkış klasörü zaten aynı ada sahip bir çıktı dosyası içeriyor.  Varolan çıktı dosyasını silin veya config dosyasında üzerine yaz seçeneğini etkinleştirin. Ardından, iş akışını yeniden gönderin.

### <a name="when-to-contact-microsoft-genomics-support"></a>Ne zaman Microsoft Genomics desteğe başvurun
Aşağıdaki hata iletilerinden görürseniz, bir iç hata oluştu. 

* `Error locating input files on worker machine`
* `Process management failure`

İş akışını yeniden göndermeyi deneyin. İş hataları yaşamaya devam ederseniz veya diğer herhangi bir sorunuz varsa, Azure portalından Microsoft Genomics desteğe başvurun.

![Azure Portal'da desteğine başvurun](./media/troubleshooting-guide/genomics-contact-support.png "Azure Portal'daki desteğe başvurun")

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede ve Microsoft Genomics hizmeti ile ilgili genel sorunları gidermek nasıl öğrendiniz. Daha fazla bilgi ve daha fazla Genel SSS için bkz: [sık sorulan sorular](frequently-asked-questions-genomics.md). 