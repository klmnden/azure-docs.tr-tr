---
title: 'Öğretici: Azure Blob Depolama alanında yapılandırılmamış verileri arama'
description: "Öğretici: Azure Search'ü kullanarak Blob Depolama alanında yapılandırılmamış verileri arama."
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 12/13/2018
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: 8c11d2e6d9a902707d3fd98f09d3474a5d2a5f64
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150787"
---
# <a name="tutorial-search-unstructured-data-in-cloud-storage"></a>Öğretici: Bulut depolama alanında yapılandırılmamış verileri arama

Bu öğreticide, şunların nasıl kullanarak yapılandırılmamış verileri arama [Azure Search](../../search/search-what-is-azure-search.md), Azure Blob Depolama'da depolanan veriler kullanma. Yapılandırılmamış veriler, önceden tanımlanmış bir şekilde düzenlenmiş olmayan veya bir veri modeline sahip olmayan verilerdir. Bir .txt dosyasına bir örnektir.

Bu öğreticide bir Azure aboneliğine sahip olmasını gerektirir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturma
> * Kapsayıcınıza veri yükleme
> * Portal aracılığıyla arama hizmeti oluşturma
> * Bir arama hizmeti bir depolama hesabına bağlama
> * Veri kaynağı oluşturma
> * Dizini yapılandırma
> * Dizin oluşturucu oluşturma
> * Arama hizmetini kullanarak kapsayıcınızda arama yapma

## <a name="prerequisites"></a>Önkoşullar

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturabilir veya mevcut bir kaynak grubunu kullanmak için seçeneğiniz vardır. Bu öğreticide, yeni bir kaynak grubu oluşturur.

[Azure Portal](https://portal.azure.com) oturum açın.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

Böylece yapabileceğiniz bir örnek veri kümesini sizin için hazırlanmış Bu öğretici için bunu kullanın. İndirme [trials.zip Klinik](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip) ve kendi klasörüne ayıklayın.

Örnek dosya, [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) adresinden alınan metin dosyalarından oluşur. Bu öğreticide, bunları Azure Search hizmetlerini kullanarak aranır örnek metin dosyası olarak kullanılır.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcılar klasörlere benzer ve blobları depolamak için kullanılır.

Bu öğreticide clinicaltrials.gov adresinden alınan metin dosyalarını depolamak için tek bir kapsayıcı kullanacaksınız.

1. Azure portalında depolama hesabınıza gidin.

2. Seçin **Gözat blobları** altında **Blob hizmeti**.

3. Yeni bir kapsayıcı ekleyin.

4. Kapsayıcı adı **veri** seçip **kapsayıcı** genel erişim düzeyi için.

5. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Örnek verileri karşıya yükleme

Artık bir kapsayıcıya sahip olduğunuza göre örnek verilerinizi içine yükleyebilirsiniz.

1. Kapsayıcınızı seçip **Yükle**'ye tıklayın.

2. Mavi klasör simgesini seçin **dosyaları** alanı ve örnek verileri ayıkladığınız yerel klasöre göz atın.

3. Ayıklanan dosyaların tümünü'ı seçip **açık**.

4. **Yükle**'yi seçerek yükleme işlemini başlatın.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/upload.png)

Karşıya yükleme işlemi biraz zaman alabilir.

Tamamlandığında, metin dosyaları karşıya onaylamak için veri kapsayıcınıza dönün.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Arama hizmeti oluşturma

Azure Search, geliştiricilere API'leri sunan ve verileriniz üzerinde arama deneyimi ekleme araçlarını bir hizmet olarak arama bulut çözümüdür.

Bu öğreticide, bir arama hizmeti clinicaltrials.gov alınan metin dosyaları aramak için kullanın.

1. Azure portalında depolama hesabınıza gidin.

2. Aşağı kaydırın ve select **Azure Search Ekle** altında **BLOB hizmeti**.

3. **Verileri İçeri Aktar** bölümünde **Hizmetinizi seçin**'e tıklayın.

4. **Yeni bir arama hizmeti oluşturmak için buraya tıklayın**'ı seçin.

5. **Yeni Arama Hizmeti** bölümünde **URL** alanına arama hizmetiniz için benzersiz bir ad girin.

6. Altında **kaynak grubu**seçin **var olanı kullan** ve daha önce oluşturduğunuz kaynak grubunu seçin.

7. İçin **fiyatlandırma katmanı**seçin **ücretsiz** katmanı ve tıklayın **seçin**.

8. Arama hizmetini oluşturmak için **Oluştur**'a tıklayın.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Arama hizmetinizi, kapsayıcınıza bağlama

Artık bir arama hizmetiniz olduğuna göre bunu blob depolamanıza ekleyebilirsiniz. Bu bölümde veri kaynağı seçme, dizin oluşturma ve dizin oluşturucu oluşturma işlemleri anlatılmaktadır.

1. Depolama hesabınıza gidin.

2. Seçin **Azure Search'ü Ekle** altında **BLOB hizmeti**.

3. Seçin **arama hizmeti** içinde **verileri içeri aktarma**ve ardından önceki bölümde oluşturduğunuz arama hizmete tıklayın. Bu açılır **yeni veri kaynağı**.

### <a name="create-a-data-source"></a>Veri kaynağı oluşturma

  Veri kaynağı, dizin oluşturulacak verileri ve verilere nasıl erişileceğini belirtir. Bir arama hizmeti bir veri kaynağını birden fazla kaz kullanabilir.

1. Veri kaynağının adını girin. **Ayıklanacak veri** bölümünde **İçerik ve meta veriler**'i seçin. Veri kaynağı blobun dizin oluşturulacak bölümlerini belirtir.

2. Kullanmakta olduğunuz blobları metin dosyaları olan çünkü **ayrıştırma modu** için **metin**.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/datasources.png)

3. Kullanılabilir depolama hesaplarını listelemek için **Depolama Kapsayıcısı**'nı seçin.

4. Depolama hesabınızı seçin ve ardından daha önce oluşturduğunuz kapsayıcıyı seçin.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/datacontainer.png)

5. Tıklayın **seçin** dönmek için **yeni veri kaynağı**seçip **Tamam** devam etmek için.

### <a name="configure-the-index"></a>Dizini yapılandırma

  Dizin, veri kaynağınızdaki alanların arama yapılabilen bir koleksiyonudur. Ayarlayın ve arama hizmetinizin hangi şekilde verilerinizi aranıp aranmayacağını bilebilmesi bu alanlarda parametrelerini yapılandırın.

1. İçinde **verileri içeri aktarma**seçin **hedef dizini Özelleştir**.

2. **Dizin adı** alanına dizininize vermek istediğiniz adı girin.

3. Seçin **alınabilir** altındaki onay kutusunu özniteliğin **metadata_storage_name**.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/valuestoselect.png)

4. Seçin **Tamam**, hangi getirir **dizin oluşturucu oluşturma**.

Dizininizin parametreleri ve bu parametreler için belirlediğiniz öznitelikler önemlidir. Parametreleri belirtin *ne* depolamak için veri ve özniteliklerini belirtin *nasıl* verileri depolamak için.

**ALAN ADI** sütunu parametreleri içerir. Aşağıdaki tabloda kullanılabilir özniteliklere ve açıklamalarına yer verilmiştir.

#### <a name="field-attributes"></a>Alan öznitelikleri

| Öznitelik | Açıklama |
| --- | --- |
| *Anahtar* |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| *Alınabilir* |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| *Filtrelenebilir* |Alanın filtre sorgularında kullanılmasını sağlar. |
| *Sıralanabilir* |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| *Modellenebilir* |Kullanıcının bağımsız filtrelemesi için modellenmiş bir gezinmede bir alanın kullanılmasını sağlar. Genellikle, Grup belgelere birlikte kullanabileceğiniz yinelemeli değerler içeren alanlar (bir tek bir marka veya hizmet kategorisine denk gelen örneğin, birden çok belge) model olarak en iyi çalışır. |
| *Aranabilir* |Alanı tam metin aranabilir şeklinde işaretler. |

### <a name="create-an-indexer"></a>Dizin oluşturucu oluşturma

  Dizin oluşturucu, veri kaynağı ile bir arama dizini arasında bağlantı kurar ve verilerinizle yeniden dizin oluşturmak için bir zamanlama sunar.

1. **Ad** alanına bir ad girin ve **Tamam**'ı seçin.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/exindexer.png)

2. Size geri almadan **verileri içeri aktarma**. Seçin **Tamam** bağlantı işlemini tamamlayın.

Blobunuzla arama hizmetiniz arasında başarıyla bağlantı kurmuş oldunuz. Portalda dizinin doldurulmuş şekilde görünmesi birkaç dakika sürebilir. Ancak arama hizmeti dizin oluşturmayı hemen başlattığından arama yapmaya doğrudan başlayabilirsiniz.

## <a name="search-your-text-files"></a>Metin dosyalarınızda arama yapma

Dosyalarınızda arama yapmak için yeni oluşturduğunuz arama hizmetinin dizininde arama gezginini açın.

Aşağıdaki adımlarda arama gezginini nerede bulabileceğiniz ve bazı örnek sorgular gösterilmiştir:

1. Tüm kaynaklara gidin ve yeni oluşturulan arama hizmetinizi bulun.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/exampleurl.png)

2. Açmak için dizininizi seçin.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/overview.png)

3. Seçin **arama Gezgini** yapabileceğiniz Canlı sorgular verileriniz üzerinde arama Gezgini'ni açın.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/indexespane.png)

4. Sorgu dizesi alanı boş durumdayken **Ara**'yı seçin. Boş sorgu, bloblarınızdaki *tüm* verileri döndürür.

   ![Yapılandırılmamış arama](media/storage-unstructured-search/emptySearch.png)

### <a name="perform-a-full-text-search"></a>Tam metin araması

Girin **Myopia** içinde **sorgu dizesi** alan ve seçim **arama**. Bu adım, dosyaların içeriğini bir arama başlatır ve "Myopia." sözcüğünü içeren bir alt kümesini, döndürür

  ![Yapılandırılmamış arama](media/storage-unstructured-search/secondMyopia.png)

### <a name="perform-a-system-properties-search"></a>Sistem özellikleri arama yapın

Tam metin arama ek olarak, sistem özelliklerine göre kullanarak arama sorguları oluşturabilirsiniz `$select` parametresi.

ENTER `$select=metadata_storage_name` tuşuna basın ve sorgu dizesi içinde **Enter**. Bu, döndürülecek yalnızca bu belirli alan neden olur.

Sorgu dizesi doğrudan URL'yi değiştirdiğinden boşluk kullanılamaz. Birden fazla alanda arama yapmak için virgül kullanın; örneğin: `$select=metadata_storage_name,metadata_storage_path`

`$select` parametresi yalnızca dizininizi tanımlama aşamasında alınabilir olarak işaretlenen alanlar için kullanılabilir.

  ![Yapılandırılmamış arama](media/storage-unstructured-search/metadatasearchunstructured.png)

Bu öğreticiyi tamamladınız ve elinizde arama yapılabilen bir yapılandırılmamış veri kümesi var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynakları kaldırmak için en kolay yolu kaynak grubunu silmektir. Kaynak grubunu kaldırdığınızda o grubun içindeki tüm kaynaklar da silinir. Aşağıdaki örnekte, kaynak grubu kaldırıldığında depolama hesabı ve kaynak grubunun kendisi de kaldırılır.

1. Azure portalda, aboneliğinizdeki kaynak gruplarını listesine gidin.
2. Silmek istediğiniz kaynak grubunu seçin.
3. Seçin **kaynak grubunu Sil** düğmesine tıklayın ve silme alanında kaynak grubunun adını girin.
4. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Search belgeleri dizin oluşturma hakkında daha fazla bilgi için bu bağlantıyı izleyin:

> [!div class="nextstepaction"]
> [Azure Search ile Azure Blob Depolama’da Belgelerin Dizinini Oluşturma](../../search/search-howto-indexing-azure-blob-storage.md)
