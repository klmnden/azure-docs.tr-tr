---
title: "Pandas ile Azure blob storage keşfedin | Microsoft Docs"
description: "Pandas kullanarak Azure blob kapsayıcısında depolanan verileri araştırmak nasıl."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: a46735dde28740087d201d7490f135349aad76f6
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Panda ile Azure blob depolama verilerini keşfedin
Bu belge, Azure blob kapsayıcısını kullanarak depolanan verileri araştırmak alınmaktadır [Pandas](http://pandas.pydata.org/) Python paket.

Aşağıdaki **menü** araçları çeşitli depolama ortamlarından verileri araştırmak için nasıl kullanılacağını açıklayan konulara bağlantılar. Bu görev bir adımdır [veri bilimi işlemi]().

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* Bir Azure depolama hesabı oluşturuldu. Yönergeler gerekiyorsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Verilerinizi bir Azure blob depolama hesabında depolanır. Yönergeler gerekiyorsa bkz [için ve Azure Storage veri taşıma](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Pandas DataFrame veri yükleme
Keşfetmek ve bir veri kümesini değiştirmek için önce blob kaynağından bir Pandas DataFrame yüklenebilir yerel bir dosyaya yüklenmelidir. Bu yordam için izlemeniz gereken adımlar şunlardır:

1. Verileri Azure blob'tan blob hizmeti kullanarak aşağıdaki Python kodu örneği ile indirme. Aşağıdaki kodda değişkeni belirli değerleriniz ile değiştirin: 
   
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

## <a name="blob-dataexploration"></a>Veri keşfi Pandas kullanma örnekleri
Pandas kullanarak verileri araştırmak için yollar bazı örnekleri şunlardır:

1. İnceleme **satır ve sütunların sayısı** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **İnceleme** ilk veya son birkaç **satırları** aşağıdaki kümesindeki:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Denetleme **veri türü** her sütun, aşağıdaki örnek kod kullanarak olarak içeri aktarıldı
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Denetleme **temel istatistikleri** için aşağıdaki gibi ayarlayın veri sütunları
   
        dataframe_blobdata.describe()
5. Her bir sütunun değeri için girdi sayısı gibi bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Eksik değerleri saymak** gerçek sayısı aşağıdaki örnek kod kullanarak her bir sütunun giriş karşılaştırması
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Varsa **eksik değerleri** verileri belirli bir sütun için bunları aşağıdaki gibi bıraktığınız:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Eksik değerleri değiştirmek için başka bir yol ile modu işlevi şu şekildedir:
   
     dataframe_blobdata_mode dataframe_blobdata.fillna = ({< column_name >: dataframe_blobdata ['< column_name >'] .mode()[0]})        
8. Oluşturma bir **histogram** bir değişken dağıtımını çizmek için depo değişken sayıda kullanarak çizim    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bakmak **bağıntıları** bir scatterplot veya yerleşik bağıntı işlevi kullanarak değişkenleri arasında
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

