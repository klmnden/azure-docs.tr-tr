---
title: REST kullanarak bir Azure Media Services hesabına dosya yükleme | Microsoft Docs
description: Medya içeriği oluşturma ve karşıya yükleme varlıklar Media Services'e almayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: b7583a0fda2fca0d8ff80879389b824a7b352a84
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752881"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>REST kullanarak bir Media Services hesabına dosya yükleme  
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

Media Services’de dijital dosyalar bir varlığa yüklenir. [Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) varlığı video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler.) içerebilir  Dosyalar bir varlığa yüklendiğinde, içeriğiniz sonraki işleme ve akışla için bulutta güvenli bir şekilde depolanır. 

Bu öğreticide, bir dosya ve onunla ilişkili başka bir işlem karşıya yüklemeyi öğrenin:

> [!div class="checklist"]
> * Karşıya yükleme işlemleri için Postman'ı ayarlama
> * Media Services’e bağlanmak 
> * Yazma izni olan bir erişim ilkesi oluşturma
> * Bir varlık oluşturun
> * SAS Bulucunun oluşturma ve karşıya yükleme URL'si oluşturun
> * Bir dosya karşıya yükleme URL'yi kullanarak blob depolamaya yükleme
> * Bir meta veri karşıya yüklediğiniz medya dosyasının varlığı oluşturun

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden geçirme [AAD kimlik doğrulamasına genel bakış ile Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md) makalesi.
- Yapılandırma **Postman** açıklandığı [Postman yapılandırmak için Media Services REST API çağrıları](media-rest-apis-with-postman.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Media Services REST API kullanırken aşağıdaki maddeler geçerlidir:
 
* Media Services REST API'si kullanarak varlıkları erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md). <br/>Bu öğreticide Postman koleksiyonunun tüm gerekli üst bilgileri ayarını üstlenir.
* Media Services IAssetFile.Name özelliğinin değeri, URL'leri akış içeriği için (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmez. Değerini **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' dosya adı uzantısı için.
* Adının uzunluğu 260 karakterden uzun olmamalıdır.
* Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için [bu](media-services-quotas-and-limitations.md) makaleye bakın.

## <a name="set-up-postman"></a>Postman’i ayarlama

Bu öğretici için Postman'ı ayarlama adımları için bkz: [Postman yapılandırma](media-rest-apis-with-postman.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

1. Bağlantı değerleri ortamınıza ekleyin. 

    Parçası olan bazı değişkenler **MediaServices** [ortam](postman-environment.md) tanımlanan işlem yürütülürken başlamadan önce el ile ayarlanması gereken [koleksiyon](postman-collection.md).

    İlk beş değişkenleri değerleri almak için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-env.png)
2. Değer için **MediaFileName** ortam değişkeni.

    Karşıya yüklemek için planlama medya dosyası adını belirtin. Bu örnekte, BigBuckBunny.mp4 karşıya yüklemek için kullanacağız. 
3. İnceleme **AzureMediaServices.postman_environment.json** dosya. Neredeyse tüm toplama işlemlerinde "test" bir betik yürütmek görürsünüz. Komut tarafından bir yanıt döndürdü ve uygun ortam değişkenlerini ayarlamak bazı değerleri alın.

    Örneğin, ilk işlem bir erişim belirteci alır ve açık ayarının **AccessToken** diğer tüm işlemlerde kullanılan ortam değişkeni.

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
4. Sol tarafında **Postman** penceresinde tıklayarak **1. AAD kimlik doğrulaması belirteci alma** -> **Azure AD belirteci Al için hizmet sorumlusu**.

    URL bölümü ile doldurulan **AzureADSTSEndpoint** ortam değişkeni (öğreticide daha önce koleksiyonu destekleyen ortam değişkenlerinin değerlerini ayarladığınız).

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postment-get-token.png)

5. **Gönder**’e basın.

    "Access_token" içeren yanıt görebilirsiniz. Bu değeri "test" komut dosyasını alır ve ayarlar **AccessToken** ortam değişkeni (yukarıda açıklandığı gibi). Ortam değişkenlerini incelemek, bu değişken artık işlemleri geri kalanında kullanılan erişim belirteci (taşıyıcı belirteci) değeri içeren görürsünüz. 

    Belirtecin süresi dolarsa "Alma Azure AD belirteç için hizmet sorumlusu" adımlayın tekrar gidin. 

## <a name="create-an-access-policy-with-write-permission"></a>Yazma izni olan bir erişim ilkesi oluşturma

### <a name="overview"></a>Genel Bakış 

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Tüm dosyaları blob depolama alanına karşıya yüklemeden önce erişim yazmak için bir varlık için haklar ilkesi ayarlayın. Bunu yapmak için AccessPolicies varlık kümesi için bir HTTP isteği gönderin. Oluşturma sonrasında bir dakika Cinsiden Süre değer tanımlama veya 500 İç sunucu hatası iletiye yanıt olarak alırsınız. AccessPolicies hakkında daha fazla bilgi için bkz. [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

### <a name="create-an-access-policy"></a>Erişim ilkesi oluşturma

1. Seçin **AccessPolicy** -> **AccessPolicy oluşturmak için karşıya yükleme**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-access-policy.png)

    "Test" betik AccessPolicy kimliğini alır ve uygun bir ortam değişkenini ayarlar.

## <a name="create-an-asset"></a>Bir varlık oluşturun

### <a name="overview"></a>Genel Bakış

Bir [varlık](https://docs.microsoft.com/rest/api/media/operations/asset) birden çok türleri veya Media Services, video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil olmak üzere nesne kümeleri için bir kapsayıcıdır. REST API'SİNDE bir varlık oluşturmak için Media Services için POST isteği gönderme ve istek gövdesinde, varlık hakkında herhangi bir özelliği bilgi yerleştirme gerektirir.

Bir varlık oluştururken ekleyebilirsiniz özelliklerinden biri **seçenekleri**. Aşağıdaki şifreleme seçeneklerden birini belirtebilirsiniz: **Hiçbiri** (varsayılan, şifreleme kullanılır) **StorageEncrypted** (için depolama istemci tarafı şifreleme ile önceden şifrelenmiş içeriğe), **CommonEncryptionProtected**, veya  **EnvelopeEncryptionProtected**. Şifrelenmiş bir varlık varsa, teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [varlık teslim ilkelerini yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

Varlığınız şifrelendiyse oluşturmalısınız bir **ContentKey** ve varlığınız için aşağıdaki makalede anlatıldığı gibi Bağla: [Bir ContentKey oluşturma](media-services-rest-create-contentkey.md). Dosyaları varlığa yükleyin sonra şifreleme özelliklerini güncelleştirmek gereken **AssetFile** varlık sırasında aldığınız değerlerle **varlık** şifreleme. Bunu kullanarak **birleştirme** HTTP isteği. 

Bu örnekte, şifrelenmemiş bir varlık oluşturuyoruz. 

### <a name="create-an-asset"></a>Bir varlık oluşturun

1. Seçin **varlıklar** -> **varlık oluşturma**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-asset.png)

    "Test" betik varlık kimliği alır ve uygun bir ortam değişkenini ayarlar.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>SAS Bulucunun oluşturma ve karşıya yükleme URL'si oluşturun

### <a name="overview"></a>Genel Bakış

Bulucu ayarlayın ve AccessPolicy aldıktan sonra gerçek dosyayı Azure depolama REST API'leri kullanarak bir Azure Blob Depolama kapsayıcısına karşıya yüklendi. Blok blobları olarak dosyaları yüklemeniz gerekir. Sayfa blobları, Azure Media Services tarafından desteklenmez.  

Azure depolama blobları ile çalışma hakkında daha fazla bilgi için bkz. [Blob hizmeti REST API'si](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Gerçek yükleme URL'si almak için (aşağıda gösterilen) bir SAS Bulucu oluşturun. Bulucular bir varlık, dosyalara erişmek istediğiniz istemciler için başlangıç saati ve bağlantı uç noktası türünü tanımlayın. Farklı istemci isteklerini ve ihtiyaçlarınızı işlemek verilen AccessPolicy ve varlık çifti için birden çok Bulucu varlık oluşturabilirsiniz. Bu Bulucuyu her StartTime değerinin yanı sıra AccessPolicy Dakika Cinsiden Süre değerini bir URL kullanılabilir sürenin uzunluğunu belirlemek için kullanır. Daha fazla bilgi için [Bulucu](https://docs.microsoft.com/rest/api/media/operations/locator).

Bir SAS URL'si aşağıdaki biçime sahiptir:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bazı dikkate alınması gereken noktalar vardır:

* Belirli bir varlık ile tek seferde ilişkilendirilen beşten fazla benzersiz Bulucu sayısı sahip olamaz. Daha fazla bilgi için Konum Belirleyicisi bakın.
* Hemen dosyalarınızı karşıya yüklemek ihtiyacınız varsa, geçerli saatten önce beş dakika, StartTime değeri ayarlamanız gerekir. Olabilir saat, istemci makinesi ile Media Services arasında eğriltme olmasıdır. Ayrıca, StartTime değeri, şu tarih saat biçiminde olmalıdır: YYYY-AA-ssZ (örneğin, "2014-05-23T17:53:50Z").    
* 30-40 ikinci olabilir kullanıma hazır olduğunda bir Bulucu için oluşturulduktan sonra gecikme.

### <a name="create-a-sas-locator"></a>Bir SAS Bulucu

1. Seçin **Bulucu** -> **SAS Bulucu**.
2. **Gönder**’e basın.

    "Test" betik "Belirttiğiniz medya dosyasının adına dayalı URL karşıya yükle" ve SAS Bulucu bilgileri oluşturur ve uygun bir ortam değişkenini ayarlar.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Bir dosya karşıya yükleme URL'yi kullanarak blob depolamaya yükleme

### <a name="overview"></a>Genel Bakış

Yükleme URL'si olduğuna göre doğrudan SAS kapsayıcıya dosyanızı karşıya yüklemek için Azure Blob API'lerini kullanarak bazı kod yazmanız gerekir. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure depolama REST API'sini kullanma](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [İkili büyük nesne KOYMA](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [BLOB'lar, Blob depolamaya yükleme](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Postman ile bir dosya karşıya yükleme

Örneğin, bir küçük .mp4 dosyasını karşıya yüklemek için Postman kullanın. Karşıya Postman aracılığıyla ikili üzerinde dosya boyutu sınırı olabilir.

Karşıya yükleme isteğini değil parçası **AzureMedia** koleksiyonu. 

Oluşturun ve yeni bir isteği ayarlayın:
1. Tuşuna **+** , yeni bir istek sekmesi oluşturma.
2. Seçin **PUT** işlemi ve Yapıştır **{{UploadURL}}** URL.
2. Bırakın **yetkilendirme** sekme olarak (Bu ayarlanmamışsa **taşıyıcı belirteci**).
3. İçinde **üstbilgileri** sekmesinde, belirtin: **Anahtar**: "x-ms-blob-type" ve **değer**: "BlockBlob".
2. İçinde **gövdesi** sekmesinde **ikili**.
4. Belirtilen ada sahip dosyayı seçin **MediaFileName** ortam değişkeni.
5. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Bir meta veri varlığı oluşturma

Dosya yüklendikten sonra varlık, varlıkla ilişkili blob depolamaya yüklediğiniz medya dosyası için bir meta verileri oluşturmak gerekir.

1. Seçin **AssetFiles** -> **CreateFileInfos**.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-file-info.png)

Dosya karşıya yüklenmelidir ve meta verileri ayarlayın.

## <a name="validate"></a>Doğrulama

Dosya başarıyla karşıya yüklendi, sorgu isteyebileceğiniz doğrulamak için [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) ve karşılaştırma **ContentFileSize** (veya diğer ayrıntıları) içinde yeni varlık görmeyi beklediğiniz için. 

Örneğin, aşağıdaki **alma** işlemi, varlık dosyası için dosya verileri getirir (ya da büyük/küçük harf, BigBuckBunny.mp4 dosyası). Sorgu kullanarak [ortam değişkenlerini](postman-environment.md) daha önce belirlediğiniz.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Yanıt boyutu, adı ve diğer bilgileri içerir.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

