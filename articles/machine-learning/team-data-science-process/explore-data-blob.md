---
title: Panda ile Azure blob depolamadaki verileri keşfedin | Microsoft Docs
description: Panda kullanarak Azure blob kapsayıcısında depolanan verileri araştırmak nasıl.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: b80fcecf28eaaf05e7fc199a9c318fd4148b9212
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497867"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Panda ile Azure blob depolamadaki verileri keşfedin
Bu belgeyi kullanarak Azure blob kapsayıcının içinde depolanan verileri araştırmak nasıl etkinleştireceğinizi de açıklar [pandas](http://pandas.pydata.org/) Python paket.

Aşağıdaki **menü** Araçlar çeşitli depolama ortamlarından verileri araştırmak için nasıl kullanılacağını açıklayan konulara bağlantılar. Bu görev bir adımdır [Data Science Process](overview.md).

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
* Verilerinizi bir Azure blob depolama hesabında depolanır. Yönergelere ihtiyacınız varsa bkz [için ve Azure Depolama'dan veri taşıma](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Pandas DataFrame verileri yükleme
Keşfedin veya bir veri kümesini değiştirmek için önce blob kaynağından bir pandas DataFrame yüklenebilir yerel bir dosyaya indirilmelidir. Bu yordam için izlenmesi gereken adımlar şunlardır:

1. Verileri Azure blob'tan blob hizmeti kullanarak aşağıdaki Python kod örneği ile indirme. Aşağıdaki kod içindeki değişkene belirli değerleriniz ile değiştirin: 
   
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
2. İçine bir pandas DataFrame indirilen dosyasından verileri okur.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Verileri keşfetme ve bu veri kümesi özellikleri oluşturmak hazırsınız.

## <a name="blob-dataexploration"></a>Veri keşfi pandas kullanma örnekleri
Panda kullanarak verileri araştırmak için gösteren bazı örnekleri şunlardır:

1. İnceleme **satır ve sütun sayısı** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **İnceleme** ilk veya son birkaç **satırları** aşağıdaki kümesindeki:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Denetleme **veri türü** her sütun, aşağıdaki örnek kodu kullanarak olarak içeri aktarıldı
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Denetleme **temel istatistikleri** sütunların veri şu şekilde ayarlayın
   
        dataframe_blobdata.describe()
5. Girdi sayısı bu değeri için her bir sütun değeri şu şekilde bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Eksik değerleri saymak** girdileri aşağıdaki örnek kodu kullanarak her sütunda gerçek sayısı
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Varsa **eksik değerleri** için belirli bir sütuna veri, bunları aşağıdaki gibi silebilirsiniz:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Eksik değerleri değiştirmek için başka bir yol ile modu işlevdir:
   
     dataframe_blobdata_mode dataframe_blobdata.fillna = ({'< column_name >': ['< column_name >'] dataframe_blobdata .mode()[0]})        
8. Oluşturma bir **histogram** bir değişkenin dağıtım çizmek için değişken sayıda depo kullanarak Çiz    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bakmak **bağıntılar** arasında bir dağılım grafiği veya yerleşik bağıntı işlevi kullanarak değişkenleri
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

