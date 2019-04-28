---
title: Örnek verileri Azure blob depolama - Team Data Science Process
description: Örnekleme, programlı olarak indirerek ve Python'da yazılan yordamları kullanarak örnekleme Azure blob depolamada depolanan verileri.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1c455106e5faa4aa20ec56f37788e0b8c324fee1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61042925"
---
# <a name="heading"></a>Örnek verileri Azure blob depolama

Bu makale, örnekleme verileri program aracılığıyla indiriliyor ve Python'da yazılan yordamları kullanarak örnekleme tarafından Azure blob depolamada depolanan kapsar.

**Neden verilerinizi örnek?**
Veri kümesini analiz etmek için planlama büyükse, genellikle aşağı örnek veriler için daha küçük ancak temsilcisi ve daha kolay yönetilebilir bir boyutunu azaltmak için iyi bir fikir olduğunu. Bu, özellik Mühendisliği veri anlama ve keşfetme kolaylaştırır. Kendi Cortana Analytics süreci rolünde hızlı prototip oluşturma veri işleme işlevleri ve makine öğrenimi modellerini etkinleştirmektir.

Bir adımda bu örnekleme görevdir [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="download-and-down-sample-data"></a>İndirme ve aşağı örnek veriler
1. Blob hizmeti aşağıdaki örnek Python kodu aracılığıyla Azure blob depolamadan veri indirin: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Yukarıda indirdiğiniz dosyadan Pandas veri-çerçeve içine verileri okuyamadı.
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Aşağı örnek verileri kullanarak `numpy`'s `random.choice` gibi:
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Artık daha fazla araştırma ve özellik oluşturma için yüzde 1 örnek ile Yukarıdaki veri çerçevesi ile çalışabilirsiniz.

## <a name="heading"></a>Karşıya yüklemek ve Azure Machine Learning içine okuyun
Aşağıdaki örnek kod, aşağı örnek veriler ve doğrudan Azure Machine Learning'de kullanmak üzere kullanabilirsiniz:

1. Veri çerçevesinin yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Aşağıdaki örnek kodu kullanarak bir Azure blobuna yerel dosya yükleyin:
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Azure Machine Learning kullanarak Azure blobundan verileri okumak [verileri içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) aşağıdaki resimde gösterildiği gibi:

![Okuyucu blob](./media/sample-data-blob/reader_blob.png)

