---
title: "Panda kullanarak Azure blob depolama verileri için özellikleri oluşturma | Microsoft Docs"
description: "Azure blob kapsayıcısında Panda Python paket depolanan veriler için özellikleri oluşturma"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 676b5fb0-4c89-4516-b3a8-e78ae3ca078d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: ea6712fcedcc61c9f88e9daa8d576ac3d202da51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a>Panda kullanarak Azure blob depolama verilerinin özelliklerini oluşturma
Bu belgede Azure blob kapsayıcısını kullanarak depolanan veriler için özellikler oluşturmayı gösteren [Pandas](http://pandas.pydata.org/) Python paket. Veriler Panda veri çerçeveye yükleme anahat oluşturma sonra gösterge değerlerle Python komut dosyalarını kullanarak ve özellikleri binning kategorik özellikleri oluşturmak nasıl gösterir.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Bu **menü** özellikleri veriler için çeşitli ortamlar oluşturmak nasıl açıklayan konulara bağlantılar. Bu görev bir adımdır [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Ön koşullar
Bu makale bir Azure blob storage hesabı oluşturduysanız ve verilerinizi var. depoladığınız varsayar. Bir hesabı ayarlamak için yönergeler gerekiyorsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account)

## <a name="load-the-data-into-a-pandas-data-frame"></a>Pandas veri çerçeveye verileri yükleme
Keşfetmek ve bir veri kümesini değiştirmek için onu blob kaynağından Pandas veri çerçevede yüklenebilir yerel bir dosyaya yüklenmelidir. Bu yordam için izlemeniz gereken adımlar şunlardır:

1. Verileri Azure blob'tan blob hizmeti kullanarak aşağıdaki örnek Python kodu ile indirme. Aşağıdaki kodda değişkeni belirli değerleriniz ile değiştirin:
   
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
2. İndirilen Dosya Pandas verileri-çerçeve içine verilerini okur.
   
        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Şimdi verileri araştırmak ve bu veri kümesi özellikleri oluşturmak hazır olursunuz.

## <a name="blob-featuregen"></a>Özellik oluşturma
Sonraki iki bölümde göstergesi değerlerle kategorik özellikleri ve Python komut dosyalarını kullanarak binning özellikleri üretmeyi gösteririz.

### <a name="blob-countfeature"></a>Gösterge değeri özellik nesil dayalı
Kategorik özellikler aşağıdaki gibi oluşturulabilir:

1. Kategorik sütunun dağılımını inceleyin:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Her sütun değerleri için gösterge değerlerini oluşturmak
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Gösterge sütunu özgün veri çerçevesi ile birleştirme
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Özgün değişkeni kaldırın:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Binning özelliği oluşturma
Binned özellikleri oluşturmak için size aşağıdaki gibi ilerleyin:

1. Sayısal bir sütun bin sütunların sırası Ekle
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Boolean değişkenleri bir dizi binning Dönüştür
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Son olarak, özgün veri çerçevesi kukla değişkenleri katılma
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <a name="sql-featuregen"></a>Verileri Azure blob geri yazma ve Azure Machine Learning ile kullanma
Veri denedikten sonra gerekli özellikleri oluşturulan, verilerini karşıya yükleyebilir (örneklenen veya featurized) bir Azure blob ve aşağıdaki adımları kullanarak Azure Machine Learning ile kullanabilir: ek özellikler Azure Machine Learning Studio'da de oluşturulabilir unutmayın.

1. Veri çerçevesi yerel dosyasına yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Verileri Azure blob aşağıdaki gibi yükleyin:
   
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
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Verileri Azure Machine Learning kullanarak blobundan okunabilir artık [veri içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) aşağıdaki ekranda gösterildiği gibi Modülü:

![Okuyucu blob](./media/data-blob/reader_blob.png)

