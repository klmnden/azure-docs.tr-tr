---
title: Azure bulut depolama alanında yapılandırılmamış verileri arama
description: Azure Search'ü kullanarak yapılandırılmamış verileri arama.
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: eba2ef280e60693cfd4402348fe61b122cdccdf6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399865"
---
# <a name="search-unstructured-data-in-cloud-storage"></a>Bulut depolama alanında yapılandırılmamış verileri arama

Bu öğreticide Azure bloblarında depolanan verileri kullanarak [Azure Search](../../search/search-what-is-azure-search.md) ile yapılandırılmamış verileri aramayı öğreneceksiniz. Yapılandırılmamış veriler, önceden tanımlanmış bir şekilde düzenlenmiş olmayan veya bir veri modeline sahip olmayan verilerdir. Örnek olarak .txt dosyaları verilebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturma
> * Kapsayıcınıza veri yükleme
> * Portal aracılığıyla arama hizmeti oluşturma
> * Arama hizmetini kullanarak kapsayıcınızda arama yapma

## <a name="download-the-sample"></a>Örneği indirme

Sizin için bir örnek veri kümesi hazırlandı. **[clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)** dosyasını indirip kendi klasörüne ayıklayın.

Örnek dosya, [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) adresinden alınan metin dosyalarından oluşur. Bu dosyaları Azure ile arama yapmak için örnek metin dosyaları olarak kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı, Azure Depolama veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz bir konum sağlar.

Şu anda **Blob** ve **Genel amaçlı** olmak üzere iki tür depolama hesabı vardır. Bu öğreticide **Genel amaçlı** depolama hesabı oluşturacaksınız.

Genel amaçlı depolama hesabı oluşturma konusunda bilgi sahibi değilseniz aşağıdaki adımları izleyebilirsiniz:

1. Soldaki menüden **Depolama Hesapları**'nı ve ardından **Ekle**'yi seçin.

2. Depolama hesabınız için benzersiz bir ad girin. 

3. **Dağıtım modeli** olarak **Kaynak Yöneticisi**'ni seçin ve **Hesap türü** açılan menüsünden **Genel amaçlı**'yı seçin.

4. **Çoğaltma** açılan menüsünden **Yerel olarak yedekli depolama (LRS)** girişini seçin.

5. **Kaynak grubu**'nda **Yeni oluştur**'u seçin ve benzersiz bir ad girin.

6. Uygun bir abonelik seçin.

7. Bir Konum belirleyin ve **Oluştur**'u seçin.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcılar klasörlere benzer ve blobları depolamak için kullanılır.

Bu öğreticide clinicaltrials.gov adresinden alınan metin dosyalarını depolamak için tek bir kapsayıcı kullanacaksınız.

1. Azure portalda depolama hesabınıza gidin.

2. **Blob Hizmeti** bölümünde **Blob'lara göz atın**'ı seçin.

3. Yeni bir kapsayıcı ekleyin.

4. Kapsayıcıya "data" adını verin ve genel erişim düzeyi olarak **Kapsayıcı**'yı seçin.

5. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin. 

  ![Yapılandırılmamış arama](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Örnek verileri karşıya yükleme

Artık bir kapsayıcıya sahip olduğunuza göre örnek verilerinizi içine yükleyebilirsiniz.

1. Kapsayıcınızı seçip **Yükle**'ye tıklayın.

2. Dosyalar alanının yanındaki mavi renkli klasör simgesini seçin ve örnek verileri ayıkladığınız yerel klasöre gidin.

3. Ayıklanan dosyaların tümünü seçin ve **Aç**'ı seçin.

4. **Yükle**'yi seçerek yükleme işlemini başlatın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/upload.png)

Yükleme işlemi zaman alabilir.

İşlem tamamlandıktan sonra veri kapsayıcınıza dönün ve metin dosyalarının yüklendiğini onaylayın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Arama hizmeti oluşturma

Azure Search, geliştiricilere, web uygulamalarındaki, mobil uygulamalardaki ve kurumsal uygulamalardaki verilerinize yönelik zengin arama deneyimi ekleme araçlarını ve API’lerini sunan, hizmet olarak arama bulut çözümüdür.

Arama hizmeti konusunda bilgi sahibi değilseniz aşağıdaki adımları izleyebilirsiniz:

1. Azure portalda depolama hesabınıza gidin.

2. Sayfayı aşağı kaydırın ve **BLOB HİZMETİ**'nin altında **Azure Search'ü Ekle**'ye tıklayın.

3. **Verileri İçeri Aktar** bölümünde **Hizmetinizi seçin**'e tıklayın.

4. **Yeni bir arama hizmeti oluşturmak için buraya tıklayın**'ı seçin.

5. **Yeni Arama Hizmeti** bölümünde **URL** alanına arama hizmetiniz için benzersiz bir ad girin.

6. **Kaynak grubu** bölümünde **Var olanı kullan**'ı seçin ve önceden oluşturduğunuz kaynak grubuna tıklayın.

7. **Fiyatlandırma katmanı** için **Ücretsiz** katmanı seçin ve **Seç**'e tıklayın.

8. Arama hizmetini oluşturmak için **Oluştur**'a tıklayın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcınıza bağlama

Artık bir arama hizmetiniz olduğuna göre bunu blob depolamanıza ekleyebilirsiniz. Bu bölümde veri kaynağı seçme, dizin oluşturma ve dizin oluşturucu oluşturma işlemleri anlatılmaktadır.

1. Depolama hesabınıza gidin.

2. **BLOB HİZMETİ**'nin altında **Azure Search'ü Ekle**'yi seçin.

3. **Verileri İçeri Aktar** bölümünde **Arama Hizmeti**'ni seçin ve önceki bölümde oluşturduğunuz arama hizmetine tıklayın. **Yeni veri kaynağı** sayfası açılır.

### <a name="new-data-source"></a>Yeni veri kaynağı

  Veri kaynağı, dizin oluşturulacak verileri ve verilere nasıl erişileceğini belirtir. Bir arama hizmeti bir veri kaynağını birden fazla kaz kullanabilir.

1. Veri kaynağının adını girin. **Ayıklanacak veri** bölümünde **İçerik ve meta veriler**'i seçin. Veri kaynağı blobun dizin oluşturulacak bölümlerini belirtir.
    
    a. İleride kullanacağınız senaryolarda **Yalnızca depolama meta verileri** seçeneğini de belirleyebilirsiniz. Dizin oluşturulan verileri standart blob özellikleri veya kullanıcı tanımlı özelliklerle sınırlamak istediğinizde bu seçimi kullanmanız gerekir.
    
    b. Dilerseniz **Tüm meta veriler**'i seçerek hem standart blob özelliklerini hem de *tüm* içerik türüne özgü meta verileri alabilirsiniz. 

2. Kullandığınız bloblar metin dosyaları olduğundan **Ayrıştırma Modu**'nu **Metin** olarak ayarlayın.
    
    a. İleride kullanacağınız senaryolarda bloblarınızın içeriğine bağlı olarak [diğer ayrıştırma modlarını](../../search/search-howto-indexing-azure-blob-storage.md) da seçmek isteyebilirsiniz.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/datasources.png)

3. Kullanılabilir depolama hesaplarını listelemek için **Depolama Kapsayıcısı**'nı seçin.

4. Depolama hesabınızı ve ardından daha önceden oluşturduğunuz kapsayıcıyı seçin.

5. **Seç**'e tıklayarak **Yeni veri kaynağı** sayfasında dönün ve devam etmek için **Tamam**'a tıklayın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>Dizini yapılandırma

  Dizin, veri kaynağınızdaki alanların arama yapılabilen bir koleksiyonudur. Dizin, arama hizmetinize, verilerinizin hangi şekilde aranabileceğini bildirir.

1. **Verileri İçeri Aktar** bölümünde **Hedef dizini özelleştir**'i seçin.
 
2. **Dizin adı** alanına dizininize vermek istediğiniz adı girin.

3. **metadata_storage_name** altındaki **Alınabilir** onay kutusunu seçin.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/valuestoselect.png)

4. **Tamam**'a tıklayın, **Dizin oluşturucu oluştur** sayfası açılır.

Dizininizin parametreleri ve bu parametreler için belirlediğiniz öznitelikler önemlidir. Parametreler *hangi* verilerin depolanacağını, öznitelikler de verilerin *nasıl* depolanacağını belirler.

**ALAN ADI** sütunu parametreleri içerir. Aşağıdaki tabloda kullanılabilir özniteliklere ve açıklamalarına yer verilmiştir.

### <a name="field-attributes"></a>Alan öznitelikleri
| Öznitelik | Açıklama |
| --- | --- |
| *Anahtar* |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| *Alınabilir* |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| *Filtrelenebilir* |Alanın filtre sorgularında kullanılmasını sağlar. |
| *Sıralanabilir* |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| *Modellenebilir* |Kullanıcının bağımsız filtrelemesi için modellenmiş bir gezinmede bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| *Aranabilir* |Alanı tam metin aranabilir şeklinde işaretler. |


### <a name="create-an-indexer"></a>Dizin oluşturucu oluşturma
    
  Dizin oluşturucu, veri kaynağı ile bir arama dizini arasında bağlantı kurar ve verilerinizle yeniden dizin oluşturmak için bir zamanlama sunar.

1. **Ad** alanına bir ad girin ve **Tamam**'ı seçin.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/exindexer.png)

2. **Verileri İçeri Aktar** sayfası açılır. Bağlantı işlemini tamamlamak için **Tamam**'ı seçin.

Blobunuzla arama hizmetiniz arasında başarıyla bağlantı kurmuş oldunuz. Portalda dizinin doldurulmuş şekilde görünmesi birkaç dakika sürebilir. Ancak arama hizmeti dizin oluşturmayı hemen başlattığından arama yapmaya doğrudan başlayabilirsiniz.

## <a name="search-your-text-files"></a>Metin dosyalarınızda arama yapma

Dosyalarınızda arama yapmak için yeni oluşturduğunuz arama hizmetinin dizininde arama gezginini açın.

Aşağıdaki adımlarda arama gezginini nerede bulabileceğiniz ve bazı örnek sorgular gösterilmiştir:

1. Tüm kaynaklar sayfasına gidin ve yeni oluşturduğunuz arama hizmetini bulun.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/exampleurl.png)

3. Dizininizi seçerek açın. 

  ![Yapılandırılmamış arama](media/storage-unstructured-search/overview.png)

4. **Arama Gezgini**'ni seçerek verilerinizde canlı sorgular oluşturabileceğiniz arama gezginini açın.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/indexespane.png)

5. Sorgu dizesi alanı boş durumdayken **Ara**'yı seçin. Boş sorgu, bloblarınızdaki *tüm* verileri döndürür.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Tam metin arama 

**Sorgu dizesi** alanına "Myopia" yazın ve **Ara**'yı seçin. Dosyaların içeriğinde arama başlatılır ve "Myopia" sözcüğünü içeren alt küme döndürülür.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Sistem özelliklerini arama

Dilerseniz `$select` parametresini kullanarak sistem parametrelerine göre arama yapan sorgular da oluşturabilirsiniz. Sorgu dizesine `$select=metadata_storage_name` yazıp Enter tuşuna bastığınızda yalnızca ilgili alan döndürülür.
    
Sorgu dizesi doğrudan URL'yi değiştirdiğinden boşluk kullanılamaz. Birden fazla alanda arama yapmak için virgül kullanın; örneğin: `$select=metadata_storage_name,metadata_storage_path`
    
`$select` parametresi yalnızca dizininizi tanımlama aşamasında alınabilir olarak işaretlenen alanlar için kullanılabilir.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/metadatasearchunstructured.png) 

Bu öğreticiyi tamamladınız ve elinizde arama yapılabilen bir yapılandırılmamış veri kümesi var.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Search kullanarak yapılandırılmamış verileri arama hakkında aşağıda örnekleri verilen konularda bilgi edindiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturma
> * Kapsayıcınıza veri yükleme
> * Arama hizmeti oluşturma
> * Arama hizmetini kullanarak kapsayıcınızda arama yapma

Azure Search ile belgelerden dizin oluşturma hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure Search ile Azure Blob Depolama’da Belgelerin Dizinini Oluşturma](../../search/search-howto-indexing-azure-blob-storage.md)