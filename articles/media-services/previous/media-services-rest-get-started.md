---
title: REST kullanarak isteğe bağlı içerik göndermeye başlama | Microsoft Docs
description: Bu öğreticide REST API kullanarak Azure Media Services ile bir üzerinde isteğe bağlı içerik teslim uygulaması gerçekleştirilmesinin adımları açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: fea9bae9fadc20622a6ca3d2e08db9cd3a92c800
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523996"
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a>REST kullanarak isteğe bağlı içerik göndermeye başlama  

[!INCLUDE [media-services-selector-get-started](../../../includes/media-services-selector-get-started.md)]

Bu hızlı başlangıçta, Azure Media Services (AMS) REST API'lerini kullanarak bir isteğe bağlı video (VoD) içerik teslim uygulaması gerçekleştirilmesinin adımları gösterilmektedir.

Öğretici, temel Media Services iş akışını ve Media Services geliştirmek için gereken en genel programlama nesnelerini ve görevleri tanıtır. Öğretici tamamlandığında, akışla aktarmak veya aşamalı yüklenmiş kodlanmış ve indirilen örnek bir medya dosyasını indirmek kullanabilirsiniz.

Aşağıdaki resimde Media Services OData modeliyle VoD uygulamaları geliştirirken en sık kullanılan nesnelerin bazıları gösterilmektedir.

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a>Önkoşullar
REST API'leri ile Media Services ile geliştirmeye başlamak için aşağıdaki önkoşullar gereklidir.

* Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Bir Media Services hesabı. Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-portal-create-account.md).
* Media Services REST API'si ile geliştirme anlama. Daha fazla bilgi için [Media Services REST API'sine genel bakış](media-services-rest-how-to-use.md).
* Bir uygulama ettiğiniz HTTP isteklerini ve yanıtlarını gönderebilirsiniz. Bu öğreticide [Fiddler](https://www.telerik.com/download/fiddler).

Bu hızlı başlangıçta, aşağıdaki görevleri gösterilir.

1. Akış uç noktalarını başlatın (Azure portalını kullanarak).
2. REST API ile Media Services hesabına bağlanın.
3. Yeni bir varlık oluşturun ve REST API'si ile bir video dosyası yükleyin.
4. Kaynak dosyayı Uyarlamalı bit hızı MP4 dosyaları REST API ile kodlayın.
5. Varlık yayımlama ve akış ve aşamalı indirme URL'lerini alın REST API ile.
6. İçeriğinizi oynatın.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Aynı günleri / erişim izinlerini, örneğin, ilkeleri kalmasına yerinde uzun bir süredir (karşıya yükleme olmayan ilkeler) yöneliktir bulucular için her zaman aynı ilke Kimliğini kullanın. Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Bu makalede kullanılan AMS REST varlıklar hakkında daha fazla ayrıntı için bkz: [Azure Media Services REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference). Ayrıca bkz [Azure Media Services kavramları](media-services-concepts.md).

>[!NOTE]
>Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="start-streaming-endpoints-using-the-azure-portal"></a>Azure portal ile akış uç noktalarını başlatma

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri bit hızı Uyarlamalı akış ile video sağlıyor. Media Services, bu akış biçimlerinin her birinin önceden paketlenmiş sürümlerini depolamanıza gerek kalmadan, uyarlamalı bit hızı MP4 ile kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış) tam vaktinde göndermenize olanak tanıyan dinamik paketleme özelliğine sahiptir.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir.

Akış uç noktasını başlatmak için aşağıdakileri yapın:

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. Ayarlar penceresinde, Akış uç noktaları'na tıklayın.
3. Varsayılan akış uç noktasına tıklayın.

    VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILARI penceresi görüntülenir.

4. Başlat simgesine tıklayın.
5. Yaptığınız değişiklikleri kaydetmek için Kaydet düğmesine tıklayın.

## <a id="connect"></a>REST API ile Media Services hesabına bağlanma

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a id="upload"></a>Yeni bir varlık oluşturun ve REST API'si ile bir video dosyasını karşıya yükleyin

Media Services’de dijital dosyalar bir varlığa yüklenir. **Varlık** varlığı video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler.) içerebilir  Dosyalar bir varlığa yüklendiğinde, içeriğiniz sonraki işleme ve akışla için bulutta güvenli bir şekilde depolanır.

Bir varlık oluştururken sağlamanız gereken değerlerin varlık oluşturma seçeneklerden biridir. **Seçenekleri** özelliği bir sabit listesi değeri bir varlık ile oluşturulan şifreleme seçeneklerini açıklar. Geçerli bir değer değil Bu listeden değerlerinin bir birleşimini aşağıdaki listeden değerlerden biri:

* **Hiçbiri** = **0** -şifreleme kullanılmaz. Bu seçeneği tercih edildiğinde, içeriğinizi Aktarımdaki veya depolama bölgesinde, bekleyen korunmaz.
    Aşamalı indirme kullanarak bir MP4 iletmeyi planlıyorsanız bu seçeneği kullanın.
* **StorageEncrypted** = **1** - clear içeriğinizi AES-256 bit şifreleme kullanarak yerel olarak şifreler ve bunu Azure bunu depolandığı bekleme sırasında şifrelenmiş depolama alanına yükler. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıktı varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama Şifrelemesinin birincil kullanım nedeni, yüksek kaliteli girdi medya dosyalarınızın güvenliğini güçlü şifrelemeyle diskte bekleyen konumda sağlamak istediğiniz durumdur.
* **CommonEncryptionProtected** = **2** -zaten şifrelenmiş ve ortak şifreleme veya PlayReady DRM (örneğin, kesintisiz akış ile korunan içerik yüklüyorsanız bu seçeneği kullanın. PlayReady DRM korumalı).
* **EnvelopeEncryptionProtected** = **4** : AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyalar kodlanmış ve şifrelenmiş Transform Manager tarafından gerekir.

### <a name="create-an-asset"></a>Bir varlık oluşturun
Bir varlık, birden fazla türleri veya Media Services, video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil olmak üzere nesne kümeleri için bir kapsayıcıdır. REST API'SİNDE bir varlık oluşturmak için Media Services için POST isteği gönderme ve istek gövdesinde, varlık hakkında herhangi bir özelliği bilgi yerleştirme gerektirir.

Aşağıdaki örnek, bir varlık oluşturma işlemi gösterilmektedir.

**HTTP isteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


**HTTP yanıtı**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a>Bir AssetFile oluşturma
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) varlığı, bir blob kapsayıcısında depolanan bir video veya ses dosyasını temsil eder. Bir varlık dosyası her zaman bir varlıkla ilişkilidir ve bir varlık, bir veya daha çok AssetFiles içerebilir. Bir varlık dosyası nesne bir blob kapsayıcısında dijital bir dosyayla ilişkili değilse, medya Hizmetleri Kodlayıcı görev başarısız olur.

Bir blob kapsayıcıya dijital medya dosyanızı karşıya yüklenmesinin ardından, kullandığınız **birleştirme** (sonraki konuda gösterildiği gibi), medya dosyası hakkındaki bilgilerle AssetFile güncelleştirmeye yönelik HTTP isteği.

**HTTP isteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP yanıtı**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Yazma izni olan AccessPolicy oluşturma
Tüm dosyaları blob depolama alanına karşıya yüklemeden önce erişim yazmak için bir varlık için haklar ilkesi ayarlayın. Bunu yapmak için AccessPolicies varlık kümesi için bir HTTP isteği gönderin. Oluşturma sonrasında bir dakika Cinsiden Süre değer tanımlama veya 500 İç sunucu hatası iletiye yanıt olarak alırsınız. AccessPolicies hakkında daha fazla bilgi için bkz. [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

Aşağıdaki örnek, bir AccessPolicy oluşturma işlemi gösterilmektedir:

**HTTP isteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

**HTTP yanıtı**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>Karşıya yükleme URL'sini alma

Gerçek yükleme URL'si almak için bir SAS Bulucu oluşturun. Bulucular bir varlık, dosyalara erişmek istediğiniz istemciler için başlangıç saati ve bağlantı uç noktası türünü tanımlayın. Farklı istemci isteklerini ve ihtiyaçlarınızı işlemek verilen AccessPolicy ve varlık çifti için birden çok Bulucu varlık oluşturabilirsiniz. Bu Bulucuyu her StartTime değerinin yanı sıra AccessPolicy Dakika Cinsiden Süre değerini bir URL kullanılabilir sürenin uzunluğunu belirlemek için kullanır. Daha fazla bilgi için [Bulucu](https://docs.microsoft.com/rest/api/media/operations/locator).

Bir SAS URL'si aşağıdaki biçime sahiptir:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Bazı dikkate alınması gereken noktalar vardır:

* Belirli bir varlık ile tek seferde ilişkilendirilen beşten fazla benzersiz Bulucu sayısı sahip olamaz. 
* Hemen dosyalarınızı karşıya yüklemek ihtiyacınız varsa, geçerli saatten önce beş dakika, StartTime değeri ayarlamanız gerekir. Olabilir saat, istemci makinesi ile Media Services arasında eğriltme olmasıdır. Ayrıca, StartTime değeri, şu tarih saat biçiminde olmalıdır: YYYY-AA-ssZ (örneğin, "2014-05-23T17:53:50Z").    
* 30-40 ikinci olabilir kullanıma hazır olduğunda bir Bulucu için oluşturulduktan sonra gecikme. Bu sorun, her ikisi için de geçerlidir [SAS URL'si](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) ve Kaynak Konum Belirleyicisi.

Aşağıdaki örnek, Type özelliği (bir SAS Bulucu için "1") ve bir isteğe bağlı kaynak konum belirleyicisi "2" istek gövdesinde tanımlanan bir SAS URL'si Bulucusu oluşturmak nasıl gösterir. **Yolu** dosyanızı karşıya yüklemek için kullanmanız gereken URL'yi döndürülen özelliğini içerir.

**HTTP isteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP yanıtı**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Bir dosyayı blob depolama kapsayıcısına karşıya yükleyin
Bulucu ayarlayın ve AccessPolicy aldıktan sonra gerçek dosyayı Azure depolama REST API'leri kullanarak bir Azure blob depolama kapsayıcısına karşıya yüklendi. Blok blobları olarak dosyaları yüklemeniz gerekir. Sayfa blobları, Azure Media Services tarafından desteklenmez.  

> [!NOTE]
> Bulucu için karşıya yüklemek istediğiniz dosyasının dosya adı eklemelisiniz **yolu** önceki bölümde alınan değer. Örneğin, `https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?`.
>
>

Azure depolama blobları ile çalışma hakkında daha fazla bilgi için bkz. [Blob hizmeti REST API'si](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

### <a name="update-the-assetfile"></a>Güncelleştirme AssetFile
Dosyanızı yüklediğinize göre FileAsset boyutunu (ve diğer) bilgileri güncelleştirin. Örneğin:

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP yanıtı**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

## <a name="delete-the-locator-and-accesspolicy"></a>Bulucu ve AccessPolicy Sil
**HTTP isteği**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP yanıtı**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

**HTTP isteği**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP yanıtı**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

## <a id="encode"></a>Kaynak dosyayı Uyarlamalı bit hızı MP4 dosyaları kümesine kodlayın

Önce Media Services, medya varlıklarına kodlanmış başlayan kümeniz, medyaya, kodlama vb. sonra istemcilere teslim edilir. Bu etkinlikler, yüksek performans ve kullanılabilirlik sağlamak için birden fazla arka plan rol örneğinde zamanlanır ve çalıştırılır. Bu etkinliklere işler adı verilir ve her bir iş varlık dosyası üzerinde asıl işi yapan atomik görevlerden oluşur (daha fazla bilgi için [iş](https://docs.microsoft.com/rest/api/media/operations/job), [görev](https://docs.microsoft.com/rest/api/media/operations/task) açıklamaları).

İle Azure Media Services en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı Uyarlamalı akış iletmektir çalışırken daha önce belirtildiği gibi. Media Services dinamik olarak Uyarlamalı bit hızı MP4 dosyaları kümesini şu biçimlerden birine paketleyebilir: HTTP canlı akış (HLS), kesintisiz akış, MPEG DASH.

Aşağıdaki bölümde, bir kodlama görevi içeren işi oluşturma işlemi gösterilmektedir. Görev e kodlamasını Ara dosyayı Uyarlamalı bit hızlı MP4'ı kullanarak bir küme içine belirtir **Media Encoder Standard**. Bölüm işleme ilerleme durumu işi izlemek nasıl de gösterir. İş tamamlandıktan sonra varlıklarınızı erişmek için gerekli olan bulucular oluşturmak olacaktır.

### <a name="get-a-media-processor"></a>Medya işlemcisi Al
Media Services'da Medya işleyicisi kodlama, biçim dönüştürme, şifreleme ve şifresini çözmeyi medya içeriği gibi ek olarak, belirli bir işleme görevi işleyen bir bileşenidir. Bu öğreticide gösterilen kodlama görevi için Media Encoder Standard kullanmak için kullanacağız.

Aşağıdaki kod Kodlayıcısı'nın kimliği ister.

**HTTP isteği**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP yanıtı**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Bir iş oluşturma
Bir veya daha fazla görevi gerçekleştirmek istediğiniz işleme türüne bağlı olarak her bir iş olabilir. REST API aracılığıyla, işlerini ve görevlerini ilgili iki yoldan biriyle oluşturabilirsiniz: OData toplu işlem veya iş varlıkları görevleri gezinti özelliği aracılığıyla tanımlı satır içi görevleri olabilir. Media Services SDK'sı, toplu işlem kullanır. Ancak, bu makaledeki kod örnekleri okunabilirlik açısından, satır içi olarak tanımlanan görevlerdir. Toplu işlem hakkında daha fazla bilgi için bkz: [açık veri Protokolü (OData) toplu işleme](https://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Aşağıdaki örnek nasıl oluşturulacağı ve bir görev belirli bir çözümleme ve kaliteli video kodlamak için ayarlama işlemiyle sonrası gösterir. Aşağıdaki bir belge bölümü tüm listesini içeren [görev ön ayarları](https://msdn.microsoft.com/library/mt269960) Media Encoder Standard işlemcisi tarafından desteklenir.  

**HTTP isteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP yanıtı**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


Herhangi bir işi isteğinin dikkat edilecek bazı önemli noktalar vardır:

* TaskBody özellikleri değişmez değer XML giriş sayısını belirtin veya çıkış görev tarafından kullanılan varlıklar için kullanmanız gerekir. Görev makale XML için XML şema tanımı içerir.
* Her iç TaskBody tanımında değeri `<inputAsset>` ve `<outputAsset>` JobInputAsset(value) veya JobOutputAsset(value) olarak ayarlanmalıdır.
* Bir görev birden fazla çıktı varlığına sahip olabilirsiniz. Bir JobOutputAsset(x) yalnızca bir kez bir işteki bir görevin çıktı olarak kullanılabilir.
* Bir görevin Giriş bir varlık JobInputAsset veya JobOutputAsset belirtebilirsiniz.
* Görevleri bir döngü form değil.
* JobInputAsset veya JobOutputAsset için geçirdiğiniz değer parametresi için bir varlık dizin değerini temsil eder. Gerçek varlıkları işi varlık tanımı InputMediaAssets ve OutputMediaAssets Gezinti özellikleri tanımlanır.

> [!NOTE]
> Media Services OData v3 tasarlandığından, tek tek varlıklar InputMediaAssets ve OutputMediaAssets gezinti özelliği koleksiyonlarında aracılığıyla başvurulan bir "__metadata: URI" ad-değer çifti.
>
>

* Media Services içinde oluşturduğunuz bir veya daha fazla varlıklara InputMediaAssets eşler. OutputMediaAssets sistem tarafından oluşturulur. Var olan bir varlık başvurusu değil.
* OutputMediaAssets assetName özniteliğini kullanarak yeniden adlandırılabilir. Bu öznitelik yoksa sonra herhangi bir iç metin değerini OutputMediaAsset adıdır `<outputAsset>` bir sonek iş adı değeri ya da iş kimliği değeri (Name özelliği olmayan tanımlandığı durumda) öğesidir. Örneğin, "SAMPLE" assetName için bir değer ayarlarsanız, OutputMediaAsset Name özelliği "Örneği için" ayarlanır. Ancak, assetName için bir değer ayarlı değil, ancak "NewJob" proje adı ayarlayın, OutputMediaAsset adı "JobOutputAsset (değer) _NewJob" olacaktır.

    Aşağıdaki örnek assetName özniteliğinin nasıl ayarlanacağı gösterilmektedir:

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* Görev zinciri oluşturmayı etkinleştirmek için:

  * Bir işi, en az iki görevi olmalıdır
  * İşin başka bir görev çıktısını olan giriştir en az bir görev olmalıdır.

Daha fazla bilgi edinmek, [Media Services REST API'si ile bir kodlama işi oluşturma](media-services-rest-encode-asset.md).

### <a name="monitor-processing-progress"></a>İşleme ilerleme durumu İzleyicisi
Aşağıdaki örnekte gösterildiği gibi durumu özelliğini kullanarak iş durumunu alabilirsiniz:

**HTTP isteği**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP yanıtı**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Bir işi iptal et
Media Services aracılığıyla CancelJob işlevi çalışan işleri iptal sağlar. Bu çağrı, bir iş durumuna iptal edildiğinde İptal denerseniz, 400 hata kodu, iptal etme, hata döndürür veya tamamlanmış.

Aşağıdaki örnek nasıl CancelJob çağrılacağını gösterir.

**HTTP isteği**

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net


Başarılı olursa, hiçbir ileti gövdesi ile bir 204 yanıtı kodu döndürülür.

> [!NOTE]
> İş kimliği URL olarak kodlamanız gerekir (normalde nb:jid:UUID: in değeri birdeğer) içinde CancelJob için bir parametre olarak geçirerek olduğunda.
>
>

### <a name="get-the-output-asset"></a>Çıktı varlığına Al
Aşağıdaki kod, çıktı varlık kimliği istemek nasıl gösterir

**HTTP isteği**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP yanıtı**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }

## <a id="publish_get_urls"></a>Varlık yayımlama ve akış ve aşamalı indirme URL'lerini alın REST API ile

Bir varlığı akışla aktarmak veya indirmek için söz konusu varlığı önce bir bulucu oluşturarak “yayımlamak” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services iki tür bulucuyu destekler: Akış medya (örneğin, MPEG DASH, HLS veya kesintisiz akış) ve medya dosyalarını indirmek için kullanılan erişim imzası (SAS) bulucuları için kullanılan OnDemandOrigin bulucuları. 

Bulucuları oluşturduktan sonra akış veya dosyaları indirmek için kullanılan URL'leri oluşturabilirsiniz.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir.

Kesintisiz Akışa ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS’ye ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH’e ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Dosya indirmek için kullanılan bir SAS URL'si aşağıdaki biçime sahiptir:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Bu bölümde, "varlıklarınızı yayımlamak" gerekli aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.  

* Salt okunur izinle AccessPolicy oluşturma
* İçerik indirmek için bir SAS URL'si oluşturma
* İçerik akışı yapmak için bir kaynak URL'sini oluşturma

### <a name="creating-the-accesspolicy-with-read-permission"></a>Salt okunur izinle AccessPolicy oluşturma
İndirme veya herhangi bir medya içeriği akışı önce ilk Okuma izinleri olan bir AccessPolicy tanımlayın ve teslim mekanizması, istemcileriniz için etkinleştirmek istediğiniz türünü belirten uygun Bulucu varlık oluşturun. Kullanılabilir özellikleri hakkında daha fazla bilgi için bkz. [AccessPolicy varlık özellikleri](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).

Aşağıdaki örnek, belirli bir varlık için Okuma izinleri AccessPolicy belirtmek gösterilmektedir.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Başarılı olursa, oluşturduğunuz AccessPolicy varlığı açıklayan 201 başarı kodu döndürülür. Ardından AccessPolicy kimliği Bulucu varlık oluşturmak için (çıkış varlık gibi) sunmak istediğiniz dosyayı içeren varlığı varlık kimliği ile birlikte kullanabilirsiniz.

> [!NOTE]
> Bu temel iş akışı (Bu konunun önceki kısımlarında anlatıldığı) bir varlığı almak, bir dosyayı karşıya yüklemeyi aynıdır. (Veya istemcilerinize) hemen dosyalarınıza erişmek ihtiyacınız varsa, ayrıca, dosyaları, karşıya yükleme gibi StartTime değeri beş dakika geçerli saatten önce ayarlayın. Çünkü istemci ve Media Services arasında eğriltme saat olabilir bu eylem gerekli değildir. StartTime değeri, şu tarih saat biçiminde olmalıdır: YYYY-AA-ssZ (örneğin, "2014-05-23T17:53:50Z").
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a>İçerik indirmek için bir SAS URL'si oluşturma
Aşağıdaki kod oluşturulur ve daha önce yüklenmiş bir medya dosyasını indirmek için kullanılabilecek bir URL almak nasıl gösterir. AccessPolicy ayarlanmış izinleri okuduğundan ve bir SAS indirme URL'sine Bulucu yolunu gösterir.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }

Döndürülen **yolu** özelliği, SAS URL'sini içerir.

> [!NOTE]
> Şifrelenmiş depolama içeriği karşıdan yüklerseniz, gerekir el ile işlemeden önce şifresini veya depolama şifre çözme MediaProcessor işleme görevde çıkış için bir OutputAsset açıkta işlenen dosyalarını ve ardından bu varlığından indirmek için kullanın. Media Services REST API'si ile bir kodlama işi oluşturma işleme hakkında daha fazla bilgi için bkz. Ayrıca, SAS URL'sini Bulucular oluşturulduktan sonra güncelleştirilemiyor. Örneğin, güncelleştirilmiş bir StartTime değeri ile aynı Bulucu yeniden kullanamazsınız. Oluşturulan tüm SAS URL'lerini şekli nedeniyle budur. Bulucunun süresi dolduktan sonra indirmek için bir varlık erişmek istiyorsanız, yeni startTime değeri ile yeni bir tane oluşturmanız gerekir.
>
>

### <a name="download-files"></a>Dosyaları indirme
Bulucu ayarlayın ve AccessPolicy aldıktan sonra Azure depolama REST API'lerini kullanarak dosyaları indirebilir.  

> [!NOTE]
> Bulucu için indirmek istediğiniz dosya için dosya adı eklemelisiniz **yolu** önceki bölümde alınan değer. Örneğin, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .

Azure depolama blobları ile çalışma hakkında daha fazla bilgi için bkz. [Blob hizmeti REST API'si](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Önceki (Uyarlamalı MP4 kümesine kodlaması) gerçekleştirilen kodlama işinin sonucu olarak, aşamalı olarak indirebilirsiniz çoklu MP4 dosyaları sahip. Örneğin:    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

### <a name="creating-a-streaming-url-for-streaming-content"></a>İçerik akışı yapmak için bir akış URL'si oluşturma
Aşağıdaki kod, bir akış URL'si Bulucusu oluşturma işlemi gösterilmektedir:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Bir akış medya yürütücüsü kesintisiz akış kaynak URL'de akışını sağlamak için kesintisiz akış adını özelliğiyle bildirim dosyası, ardından "/ MANIFEST" yolunu eklemeniz gerekir.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS akış için URL'ye (format = m3u8-aapl) sonra "/ MANIFEST".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH akış için URL'ye (format mpd-time-CSF) sonra "/ MANIFEST".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>İçeriğinizi oynatma
Videonuzu akışla aktarmak için [Azure Media Services Oynatıcısı](https://amsplayer.azurewebsites.net/azuremediaplayer.html)’nı kullanın.

Aşamalı indirmeyi test etmek için bir URL (örneğin, IE, Chrome, Safari) bir tarayıcıya yapıştırın.

## <a name="next-steps-media-services-learning-paths"></a>Sonraki Adımlar: Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
