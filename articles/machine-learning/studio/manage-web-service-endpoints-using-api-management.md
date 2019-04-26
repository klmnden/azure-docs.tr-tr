---
title: API Yönetimi'ni kullanarak web hizmetlerini yönetme
titleSuffix: Azure Machine Learning Studio
description: API Management kullanarak AzureML web hizmetlerini yönetmek nasıl gösteren bir kılavuz. Kullanıcı erişimi, kullanımı azaltma ve izleme Panosu tanımlayarak, REST API uç noktalarını yönetin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 11/03/2017
ms.openlocfilehash: 0d79bc167ea0416218a4d4822bcd6221699643ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347388"
---
# <a name="manage-azure-machine-learning-studio-web-services-using-api-management"></a>API Yönetimi'ni kullanarak Azure Machine Learning Studio web hizmetlerini yönetme
## <a name="overview"></a>Genel Bakış
Bu kılavuzda, Azure Machine Learning Studio web hizmetlerinizi yönetmek için API Yönetimi'ni kullanarak hızla gösterilir.

## <a name="what-is-azure-api-management"></a>Azure API Management nedir?
Azure API Management REST API uç noktalarınızın kullanıcı erişimi, kullanımı azaltma ve izleme Panosu tanımlayarak yönetmenizi sağlayan bir Azure hizmetidir. Tıklayın [burada](https://azure.microsoft.com/services/api-management/) Azure API Management hakkında ayrıntılı bilgi için. Tıklayın [burada](/azure/api-management/import-and-publish) Azure API Management'i kullanmaya başlama hakkında bir kılavuz için. Bu kılavuzu temel alır, bu diğer Kılavuzu, ürünler, geliştirici abonelikleri ve kullanım yönelik Kompozit oluşturma bildirimi yapılandırmaları, fiyatlandırma katmanı, yanıt işleme, kullanıcı kimlik doğrulaması dahil olmak üzere diğer konuları kapsar.

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzu tamamlamak için gerekir:

* Bir Azure hesabı. Bir Azure hesabınız yoksa, tıklayın [burada](https://azure.microsoft.com/pricing/free-trial/) ücretsiz bir deneme hesabı oluşturma hakkında ayrıntılar için.
* AzureML hesaptır. AzureML hesabınız yoksa, tıklayın [burada](https://studio.azureml.net/) ücretsiz bir deneme hesabı oluşturma hakkında ayrıntılar için.
* Çalışma alanı, hizmet ve web hizmeti olarak AzureML deneme için apı_key. Tıklayın [burada](create-experiment.md) AzureML deneme oluşturma hakkında ayrıntılar için. Tıklayın [burada](publish-a-machine-learning-web-service.md) bir web hizmeti olarak bir AzureML dağıtma hakkında ayrıntılı bilgi denemek için. Alternatif olarak, ek A basit bir AzureML deneme test oluşturma ve bir web hizmeti olarak dağıtma hakkında yönergeler vardır.

## <a name="create-an-api-management-instance"></a>API Management örneği oluşturma

API Management örneği ile Azure Machine Learning web hizmetini yönetebilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **+ Kaynak oluştur**’u seçin.
3. Arama kutusuna "API Yönetimi" yazın ve ardından "API Yönetimi" kaynağı seçin.
4. **Oluştur**’a tıklayın.
5. **Adı** değerini (Bu örnekte "demoazureml") benzersiz bir URL oluşturmak için kullanılır.
6. Seçin bir **abonelik**, **kaynak grubu**, ve **konumu** hizmet örneğinizin.
7. İçin bir değer belirtin **kuruluş adı** (Bu örnekte "demoazureml" kullanır).
8. Girin, **yönetici e-posta** -API Management sisteminden gelen bildirimler için bu e-posta kullanılır.
9. **Oluştur**’a tıklayın.

Bu, oluşturulacak yeni bir hizmet 30 dakikaya kadar sürebilir.

![hizmet oluşturma](./media/manage-web-service-endpoints-using-api-management/create-service.png)


## <a name="create-the-api"></a>API oluşturma
Hizmet örneği oluşturulduktan sonra sonraki adıma API oluşturmaktır. Bir API, istemci uygulamasından çağrılabilen işlemler grubundan oluşur. API işlemleri mevcut web hizmetlerine taşınır. Bu kılavuz, mevcut AzureML RRS ve BES web hizmetlerine proxy API'leri oluşturur.

API oluşturmak için:

1. Azure portalında, oluşturduğunuz hizmet örneği açın.
2. Sol gezinti bölmesinde seçin **API'leri**.

   ![API yönetimi menüsü](./media/manage-web-service-endpoints-using-api-management/api-management.png)

1. Tıklayın **API ekleme**.
2. Girin bir **Web API adı** (Bu örnekte "AzureML tanıtım API" kullanır).
3. İçin **Web hizmeti URL'si**, girin "`https://ussouthcentral.services.azureml.net`".
4. Girin bir ** Web API URL'si soneki ". Bu, müşterilerin hizmeti örneğine (Bu örnekte "azureml-demo" kullanır) isteklerini göndermek için kullanacağı URL'yi son parçası olur.
5. İçin **Web API'si URL şeması**seçin **HTTPS**.
6. İçin **ürünleri**seçin **başlangıç**.
7. **Kaydet**’e tıklayın.


## <a name="add-the-operations"></a>İşlem ekleme

İşlemleri eklenir ve bir API'ye yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için tıklayın **yayımcı portalı** API Management hizmetiniz için Azure portalında **API'leri**, **Operations**, ardından**Ekleme işlemi**.

![ekleme işlemi](./media/manage-web-service-endpoints-using-api-management/add-an-operation.png)

**Yeni işlem** penceresi görüntülenir ve **imza** sekmesi varsayılan olarak seçilir.

## <a name="add-rrs-operation"></a>RRS işlem ekleme
İlk AzureML RRS hizmeti için bir işlem oluşturun:

1. İçin **HTTP fiili**seçin **POST**.
2. İçin **URL şablonu**, türü "`/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}`".
3. Girin bir **görünen ad** (Bu örnekte, "RRS yürütme" kullanır).

   ![rrs-işlem-imza ekle](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

4. Tıklayın **yanıtları** > **ekleme** seçeneğini ve üzerinde **200 Tamam**.
5. Tıklayın **Kaydet** bu işlem kaydedin.

   ![Ekle-rrs-işlem-yanıtı](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>BES işlem ekleme

> [!NOTE]
> Çok benzer RRS işlemi ekleme olarak ekran görüntüleri burada BES işlemleri için dahil edilmez.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Bir toplu işlem yürütme işi Gönder (ancak başlatma)

1. Tıklayın **ekleme işlemi** BES işlemi API'ye işlem ekleme.
2. İçin **HTTP fiili**seçin **POST**.
3. İçin **URL şablonu**, türü "`/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}`".
4. Girin bir **görünen ad** (Bu örnekte, "BES Gönder" kullanır).
5. Tıklayın **yanıtları** > **ekleme** seçeneğini ve üzerinde **200 Tamam**.
6. **Kaydet**’e tıklayın.

### <a name="start-a-batch-execution-job"></a>Toplu işlem yürütme işi başlatma

1. Tıklayın **ekleme işlemi** BES işlemi API'ye işlem ekleme.
2. İçin **HTTP fiili**seçin **POST**.
3. İçin **HTTP fiili**, türü "`/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}`".
4. Girin bir **görünen ad** (Bu örnekte, "BES Başlat" kullanır).
6. Tıklayın **yanıtları** > **ekleme** seçeneğini ve üzerinde **200 Tamam**.
7. **Kaydet**’e tıklayın.

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a>Durum ya da bir toplu iş yürütmeye ilişkin iş sonucunu Al

1. Tıklayın **ekleme işlemi** BES işlemi API'ye işlem ekleme.
2. İçin **HTTP fiili**seçin **alma**.
3. İçin **URL şablonu**, türü "`/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}`".
4. Girin bir **görünen ad** (Bu örnekte "BES durumu" kullanır).
6. Tıklayın **yanıtları** > **ekleme** seçeneğini ve üzerinde **200 Tamam**.
7. **Kaydet**’e tıklayın.

### <a name="delete-a-batch-execution-job"></a>Bir toplu iş yürütme işini sil

1. Tıklayın **ekleme işlemi** BES işlemi API'ye işlem ekleme.
2. İçin **HTTP fiili**seçin **Sil**.
3. İçin **URL şablonu**, türü "`/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}`".
4. Girin bir **görünen ad** (Bu örnekte, "BES Sil" kullanır).
5. Tıklayın **yanıtları** > **ekleme** seçeneğini ve üzerinde **200 Tamam**.
6. **Kaydet**’e tıklayın.

## <a name="call-an-operation-from-the-developer-portal"></a>Geliştirici portalından işlem çağırma

İşlemler görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Geliştirici portalından çağrılabilir. Bu adımda çağıracaksınız **RRS yürütme** eklenmişse yöntemi **AzureML tanıtım API**. 

1. Tıklayın **Geliştirici Portalı**.

   ![Geliştirici Portalı](./media/manage-web-service-endpoints-using-api-management/developer-portal.png)

2. Tıklayın **API'leri** üst menü ve ardından **AzureML tanıtım API** kullanılabilen işlemleri öğrenin.

   ![demoazureml-api](./media/manage-web-service-endpoints-using-api-management/demoazureml-api.png)

3. Seçin **RRS yürütme** işlemi için. Tıklayın **deneyin**.

   ![BT deneyin](./media/manage-web-service-endpoints-using-api-management/try-it.png)

4. İçin **parametreleri**, türü, **çalışma** ve **hizmet**, tür "için 2.0 **apiversion**, için"doğru"**ayrıntıları**. Bulabilirsiniz, **çalışma** ve **hizmet** AzureML web hizmeti panosundaki (bkz **web hizmetini Test** A ek olarak).

   İçin **istek üst**, tıklayın **üst bilgi Ekle** "Content-Type" ve "application/json" yazın. Tıklayın **üst bilgi Ekle** yeniden ve "Yetkilendirme" yazın ve "taşıyıcı  *\<API ANAHTARINI hizmetiniz\>*". AzureML web hizmeti panosundaki API anahtarı bulabilirsiniz (bkz **web hizmetini Test** A ek olarak).

   İçin **istek gövdesi**, türü `{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}`.

   ![azureml-demo-api](./media/manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

5. Tıklayın **Gönder**.

   ![Gönder](./media/manage-web-service-endpoints-using-api-management/send.png)

Bir işlem çağrıldıktan sonra Geliştirici Portalı görüntüler **istenen URL'ye** arka uç hizmetinden **yanıt durumu**, **yanıt üstbilgilerini**ve tüm  **Yanıt içeriği**.

![yanıt durumu](./media/manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Ek A - oluşturma ve sınama basit AzureML web hizmeti
### <a name="creating-the-experiment"></a>Deneme oluşturma
Aşağıda basit bir AzureML deneme oluşturma ve bir web hizmeti olarak dağıtma adımları verilmiştir. Rastgele metin sütunu giriş ve bir tamsayı olarak temsil edilen bir özellik kümesi döndürür web hizmeti alır. Örneğin:

| Metin | Karma metin |
| --- | --- |
| Bu iyi bir günüdür |1 1 2 2 0 2 0 1 |

Tercih ettiğiniz bir tarayıcıyı kullanarak ilk olarak gidin: [ https://studio.azureml.net/ ](https://studio.azureml.net/) ve oturum açma kimlik bilgilerinizi girin. Ardından, yeni bir boş deneme oluşturun.

![Arama deneme şablonları](./media/manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Yeniden adlandırın **SimpleFeatureHashingExperiment**. Genişletin **kaydedilmiş veri kümeleri** sürükleyin **Amazon kitap incelemelerinden** sürükleyip denemenize.

![Basit-özellik-karma-deneme](./media/manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Genişletin **veri dönüştürme** ve **işleme** sürükleyin **kümesindeki sütunları seçme** sürükleyip denemenize. Connect **incelemeleri Amazon'dan kitap** için **veri kümesinde sütun seçme**.

![Kitap incelemeleri veri kümesi modülü bir proje sütunları modülüne bağlayın](./media/manage-web-service-endpoints-using-api-management/project-columns.png)

Tıklayın **kümesindeki sütunları seçme** ve ardından **Sütun seçiciyi Başlat** seçip **Sütun2**. Bu değişiklikleri uygulamak için onay işaretine tıklayın.

![Sütun adları kullanarak sütunları seçin](./media/manage-web-service-endpoints-using-api-management/select-columns.png)

Genişletin **metin analizi** sürükleyin **özellik karma** denemeyi sürükleyin. Connect **veri kümesinde sütun seçme** için **özellik karma**.

![bağlanma-project-sütunları](./media/manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Tür **3** için **bitsize karma**. Bu 8 (23) oluşturacak sütun.

![karma bitsize](./media/manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Bu noktada, tıklayın isteyebilirsiniz **çalıştırma** denemeyi test etmek için.

![Çalıştırma](./media/manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Web hizmeti oluşturma
Artık bir web hizmeti oluşturun. Genişletin **Web hizmeti** sürükleyin **giriş** sürükleyip denemenize. Connect **giriş** için **özellik karma**. Ayrıca **çıkış** sürükleyip denemenize. Connect **çıkış** için **özellik karma**.

![Çıkış-için-özellik-karma](./media/manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Tıklayın **web hizmeti yayımlama**.

![Yayımlama-web hizmeti](./media/manage-web-service-endpoints-using-api-management/publish-web-service.png)

Tıklayın **Evet** denemeyi yayımlamak için.

![Evet-yayımlama](./media/manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a>Web hizmetini test edin
Bir AzureML web hizmeti, RSS (istek/yanıt hizmeti) ve BES (toplu yürütme hizmeti) uç noktalar oluşur. RSS, zaman uyumlu yürütme için ' dir. BES için zaman uyumsuz iş yürütme ' dir. Aşağıdaki örnek Python kaynağı ile web hizmetini test etmek için indirmek ve Python için Azure SDK'sını yüklemek ihtiyacınız (bakın: [Python yükleme](../../python-how-to-install.md)).

Ayrıca gerekir **çalışma**, **hizmet**, ve **apı_key** denemenizin örnek kaynağı için. Çalışma alanını ve hizmet tıklayarak bulabilirsiniz **istek/yanıt** veya **toplu iş yürütme** web hizmeti panosundaki denemeniz için.

![Bul çalışma ve-hizmet](./media/manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Bulabilirsiniz **apı_key** denemenizi web hizmeti panosundaki tıklayarak.

![bulma API anahtarı](./media/manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Test RRS uç noktası
##### <a name="test-button"></a>Test düğmesi
RRS uç nokta test etmek için kolay bir yolu **Test** web hizmeti panosundaki.

![test](./media/manage-web-service-endpoints-using-api-management/test.png)

Tür **bu iyi bir günüdür** için **Sütun2**. Onay işaretine tıklayın.

![verileri girin](./media/manage-web-service-endpoints-using-api-management/enter-data.png)

Benzer bir şey görürsünüz

![örnek çıktı](./media/manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Örnek Kod
RRS, test etmek için başka bir istemci kodunuz içinden yoludur. Tıklarsanız **istek/yanıt** Pano ve En Alta kadar kaydır için örnek kod göreceğiniz C#, Python ve R'dir İstek URI'si, dahil olmak üzere RRS isteğinin sözdizimi da göreceksiniz üstbilgi ve gövde.

Bu kılavuz, çalışan bir Python örnek gösterir. İle değiştirmeniz gerekecektir **çalışma**, **hizmet**, ve **apı_key** denemenizin.

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
Tıklayın **toplu yürütme** kaydırın ve Pano. Örnek kod için göreceğiniz C#, Python ve R'dir BES istekler bir iş gönderdiniz, bir işi başlatmak, durumu veya bir iş sonuçlarını almak ve bir işi silmek için söz dizimi de görürsünüz.

Bu kılavuz, çalışan bir Python örnek gösterir. İle değiştirmeniz gerekir **çalışma**, **hizmet**, ve **apı_key** denemenizin. Ayrıca, değiştirmenize gerek **depolama hesabı adı**, **depolama hesabı anahtarı**, ve **depolama kapsayıcısı adı**. Son olarak, konumunu değiştirmeniz gerekecektir **giriş dosyası** ve konumunu **çıkış dosyası**.

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
        # If you are using Python 3+, replace urllib2 with urllib.request in the following code
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
