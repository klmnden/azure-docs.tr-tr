---
title: Yapılandırılmamış verileri Azure bulut depolama alanında arama
description: Yapılandırılmamış verileri Azure arama'yı kullanarak aramayı.
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogara
ms.custom: mvc
ms.openlocfilehash: 930b735eb03aea6ce701b694ca527049b4c3f24d
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
ms.locfileid: "23930207"
---
# <a name="search-unstructured-data-in-cloud-storage"></a>Bulut depolama alanında yapılandırılmamış verileri arama

Bu öğreticide, yapılandırılmamış verileri kullanarak aramak öğrenin [Azure Search](../../search/search-what-is-azure-search.md) Azure BLOB'ları depolanan verileri kullanıyor. Yapılandırılmamış verileri önceden tanımlanmış bir şekilde düzenlenmiştir değil ya da bir veri modeli yok verilerdir. Bir örnek .txt dosyası olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturma
> * Kapsayıcıya veri yükleme
> * Portal üzerinden bir arama hizmeti oluşturun
> * Arama hizmeti, kapsayıcı aramak için kullanın

## <a name="download-the-sample"></a>Örneği indirme

Bir örnek veri kümesi için hazırlandı. **Karşıdan [trials.zip Klinik](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)**  ve kendi klasörüne sıkıştırmasını açın.

Alınan metin dosyaları örnek oluşan [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Azure kullanarak aramak için örnek metin dosyası olarak bunları kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bir depolama hesabı depolamak ve Azure Storage veri nesnelerinizi erişmek için benzersiz bir konum sağlar.

Şu anda, iki tür depolama hesabı vardır **Blob** ve **genel amaçlı**. Bu öğretici için oluşturduğunuz bir **genel amaçlı** depolama hesabı.

Genel amaçlı bir depolama hesabı oluşturma işlemine tanıdık değilse, oluşturmak nasıl aşağıda verilmiştir:

1. Sol menüden seçin **depolama hesapları**seçeneğini belirleyip **Ekle**.

2. Depolama hesabınız için benzersiz bir ad girin. 

3. Seçin **Resource Manager** için **dağıtım modeli** seçip **genel amaçlı** gelen **tür hesap** açılır.

4. Seçin **yerel olarak yedekli depolama (LRS)** gelen **çoğaltma** açılır.

5. Altında **kaynak grubu**seçin **Yeni Oluştur** ve benzersiz bir ad girin.

6. Uygun bir abonelik seçin.

7. Bir konum seçin ve Seç **oluşturun.**

  ![Yapılandırılmamış arama](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcıları klasörlere benzer ve blobları depolamak için kullanılır.

Bu öğretici için clinicaltrials.gov alınan metin dosyalarını depolamak için tek bir kapsayıcı kullanın.

1. Azure portalında depolama hesabınıza gidin.

2. Seçin **Gözat BLOB'lar** altında **Blob hizmeti.**

3. Yeni bir kapsayıcı ekleyin.

4. "Data" kapsayıcı adı ve seçin **kapsayıcı** genel erişim düzeyi için.

5. Seçin **Tamam** kapsayıcısı oluşturmak için. 

  ![Yapılandırılmamış arama](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Örnek veri yükleme

Bir kapsayıcı sahip olduğunuza göre örnek veri yükleyebilir.

1. Kapsayıcı seçip **karşıya**.

2. Örnek verileri ayıkladığınız yerel klasöre gidin ve dosyaları alan yanında gösterilen mavi klasör simgesini seçin.

3. Tüm ayıklanan dosyaları seçip **Aç**

4. Seçin **karşıya** karşıya yükleme işlemini başlatmak için.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/upload.png)

Karşıya yükleme işlemi biraz zaman alabilir.

İşlem tamamlandıktan sonra metin dosyaları karşıya onaylamak için geri, verileri kapsayıcısına gidin.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Bir arama hizmeti oluşturun

Azure arama geliştiricilere API'ler sağlar ve web, mobil ve kurumsal uygulamalarda, veriler üzerinde zengin arama deneyimi eklemek için Araçlar bir bulut hizmet olarak arama çözümüdür.

Bir arama hizmeti oluşturma işlemine bilmiyorsanız, şöyle oluşturmak:

1. Azure portalında depolama hesabınıza gidin.

2. Aşağı kaydırın ve tıklatın **Azure arama Ekle** altında **BLOB hizmeti**.

3. Altında **veri içeri aktarma**seçin **hizmetinizi çekme**.

4. Seçin **yeni bir arama hizmeti oluşturmak için buraya tıklayın**.

5. İçinde **yeni arama hizmeti** arama hizmetiniz için benzersiz bir ad girin **URL** alan.

6. Altında **kaynak grubu**seçin **var olanı kullan** ve önceden oluşturduğunuz kaynak grubunu seçin.

7. İçin **fiyatlandırma katmanı**seçin **serbest** katmanı ve tıklayın **seçin**.

8. Seçin **oluşturma** arama hizmeti oluşturmak için.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcıya Bağlan

Bir arama hizmeti sahip olduğunuza göre blob depolama alanına ekleyebilirsiniz. Bu bölüm veri kaynağı seçme, dizin oluşturma ve bir dizin oluşturucu oluşturma sürecinde yardımcı olur.

1. Depolama hesabınıza gidin.

2. Seçin **Azure arama ekleyin** altında **BLOB hizmeti.**

3. Seçin **arama hizmeti** içinde **veri içeri aktarma** tıkladıktan sonra önceki bölümde oluşturduğunuz arama hizmeti. Bu açılır **yeni veri kaynağı**.

### <a name="new-data-source"></a>Yeni veri kaynağı

  Bir veri kaynağı dizini ve verilere erişmek nasıl veri belirtir. Bir veri kaynağı, birden çok kez aynı arama hizmeti tarafından kullanılabilir.

1. Veri kaynağı için bir ad girin. Altında **ayıklamak için veri**seçin **içerik ve meta veri**. Blob hangi kısımlarının dizine veri kaynağını belirtir.
    
    a. Kendi gelecekteki senaryolarda öğesini de seçebilirsiniz **yalnızca depolama meta verileri**. Standart blob özellikleri veya kullanıcı tanımlı özellikleri dizine verileri sınırlamak isterseniz, bu seçim yapacağı.
    
    b. Ayrıca seçebilirsiniz **tüm meta veriler** hem standart blob özellikleri almak için ve *tüm* içerik türü belirli meta veriler. 

2. Kullanmakta olduğunuz BLOB'ları metin dosyaları olduğundan, ayarlamak **ayrıştırma modu** için **metin**.
    
    a. Kendi gelecekteki senaryolarda seçmek isteyebilirsiniz [ayrıştırma modlardan](../../search/search-howto-indexing-azure-blob-storage.md) bloblarınızın içeriğini bağlı olarak.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/datasources.png)

3. Seçin **depolama kapsayıcısı** kullanılabilir depolama hesaplarını listelemek için.

4. Depolama hesabınızı seçin ve daha önce oluşturduğunuz kapsayıcısı seçin.

5. Tıklatın **seçin** geri dönmek için **yeni veri kaynağı** seçip **Tamam** devam etmek için.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>Dizin yapılandırın

  Bir dizin, veri kaynağından aranabilir alanları koleksiyonudur. Hangi yollarla verilerinizi aranıp aranmayacağını arama hizmetinizi nasıl bilir dizinidir.

1. İçinde **veri içeri aktarma** seçin **hedef dizini Özelleştir**.
 
2. Dizininizde için bir ad girin **dizin adı** alan.

3. Seçin **alınabilir** özniteliğin onay kutusu altında **metadata_storage_name**.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/valuestoselect.png)

4. Tıklatın **Tamam**, hangi getirir **bir dizin oluşturucu yapın**.

Dizininizi ve bu parametrelerin size öznitelikleri parametrelerinin önemlidir. Parametreleri belirtin *ne* depolamak, öznitelikler belirtmek için veri *nasıl* bu verileri depolamak için.

**Alan adı** sütun parametreleri içerir. Aşağıdaki tabloda kullanılabilir öznitelikleri ve açıklamalarının listesini sağlar.

### <a name="field-attributes"></a>Alan öznitelikleri
| Öznitelik | Açıklama |
| --- | --- |
| *Anahtar* |Belge araması için kullanılan her bir belgenin benzersiz Kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| *Alınabilir* |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| *Filtrelenebilir* |Alanın filtre sorgularında kullanılmasını sağlar. |
| *Sıralanabilir* |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| *Modellenebilir* |Kullanıcının bağımsız filtrelemesi için modellenmiş gezinti yapısında kullanılacak bir alan sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| *Aranabilir* |Alanı tam metin aranabilir şeklinde işaretler. |


### <a name="create-an-indexer"></a>Bir dizin oluşturucu yapın
    
  Bir dizin oluşturucu, arama dizini ile bir veri kaynağına bağlanır ve verilerinizi yeniden dizin oluşturma için bir zamanlama sağlar.

1. Bir ad girin **adı** alan ve seçin **Tamam**.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/exindexer.png)

2. Size geri yönlendirilirsiniz **veri içeri aktarma**seçin **Tamam** bağlantı işlemini tamamlamak için.

Şimdi başarıyla, blob arama hizmetinize bağlı. Dizin doldurulur göstermesini portalı için birkaç dakika sürer. Ancak, arama hizmeti hemen hemen arama başlayabilmesi için dizin oluşturma işlemi başlar.

## <a name="search-your-text-files"></a>Metin dosyaları arama

Dosyalarınızı aramak için yeni oluşturulan arama hizmetinizi dizini içinde arama Gezgini'ni açın.

Aşağıdaki adımlar, arama Gezgini nerede bulacağını Göster ve bazı örnek sorgular içerir:

1. Tüm kaynaklara gidin ve yeni oluşturulan arama hizmetinizi bulun.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/exampleurl.png)

3. Dizininizi açın için seçin. 

  ![Yapılandırılmamış arama](media/storage-unstructured-search/overview.png)

4. Seçin **arama Gezgini** yapabileceğiniz dinamik sorgular verilerinizi arama Gezgini'ni açın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/indexespane.png)

5. Seçin **arama** sorgu dizesi alanının boş olsa. Boş bir sorgunun döndürdüğü *tüm* , blob'lara ait veriler.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Tam metin arama 

"Myopia" girin **sorgu dizesi** alan ve seçin **arama**. Dosya içeriğini bir arama başlatmak ve "Myopia." sözcüğünü içeren bir alt kümesini, döndürme

  ![Yapılandırılmamış arama](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Sistem özellikleri arama

Kullanarak sistem özelliklerine göre arama sorguları oluşturabilirsiniz `$select` parametresi. Girin `$select=metadata_storage_name` basın ve sorgu dizesi, yalnızca belirli Bu alanda döndürme girin.
    
Alanları verilmeyen biçimde sorgu dizesi doğrudan URL değiştiriyor. Birden çok alan aramak için bir virgül gibi kullanın:`$select=metadata_storage_name,metadata_storage_path`
    
`$select` Parametresi yalnızca dizininizi tanımlarken alınabilir olarak işaretlenmiş alanlar ile kullanılabilir.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/metadatasearchunstructured.png) 

Bu öğretici tamamladınız ve yapılandırılmamış verileri aranabilir kümesine sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Search nasıl gibi kullanarak yapılandırılmamış veri arama hakkında öğrenilen:

> [!div class="checklist"]
> * Kaynak grubu oluşturma
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturma
> * Kapsayıcıya karşıya veri yükleme
> * Bir arama hizmeti oluşturun
> * Kapsayıcı aramak için arama hizmetini kullanma

Azure Search belgeleri dizin oluşturma hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure arama ile Azure Blob Storage belgelerde dizin oluşturma](../../search/search-howto-indexing-azure-blob-storage.md)