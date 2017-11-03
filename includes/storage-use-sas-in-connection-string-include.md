Bir depolama hesabındaki kaynaklara erişim veren bir paylaşılan erişim imzası (SAS) URL sahip değilse, SAS bağlantı dizesinde kullanabilirsiniz. SAS istek kimliğini doğrulamak için gereken bilgileri içerdiğinden, SAS bağlantı dizesiyle protokolü, hizmet uç noktası ve kaynağa erişmek için gerekli kimlik bilgilerini sağlar.

Paylaşılan erişim imzası içeren bir bağlantı dizesi oluşturmak için dizesi şu biçimde belirtin:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Her hizmet uç noktası isteğe bağlı olsa en az bir bağlantı dizesi içermelidir.

> [!NOTE]
> HTTPS ile SAS kullanarak bir en iyi uygulama olarak önerilir.
>
> Yapılandırma dosyasında bir bağlantı dizesi bir SAS belirtiyorsanız URL'de bulunan özel karakterleri kodlayın gerekebilir.
>
>

### <a name="service-sas-example"></a>Hizmet SAS örneği
Burada, Blob storage için hizmet SAS içeren bir bağlantı dizesi örneği verilmiştir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

Ve özel karakterler kodlama ile aynı bağlantı dizesini bir örneği burada verilmiştir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Hesap SAS örneği
Burada, Blob ve dosya depolama için hesap SAS içeren bir bağlantı dizesi örneği verilmiştir. Her iki hizmet için uç noktalar belirttiğiniz dikkat edin:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

Ve URL kodlaması ile aynı bağlantı dize örneği şöyledir:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

