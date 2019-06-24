---
title: Azure işlevleri'ni kullanarak bir Batch işi tetikleme
description: Öğretici - bir depolama blobu için eklenen OCR taranan belgelere uygulayabilir.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc
ms.openlocfilehash: d5a5197227ff62ca0c610e2c4e269480690d3faf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342037"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Öğretici: Azure işlevleri'ni kullanarak bir Batch işi tetikleme

Bu öğreticide, Azure işlevleri'ni kullanarak bir Batch işini tetiklemek öğreneceksiniz. Bir Azure depolama blob kapsayıcısına eklenen belgeler optik karakter tanıma (OCR) Azure Batch uygulanmış olan örnek gösterilecektir. OCR işleme kolaylaştırmak için bir Batch OCR iş blob kapsayıcısına bir dosya eklendiğinde her çalıştığında bir Azure işlevi yapılandıracaksınız.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Bkz: [Batch hesabı oluşturma](quick-create-portal.md#create-a-batch-account) hesaplarını bağlayın ve oluşturma hakkında daha fazla bilgi için.
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Bir Batch havuzu ve Batch Gezgini kullanarak Batch işi oluşturma

Bu bölümde, Batch Gezgini, Batch havuzu ve OCR görevleri çalıştırılacak bir Batch işi oluşturmak için kullanacaksınız. 

### <a name="create-a-pool"></a>Havuz oluşturma

1. Batch Gezgini'ne Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Seçerek bir havuz oluşturma **havuzları** sol kenar çubuğundaki sonra **Ekle** arama formu düğmesini. 
    1. Bir kimlik seçin ve görünen adı. Kullanacağız `ocr-pool` bu örneğin.
    1. Ölçeklendirme türü ayarlayın **boyutu sabit**, adanmış düğüm sayısı 3'e ayarlayın.
    1. Seçin **Ubuntu 18.04 LTS** işletim sistemi olarak.
    1. Seçin `Standard_f2s_v2` olarak sanal makine boyutu.
    1. Başlangıç görevi etkinleştirmek ve Ekle komutu `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"`. Kullanıcı kimliği olarak ayarladığınızdan emin olun **görev varsayılan kullanıcı (Yönetici)** , komutları ile eklemek başlangıç görevleri sağlayan `sudo`.
    1. **Tamam**’ı seçin.
### <a name="create-a-job"></a>Bir iş oluşturma

1. Seçerek havuzda bir iş oluşturma **işleri** sol kenar çubuğundaki sonra **Ekle** arama formu düğmesini. 
    1. Bir kimlik seçin ve görünen adı. Kullanacağız `ocr-job` bu örneğin.
    1. Havuz kümesine `ocr-pool`, veya örneğin adı havuzunuz için seçtiğiniz.
    1. **Tamam**’ı seçin.


## <a name="create-blob-containers"></a>BLOB kapsayıcıları oluşturma

Burada depolama giriş ve Çıkış dosyalarını OCR toplu işlemi için blob kapsayıcıları oluşturacaksınız.

1. Depolama Gezgini'ni Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Batch hesabınıza bağlı depolama hesabı kullanarak oluşturduğunuz iki blob kapsayıcıları (bir giriş dosyaları için bir çıkış dosyaları için) bölümündeki adımları izleyerek [bir blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#create-a-blob-container).

Bu örnekte, giriş kapsayıcısına adlı `input` ve burada tüm belgeler OCR olmadan işleme için Başlangıçta yüklenir. Çıkış kapsayıcısı adlı `output` ve burada toplu OCR ile işlenen belge yazar.  
    * Bu örnekte biz bizim giriş kapsayıcısına ararız `input`ve bizim çıkış kapsayıcısı `output`.  
    * Burada başlangıçta OCR olmayan tüm belgeler karşıya giriş kapsayıcıdır.  
    * Batch işi OCR belgelerle nereye yazdığını çıkış kapsayıcıdır.  

Depolama Gezgini'nde, çıkış kapsayıcısı için bir paylaşılan erişim imzası oluşturma. Çıkış kapsayıcısı üzerinde sağ tıklatıp seçerek bunu **paylaşılan erişim imzası Al...** . Altında **izinleri**, kontrol **yazma**. Diğer bir izinler gereklidir.  

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu bölümde OCR toplu işlem giriş kapsayıcınıza bir dosyayı SFTP tetikleyen Azure işlevi oluşturacaksınız.

1. Bağlantısındaki [Azure Blob Depolama ile tetiklenen bir işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-storage-blob-triggered-function) bir işlev oluşturmak için.
    1. Bir depolama hesabı için bir istem göründüğünde, Batch hesabınıza bağlı depolama hesabını kullanın.
    1. İçin **çalışma zamanı yığını**, .NET seçin. Size sunduğumuz işlevi yazacaksınız C# Batch .NET SDK'sını yararlanmak için.
1. Blob ile tetiklenen bir işlev oluşturulduktan sonra kullanmak [ `run.csx` ](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) ve [ `function.proj` ](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) işlevindeki github'dan.
    * `run.csx` Giriş blob kapsayıcınızın yeni bir blob eklendiğinde çalışır.
    * `function.proj` Örneğin, Batch .NET SDK, işlev kodunuzun dış kitaplıkları listeler.
1. Değişkenlerde yer tutucu değerlerini değiştirmek `Run()` işlevi `run.csx` Batch ve storage kimlik bilgileri yansıtmak için dosya. Batch ve storage hesabı kimlik bilgilerinizle Azure portalında bulabilirsiniz **anahtarları** Batch hesabınızın bölümü.
    * Batch ve storage hesabı kimlik bilgilerinizi Azure portalında almak **anahtarları** Batch hesabınızın bölümü. 

## <a name="trigger-the-function-and-retrieve-results"></a>İşlevi tetikleyin ve sonuçları Al

Tüm taranan dosyalar karşıya [ `input_files` ](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) giriş kapsayıcınız için GitHub üzerindeki dizin. İzlemek için bir görev eklendikten onaylamak için Batch Gezgini `ocr-pool` her dosya için. Birkaç saniye sonra çıkış kapsayıcısı için uygulanan OCR dosyasıyla eklenir. Dosyanın sonra görünür ve Depolama Gezgini'ni üzerinde alınabilir.

Ayrıca, günlükleri dosya giriş kapsayıcınıza karşıya yüklediğiniz her dosya için bu gibi iletiler bulabileceğiniz Azure işlevleri web Düzenleyicisi penceresinin alt kısmındaki izleyebilirsiniz:

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-06-21T20:02:35.129 [Information] Name of output text file: <outputTxtFile>
2019-06-21T20:02:35.130 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

Çıkış dosyaları depolama Gezgini'nden yerel makinenize indirmek için önce sonra seçin ve istediğiniz dosyaları seçin **indirme** üstteki Şeritte. 

> [!TIP]
> İndirilen dosyaları içinde bir PDF okuyucu açtıysanız aranabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Havuzlar ve işler oluşturmak için Batch Gezgini'ni kullanın
> * Blob kapsayıcıları ve paylaşılan erişim imzası (SAS) oluşturmak için Depolama Gezgini'ni kullanma
> * Azure blob ile tetiklenen işlev oluşturma
> * Depolama hizmetine giriş dosyaları yükleme
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

* Zamanlamak ve Batch iş yüklerini işlemek üzere .NET API kullanarak daha fazla örnek için bkz: [github'da örnekleri](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp). 

* Batch iş yüklerini çalıştırmak için kullanabileceğiniz daha fazla Azure işlevleri Tetikleyicileri görmek için bkz: [Azure işlevleri belgelerinde](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).
