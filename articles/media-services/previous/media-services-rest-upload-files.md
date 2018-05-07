---
title: "Dosyaları REST kullanarak bir Azure Media Services hesabına veri yükleme | Microsoft Docs"
description: "Oluşturma ve karşıya varlıklar Media Services'e medya içeriği alma hakkında bilgi."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: juliako
ms.openlocfilehash: 4ba6fdcec8d71326b02d71dbad429be8c2052171
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Dosyaları REST kullanarak bir Media Services hesabına veri yükleme
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

Media Services’de dijital dosyalar bir varlığa yüklenir. [Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) varlık içerebilir video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta verileri.)  Dosyaları varlığa yüklendiğinde, içeriğiniz sonraki işleme ve akışla için bulutta güvenli bir şekilde depolanır. 

Bu öğreticide, bir dosya ve onunla ilişkili başka bir işlem karşıya öğrenin:

> [!div class="checklist"]
> * Postman karşıya yükleme işlemleri için ayarlama
> * Media Services’e bağlanmak 
> * Yazma izni olan bir erişim ilkesi oluşturma
> * Bir varlık oluşturun
> * Bir SAS Bulucu oluşturun ve karşıya yükleme URL'si oluşturun
> * Bir dosyayı karşıya yükleme URL'yi kullanarak blob depolama alanına karşıya yüklemek
> * Varlığı karşıya yüklediğiniz medya dosyasının bir meta veri oluşturma

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Azure portalını kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden geçirme [AAD kimlik doğrulamasına genel bakış ile Azure Media Services API erişme](media-services-use-aad-auth-to-access-ams-api.md) makalesi.
- Yapılandırma **Postman** açıklandığı gibi [Postman yapılandırmak için Media Services REST API çağrıları](media-rest-apis-with-postman.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Media Services REST API kullanırken aşağıdaki maddeler geçerlidir:
 
* Media Services REST API kullanarak varlıkları erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md). <br/>Bu öğreticide kullanılan Postman koleksiyonu alır gereken tüm üstbilgileri gerçekleşmiş olur.
* Media Services IAssetFile.Name özelliğinin değeri, URL akış içeriğini (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmiyor. Değeri **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' dosya adı uzantısı için.
* Adının uzunluğu 260 karakterden uzun olmamalıdır.
* Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Bkz: [bu](media-services-quotas-and-limitations.md) dosya boyutu sınırlaması hakkındaki ayrıntılar için makale.

## <a name="set-up-postman"></a>Postman ayarlayın

Bu öğretici için Postman ayarlama adımları için bkz: [yapılandırma Postman](media-rest-apis-with-postman.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

1. Bağlantı değerleri ortamınıza ekleyin. 

    Parçası olan bazı değişkenler **MediaServices** [ortam](postman-environment.md) tanımlanan işlemleri yürütülürken başlamadan önce el ile ayarlanması gereken [koleksiyonu](postman-collection.md).

    İlk beş değişkenleri için değerleri almak için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-env.png)
2. Değeri belirtin **MediaFileName** ortam değişkeni.

    Karşıya yüklemek için planlama medya dosya adını belirtin. Bu örnekte, biz BigBuckBunny.mp4 karşıya alınacaktır. 
3. İncelemek **AzureMediaServices.postman_environment.json** dosya. Neredeyse tüm işlemleri koleksiyonundaki "test" komut dosyası yürütme görürsünüz. Komut dosyaları tarafından bir yanıt döndürdü ve uygun ortam değişkenlerini ayarlama bazı değerler alır.

    Örneğin, ilk işlem bir erişim belirteci alan ve ayarlandığını **AccessToken** diğer tüm işlemlerinde kullanılan ortam değişkeni.

    ```    
    "listen": "test",
    "script": {
        "type": "text/javascript",
        "exec": [
            "var json = JSON.parse(responseBody);",
            "postman.setEnvironmentVariable(\"AccessToken\", json.access_token);"
        ]
    }
    ```
4. Sol tarafındaki **Postman** penceresinde, tıklatıldığında **1. AAD kimlik doğrulama belirteci alma** -> **alma Azure AD belirteci için hizmet asıl**.

    URL bölümü doldurulup **AzureADSTSEndpoint** (değeri, bu öğreticide daha önce) ortam değişkenini ayarla.
    
5. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postment-get-token.png)

    "Access_token" içeren yanıt görebilirsiniz. Bu değer "test" komut dosyası alır ve ayarlar **AccessToken** (yukarıda açıklandığı gibi) ortam değişkeni. Ortam değişkenlerini incelerseniz, bu değişken şimdi işlemleri geri kalanı kullanılan erişim belirteci (taşıyıcı belirteci) değeri içeren görürsünüz. 

    Belirtecin süresi dolarsa "Get Azure AD belirteci için hizmet asıl" adım yeniden gidin. 

## <a name="create-an-access-policy-with-write-permission"></a>Yazma izni olan bir erişim ilkesi oluşturma

### <a name="overview"></a>Genel Bakış 

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Blob depolama alanına herhangi bir dosya karşıya yüklemeden önce erişim için bir varlık yazma İlkesi hakları ayarlayın. Bunu yapmak için AccessPolicies varlık kümesi için bir HTTP isteği gönderin. Oluşturulduktan sonra bir dakika Cinsiden Süre değer tanımlama veya yanıt olarak bir 500 İç sunucu hata iletisi alırsınız. AccessPolicies hakkında daha fazla bilgi için bkz: [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

### <a name="create-an-access-policy"></a>Erişim ilkesi oluşturma

1. Seçin **AccessPolicy** -> **karşıya yükleme için AccessPolicy oluşturma**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-access-policy.png)

    "Test" komut dosyası AccessPolicy kimliğini alır ve uygun ortam değişkenini ayarlar.

## <a name="create-an-asset"></a>Bir varlık oluşturun

### <a name="overview"></a>Genel Bakış

Bir [varlık](https://docs.microsoft.com/rest/api/media/operations/asset) birden çok türleri veya Media Services, video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil olmak üzere nesne kümeleri için bir kapsayıcıdır. REST API bir varlık oluşturmak için Media Services POST isteği gönderme ve istek gövdesinde Varlığınızı ilgili herhangi bir özellik bilgi yerleştirme gerekir.

Bir varlık oluştururken ekleyebilirsiniz özellikleri biri **seçenekleri**. Aşağıdaki şifreleme seçeneklerden birini belirtebilirsiniz: **hiçbiri** (varsayılan, şifreleme kullanılır) **StorageEncrypted** (istemci-tarafı depolama şifrelemesi ile önceden şifrelenmiş olduğu içerik için) **CommonEncryptionProtected**, veya **EnvelopeEncryptionProtected**. Şifrelenmiş bir varlık olduğunda teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için bkz: [varlık teslim ilkeleri yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

Varlığınızı şifrelenmişse, oluşturmalısınız bir **ContentKey** ve aşağıdaki makalesinde açıklandığı gibi varlık Bağla: [bir ContentKey oluşturma](media-services-rest-create-contentkey.md). Dosyaları varlığa yükleme sonra şifreleme özellikleri güncelleştirmek gereken **AssetFile** aldığınız sırasında değerlerle varlık **varlık** şifreleme. Bunu kullanarak **birleştirme** HTTP isteği. 

Bu örnekte, şifrelenmemiş bir varlık oluşturuyoruz. 

### <a name="create-an-asset"></a>Bir varlık oluşturun

1. Seçin **varlıklar** -> **varlık oluşturma**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-asset.png)

    "Test" komut dosyası varlık kimliğini alır ve uygun ortam değişkenini ayarlar.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>Bir SAS Bulucu oluşturun ve karşıya yükleme URL'si oluşturun

### <a name="overview"></a>Genel Bakış

Bulucu ayarlamak ve AccessPolicy olduktan sonra gerçek dosya Azure Storage REST API'lerini kullanarak bir Azure Blob Storage kapsayıcısı yüklenir. Blok blobları dosyaları yüklemeniz gerekir. Sayfa bloblarını Azure Media Services tarafından desteklenmiyor.  

Azure depolama BLOB'ları ile çalışma hakkında daha fazla bilgi için bkz: [Blob hizmeti REST API'si](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Gerçek yükleme URL'si almak için (aşağıda gösterilen) bir SAS Bulucu oluşturun. Bulucular bir varlık içindeki dosyalara erişmek istediğiniz istemciler için başlangıç saatini ve bağlantı uç noktasının türünü tanımlayın. Farklı istemci isteklerini gereksinimlerini karşılamak belirli bir AccessPolicy ve varlık çifti için birden çok Bulucu varlık oluşturabilirsiniz. Her bu Bulucuyu StartTime değerinin yanı sıra AccessPolicy Dakika Cinsiden Süre değerinin bir URL kullanılabilir süreyi belirlemek için kullanır. Daha fazla bilgi için bkz: [Bulucu](https://docs.microsoft.com/rest/api/media/operations/locator).

Bir SAS URL'si aşağıdaki biçime sahiptir:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bazı dikkate alınması gereken noktalar vardır:

* Aynı anda belirli bir varlıkla ilişkilendirilen beşten fazla benzersiz Bulucular sahip olamaz. Daha fazla bilgi için Bulucu bakın.
* Dosyalarınızı hemen karşıya gerekiyorsa, geçerli tarihten önce beş dakika StartTime değeri ayarlamanız gerekir. Bu; çünkü istemci makine ve Media Services arasında eğme saat olabilir. Ayrıca, StartTime değeri aşağıdaki tarih saat biçiminde olmalıdır: YYYY-MM-: ssZ (örneğin, "2014-05-23T17:53:50Z").    
* 30-40 saniyenin olması için bir Bulucu kullanılabilir olduğunda oluşturulduktan sonra gecikme.

### <a name="create-a-sas-locator"></a>SAS Bulucu oluşturun

1. Seçin **Bulucu** -> **SAS Bulucu oluşturmanız**.
2. **Gönder**’e basın.

    "Test" komut dosyası "Belirttiğiniz medya dosyasının adını temel alarak URL karşıya yükle" ve SAS Bulucu bilgileri oluşturur ve uygun ortam değişkenini ayarlar.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Bir dosyayı karşıya yükleme URL'yi kullanarak blob depolama alanına karşıya yüklemek

### <a name="overview"></a>Genel Bakış

Karşıya yükleme URL'si sahip olduğunuza göre doğrudan SAS kapsayıcıya dosyanızı karşıya yüklemek için Azure Blob API'lerini kullanarak biraz kod yazmanız gerekir. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Storage REST API kullanma](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [BLOB YERLEŞTİRME](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [Blob depolama alanına BLOB yükleme](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Postman bir dosyayı karşıya

Örnek olarak, bir küçük .mp4 dosyayı karşıya yüklemeyi Postman kullanın. İkili Postman aracılığıyla karşıya dosya boyutu sınırı olabilir.

Karşıya yükleme isteği değil parçası **AzureMedia** koleksiyonu. 

Oluşturun ve yeni bir isteği ayarlamak:
1. Tuşuna  **+** , yeni bir istek sekme oluşturmak için.
2. Seçin **PUT** işlemi ve Yapıştır **{{UploadURL}}** URL.
2. Bırakın **yetkilendirme** sekmesinde olduğundan (Bu ayarlanmamışsa **taşıyıcı belirteci**).
3. İçinde **üstbilgileri** sekmesinde, belirtin: **anahtar**: "x-ms-blob-type" ve **değeri**: "BlockBlob".
2. İçinde **gövde** sekmesini tıklatın, **ikili**.
4. Belirtilen ada sahip bir dosya seçin **MediaFileName** ortam değişkeni.
5. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Bir meta veri varlığı oluşturun

Dosya karşıya sonra varlık, varlıkla ilişkilendirilen blob depolama alanına karşıya medya dosyasının içindeki bir meta veri oluşturmanız gerekir.

1. Seçin **AssetFiles** -> **CreateFileInfos**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-file-info.png)

Dosyayı karşıya yüklenmelidir ve meta verilerini ayarlayın.

## <a name="validate"></a>Doğrulama

Dosyası başarıyla karşıya yüklendi, sorgu isteyebilirsiniz doğrulamak için [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) ve karşılaştırma **ContentFileSize** (veya diğer ayrıntıları) yeni varlık görmeyi beklediğiniz için. 

Örneğin, aşağıdaki **almak** işlemi, varlık dosyası için dosya verileri getirir (ya da durumda BigBuckBunny.mp4 dosyası). Sorgu kullanarak [ortam değişkenleri](postman-environment.md) , daha önce ayarlayın.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Yanıt boyutu, adı ve diğer bilgileri içerir.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

