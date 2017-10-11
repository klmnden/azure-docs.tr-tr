## <a name="configure-your-application-to-access-azure-storage"></a>Azure depolama alanına erişmek için uygulamanızı yapılandırın
Uygulamanızı depolama hizmetlerine erişmek için kimliğini doğrulamak için iki yolu vardır:

* Paylaşılan anahtar: Paylaşılan yalnızca test amacıyla anahtarı kullan
* Paylaşılan erişim imzası (SAS): Üretim uygulamaları SAS kullanılmak

### <a name="shared-key"></a>Paylaşılan Anahtar
Paylaşılan anahtar kimlik doğrulaması, uygulamanızın hesap adı ve hesap anahtarı depolama hizmetlerine erişmek için kullanacağı anlamına gelir. Hızlı bir şekilde bu kitaplığının nasıl kullanıldığını gösteren amacıyla, biz bu Başlarken, paylaşılan anahtar kimlik doğrulaması kullanır.

> [!WARNING] 
> **Yalnızca test amacıyla paylaşılan anahtar kimlik doğrulaması kullanın!** Hesap adı ve hesap anahtarı, ilişkili depolama hesabına tam okuma/yazma erişimi vermek, uygulamanızı indirmeleri her kişiye dağıtılacaktır. Bu **değil** güvenilmeyen istemciler tarafından tehlikeye anahtarınızı sahip risk gibi iyi bir uygulamadır.
> 
> 

Paylaşılan anahtar kimlik doğrulaması kullanırken, oluşturacağınız bir [bağlantı dizesi](../articles/storage/common/storage-configure-connection-string.md). Bağlantı dizesi oluşur:  

* **DefaultEndpointsProtocol** -HTTP veya HTTPS seçebilirsiniz. Ancak, HTTPS kullanılması önerilir.
* **Hesap adı** -depolama hesabınızın adını
* **Hesap anahtarı** - üzerinde [Azure Portal](https://portal.azure.com), depolama hesabınıza gidin ve tıklayın **anahtarları** bu bilgileri bulmak için simge.
* (İsteğe bağlı) **EndpointSuffix** -bu Azure Çin ya da Azure yönetimi gibi farklı uç nokta sonekleri ile bölgelerdeki depolama hizmetleri için kullanılır.

Burada, paylaşılan anahtar kimlik doğrulaması kullanarak bağlantı dizesi örneği verilmiştir:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Paylaşılan Erişim İmzaları (SAS)
Bir mobil uygulama için bir paylaşılan erişim imzası (SAS) kullanarak Azure Storage hizmeti karşı bir istemci tarafından bir istek kimlik doğrulaması için önerilen yöntem gerçekleşir. SAS bir belirtilen süre, belirtilen bir izin kümesi ile bir istemci bir kaynağa erişim izni vermesini sağlar.
Depolama hesabı sahibi kullanmak mobil istemcileriniz için bir SAS oluşturmanız gerekir. SAS oluşturmak için büyük olasılıkla istemcilerinize dağıtılacak SAS oluşturur ayrı bir hizmet yazma istersiniz. Test amacıyla kullanabilirsiniz [Microsoft Azure Storage Gezgini](http://storageexplorer.com) veya [Azure Portal](https://portal.azure.com) SAS oluşturmak için. SAS oluşturduğunuzda, SAS geçerli olduğu zaman aralığını ve istemciye SAS verir izinleri belirtebilirsiniz.

Aşağıdaki örnek, Microsoft Azure Storage Gezgini SAS oluşturmak için nasıl kullanılacağını gösterir.

1. Henüz yapmadıysanız [Microsoft Azure Storage Gezgini yükleyin](http://storageexplorer.com)
2. Aboneliğinize bağlanın.
3. Depolama hesabınıza tıklayın ve alt sol "Eylemler" sekmesini tıklatın. "Paylaşılan erişim"bağlantı dizesi"için SAS oluşturmak için imzası Al"'i tıklatın.
4. Burada, verir okuma ve yazma izinlerine hizmeti, kapsayıcı ve nesne düzeyinde depolama hesabının blob hizmeti için bir SAS bağlantı dizesi örneği verilmiştir.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Bir SAS kullanırken görebileceğiniz gibi hesap anahtarınızı uygulamanızda gösterme değil. SAS ve SAS kullanıma kullanarak için en iyi uygulamalar hakkında daha fazla bilgiyi [paylaşılan erişim imzaları: SAS modelini anlama](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

