---
title: Jenkins sürekli tümleştirme çözümüyle Azure depolama kullanma
description: Bu öğreticide, Azure blob hizmeti için bir havuz oluştururken yapıtlar bir Jenkins sürekli tümleştirme çözümü tarafından oluşturulan kullanma gösterilmektedir.
ms.topic: article
ms.author: tarcher
author: tarcher
services: devops
ms.service: storage
custom: jenkins
ms.date: 07/31/2018
ms.subservice: common
ms.openlocfilehash: d9ef6f5056fdbd7187c92c98d1c884a5314c29a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153659"
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Jenkins sürekli tümleştirme çözümüyle Azure depolama kullanma

Bu makalede, bir yapı işleminde kullanılacak bir Jenkins sürekli tümleştirme (CI) çözümü tarafından oluşturulan yapılar için bir havuz olarak ya da indirilebilir dosyaları için kaynak olarak Blob Depolama kullanma gösterilmektedir. Burada, bu çözüm faydalı bulabileceğiniz senaryoları biri olduğunda, Çevik Geliştirme ortamında (Java veya diğer diller kullanılarak) kodlamaya, derleme çalışıyor tabanlı üzerinde sürekli tümleştirme ve derleme yapıtları için bir depo gerekir böylece , örneğin, onları müşterilerinize, diğer kuruluşun üyeleriyle paylaşma veya bir arşiv tutmak. Diğer dosyalar giriş yapının bir parçası indirmek için örneğin, bağımlılıkları, derleme işi gerektirdiğinde, başka bir senaryodur.

Jenkins CI, Microsoft'un sunduğu için Bu öğreticide, Azure depolama eklentisi kullanacaksınız.

## <a name="jenkins-overview"></a>Jenkins genel bakış
Jenkins, kolayca kod değişikliklerini tümleştirmek geliştiricilere vererek bir yazılım projesi için sürekli tümleştirme sağlar ve derlemeler otomatik olarak ve sık, böylece geliştiricilerin üretkenliğini artırma üretti. Yapılar tutulur ve derleme yapıtları için çeşitli depoları karşıya yüklenebilir. Bu makalede, derleme yapıtlarını bir deposu olarak Azure blob depolama kullanma gösterilmektedir. Ayrıca, Azure blob depolama alanından bağımlılıklarını karşıdan yükleme de gösterilir.

Jenkins hakkında daha fazla bilgi şu adreste bulunabilir: [karşılamak Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Blob hizmeti kullanmanın avantajları
Çevik Geliştirme derleme yapıtlarınızı barındırmak için Blob hizmeti kullanmanın avantajları şunlardır:

* Yüksek kullanılabilirlik, yapılar ve/veya indirilebilir bağımlılıkları.
* Jenkins CI çözümünüz derleme yapıtlarınızı yüklediğinde performans.
* Müşteriler ve iş ortakları, derleme yapıtları yüklediğinizde performans.
* Kullanıcı erişim ilkeleri, anonim erişim, sona erme tabanlı paylaşılan erişim imzası erişim, özel arasından seçim yapma olanağıyla üzerinden erişim, vb. denetler.

## <a name="prerequisites"></a>Önkoşullar
* Jenkins sürekli tümleştirme çözümü.
  
    Jenkins CI çözümü şu anda sahip değilseniz, aşağıdaki yöntemi kullanarak Jenkins CI çözümü çalıştırabilirsiniz:
  
  1. Bir Java özellikli makinede gelen jenkins.war indirin <https://jenkins-ci.org>.
  2. Bir komut jenkins.war içeren klasöre açılan isteminde çalıştırın:
     
      `java -jar jenkins.war`

  3. Tarayıcınızda açın `http://localhost:8080/` yüklemek ve Azure depolama eklentisi yapılandırmak için kullanacağınız Jenkins panosunu açın.
     
      Tipik bir Jenkins CI çözüm Jenkins war komut satırını çalıştıran, bir hizmet olarak çalıştırmak için ayarlanması ancak bu öğretici için yeterli olacaktır.
* Bir Azure hesabı. Bir Azure hesabı için kaydolabilirsiniz <https://www.azure.com>.
* Bir Azure depolama hesabı. Bir depolama hesabınız yoksa, adımları kullanarak bir tane oluşturabilirsiniz [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).
* Jenkins CI çözümüne konusunda önerilir, ancak aşağıdaki içeriği Blob hizmeti için Jenkins CI deposu olarak kullanılırken gerekli olan adımları derleme yapıları göstermek için basit bir örneği kullanacağınız, gerekli değildir.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Jenkins CI ile Blob hizmetini kullanma
Jenkins ile Blob hizmeti kullanmak için Azure depolama eklentisini yükleme, depolama hesabınızı kullanmak için eklentiyi yapılandırmak ve ardından derleme yapıtları depolama hesabınıza yükleyen bir derleme sonrası eylem oluşturmak gerekir. Aşağıdaki bölümlerde bu adımlar açıklanmaktadır.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure depolama eklentisi yükleme
1. Jenkins panosunda seçin **Jenkins'i Yönet**.
2. İçinde **Jenkins'i Yönet** sayfasında **Eklentileri Yönet**.
3. **Available** (Kullanılabilir) sekmesini seçin.
4. İçinde **Yapıt karşıya yükleme yapabilen kullanıcıları** bölümünde onay **Microsoft Azure depolama eklentisi**.
5. Şunlardan birini seçin **yeniden yükleme** veya **hemen indirin ve yeniden başlatma işleminden sonra yükleme**.
6. Jenkins yeniden başlatın.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azure depolama eklentisi, depolama hesabınızı kullanacak şekilde yapılandırma
1. Jenkins panosunda seçin **Jenkins'i Yönet**.
2. İçinde **Jenkins'i Yönet** sayfasında **yapılandırma sistemi**.
3. İçinde **Microsoft Azure depolama hesabı Yapılandırması** bölümü:
   1. Elde edebileceğiniz, depolama hesabı adı girin [Azure portalı](https://portal.azure.com).
   2. Depolama hesabı anahtarınızı, gelen da elde edilebilir girin [Azure portalı](https://portal.azure.com).
   3. Varsayılan değer **Blob Hizmeti uç noktası URL'si** Azure genel Bulutu kullanıyorsanız. Farklı Azure Bulutu kullanıyorsanız, belirtilen uç noktası kullanma [Azure portalı](https://portal.azure.com) depolama hesabınız için. 
   4. Seçin **depolama kimlik bilgilerini doğrulama** depolama hesabınızı doğrulamak için. 
   5. [İsteğe bağlı] Jenkins cı Seç kullandırılır istediğiniz ek depolama hesapları varsa **daha fazla depolama hesapları ekleme**.
   6. Seçin **Kaydet** ayarlarınızı kaydetmek için.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Derleme yapıtlarınızı depolama hesabınıza yükleyen bir derleme sonrası eylem oluşturma
Eğitim amaçları için ilk birkaç dosya oluşturun ve dosyalarını depolama hesabınıza yüklemek için oluşturma sonrası eylem eklemek bir iş oluşturmanız gerekir.

1. Jenkins panosunda seçin **yeni öğe**.
2. İş adı **MyJob**seçin **yazılım serbest stil projeyi**ve ardından **Tamam**.
3. İçinde **derleme** select iş Yapılandırması bölümünü **derleme adımı Ekle** seçip **yürütme Windows toplu komutuyla**.
4. İçinde **komut**, aşağıdaki komutları kullanın:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. İçinde **derleme sonrası eylemlerde** select iş Yapılandırması bölümünü **oluşturma sonrası eylem Ekle** seçip **Azure Blob depolama alanına yapıtları karşıya**.
6. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
7. İçin **kapsayıcı adı**, kapsayıcı adı belirtin. (Derleme yapıtları karşıya yüklendiğinde zaten yoksa, bir kapsayıcı oluşturulur.) Ortam değişkenlerini kullanma, bu nedenle bu örnekte girin `${JOB_NAME}` kapsayıcı adı.
   
    **İpucu**
   
    Aşağıda **komut** bölümü bir komut dosyası için girmiş olduğunuz **yürütme Windows toplu komutuyla** ortam değişkenlerini Jenkins tarafından tanınan bir bağlantıdır. Ortam değişken adları ve açıklamaları öğrenmek için bu bağlantıyı seçin. Gibi özel karakterler içeren ortam değişkenlerini **BUILD_URL** ortam değişkeni, bir kapsayıcı adı veya ortak sanal yol olarak izin verilmez.
8. Seçin **yeni kapsayıcı genel hale varsayılan olarak** bu örneğin. (Özel kapsayıcınızı kullanmak isterseniz, bu makalenin kapsamı dışında olan erişime izin vermek için paylaşılan erişim imzası oluşturmanız gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi [kullanarak paylaşılan erişim imzaları (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [İsteğe bağlı] Seçin **karşıya yüklemeden önce temiz kapsayıcı** derleme yapıtları karşıya önce içeriğini temizlenmesi için kapsayıcıyı istiyorsanız (kapsayıcı içeriğini temizlemek istemiyorsanız, işaretsiz bırakın).
10. İçin **yüklenecek yapıları listesi**, girin `text/*.txt`.
11. İçin **ortak sanal yol için karşıya yüklenen yapıtları**, bu öğreticinin amaçları girin `${BUILD\_ID}/${BUILD\_NUMBER}`.
12. Seçin **Kaydet** ayarlarınızı kaydetmek için.
13. Jenkins panosunda seçin **artık yapı** çalıştırılacak **MyJob**. Konsol çıktısı için durumu inceleyin. Derleme yapıtları karşıya yüklemek derleme sonrası eylemi başlatıldığında, Azure depolama için bildirilen durum iletileri konsol çıkışında dahil edilir.
14. İşin işlemin başarıyla tamamlanmasından sonra ortak blob açarak derleme yapıtları inceleyebilirsiniz.
    1. [Azure Portal](https://portal.azure.com)’da oturum açın.
    2. **Depolama**’yı seçin.
    3. Jenkins için kullanılan depolama hesabı adı seçin.
    4. Seçin **kapsayıcıları**.
    5. Adlı kapsayıcıyı seçin **myjob**, Jenkins işi oluşturduğunuzda, size atanan iş adı küçük harfli sürümünü olduğu. Kapsayıcı ve blob adları, küçük harf (ve büyük küçük harfe duyarlı) Azure depolama alanında. Adlı bir kapsayıcı için BLOB listesini içinde **myjob**, görmelisiniz **hello.txt** ve **date.txt**. Bu öğelerden birini URL'sini kopyalayın ve tarayıcınızda açın. Karşıya yüklenen bir metin dosyası bir derleme yapıtı görürsünüz.

Yapıtları Azure blob depolama alanına yükler. yalnızca bir derleme sonrası eylem iş başına oluşturulabilir. Yapıtları Azure blob depolama alanına yüklemek için tek derleme sonrası eylem farklı dosya (joker karakterler dahil) ve dosyaların yollarını belirtebilirsiniz **yüklenecek yapıları listesi** ayırıcı olarak noktalı virgül kullanarak. Örneğin, Jenkins yapı JAR dosyaları ve çalışma alanınızın TXT dosyaları üretir **derleme** klasörünü açın ve istediğiniz her ikisi de Azure blob depolama alanına yüklemek için aşağıdaki değeri kullanın **yüklenecekyapılarılistesi** seçeneği: `build/\*.jar;build/\*.txt`. Çift iki nokta üst üste söz dizimi içinde blob adı kullanmak için bir yol belirtmek için de kullanabilirsiniz. Jar dosyaları dışındaki kullanarak karşıya istiyorsanız, örneğin, **ikili dosyaları** blob yolu ve TXT dosyaları kullanarak karşıya **bildirimler** blob yolu kullanmak için aşağıdaki değeri **listesi Yüklenecek yapıları** seçeneği: `build/\*.jar::binaries;build/\*.txt::notices`.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Azure blob Depolama'dan indirilen bir derleme adımı oluşturma
Yapınızda öğeleri dahil etmek istiyorsanız yararlı olan Azure blob depolama alanından öğeleri indirmek için bir derleme adımı yapılandırmak için aşağıdaki adımları gösterilmektedir. Bu düzeni kullanarak bir Azure blob depolamada kalıcı hale getirmek için isteyebileceğiniz jar dosyaları dışındaki örneğidir.

1. İçinde **derleme** select iş Yapılandırması bölümünü **derleme adımı Ekle** seçip **Azure Blob Depolama'dan indirme**.
2. İçin **depolama hesabı adı**, kullanılacak depolama hesabını seçin.
3. İçin **kapsayıcı adı**, indirmek istediğiniz BLOB'ları içeren kapsayıcının adını belirtin. Ortam değişkenlerini kullanabilirsiniz.
4. İçin **Blob adı**, blob adı belirtin. Ortam değişkenlerini kullanabilirsiniz. Ayrıca, blob adının ilk letter(s) belirttikten sonra bir joker karakter olarak bir yıldız işareti kullanabilirsiniz. Örneğin, **proje\\*** tüm BLOB adları ile başlayıp belirtebilirdiniz **proje**.
5. [İsteğe bağlı] İçin **indirme yolu**, Jenkins makine, Azure blob depolama alanından dosyaları indirmek istediğiniz yolu belirtin. Ortam değişkenlerini de kullanılabilir. (Bir değer belirtmezseniz **indirme yolu**, Azure blob depolama biriminden dosyaları işin çalışma alanına yüklenir.)

Azure blob depolama alanından indirmek istediğiniz ek öğeler varsa, ek derleme adımları oluşturabilirsiniz.

Bir derlemeyi çalıştırdıktan sonra derleme geçmişi konsol çıkışı denetleyin ya da beklediğiniz blobları başarıyla yüklenmiş olup olmadığını görmek için indirme konumunda, bakın.  

## <a name="components-used-by-the-blob-service"></a>Blob hizmeti tarafından kullanılan bileşenler
Bu bölümde, Blob hizmeti bileşenlerini genel bir bakış sağlar.

* **Depolama hesabı**: Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Bloblara erişmek için ad alanı en üst düzey bir depolama hesabıdır. Kendi toplam boyutu altında 100 TB olduğu sürece bir hesapta kapsayıcılar, sınırsız sayıda olabilir.
* **kapsayıcı**: Bir kapsayıcı, BLOB'ları kümesi bir gruplandırmasını sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.
* **BLOB**: Bir dosya herhangi bir türü ve boyutu. Azure Depolama'da depolanan BLOB'ları iki tür vardır: blok ve sayfa blobları. Çoğu dosya blok blobudur. Tek bir blok blobu en fazla 200 GB boyutunda olabilir. Bu öğreticide, blok blobları kullanılır. Bir dosyadaki bayt aralıkları sık değişiklik yapıldığında, sayfa blobları, başka bir blob türü, en fazla 1 TB boyut ve daha verimli olabilir. BLOB'ları hakkında daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](https://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL biçimi**: BLOB'ları şu URL biçimi kullanılarak adreslenebilir:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Yukarıdaki biçimini genel Azure bulutuna uygular. Farklı Azure Bulutu kullanıyorsanız, uç nokta içinde kullanmak [Azure portalı](https://portal.azure.com) URL uç noktanızı belirlemek için.)
  
    Yukarıdaki biçimde `storageaccount` depolama hesabınızın adını temsil eden `container_name` , kapsayıcının adını temsil eder ve `blob_name` sırasıyla, blob adını temsil eder. Kapsayıcı adı içinde bir eğik ayrılmış birden çok yol olabilir **/**. Bu öğreticide kullanılan örnek kapsayıcı adı **MyJob**, ve **${yapı\_kimliği} / ${yapı\_numarası}** bir URL'ye sahip blob kaynaklanan ortak sanal yol için kullanıldı aşağıdaki biçimdedir:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Jenkins karşılamak](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
* [Java için Azure depolama SDK'si](https://github.com/azure/azure-storage-java)
* [Azure Depolama İstemcisi SDK Başvurusu](http://dl.windowsazure.com/storage/javadoc/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](https://blogs.msdn.com/b/windowsazurestorage/)

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).
