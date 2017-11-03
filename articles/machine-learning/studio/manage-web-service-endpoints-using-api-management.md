---
title: "AzureML web hizmetleri API Management kullanarak yönetmeyi öğrenin | Microsoft Docs"
description: "API Management kullanarak AzureML web hizmetlerini yönetmek nasıl gösteren bir kılavuzdur."
keywords: "machine Learning, API Yönetimi"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 53a6b18fb74db46ccb66c7c70851a9bf364e927c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>API Yönetimini kullanarak AzureML web hizmetlerini yönetme hakkında bilgi edinin
## <a name="overview"></a>Genel Bakış
Bu kılavuz hızla AzureML web hizmetleri yönetmek için API Yönetimi'ni kullanarak nasıl başlayacağınızı gösterir.

## <a name="what-is-azure-api-management"></a>Azure API Management nedir?
Azure API Management REST API uç noktalarınızı kullanıcı erişimi, kullanım azaltma ve Pano izleme tanımlayarak yönetmenizi sağlayan bir Azure hizmetidir. Tıklatın [burada](https://azure.microsoft.com/services/api-management/) Azure API Management hakkında ayrıntılı bilgi için. Tıklatın [burada](../../api-management/api-management-get-started.md) Azure API Management ile çalışmaya başlama konusunda Kılavuzu. Bu kılavuz temel alır, bu diğer Kılavuzu, ürünler, geliştirici abonelikleri ve kullanım dashboarding oluşturma bildirim yapılandırmaları, fiyatlandırma katmanı, yanıt işleme, kullanıcı kimlik doğrulaması dahil olmak üzere daha fazla konuları kapsar.

## <a name="what-is-azureml"></a>AzureML nedir?
AzureML kolayca oluşturun, dağıtın ve Gelişmiş analiz çözümleri paylaşmak olanak tanıyan machine learning için bir Azure hizmetidir. Tıklatın [burada](https://azure.microsoft.com/services/machine-learning/) AzureML hakkında ayrıntılı bilgi için.

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzu tamamlamak için gerekir:

* Bir Azure hesabı. Bir Azure hesabınız yoksa tıklatın [burada](https://azure.microsoft.com/pricing/free-trial/) ücretsiz bir deneme hesabı oluşturma hakkında ayrıntılı bilgi için.
* AzureML hesaptır. AzureML hesabınız yoksa tıklatın [burada](https://studio.azureml.net/) ücretsiz bir deneme hesabı oluşturma hakkında ayrıntılı bilgi için.
* Çalışma alanı, hizmet ve bir web hizmeti olarak dağıtılan bir AzureML deneme apı_key. Tıklatın [burada](create-experiment.md) AzureML deneme oluşturma hakkında ayrıntılı bilgi için. Tıklatın [burada](publish-a-machine-learning-web-service.md) bir web hizmeti olarak bir AzureML dağıtma hakkında ayrıntılar denemek için. Alternatif olarak, ek A oluşturma ve basit bir AzureML deneme sınamak ve bir web hizmeti olarak dağıtma hakkında yönergeler vardır.

## <a name="create-an-api-management-instance"></a>API Management örneği oluşturma
AzureML web hizmetini yönetmek için API Management'i kullanmaya yönelik adımlar aşağıda verilmiştir. İlk olarak bir hizmet örneği oluşturun. Oturum [Klasik Portal](https://manage.windowsazure.com/) tıklatıp **yeni** > **uygulama hizmetleri** > **API Management** > **oluşturma**.

![Örnek Oluştur](./media/manage-web-service-endpoints-using-api-management/create-instance.png)

Benzersiz bir belirtin **URL**. Bu kılavuzu kullanır **demoazureml** – farklı bir şey seçmeniz gerekecektir. Hizmet örneğiniz için istediğiniz **Abonelik** ve **Bölge**’yi seçin. Seçimlerinizi yaptıktan sonra İleri düğmesine tıklayın.

![oluşturma-service-1](./media/manage-web-service-endpoints-using-api-management/create-service-1.png)

İçin bir değer belirtin **kuruluş adı**. Bu kılavuzu kullanır **demoazureml** – farklı bir şey seçmeniz gerekecektir. E-posta adresinizi girin **yönetici e-posta** alan. Bu e-posta adresi API Management sisteminden gelen bildirimler için kullanılır.

![oluşturma-service-2](./media/manage-web-service-endpoints-using-api-management/create-service-2.png)

Hizmet örneğinizi oluşturmak için bu onay kutusuna tıklayın. *Oluşturulacak yeni bir hizmet otuz dakika kadar sürdüğünü*.

## <a name="create-the-api"></a>API oluşturma
Hizmet örneği oluşturulduktan sonra sonraki adıma API oluşturmaktır. Bir API, istemci uygulamasından çağrılabilen işlemler grubundan oluşur. API işlemleri mevcut web hizmetlerine taşınır. Bu kılavuz API'leri varolan AzureML RRS ve BES web hizmetlerine proxy oluşturur.

API oluşturulur ve klasik Azure Portalı aracılığıyla erişilen API yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için hizmet örneği seçin.

![Select hizmet örneği](./media/manage-web-service-endpoints-using-api-management/select-service-instance.png)

Tıklatın **Yönet** API Management hizmetiniz için Klasik Azure Portalı'nda.

![yönetme hizmeti](./media/manage-web-service-endpoints-using-api-management/manage-service.png)

Tıklatın **API'leri** gelen **API Management** sol menüsünde ve ardından **API Ekle**.

![API yönetimi menüsü](./media/manage-web-service-endpoints-using-api-management/api-management-menu.png)

Tür **AzureML Demo API** olarak **Web API adı**. Tür **https://ussouthcentral.services.azureml.net** olarak **Web hizmeti URL'si**. Tür **azureml demo** olarak **Web API'si URL soneki**. Denetleme **HTTPS** olarak **Web API'si URL** düzeni. Seçin **Starter** olarak **ürünleri**. Tamamlandığında tıklatarak **kaydetmek** API oluşturmak için.

![Yeni-API Ekle](./media/manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-the-operations"></a>İşlem ekleme
Tıklatın **ekleme işlemi** bu API işlemleri eklemek için.

![ekleme işlemi](./media/manage-web-service-endpoints-using-api-management/add-operation.png)

**Yeni işlem** penceresi görüntülenir ve **imza** sekmesi varsayılan olarak seçilir.

## <a name="add-rrs-operation"></a>RRS işlemi ekleyin
İlk AzureML RRS hizmeti için bir işlem oluşturun. Seçin **POST** olarak **HTTP fiili**. Türü **/workspaces/ {çalışma} /services/ {hizmeti} / execute? api-version {apiversion} = & Ayrıntıları {ayrıntıları} =** olarak **URL şablon**. Tür **RRS yürütme** olarak **görünen adı**.

![rrs-işlemi-imza ekle](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Tıklatın **yanıtları** > **ekleme** seçin ve sol **200 Tamam**. Tıklatın **kaydetmek** bu işlemi kaydetmek için.

![Add-rrs işlemi-yanıt](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>BES işlemleri ekleme
Çok benzer RR işlemi ekleme olarak ekran görüntüleri BES işlemleri için dahil edilmez.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Bir toplu iş yürütme iş gönderme (ancak başlatılmamasına)
Tıklatın **ekleme işlemi** API için AzureML BES işlemi eklemek için. Seçin **POST** için **HTTP fiili**. Türü **/workspaces/ {çalışma} /services/ {hizmeti} / işleri? api-version = {apiversion}** için **URL şablon**. Tür **BES gönderme** için **görünen adı**. Tıklatın **yanıtları** > **ekleme** seçin ve sol **200 Tamam**. Tıklatın **kaydetmek** bu işlemi kaydetmek için.

### <a name="start-a-batch-execution-job"></a>Bir toplu iş yürütme işi Başlat
Tıklatın **ekleme işlemi** API için AzureML BES işlemi eklemek için. Seçin **POST** için **HTTP fiili**. Türü **/workspaces/ {çalışma} /services/ {hizmeti} /jobs/ {JobId} / start? api-version = {apiversion}** için **URL şablon**. Tür **BES Başlat** için **görünen adı**. Tıklatın **yanıtları** > **ekleme** seçin ve sol **200 Tamam**. Tıklatın **kaydetmek** bu işlemi kaydetmek için.

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a>Durum veya toplu iş yürütme iş sonucu alın
Tıklatın **ekleme işlemi** API için AzureML BES işlemi eklemek için. Seçin **almak** için **HTTP fiili**. Tür **/workspaces/ {çalışma} /services/ {hizmeti} /jobs/ {JobId}? api-version = {apiversion}** için **URL şablon**. Tür **BES durum** için **görünen adı**. Tıklatın **yanıtları** > **ekleme** seçin ve sol **200 Tamam**. Tıklatın **kaydetmek** bu işlemi kaydetmek için.

### <a name="delete-a-batch-execution-job"></a>Bir toplu iş yürütme iş Sil
Tıklatın **ekleme işlemi** API için AzureML BES işlemi eklemek için. Seçin **silmek** için **HTTP fiili**. Tür **/workspaces/ {çalışma} /services/ {hizmeti} /jobs/ {JobId}? api-version = {apiversion}** için **URL şablon**. Tür **BES silmek** için **görünen adı**. Tıklatın **yanıtları** > **ekleme** seçin ve sol **200 Tamam**. Tıklatın **kaydetmek** bu işlemi kaydetmek için.

## <a name="call-an-operation-from-the-developer-portal"></a>Geliştirici Portalı'ndan bir işlem çağırma
İşlemleri görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Geliştirici portalından çağrılabilir. Bu kılavuzu adımda çağıracaksınız **RRS yürütme** eklendi yöntemi **AzureML Demo API**. Tıklatın **Geliştirici Portalı** üst menüsünde Klasik portalın sağ.

![Geliştirici Portalı](./media/manage-web-service-endpoints-using-api-management/developer-portal.png)

Tıklatın **API'leri** üst menüsünden ve ardından **AzureML Demo API** kullanılabilir işlemleri görmek için.

![demoazureml API](./media/manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Seçin **RRS yürütme** işlemi için. Tıklatın **deneyin**.

![try-BT](./media/manage-web-service-endpoints-using-api-management/try-it.png)

İstek parametreleri için yazın, **çalışma**, **hizmet**, **2.0** için **apiversion**, ve **true** için **ayrıntıları**. Bulabilirsiniz, **çalışma** ve **hizmet** AzureML web hizmeti panosundaki (bkz **web hizmetini sınama** ek A içinde).

İstek üstbilgilerini için tıklatın **Ekle üstbilgi** ve türü **Content-Type** ve **uygulama/json**, ardından **Ekle üstbilgi** ve türü **yetkilendirme** ve **taşıyıcı <YOUR AZUREML SERVICE API-KEY>** . Bulabilirsiniz, **API anahtarı** AzureML web hizmeti panosundaki (bkz **web hizmetini sınama** ek A içinde).

Tür **{"Girişler": {"input1": {"ColumnNames": "Değerlerini" ["Col2"]: [["Bu bir iyi günüdür"]]}}, "GlobalParameters": {}}** istek gövdesi için.

![azureml demo API](./media/manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Tıklatın **Gönder**.

![Gönder](./media/manage-web-service-endpoints-using-api-management/send.png)

Bir işlem çağrıldıktan sonra Geliştirici Portalı görüntüler **istenen URL** arka uç hizmetinden **yanıt durumu**, **yanıt üstbilgilerini**ve tüm **yanıt içeriği**.

![yanıt durumu](./media/manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Ek A - web hizmeti oluşturma ve basit bir AzureML test etme
### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıda basit bir AzureML deneme oluşturma ve bir web hizmeti olarak dağıtma adımları verilmiştir. Farklı bir sütun rastgele metin giriş ve bir tamsayı olarak temsil özellik kümesi döndürür web hizmeti alır. Örneğin:

| Metin | Karma metin |
| --- | --- |
| Bu iyi bir günüdür |1 1 2 2 0 2 0 1 |

İlk olarak, tercih ettiğiniz bir tarayıcı kullanarak gidin için: [https://studio.azureml.net/](https://studio.azureml.net/) ve oturum açma kimlik bilgilerinizi girin. Ardından, yeni bir boş deneme oluşturun.

![Arama deneme şablonları](./media/manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

İçin yeniden adlandırmadan **SimpleFeatureHashingExperiment**. Genişletme **kaydedilen veri kümeleri** ve sürükleyin **Kitap incelemeleri Amazon gelen** denemenizi üzerine.

![Basit-özellik-karma-deneme](./media/manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Genişletme **veri dönüştürme** ve **işleme** ve sürükleyin **Select Columns in Dataset sütun** denemenizi üzerine. Bağlanma **incelemeler Amazon kitap** için **sütun kümesinde seçin**.

![select-columns](./media/manage-web-service-endpoints-using-api-management/project-columns.png)

Tıklatın **Select Columns in Dataset sütun** ve ardından **başlatma Sütun seçiciyi** seçip **Col2**. Bu değişiklikleri uygulamak için onay işaretine tıklayın.

![select-columns](./media/manage-web-service-endpoints-using-api-management/select-columns.png)

Genişletme **metin analizi** ve sürükleyin **özellik karma** deneme üzerine. Bağlantı **sütun kümesinde seçin** için **özellik karma**.

![Connect-proje-sütunlar](./media/manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Tür **3** için **bitsize karma**. Bu 8 (23) oluşturacak sütun.

![karma bitsize](./media/manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Bu noktada, tıklatın isteyebilirsiniz **çalıştırmak** deneme test etmek için.

![çalıştırma](./media/manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Web hizmeti oluşturma
Şimdi bir web hizmeti oluşturun. Genişletme **Web hizmeti** ve sürükleyin **giriş** denemenizi üzerine. Bağlantı **giriş** için **özellik karma**. Ayrıca sürükleyin **çıkış** denemenizi üzerine. Bağlantı **çıkış** için **özellik karma**.

![Çıktı-için-özellik-karma](./media/manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Tıklatın **web hizmeti yayımlama**.

![Yayımlama-web hizmeti](./media/manage-web-service-endpoints-using-api-management/publish-web-service.png)

Tıklatın **Evet** deneme yayımlanacağını.

![Yayımlama Evet](./media/manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a>Web hizmetini sınama
AzureML web hizmeti RSS (istek/yanıt hizmeti) ve BES (toplu yürütme hizmeti) uç noktaları oluşur. RSS için zaman uyumlu yürütme ' dir. BES için zaman uyumsuz iş yürütme ' dir. Aşağıdaki örnek Python kaynağı, web hizmetiyle test etmek için indirme ve Python için Azure SDK'sını yüklemek gerekebilir (bkz: [Python yüklemek nasıl](../../python-how-to-install.md)).

Ayrıca gerekir **çalışma**, **hizmet**, ve **apı_key** denemenizi aşağıdaki örnek kaynağı için. Ya da tıklatarak çalışma ve hizmet bulabilirsiniz **istek/yanıt** veya **toplu iş yürütme** web hizmeti panosundaki denemeniz için.

![Bul çalışma-ve-hizmet](./media/manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Bulabileceğiniz **apı_key** denemenizi web hizmeti panosundaki tıklatarak.

![Bul API anahtarı](./media/manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Test RRS uç noktası
##### <a name="test-button"></a>Test düğmesi
Tıklattığınızdan RR endpoint test etmek için kolay bir yoludur **Test** web hizmeti panosundaki.

![test etme](./media/manage-web-service-endpoints-using-api-management/test.png)

Tür **bu iyi bir günüdür** için **col2**. Onay işaretine tıklayın.

![veri girme](./media/manage-web-service-endpoints-using-api-management/enter-data.png)

Şöyle görürsünüz

![örnek çıktı](./media/manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Örnek kod
RRS test etmek için başka bir istemci kodunuzdan yoldur. Tıklatırsanız **istek/yanıt** Pano ve aşağısına kaydırın, C#, Python ve r için örnek kod görürsünüz İstek URI'si, dahil olmak üzere RRS isteği ayrıca görürsünüz üstbilgi ve gövde.

Bu kılavuz, çalışan bir Python örneği gösterir. İle değiştirmeniz gerekecektir **çalışma**, **hizmet**, ve **apı_key** denemenizi biri.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>Test BES uç noktası
Tıklatın **toplu yürütme** Pano ve aşağısına kaydırın. C#, Python ve r için örnek kod görürsünüz Ayrıca, bir işi göndermek, bir işi başlatmak, durum ya da bir iş sonuçlarını almak ve bir işi silmek için BES istekleri söz dizimi görürsünüz.

Bu kılavuz, çalışan bir Python örneği gösterir. İle değişiklik yapmanız **çalışma**, **hizmet**, ve **apı_key** denemenizi biri. Ayrıca, değişiklik yapmanız **depolama hesabı adı**, **depolama hesabı anahtarı**, ve **depolama kapsayıcısı adı**. Son olarak, konumunu değiştirmeniz gerekecektir **giriş dosyası** ve konumunu **çıktı dosyası**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
