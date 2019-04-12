---
title: Gelişmiş analiz - Team Data Science Process ile Azure blob verilerini işleme
description: Verileri araştırmak ve Gelişmiş analiz kullanarak Azure Blob depolamada depolanan verilerden özellikler oluşturun.
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
ms.openlocfilehash: a91c4d9f5dcdcee436f2dbf012eb5485b7a92192
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495626"
---
# <a name="heading"></a>Azure blob verilerini Gelişmiş analiz ile işleme
Bu belge, veri keşfetmek ve Azure Blob Depolama alanında depolanan verilerden oluşturma özellikleri kapsar. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Verileri bir Pandas veri çerçevesine yükleyin
Keşfedin ve bir veri kümesini değiştirmek için blob kaynaktan Pandas veri çerçevesine yüklenebilir, yerel bir dosyaya indirilmelidir. Bu yordam için izlenmesi gereken adımlar şunlardır:

1. Verileri Azure blob'tan blob hizmetini kullanarak aşağıdaki örnek Python kodu ile indirme. Değişkeni aşağıdaki kodda belirli değerleriniz ile değiştirin: 
   
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
2. İçine bir Pandas veri çerçevesine indirilen dosyadaki verileri okuyamadı.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Verileri keşfetme ve bu veri kümesi özellikleri oluşturmak hazırsınız.

## <a name="blob-dataexploration"></a>Veri keşfi
Panda kullanarak verileri araştırmak için gösteren bazı örnekleri şunlardır:

1. Satır ve sütun sayısını denetleme 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Aşağıda gösterildiği gibi veri kümesindeki ilk veya son birkaç satır inceleyin:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Her sütun, aşağıdaki örnek kodu kullanarak olarak içeri aktarılan veri türünü denetleyin
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Veri kümesindeki sütunları temel istatistikleri gibi denetleyin
   
        dataframe_blobdata.describe()
5. Girdi sayısı bu değeri için her bir sütun değeri şu şekilde bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Aşağıdaki örnek kodu kullanarak her bir sütunun girişleri gerçek sayısı eksik değerleri Say
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Verileri belirli bir sütun için eksik değerler varsa, bunları aşağıdaki gibi silebilirsiniz:
   
        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape
   
   Eksik değerleri değiştirmek için başka bir yol ile modu işlevdir:
   
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Bir değişkenin dağıtım çizmek için değişken sayıda depo kullanarak bir çubuk grafik çizim oluşturma    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bir dağılım grafiği veya yerleşik bağıntı işlevi kullanarak değişkenleri arasında bağıntılar bakın
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Özellik oluşturma
Biz, Python kullanarak aşağıdaki gibi özellikleri oluşturabilirsiniz:

### <a name="blob-countfeature"></a>Gösterge değeri temel özellik oluşturma
Kategorik özellikleri şu şekilde oluşturulabilir:

1. Kategorik sütunun dağılımını inceleyin:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Her sütun değerleri için değerler gösterge oluştur
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Gösterge sütunu özgün veri çerçevesi ile birleştirme 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Özgün değişken kaldırın:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Gruplama özellik oluşturma
Binned özellikler oluşturmak için size aşağıdaki gibi ilerleyin:

1. Bir dizi sayısal bir sütun depo için sütunları Ekle
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Bir dizi Boole değişkenleri için gruplama işlemi Dönüştür
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Son olarak, özgün veri çerçevesine sahte değişkenleri katılın
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>Verileri Azure blob için geri yazma ve Azure Machine Learning'de kullanma
Veri denedikten sonra gerekli özellikleri oluşturulan verileri karşıya yükleyebilirsiniz (örneklenen veya özellikleri tespit) için bir Azure blob ve Azure Machine Learning'deki aşağıdaki adımları kullanarak kullanma: Azure Machine Learning Studio'da de ek özellikler oluşturulmuş olduğunu unutmayın. 

1. Veri çerçevesinin yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Verileri Azure blobuna şu şekilde yükleyin:
   
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
3. Azure Machine Learning kullanarak blob verilerin okunacağı artık [verileri içeri aktarma] [ import-data] aşağıdaki ekranda gösterildiği gibi Modülü:

![Okuyucu blob][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

