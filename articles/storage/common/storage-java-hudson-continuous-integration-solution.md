---
title: Hudson Blob storage ile kullanma | Microsoft Docs
description: "Hudson ile Azure Blob storage depo olarak derleme yapıları için nasıl kullanılacağını açıklar."
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 119becdd-72c4-4ade-a439-070233c1e1ac
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: e54bedff5f744004288e132efbed8c3e7981f8a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Hudson Sürekli Tümleştirme çözümüyle Azure Depolama'yı kullanma
## <a name="overview"></a>Genel Bakış
Aşağıdaki bilgiler Blob Depolama Hudson sürekli tümleştirme (CI) çözümü tarafından oluşturulan yapı yapılarının deposu olarak ya da indirilebilir dosyalarını kaynağı olarak bir yapı işleminde kullanılacak nasıl kullanılacağını gösterir. (Java veya diğer dilleri kullanarak) Çevik Geliştirme ortamında kodlama bu yararlı bulabileceğiniz senaryolardan biri, olduğunda, üzerinde sürekli tümleştirme derlemeleri çalıştırıyorsanız ve böylece, örneğin, bunları müşterilerinizin diğer kuruluş üyeleriyle paylaşmak veya bir arşiv tutmak derleme yapıtları için bir depo gerekir.  Derleme işi diğer dosyaları, örneğin, giriş derleme bir parçası olarak indirmek için bağımlılıkları gerektirdiğinde başka bir senaryodur.

Bu öğreticide, Azure depolama eklentisi Hudson Microsoft tarafından kullanılabilir hale CI için kullanır.

## <a name="introduction-to-hudson"></a>Hudson giriş
Derlemeleri otomatik olarak ve sık, böylece geliştiriciler verimliliğini artırma ürettiğini ve kendi kod değişiklikleri kolayca tümleştirmek geliştiricilerine vererek yazılım projenin sürekli tümleştirme Hudson sağlar. Derlemeleri sürümlü ve derleme yapıları için çeşitli depoları karşıya yüklenebilir. Bu makalede, Azure Blob Depolama derleme yapıları deposu kullanmak nasıl yapacağınızı gösterir. Ayrıca Azure Blob depolama alanından bağımlılıkları indirmek nasıl yapacağınızı gösterir.

Hudson hakkında daha fazla bilgi bulunabilir [karşılamak Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Blob hizmeti kullanmanın yararları
Çevik Geliştirme derleme yapıtları barındırmak için Blob hizmeti kullanmanın avantajları şunlardır:

* Yüksek kullanılabilirlik derleme yapıları ve/veya indirilebilir bağımlılıkları.
* Hudson CI çözümünüzü derleme yapıtları yüklediğinde performans.
* Müşterileri ve ortakları derleme yapıtları yüklediğinizde performans.
* Kullanıcı erişim ilkeleri, anonim erişim, sona erme tabanlı paylaşılan erişim imzası erişim, özel arasında bir seçim ile üzerinden erişim, vb. denetler.

## <a name="prerequisites"></a>Ön koşullar
Blob hizmeti Hudson CI çözümünüzle birlikte kullanmak için aşağıdaki gerekir:

* Bir Hudson sürekli tümleştirme çözümü.
  
    Şu anda Hudson CI çözüm yoksa, aşağıdaki yöntemi kullanarak bir Hudson CI çözüm çalıştırabilirsiniz:
  
  1. Java etkin bir makinede gelen Hudson WAR karşıdan <http://hudson-ci.org/>.
  2. Hudson WAR çalıştırmak Hudson WAR içeren klasöre açılmış bir komut isteminde. Örneğin, sürüm 3.1.2 yüklediyseniz:
     
      `java -jar hudson-3.1.2.war`

  3. Tarayıcınızda açın `http://localhost:8080/`. Bu Hudson Pano açar.
  4. İlk kurulum sırasında Hudson ilk kullanımı sonucunda tamamlamak `http://localhost:8080/`.
  5. İlk kurulumu tamamlamak Hudson WAR çalışan örneği iptal, Hudson WAR yeniden başlatmadan sonra ve Hudson Panoyu yeniden açmak `http://localhost:8080/`, yüklemek ve Azure Storage eklentisini yapılandırmak için kullanacağınız.
     
      Tipik bir Hudson CI çözüm komut satırında Hudson war çalışan bir hizmet olarak çalışacak şekilde ayarlanması ancak bu öğretici için yeterli olacaktır.
* Bir Azure hesabı. Azure hesabı için kaydolabilirsiniz <http://www.azure.com>.
* Bir Azure depolama hesabı. Bir depolama hesabı zaten sahip değilseniz, verilen adımları kullanarak bir tane oluşturabilirsiniz [depolama hesabı oluşturma](../common/storage-create-storage-account.md#create-a-storage-account).
* Hudson CI çözüm aşina önerilir, ancak aşağıdaki içeriğe derleme yapıları Blob hizmeti Hudson CI için depo olarak kullanırken gerekli adımları göstermek için temel bir örnek kullanacağını gibi gerekli değildir.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Blob hizmeti Hudson CI ile kullanma
Blob hizmeti Hudson ile kullanmak için Azure Storage eklentisini yükleme, depolama hesabınızı kullanmak için eklentiyi yapılandırmak ve ardından depolama hesabınıza derleme yapıtları yükleyen oluşturma sonrası eylem oluşturmak gerekir. Bu adımlar aşağıdaki bölümlerde açıklanmaktadır.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure Storage eklentisini yükleme
1. Hudson Pano içinde tıklatın **yönetmek Hudson**.
2. Üzerinde **yönetmek Hudson** sayfasında, **eklentileri yönetme**.
3. Tıklatın **kullanılabilir** sekmesi.
4. Tıklatın **diğerleri**.
5. İçinde **yapı Uploaders** bölümünde, select **Microsoft Azure depolama eklentisi**.
6. **Yükle**'ye tıklayın.
7. Yükleme tamamlandıktan sonra Hudson yeniden başlatın.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Depolama hesabınızı kullanmak için Azure Storage eklentisi yapılandırma
1. Hudson Pano içinde tıklatın **yönetmek Hudson**.
2. Üzerinde **yönetmek Hudson** sayfasında, **yapılandırma sistem**.
3. İçinde **Microsoft Azure depolama hesabı Yapılandırması** bölümü:
   
    a. Elde edebilirsiniz, depolama hesabı adı girin [Azure Portal](https://portal.azure.com).
   
    b. Depolama hesabı anahtarınızı, gelen da elde edilebilir girin [Azure Portal](https://portal.azure.com).
   
    c. Varsayılan değer **Blob Hizmeti uç noktası URL'si** genel Azure bulut kullanıyorsanız. Farklı bir Azure bulut kullanıyorsanız, uç nokta belirtildiği gibi kullanın [Azure Portal](https://portal.azure.com) depolama hesabınız için.
   
    d. Tıklatın **depolama kimlik bilgilerini doğrulama** depolama hesabınızı doğrulamak için.
   
    e. [İsteğe bağlı] Ek varsa, kullanmak istediğiniz depolama hesapları, Hudson CI kullanılabilir hale, tıklatın **daha fazla depolama hesapları ekleme**.
   
    f. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Depolama hesabınıza derleme yapıtları yükleyen oluşturma sonrası eylem oluşturma
Yönerge amacıyla ilk biz birkaç dosyalarını oluşturmak ve sonra depolama hesabınıza dosyaları karşıya oluşturma sonrası eylem eklemek bir iş oluşturmanız gerekir.

1. Hudson Pano içinde tıklatın **yeni iş**.
2. İş adı **MyJob**, tıklatın **serbest stili yazılım işi yapı**ve ardından **Tamam**.
3. İçinde **yapı** bölüm iş yapılandırılmasını tıklatın **Ekle derleme adımı** ve seçin **yürütme Windows toplu iş komutu**.
4. İçinde **komutu**, aşağıdaki komutları kullanın:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. İçinde **oluşturma sonrası eylemleri** bölüm iş yapılandırılmasını tıklatın **yapıları Microsoft Azure Blob depolama alanına karşıya**.
6. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
7. İçin **kapsayıcı adı**, kapsayıcı adı belirtin. (Derleme yapıları yüklenirken zaten yoksa, kapsayıcı oluşturulacak.) Ortam değişkenlerini kullanma, bu nedenle bu örnek için girin **${JOB_NAME}** kapsayıcı adı.
   
    **İpucu**
   
    Aşağıda **komutu** bölüm için bir komut dosyası girmiş **yürütme Windows toplu iş komutu** Hudson tarafından tanınan ortam değişkenleri bir bağlantıdır. Ortam değişkeni adları ve açıklamaları öğrenmek için bu bağlantıyı tıklatın. Özel içeren ortam değişkenleri karakter olduğunu, gibi Not **BUILD_URL** ortam değişkeni, bir kapsayıcı adı veya ortak sanal yol olarak izin verilmez.
8. Tıklatın **olun yeni kapsayıcı Ortak varsayılan olarak** Bu örnek için. (Özel bir kapsayıcı kullanmak istiyorsanız, erişime izin vermek için bir paylaşılan erişim imzası oluşturmanız gerekir. Bu makalenin kapsamı dışındadır olmasıdır. Paylaşılan erişim imzaları hakkında daha fazla bilgiyi [kullanarak paylaşılan erişim imzaları (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [İsteğe bağlı] Tıklatın **karşıya yüklemeden önce temiz kapsayıcı** derleme yapıları karşıya önce içeriği temizlenecek kapsayıcı istiyorsanız (kapsayıcı içeriğini temizlemek istemiyorsanız bu işaretli bırakın).
10. İçin **karşıya yüklemek için yapıları listesi**, girin  **metin /*.txt**.
11. İçin **karşıya yüklenen yapıları için ortak sanal yol**, girin **${yapı\_kimliği} / ${yapı\_numarası}**.
12. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.
13. Hudson panosunda tıklatın **şimdi yapı** çalıştırmak için **MyJob**. Durum için konsol çıkışı inceleyin. Derleme yapıları karşıya yüklemek oluşturma sonrası eylem başladığında, durum iletileri için Azure Storage konsol çıktısı dahil edilir.
14. İş başarıyla tamamlandıktan sonra ortak blob açarak derleme yapıları inceleyebilirsiniz.
    
    a. [Azure Portal](https://portal.azure.com)'da oturum açın.
    
    b. Tıklatın **depolama**.
    
    c. Hudson için kullanılan depolama hesabının adına tıklayın.
    
    d. Tıklatın **kapsayıcıları**.
    
    e. Adlı kapsayıcıyı tıklatın **myjob**, küçük harfli sürümünü Hudson proje oluşturduğunuzda, size atanan iş adı olduğu. Kapsayıcı adları ve blob adları küçük harf (ve büyük küçük harfe duyarlı) Azure depolama. BLOB'ları adlı kapsayıcının listesi içinde **myjob** görmeniz gerekir **hello.txt** ve **date.txt**. URL bu öğelerden birini kopyalayın ve tarayıcınızda açın. Karşıya yüklenen metin dosyası yapı yapı görürsünüz.

Yapıları Azure Blob depolama alanına yükler yalnızca bir oluşturma sonrası eylem iş başına oluşturulabilir. Yapıları Azure Blob depolama alanına yüklemek için tek oluşturma sonrası eylemi (joker karakterleri dahil olmak üzere) farklı dosyaları ve yolları içindeki dosyalara belirtebilirsiniz Not **karşıya yüklemek için yapıları listesi** ayırıcı olarak noktalı virgül kullanıyor. Örneğin, sizin Hudson yapı JAR dosyaları ve TXT dosyaları alanınızdaki 's üretir **yapı** klasörü ve istediğiniz hem de Azure Blob Depolama karşıya yüklemek için aşağıdakileri kullanın **karşıya yüklemek için yapıları listesi** değer: **yapı /\*.jar; derleme /\*.txt**. Ayrıca, bir yolu blob adını belirtmek için çift iki nokta üst üste sözdizimini kullanabilirsiniz. Kavanoz kullanarak karşıya istiyorsanız, örneğin, **ikili dosyaları** blob yolu ve kullanarak karşıya için TXT dosyaları **bildirimler** blob yolunda için aşağıdakileri kullanın **karşıya yüklemek için yapıları listesi** değeri: **yapı /\*. jar::binaries; derleme /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Azure Blob Storage'dan indirilen bir derleme adımı oluşturma
Aşağıdaki adımlar, Azure Blob depolama alanından öğeleri indirmek için bir derleme adımı yapılandırma gösterir. Bu yapılandırma, örneğin, Azure Blob depolama alanına tutmak Kavanoz öğeleri dahil etmek istediğiniz durumlarda yararlı olacaktır.

1. İçinde **yapı** bölüm iş yapılandırmasına, tıklatın **Ekle derleme adımı** ve seçin **Azure Blob depolama alanından karşıdan**.
2. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
3. İçin **kapsayıcı adı**, yüklemek istediğiniz BLOB'ları olan kapsayıcının adını belirtin. Ortam değişkenleri kullanabilirsiniz.
4. İçin **Blob adı**, blob adı belirtin. Ortam değişkenleri kullanabilirsiniz. Ayrıca, blob adının ilk letter(s) belirttikten sonra bir joker karakter olarak bir yıldız işareti kullanabilirsiniz. Örneğin, **proje\***  adları ile başlayan tüm BLOB'lar belirtirsiniz **proje**.
5. [İsteğe bağlı] İçin **indirme yolunu**, Azure Blob depolama alanından dosyalarını indirmek istediğiniz Hudson makinede yolunu belirtin. Ortam değişkenleri de kullanılabilir. (İçin bir değer belirtmezseniz, **indirme yolunu**, Azure Blob depolama biriminden dosyaları işin çalışma alanına yüklenir.)

Azure Blob depolama alanından indirmek istediğiniz başka öğeler varsa, ek yapılandırma adımları oluşturabilirsiniz.

Bir yapı çalıştırdıktan sonra yapı geçmiş konsol çıktısı denetleyin veya beklediğiniz BLOB'ları başarıyla yüklenmiş olup olmadığını görmek için indirme konumunda, bakın.

## <a name="components-used-by-the-blob-service"></a>Blob hizmeti tarafından kullanılan bileşenler
Blob hizmeti bileşenlerini genel bir bakış sağlar.

* **Depolama hesabı**: tüm Azure Storage erişimi bir depolama hesabıyla yapılır. BLOB'ları erişmek için ad alanı en yüksek düzeyde budur. Altında 100 TB toplam kendi boyuttur sürece bir hesapta sınırsız sayıda kapsayıcı, olabilir.
* **Kapsayıcı**: BLOB'lar kümesinin bir gruplandırma bir kapsayıcı sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.
* **BLOB**: bir dosya türü ve boyutu. Azure Storage'da depolanan BLOB'ları iki tür vardır: blok ve sayfa blobları. Blok blobları çoğu dosyalarıdır. Tek bir blok blobu 200 GB boyutunda olabilir. Bu öğretici blok blobları kullanır. Bir dosyada bayt aralıkları sık değiştirildiğinde sayfa blobları, başka bir blob türü, boyut ve daha verimli olan 1 TB'ye kadar olabilir. BLOB'ları hakkında daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Bloblarını](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL biçimi**: BLOB'lar şu URL biçimi kullanılarak adreslenebilir:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Yukarıdaki biçimi için ortak Azure bulut uygular. Farklı bir Azure bulut kullanıyorsanız, uç noktalar kullanın [Azure Portal](https://portal.azure.com) URL uç noktanızı belirlemek için.)
  
    Yukarıdaki biçiminde `storageaccount` depolama hesabınızın adını temsil eden `container_name` , kapsayıcının adını temsil eder ve `blob_name` , blob adı sırasıyla temsil eder. Kapsayıcı adı içinde birden fazla yol eğik tarafından ayrılmış, olabilir  **/** . Bu öğreticideki örnek kapsayıcı adı **MyJob**, ve **${yapı\_kimliği} / ${yapı\_numarası}** URL'si aşağıdaki biçime sahip blob kaynaklanan ortak sanal yol için kullanıldı:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Sonraki adımlar
* [Karşılayan Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Azure depolama için Java SDK'sı](https://github.com/azure/azure-storage-java)
* [Azure Storage istemci SDK'sı başvurusu](http://dl.windowsazure.com/storage/javadoc/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](http://blogs.msdn.com/b/windowsazurestorage/)

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).