---
title: "Azure Hızlı Başlangıcı - Ruby kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: "Ruby kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı kısa sürede öğrenin"
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 12/7/2017
ms.author: v-ruogun
ms.openlocfilehash: 3b0bc01047b9aa7459cf6cc33f004cf7506e5826
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-ruby"></a>Ruby kullanarak nesneleri Azure Blob depolama içine/dışına aktarma
Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için Ruby'yi nasıl kullanabileceğinizi öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için: 
* [Ruby](https://www.ruby-lang.org/en/downloads/)'yi yükleyin
* rubygem paketini kullanarak [Ruby için Azure Depolama kitaplığını](https://docs.microsoft.com/azure/storage/blobs/storage-ruby-how-to-use-blob-storage#configure-your-application-to-access-storage) yükleyin. 

```
gem install azure-storage
```

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:
Bu [hızlı başlangıçta](https://github.com/Azure-Samples/storage-blobs-ruby-quickstart.git) kullanılan örnek uygulama, temel bir Ruby uygulamasıdır.  

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-ruby-quickstart.git 
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Örnek Ruby uygulamasını indirmek için storage-blobs-ruby-quickstart klasörünün içindeki example.rb dosyasını açın.  

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Uygulamada, uygulamanız için `Client` örneği oluşturmak amacıyla depolama hesabı adınızı ve hesap anahtarınızı sağlamanız gerekir. IDE'nizdeki Çözüm Gezgini'nde `example.rb` dosyasını açın. **accountname** ve **accountkey** değerlerini, hesap adınız ve anahtarınızla değiştirin. 

```ruby 
client = Azure::Storage.client(storage_account_name: account_name, storage_access_key: account_key)
```

## <a name="run-the-sample"></a>Örneği çalıştırma
Bu örnek, "Belgeler" klasöründe bir sınama dosyası oluşturur. Örnek program sınama dosyasını Blob depolamaya yükler, kapsayıcıdaki blobları listeler ve dosyayı yeni bir adla indirir. 

Örnek uygulamayı çalıştırın. Aşağıdaki çıktı, uygulama çalıştırılırken döndürülen çıktının bir örneğidir:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Devam etmek için herhangi bir tuşa bastığınızda, örnek program depolama kapsayıcısını ve dosyaları siler. Devam etmeden önce, iki dosya için "Belgeler" klasörünüzü kontrol edin. Dosyaları açarak aynı olduklarını görebilirsiniz.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın işlevini gördüğünüze göre, koda göz atmak için example.rb dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma
İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir ve her bir dosya, listede yanında yer alan dosya tarafından kullanılır.

* Azure depolama örneğini oluşturmak **istemci** nesne bağlantı kimlik bilgilerini ayarlayın. 
* Depolama hesabınızdaki Blob hizmetine işaret eden bir **BlobService** nesnesi oluşturun. 
* Eriştiğiniz kapsayıcıyı ifade eden bir **Container** nesnesi oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.

Bulut Blob kapsayıcısına sahip olduğunuzda, dilediğiniz bloba işaret eden **Block** blob nesnesi oluşturabilir ve karşıya yükleme, indirme ve kopyalama işlemleri gerçekleştirebilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Bu bölümde Azure depolama istemcisi örneği kurdunuz, blob hizmeti nesnesini oluşturdunuz, yeni bir kapsayıcı oluşturdunuz ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak şekilde ayarladınız. Bu kapsayıcının adı **quickstartblobs**'dur. 

```ruby 
# Setup a specific instance of an Azure::Storage::Client
client = Azure::Storage.client(storage_account_name: account_name, storage_access_key: account_key)

# Create the BlobService that represents the Blob service for the storage account
blob_service = client.blob_client

# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
container = blob_service.create_container(container_name)   

# Set the permission so the blobs are public.
blob_service.set_container_acl(container_name, "container")
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır.  

Bir dosyayı bloba yüklemek için, yerel diskinizdeki dizin adıyla dosya adını birleştirerek dosyanın tam yolunu alın. Sonra, dosyayı belirtilen yola **create\_block\_blob()** metoduyla yükleyebilirsiniz. 

Örnek kod, karşıya yükleme ve indirme için kullanılacak yerel bir dosya oluşturur, karşıya yüklenecek dosyayı **file\_path\_to\_file** olarak ve blob adını **local\_file\_name** olarak depolar. Aşağıdaki örnek, dosyayı **quickstartblobs** adlı kapsayıcınıza yükler.

```ruby
# Create a file in Documents to test the upload and download.
local_path=File.expand_path("~/Documents")
local_file_name ="QuickStart_" + SecureRandom.uuid + ".txt"
full_path_to_file =File.join(local_path, local_file_name)

# Write text to the file.
file = File.open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

puts "Temp file = " + full_path_to_file
puts "\nUploading to Blob storage as blob" + local_file_name

# Upload the created file, using local_file_name for the blob name
blob_service.create_block_blob(container.name, local_file_name, full_path_to_file)
```

Bir blok blobunun içeriğini kısmen güncelleştirmek için **create\_block\_list()** metodunu kullanın. Blok bloblarının boyutu 4,7 TB'yi bulabilir ve bu bloblar Excel elektronik tablolarından büyük video dosyalarına kadar birçok türde olabilir. Sayfa blobları öncelikli olarak IaaS VM'leri desteklemek için kullanılan VHD dosyalarında kullanılır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Ekleme blobu tek bir yazar modelinde kullanılabilir. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

**list\_blobs()** metodunu kullanarak kapsayıcıdaki dosyaların listesini alabilirsiniz. Aşağıdaki kod blob listesini alır, ardından bu bloblarda döngü yapar ve kapsayıcıda bulunan blobların adlarını gösterir.  

```ruby
# List the blobs in the container
puts "\n List blobs in the container"
blobs = blob_service.list_blobs(container_name)
blobs.each do |blob|
    puts "\t Blob name #{blob.name}"   
end  
```

### <a name="download-the-blobs"></a>Blobları indirme

**get\_blob()** metodunu kullanarak blobları yerel diskinize indirin. Aşağıdaki kod, önceki bir bölüme yüklenen blobu indirir. Yerel diskte yer alan iki dosyayı da görebilmeniz için, blob adının sonuna "_DOWNLOADED" son eki getirilir. 

```ruby
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = File.join(local_path, local_file_name.gsub('.txt', '_DOWNLOADED.txt'))

puts "\n Downloading blob to " + full_path_to_file2
blob, content = blob_service.get_blob(container_name,local_file_name)
File.open(full_path_to_file2,"wb") {|f| f.write(content)}
```

### <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, **delete\_container()** metodunu kullanarak kapsayıcının tamamını silebilirsiniz. Oluşturulan dosyalara artık ihtiyaç duymuyorsanız dosyaları silmek için **delete\_blob()** metodunu kullanırsınız.

```ruby
# Clean up resources. This includes the container and the temp files
blob_service.delete_container(container_name)
File.delete(full_path_to_file)
File.delete(full_path_to_file2)    
```

## <a name="next-steps"></a>Sonraki adımlar
 
Bu hızlı başlangıçta, dosyaları Ruby kullanarak yerel bir disk ve Azure blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](./storage-ruby-how-to-use-blob-storage.md)


Depolama Gezgini ve Bloblar hakkında daha fazla bilgi için bkz. [Azure Blob depolama kaynaklarını Depolama Gezgini'yle yönetme](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
