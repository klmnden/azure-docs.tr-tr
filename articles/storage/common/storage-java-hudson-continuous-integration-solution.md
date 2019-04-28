---
title: Hudson Blob Depolama ile kullanma | Microsoft Docs
description: Hudson ile Azure Blob Depolama deposu olarak derleme yapıtları için nasıl kullanılacağını açıklar.
services: storage
author: seguler
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.subservice: common
ms.openlocfilehash: d00bf87a80e13808c42a5839ad0f4508ad7214b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61477493"
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Hudson Sürekli Tümleştirme çözümüyle Azure Depolama'yı kullanma
## <a name="overview"></a>Genel Bakış
Aşağıdaki bilgiler, bir yapı işleminde kullanılacak bir depo Hudson sürekli tümleştirme (CI) çözümü tarafından oluşturulan yapılar veya indirilebilir dosyaları için kaynak olarak Blob Depolama kullanma işlemi gösterilmektedir. Burada, bu kullanışlı olabilecek senaryoları biri, Çevik Geliştirme ortamında (Java veya diğer diller kullanılarak) kodlamaya, üzerinde sürekli tümleştirme yapıları çalıştırıyorsanız ve derleme yapıtları için bir depo ihtiyacınız olduğunda, böylelikle yapabilirsiniz, Örneğin, onları müşterilerinize, diğer kuruluşun üyeleriyle paylaşma veya bir arşiv tutmak.  Diğer dosyalar giriş yapının bir parçası indirmek için örneğin, bağımlılıkları, derleme işi gerektirdiğinde, başka bir senaryodur.

Bu öğreticide, Azure depolama eklentisi için Microsoft tarafından sunulan Hudson CI kullanacaklardır.

## <a name="introduction-to-hudson"></a>Hudson giriş
Hudson kolayca kod değişikliklerini tümleştirmek geliştiricilere vererek bir yazılım projesi için sürekli tümleştirme sağlar ve derlemeler otomatik olarak ve sık, böylece geliştiricilerin üretkenliğini artırma üretti. Yapılar tutulur ve derleme yapıtları için çeşitli depoları karşıya yüklenebilir. Bu makale, derleme yapıtlarını bir deposu olarak Azure Blob Depolama kullanma işlemini gösterir. Ayrıca, Azure Blob depolama alanından bağımlılıklarını karşıdan yükleme de gösterilir.

Hudson hakkında daha fazla bilgi şu adreste bulunabilir: [karşılamak Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Blob hizmeti kullanmanın avantajları
Çevik Geliştirme derleme yapıtlarınızı barındırmak için Blob hizmeti kullanmanın avantajları şunlardır:

* Yüksek kullanılabilirlik, yapılar ve/veya indirilebilir bağımlılıkları.
* Derleme yapıtlarınızı Hudson CI çözümünüzü yüklediğinde performans.
* Müşteriler ve iş ortakları, derleme yapıtları yüklediğinizde performans.
* Kullanıcı erişim ilkeleri, anonim erişim, sona erme tabanlı paylaşılan erişim imzası erişim, özel arasından seçim yapma olanağıyla üzerinden erişim, vb. denetler.

## <a name="prerequisites"></a>Önkoşullar
Blob hizmeti Hudson CI çözümünüz ile kullanmak için aşağıdakilere ihtiyacınız olacak:

* Hudson sürekli tümleştirme çözümü için.
  
    Hudson CI çözümü şu anda sahip değilseniz, aşağıdaki yöntemi kullanarak Hudson CI çözümü çalıştırabilirsiniz:
  
  1. Bir Java özellikli makinede gelen Hudson WAR indirme <http://hudson-ci.org/>.
  2. Bir komut isteminde, Hudson WAR çalıştırma Hudson WAR içeren klasöre açılır. Örneğin, sürüm 3.1.2 indirdiyseniz:
     
      `java -jar hudson-3.1.2.war`

  3. Tarayıcınızda açın `http://localhost:8080/`. Bu, Hudson Pano açılır.
  4. İlk kurulum sırasında ilk Hudson kullanımı sonucunda tamamlamak `http://localhost:8080/`.
  5. Sonra ilk kurulumu tamamlamak, Hudson WAR çalışan örneği iptal, Hudson WAR yeniden başlatın ve Hudson Panoyu yeniden açın `http://localhost:8080/`, yüklemek ve Azure depolama eklentisi yapılandırmak için kullanacağınız.
     
      Tipik bir Hudson CI çözüm Hudson war komut satırını çalıştıran, bir hizmet olarak çalıştırmak için ayarlanması ancak bu öğretici için yeterli olacaktır.
* Bir Azure hesabı. Bir Azure hesabı için kaydolabilirsiniz <https://www.azure.com>.
* Bir Azure depolama hesabı. Bir depolama hesabınız yoksa, adımları kullanarak bir tane oluşturabilirsiniz [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).
* Hudson CI çözümüne konusunda önerilir, ancak aşağıdaki içeriği Blob hizmeti için Hudson CI deposu olarak kullanılırken gerekli olan adımları derleme yapıları göstermek için basit bir örneği kullanacağınız, gerekli değildir.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Hudson CI ile Blob hizmetini kullanma
Blob hizmeti ile Hudson kullanmak için Azure depolama eklentisini yükleme, depolama hesabınızı kullanmak için eklentiyi yapılandırmak ve ardından derleme yapıtları depolama hesabınıza yükleyen bir derleme sonrası eylem oluşturmak gerekir. Aşağıdaki bölümlerde bu adımlar açıklanmaktadır.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure depolama eklentisi yükleme
1. Hudson panosunda tıklayın **yönetme Hudson**.
2. Üzerinde **yönetme Hudson** sayfasında **Eklentileri Yönet**.
3. Tıklayın **kullanılabilir** sekmesi.
4. Tıklayın **başkalarının**.
5. İçinde **Yapıt karşıya yükleme yapabilen kullanıcıları** bölümünden **Microsoft Azure depolama eklentisi**.
6. **Yükle**'ye tıklayın.
7. Yükleme tamamlandıktan sonra Hudson yeniden başlatın.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azure depolama eklentisi, depolama hesabınızı kullanacak şekilde yapılandırma
1. Hudson panosunda tıklayın **yönetme Hudson**.
2. Üzerinde **yönetme Hudson** sayfasında **yapılandırma sistemi**.
3. İçinde **Microsoft Azure depolama hesabı Yapılandırması** bölümü:
   
    a. Elde edebileceğiniz, depolama hesabı adı girin [Azure portalı](https://portal.azure.com).
   
    b. Depolama hesabı anahtarınızı, gelen da elde edilebilir girin [Azure portalı](https://portal.azure.com).
   
    c. Varsayılan değer **Blob Hizmeti uç noktası URL'si** Azure genel Bulutu kullanıyorsanız. Farklı Azure Bulutu kullanıyorsanız, belirtilen uç noktası kullanma [Azure portalı](https://portal.azure.com) depolama hesabınız için.
   
    d. Tıklayın **depolama kimlik bilgilerini doğrulama** depolama hesabınızı doğrulamak için.
   
    e. [İsteğe bağlı] Ek varsa, kullanmak istediğiniz depolama hesapları için Hudson CI kullanılabilir hale ye **daha fazla depolama hesapları ekleme**.
   
    f. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Derleme yapıtlarınızı depolama hesabınıza yükleyen bir derleme sonrası eylem oluşturma
Yönerge amacıyla ilk biz birkaç dosya oluşturun ve dosyalarını depolama hesabınıza yüklemek için oluşturma sonrası eylem eklemek bir iş oluşturmanız gerekir.

1. Hudson panosunda tıklayın **yeni iş**.
2. İş adı **MyJob**, tıklayın **derleme serbest stil yazılım işi**ve ardından **Tamam**.
3. İçinde **derleme** bölümü iş yapılandırması **derleme adımı Ekle** seçin **yürütme Windows toplu komutuyla**.
4. İçinde **komut**, aşağıdaki komutları kullanın:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. İçinde **derleme sonrası eylemlerde** bölümü iş yapılandırması **Microsoft Azure Blob depolama alanına yapıtları karşıya**.
6. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
7. İçin **kapsayıcı adı**, kapsayıcı adı belirtin. (Derleme yapıtları karşıya yüklendiğinde zaten yoksa, bir kapsayıcı oluşturulur.) Ortam değişkenlerini kullanma, bu nedenle bu örnekte girin **${JOB_NAME}** kapsayıcı adı.
   
    **İpucu**
   
    Aşağıda **komut** bölümü bir komut dosyası için girmiş olduğunuz **yürütme Windows toplu komutuyla** ortam değişkenlerini Hudson tarafından tanınan bir bağlantıdır. Ortam değişken adları ve açıklamaları öğrenmek için bu bağlantıyı tıklatın. Özel içeren ortam değişkenlerini olduğunu, gibi karakterler Not **BUILD_URL** ortam değişkeni, bir kapsayıcı adı veya ortak sanal yol olarak izin verilmez.
8. Tıklayın **yeni kapsayıcı genel hale varsayılan olarak** bu örneğin. (Özel kapsayıcınızı kullanmak isterseniz, erişime izin vermek için paylaşılan erişim imzası oluşturmanız gerekir. Bu makalenin kapsamı dışındadır olmasıdır. Paylaşılan erişim imzaları hakkında daha fazla bilgi [kullanarak paylaşılan erişim imzaları (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [İsteğe bağlı] Tıklayın **karşıya yüklemeden önce temiz kapsayıcı** derleme yapıtları karşıya önce içeriğini temizlenmesi için kapsayıcıyı istiyorsanız (kapsayıcı içeriğini temizlemek istemiyorsanız, işaretsiz bırakın).
10. İçin **yüklenecek yapıları listesi**, girin **metin/*.txt**.
11. İçin **ortak sanal yol için karşıya yüklenen yapıtları**, girin **${yapı\_kimliği} / ${yapı\_numarası}**.
12. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.
13. Hudson Panoda tıklayın **artık yapı** çalıştırılacak **MyJob**. Konsol çıktısı için durumu inceleyin. Oluşturma sonrası eylem derleme yapıtları karşıya başladığında durum iletilerini Azure depolama için konsol çıkışında dahil edilir.
14. İşin işlemin başarıyla tamamlanmasından sonra ortak blob açarak derleme yapıtları inceleyebilirsiniz.
    
    a. [Azure Portal](https://portal.azure.com)’da oturum açın.
    
    b. Tıklayın **depolama**.
    
    c. Hudson için kullanılan depolama hesabının adına tıklayın.
    
    d. Tıklayın **kapsayıcıları**.
    
    e. Adlandırılmış kapsayıcısını **myjob**, küçük harfli sürümünü Hudson iş oluşturduğunuzda, size atanan iş adı olduğu. Kapsayıcı ve blob adları, küçük harf (ve büyük küçük harfe duyarlı) Azure Depolama'daki. Adlı bir kapsayıcı için BLOB listesini içinde **myjob** görmelisiniz **hello.txt** ve **date.txt**. Bu öğelerden birini URL'sini kopyalayın ve tarayıcınızda açın. Karşıya yüklenen bir metin dosyası bir derleme yapıtı görürsünüz.

Yapıtları Azure Blob depolama alanına yükler. yalnızca bir derleme sonrası eylem iş başına oluşturulabilir. Yapıtları Azure Blob depolama alanına yüklemek için tek derleme sonrası eylem farklı dosya (joker karakterler dahil) ve dosyaların yollarını belirtebileceğinizi aklınızda bulundurun **yüklenecek yapıları listesi** ayırıcı olarak noktalı virgül kullanarak. Örneğin, yapı, Hudson JAR dosyaları ve çalışma alanınızın TXT dosyaları üretir **derleme** klasörünü açın ve istediğiniz her ikisi de Azure Blob depolama alanına yüklemek için aşağıdaki **yüklenecek yapıları listesi** değer: **derleme /\*.jar; derleme /\*.txt**. Çift iki nokta üst üste söz dizimi içinde blob adı kullanmak için bir yol belirtmek için de kullanabilirsiniz. Jar dosyaları dışındaki kullanarak karşıya istiyorsanız, örneğin, **ikili dosyaları** blob yolu ve TXT dosyaları kullanarak karşıya **bildirimler** blob yolu için aşağıdakileri kullanın **listesi, yapılar Karşıya yüklenecek** değer: **derleme /\*. jar::binaries; derleme /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Azure Blob Depolama'dan indirilen bir derleme adımı oluşturma
Aşağıdaki adımlar, yapılandırma öğeleri Azure Blob Depolama'yı indirmek için bir derleme adımı gösterir. Bu derleme, örneğin, Azure Blob Depolama alanında tutan jar dosyaları dışındaki öğeleri dahil etmek istiyorsanız yararlı olacaktır.

1. İçinde **derleme** bölümü iş yapılandırması **derleme adımı Ekle** seçin **Azure Blob Depolama'dan indirme**.
2. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
3. İçin **kapsayıcı adı**, indirmek istediğiniz BLOB'ları içeren kapsayıcının adını belirtin. Ortam değişkenlerini kullanabilirsiniz.
4. İçin **Blob adı**, blob adı belirtin. Ortam değişkenlerini kullanabilirsiniz. Ayrıca, blob adının ilk letter(s) belirttikten sonra bir joker karakter olarak bir yıldız işareti kullanabilirsiniz. Örneğin, **proje\\*** tüm BLOB adları ile başlayıp belirtebilirdiniz **proje**.
5. [İsteğe bağlı] İçin **indirme yolu**, Hudson makine, Azure Blob depolama alanından dosyaları indirmek istediğiniz yolu belirtin. Ortam değişkenlerini de kullanılabilir. (Bir değer belirtmezseniz **indirme yolu**, Azure Blob depolama biriminden dosyaları işin çalışma alanına yüklenir.)

Azure Blob depolama alanından indirmek istediğiniz ek öğeler varsa, ek derleme adımları oluşturabilirsiniz.

Bir derlemeyi çalıştırdıktan sonra derleme geçmişi konsol çıkışı denetleyin ya da beklediğiniz blobları başarıyla yüklenmiş olup olmadığını görmek için indirme konumunda, bakın.

## <a name="components-used-by-the-blob-service"></a>Blob hizmeti tarafından kullanılan bileşenler
Blob hizmeti bileşenlerini genel bir bakış sağlar.

* **Depolama hesabı**: Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Bloblara erişmek için ad alanı en üst düzey budur. Kendi toplam boyutu altında 100 TB olduğu sürece bir hesapta kapsayıcılar, sınırsız sayıda olabilir.
* **kapsayıcı**: Bir kapsayıcı, BLOB'ları kümesi bir gruplandırmasını sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.
* **BLOB**: Bir dosya herhangi bir türü ve boyutu. Azure Depolama'da depolanan BLOB'ları iki tür vardır: blok ve sayfa blobları. Çoğu dosya blok blobudur. Tek bir blok blobu en fazla 200 GB boyutunda olabilir. Bu öğreticide, blok blobları kullanılır. Bir dosyadaki bayt aralıkları sık değişiklik yapıldığında, sayfa blobları, başka bir blob türü, en fazla 1 TB boyut ve daha verimli olabilir. BLOB'ları hakkında daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](https://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL biçimi**: BLOB'ları şu URL biçimi kullanılarak adreslenebilir:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Yukarıdaki biçimini genel Azure bulutuna uygular. Farklı Azure Bulutu kullanıyorsanız, uç nokta içinde kullanmak [Azure portalı](https://portal.azure.com) URL uç noktanızı belirlemek için.)
  
    Yukarıdaki biçimde `storageaccount` depolama hesabınızın adını temsil eden `container_name` , kapsayıcının adını temsil eder ve `blob_name` sırasıyla, blob adını temsil eder. Kapsayıcı adı içinde bir eğik ayrılmış birden çok yol olabilir **/**. Bu öğreticideki örnek kapsayıcı adı **MyJob**, ve **${yapı\_kimliği} / ${yapı\_numarası}** blobun URL'sini sahip kaynaklanan ortak sanal yol için kullanıldı Aşağıdaki biçimi:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Sonraki adımlar
* [Meet Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Java için Azure depolama SDK'si](https://github.com/azure/azure-storage-java)
* [Azure Depolama İstemcisi SDK Başvurusu](http://dl.windowsazure.com/storage/javadoc/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](https://blogs.msdn.com/b/windowsazurestorage/)

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).
