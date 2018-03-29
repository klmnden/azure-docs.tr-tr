---
title: İşlem gelişmiş analizler ile Azure blob veri | Microsoft Docs
description: İşlem verileri Azure Blob depolamada.
services: machine-learning,storage
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev
ms.openlocfilehash: 8a3331bb3e78520424486deb1797dd0c89b28efd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="heading"></a>Gelişmiş analytics ile Azure blob verileri işleme
Bu belge, araştırma veri ve Azure Blob storage'da depolanan verileri oluşturma özellikleri kapsar. 

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

## <a name="blob-dataexploration"></a>Veri keşfi
Pandas kullanarak verileri araştırmak için yollar bazı örnekleri şunlardır:

1. Satır ve sütun sayısını inceleyin. 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Aşağıdaki şekilde kümesindeki ilk veya son birkaç satır inceleyin:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Her sütun, aşağıdaki örnek kod kullanılarak olarak içe aktarılan veri türünü denetleyin
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Veri kümesi sütunları temel istatistikleri aşağıdaki gibi denetleyin
   
        dataframe_blobdata.describe()
5. Her bir sütunun değeri için girdi sayısı gibi bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Aşağıdaki örnek kod kullanarak her sütunda girişleri gerçek sayısını karşı eksik değerleri Say
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Verileri belirli bir sütun için eksik değerleri varsa, aşağıdaki gibi silebilirsiniz:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape
   
   Eksik değerleri değiştirmek için başka bir yol ile modu işlevi şu şekildedir:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Bir değişkenin dağıtım çizmek için depo değişken sayıda kullanarak bir histogram çizim oluşturma    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bir scatterplot veya yerleşik bağıntı işlevi kullanarak değişkenleri arasındaki bağıntıları bakın
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Özellik oluşturma
Biz, Python gibi kullanarak özellik oluşturabilirsiniz:

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
3. Verileri Azure Machine Learning kullanarak blobundan okunabilir artık [veri içeri aktarma] [ import-data] aşağıdaki ekranda gösterildiği gibi Modülü:

![Okuyucu blob][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

