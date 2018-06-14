---
title: Gözat ve Sunucu Gezgini kullanarak depolama kaynaklarını yönetmek | Microsoft Docs
description: Göz atma ve Sunucu Gezgini kullanarak depolama kaynaklarını yönetme
services: visual-studio-online
author: ghogen
manager: douge
assetId: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 8/24/2017
ms.author: ghogen
ms.openlocfilehash: 0beeb8fb7f7e46db97e179f3eacf3c68dd92cff3
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31798589"
---
# <a name="browse-and-manage-storage-resources-by-using-server-explorer"></a>Gözat ve Sunucu Gezgini kullanarak depolama kaynaklarını yönetmek

[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Genel Bakış

Microsoft Visual Studio için Azure araçlarını yüklediyseniz, depolama hesapları blob, kuyruk ve tablo verileri için Azure görüntüleyebilirsiniz. Azure **depolama** Sunucu Gezgininde yerel depolama öykünücüsü hesabınızı ve diğer Azure storage hesaplarınızı veri gösterir.

Sunucu Gezgini Visual Studio'nun menü çubuğunda görüntülemek için seçin **Görünüm** > **Sunucu Gezgini**. **Depolama** düğüm tüm her Azure aboneliği veya bağlı olduğunuz sertifika altında mevcut depolama hesaplarını gösterir. Depolama hesabınız görünmüyorsa, bu yönergeleri izleyerek ekleyebileceğiniz [bu makalenin ilerisinde yer](#add-storage-accounts-by-using-server-explorer).

Azure SDK 2.7 başlayarak, Cloud Explorer görüntülemek ve Azure kaynaklarınızı yönetmek için de kullanabilirsiniz. Daha fazla bilgi için bkz: [bulut Gezgini ile yönetme Azure kaynaklarını](vs-azure-tools-resources-managing-with-cloud-explorer.md).

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Visual Studio'da depolama kaynakları görüntülemek ve yönetmek

Sunucu Gezgini otomatik olarak depolama öykünücüsü hesabınızda BLOB, kuyruklar ve tablolar listesini gösterir. Depolama öykünücüsü hesabı altında Sunucu Gezgini içinde listelenen **depolama** düğümü olarak **geliştirme** düğümü.

Depolama öykünücüsü hesabın kaynakları görmek için genişletme **geliştirme** düğümü. ' Nı depolama öykünücüsünü başlatılmış kurmadı varsa **geliştirme** düğümü, otomatik olarak başlar. Bu işlem birkaç saniye sürebilir. Depolama öykünücüsü başlatılırken diğer Visual Studio alanlarında çalışmaya devam edebilirsiniz.

Bir depolama hesabında kaynakları görüntülemek için depolama hesabının gördüğünüz Sunucu Gezgininde düğümünü **BLOB'lar**, **sıraları**, ve **tabloları** düğümleri.

## <a name="work-with-blob-resources"></a>BLOB kaynakların ile çalışma

**BLOB'lar** düğümü kapsayıcıları seçilen depolama hesabı için bir liste görüntüler. BLOB kapsayıcıları blob dosyaları içerir ve bu BLOB'lar klasörler ve alt klasörler halinde düzenleyebilirsiniz. Daha fazla bilgi için bkz: [Blob storage kullanma konusunda](storage/blobs/storage-dotnet-how-to-use-blobs.md).

### <a name="to-create-a-blob-container"></a>Bir blob kapsayıcısını oluşturmak için

1. Kısayol menüsünü açın **BLOB'lar** düğümünü ve ardından **Blob kapsayıcısı oluşturmak**.
1. İçinde **Blob kapsayıcısı oluşturmak** iletişim kutusunda, yeni kapsayıcının adını girin.  
1. Klavyenizi veya Select girin tıklatın veya blob kapsayıcısında kaydetmek için ad alanı dışında dokunun.

   > [!NOTE]
   > Blob kapsayıcı adı bir sayı (0-9) veya küçük harf (a-z) ile başlamalıdır.

### <a name="to-delete-a-blob-container"></a>Bir blob kapsayıcısını silmek için

Kaldırın ve ardından istediğiniz blob kapsayıcısı için kısayol menüsünü açın **silmek**.

### <a name="to-display-a-list-of-the-items-in-a-blob-container"></a>Bir blob kapsayıcısında öğelerin listesini görüntülemek için

Listeden bir blob kapsayıcı adı için kısayol menüsünü açın ve ardından **açık**.

Bir blob kapsayıcı içeriğini görüntülediğinizde, blob kapsayıcı görünüm olarak bilinen bir sekmede görüntülenir.

![BLOB kapsayıcı görünümü](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

Blob kapsayıcı görünümü sağ üst köşesindeki düğmeleri kullanarak BLOB'ları üzerinde aşağıdaki işlemleri gerçekleştirebilirsiniz:

* Bir filtre değeri girin ve bunu uygulayabilirsiniz.
* BLOB'ları kapsayıcıda listesini yenileyin.
* Bir dosyayı karşıya yükleyin.
* Bir blobu silin. (Bir dosya bir blob kapsayıcısından temel alınan dosya silindiğinde değil. Bunu yalnızca blob kapsayıcısından kaldırır.)
* Bir blob açın.
* Bir blob yerel bilgisayara kaydedin.

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Bir klasörü veya alt klasör bir blob kapsayıcısında oluşturmak için

1. Blob kapsayıcısı Cloud Explorer'da seçin. Kapsayıcı penceresinde seçin **karşıya Blob** düğmesi.

1. İçinde **yeni dosyasını karşıya yükle** iletişim kutusunda **Gözat** karşıya yüklemek istediğiniz dosyayı belirtmek üzere düğmesine tıklayın ve ardından bir klasör adı girin **(isteğe bağlı) klasör** kutusu.

   ![Blob klasörüne bir dosya karşıya yükleme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

   Aynı adımını izleyerek kapsayıcı klasörlerde alt klasörler ekleyebilirsiniz. Bir klasör adı belirtmezseniz, dosyanın en üst düzeye blob kapsayıcısının yüklenir. Belirtilen klasöre kapsayıcısında dosya görünür.

   ![Klasör bir blob kapsayıcıya eklendi](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Klasörü çift tıklatın veya Enter klasörünün içeriğini görmek için seçin. Kapsayıcının klasöründe olduğunuzda kapsayıcı köküne seçerek dönebilirsiniz **açık üst dizin** (okunu).

### <a name="to-delete-a-container-folder"></a>Bir kapsayıcı klasörü silmek için

Klasördeki tüm dosyaları silin.

Blob kapsayıcıları klasörlerde sanal klasörlerdir için boş bir klasöre oluşturulamıyor. Ayrıca dosya içeriğini silmek için bir klasör silinemez, ancak bunun yerine klasörü silmek için bir klasörün tüm içeriğini silmeniz gerekir.

### <a name="to-filter-blobs-in-a-container"></a>BLOB'ları bir kapsayıcıda filtre uygulamak için

Ortak bir önek belirterek görüntülenen BLOB'ları filtreleyebilirsiniz.

Önek girerseniz, örneğin, **hello** filtre metni kutusuna ve ardından **yürütme** (**!**) "hello" ile başlayan BLOB'lar düğmesi görünür.

![Filtre metin kutusu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

Filtre metni kutusuna duyarlıdır ve joker karakterlerle filtreleme desteklemiyor. BLOB'ları yalnızca önekiyle filtrelenebilir. Sanal bir hiyerarşideki BLOB'lar düzenlemek için bir sınırlayıcı kullanıyorsanız öneki bir sınırlayıcı içerebilir. Örneğin, ön ekini temel filtreleme "HelloFabric /" Bu dize ile başlayan tüm BLOB'ları döndürür.

### <a name="to-download-blob-data"></a>BLOB verilerini yüklemek için

Cloud Explorer'da aşağıdaki yöntemlerden birini kullanın:

* Bir veya daha fazla BLOB'lar için kısayol menüsünü açın ve ardından **açık**.
* Blob adı seçin ve ardından **açık** düğmesi.
* Blob adını çift tıklatın.

Bir blob Yükleme ilerlemesini görünür **Azure etkinlik günlüğü** penceresi.

Bu dosya türü için varsayılan düzenleyicisinde blob açar. İşletim sistemi dosya türü tanısa, dosyayı yerel olarak yüklenmiş bir uygulamada açar. Aksi takdirde, blob dosya türü için uygun bir uygulama seçin istenir. Bir blob yüklediğinizde oluşturduğunuz yerel dosya salt okunur olarak işaretlendi.

BLOB verilerini yerel olarak önbelleğe ve Azure Blob depolamada blob'un son değiştirme zamanı karşılaştırılarak. Son yüklemenizden sonra blob güncelleştirilmişse, yeniden yüklenir. Aksi takdirde, blob yerel diskten yüklenir.

Varsayılan olarak, bir blob geçici bir dizine yüklenir. Belirli bir dizine BLOB indirmek için seçilen blob adları için kısayol menüsünü açın ve seçin **Kaydet**. Bu şekilde bir blob kaydettiğinizde, blob Dosya açılmadı ve yerel dosya okuma/yazma özniteliklerle oluşturulur.

### <a name="to-upload-blobs"></a>BLOB karşıya yüklemek için

BLOB'ları yüklemeye seçin **karşıya Blob** düğmesini kapsayıcı blob kapsayıcı görünümünde görüntülemek için açık olduğunda.

Karşıya yüklemek için bir veya daha fazla seçebilir ve tüm dosya türlerini karşıya yükleyebilirsiniz. **Azure etkinlik günlüğü** penceresi karşıya yükleme ilerlemesini gösterir. Blob verilerle çalışma hakkında daha fazla bilgi için bkz: [.NET ile Azure Blob storage kullanma](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>BLOB'larını transfer günlükleri görüntülemek için

Azure Tanılama verileri Azure uygulamanızı günlüğe kaydetmek için kullandığınız ve depolama hesabınıza günlükleri aktarılan Bu günlükler için oluşturulan Azure kapsayıcıları görürsünüz. Özellikle Azure dağıtıldıktan varsa Server Explorer'da bu günlükleri görüntüleme, uygulamanızın sorunları tanımlamak için kolay bir yoludur.

Azure Tanılama hakkında daha fazla bilgi için bkz: [kullanarak Azure tanılama tarafından günlüğü verilerini toplamak](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>URL için bir blob almak için

Blob'un kısayol menüsünü açın ve ardından **kopya URL**.

### <a name="to-edit-a-blob"></a>Bir blob düzenlemek için

Blob seçin ve ardından **açık Blob** düğmesi.

Dosyanın geçici bir konuma indirilir ve yerel bilgisayarda açılır. Blob değişiklikleri yaptıktan sonra yeniden yükleyin.

## <a name="work-with-queue-resources"></a>Queue kaynaklarına ile çalışma

Depolama Hizmetleri sorguları bir Azure depolama hesabında barındırılır. Bulut bir ileti geçirme mekanizması birbirleriyle ve diğer hizmetleri ile iletişim kurmak hizmet rollerinin izin verdiği için bunları kullanabilirsiniz. Sıranın program aracılığıyla bir bulut hizmeti aracılığıyla ve dış istemcilere web hizmeti üzerinden erişebilirsiniz. Sıranın Visual Studio'da doğrudan Sunucu Gezgini kullanarak da erişebilirsiniz.

Kuyrukları kullanan bir bulut hizmet geliştirirken sıraları oluşturmak ve bunlarla etkileşimli olarak geliştirmek ve kodunuzu test ederken çalışmak için Visual Studio kullanmak isteyebilirsiniz.

Sunucu Gezgini'nde, bir depolama hesabında sıralarını görüntüleyin, oluşturun ve sırayı silmek, kendi iletilerini görüntülemek için bir sırayı açmak ve iletileri kuyruğa Ekle. Bir kuyruk görüntüleme için açtığınızda, tek bir ileti görüntüleyebilir ve sol üst köşede düğmelerini kullanarak sıra üzerinde aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Kuyruk görünümü yenileyin.
* Kuyruğa bir ileti ekleyin.
* En üstteki ileti dequeue.
* Tüm sıranın temizleyin.

Aşağıdaki görüntü iki ileti içeren bir kuyruk göstermektedir:

![Bir kuyruğu görüntüleme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Kuyruklar Hizmetleri depolama hakkında daha fazla bilgi için bkz: [.NET kullanarak Azure kuyruk depolamaya başlayın](http://go.microsoft.com/fwlink/?LinkID=264702). Kuyruklar depolama hizmetleri için web hizmeti hakkında bilgi için bkz [kuyruk hizmeti kavramları](http://go.microsoft.com/fwlink/?LinkId=264788). Visual Studio kullanarak bir depolama hizmetleri sıraya ileti gönderme hakkında daha fazla bilgi için bkz: [depolama hizmetleri kuyruğuna iletiler gönderme](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> Depolama Hizmetleri sıraları Azure Service Bus sıralarından farklıdır. Service Bus kuyruklarını hakkında daha fazla bilgi için bkz: [Service Bus kuyrukları, konu başlıkları ve abonelikler](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-queues-topics-subscriptions).

## <a name="work-with-table-resources"></a>Tablo kaynakları ile çalışma

Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Kimliği doğrulanmış çağrılarından içinden ve dışından Azure bulut kabul eden bir NoSQL veri deposu hizmetidir. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

### <a name="to-create-a-table"></a>Bir tablo oluşturmak için

1. Cloud Explorer'da seçin **tabloları** depolama hesabı ve ardından düğümünün **Create Table**.
1. İçinde **Create Table** iletişim kutusunda, tablo için bir ad girin.

### <a name="to-view-table-data"></a>Tablo verileri görüntülemek için

1. Cloud Explorer'da açın **Azure** düğümünü ve ardından açın **depolama** düğümü.
1. İlginizi çekiyor mu ve açın depolama hesabı düğümünü açın **tabloları** depolama hesabı için tabloların bir listesini görmek için düğüm.
1. Bir tablo için kısayol menüsünü açın ve ardından **görünüm tablosu**.

    ![Çözüm Gezgini'nde bir Azure tablosu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tablo, varlıkları (satırlarda gösterilen) ve (sütunlarda gösterilir) özellikleri tarafından düzenlenir. Örneğin, bir sonraki çizimde Tablo Tasarımcısı'nda listelenen varlıkları gösterir.

### <a name="to-edit-table-data"></a>Tablo verisi düzenlemek için

Tablo Tasarımcısı'nda bir varlık (tek satır) ya da bir özellik (tek bir hücre) için kısayol menüsünü açın ve ardından **Düzenle**.

    ![Add or edit a table entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

Tek bir tabloyu varlıklarda özellikleri (sütunları) aynı kümesine sahip gerekli değildir. Tablo veri görüntüleme ve düzenleme aşağıdaki kısıtlamaları göz önünde bulundurun:

* Görüntüleyemez veya ikili verileri düzenleme (`type byte[]`), ancak bir tabloda depolayabilirsiniz.
* Düzenleyemezsiniz **PartitionKey** veya **RowKey** Azure Table storage bu işlemi desteklemediğinden, değerleri.
* Adlı bir özellik oluşturulamıyor **zaman damgası**. Azure storage Hizmetleri bu ada sahip bir özelliğini kullanın.
* Girerseniz bir **DateTime** değeri, bilgisayarınızın bölge ve dil ayarları uygun biçimde izlemelidir (örneğin, GG/AA/YYYY SS: dd: [AM | PM] İngilizce (ABD) için).

### <a name="to-add-entities"></a>Varlıkları eklemek için

1. Tablo Tasarımcısı'nda seçin **varlık Ekle** düğmesi.

    ![Varlık düğme ekleme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. İçinde **varlık Ekle** iletişim kutusunda, değerlerini girin **PartitionKey** ve **RowKey** özellikleri.

    ![Varlık Ekle iletişim kutusu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Değerleri dikkatle girin. Varlık silin ve yeniden ekleyin sürece iletişim kutusunu kapattıktan sonra onları değiştiremezsiniz.

### <a name="to-filter-entities"></a>Varlıkları filtre uygulamak için

Sorgu Oluşturucusu'nu kullanırsanız, bir tabloda görünen varlıkları kümesinin özelleştirebilirsiniz.

1. Sorgu Oluşturucusu'nu açmak için bir tablo görüntülemek için açın.
1. Seçin **Sorgu Oluşturucusu** Tablo görünümünün araç çubuğunda.

    **Sorgu Oluşturucusu** iletişim kutusu görüntülenir. Aşağıdaki çizimde oluşturulmakta olan bir sorgu Sorgu Oluşturucu'da gösterir.

    ![Sorgu Oluşturucusu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
1. Bitirdiğinizde, sorgu oluşturma iletişim kutusunu kapatın. Sorgu sonuç metin biçiminde bir metin kutusuna bir WCF Veri Hizmetleri filtre olarak görünür.
1. Sorguyu çalıştırmak için yeşil üçgenle simgesini seçin.

Ayrıca girerseniz filtre metin kutusundaki doğrudan bir WCF Veri Hizmetleri filtre dizesi Tablo Tasarımcısı'nda görüntülenen varlık verilerini filtre uygulayabilirsiniz. Bu tür bir dize bir SQL WHERE yan tümcesine benzer, ancak sunucuya bir HTTP isteği olarak gönderilir. Filtre dizeleri oluşturma hakkında daha fazla bilgi için bkz: [oluşturma filtre dizeleri Tablo Tasarımcısı için](https://msdn.microsoft.com/library/azure/ff683669.aspx).

Aşağıdaki çizimde bir geçerli filtre dizesi örneği gösterilmektedir:

![Filtre dizesi](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

## <a name="refresh-storage-data"></a>Depolama veri yenileme

Sunucu Gezgini bağlandığı veya bir depolama hesabından verileri alır, işlemi için bir dakika tamamlanması sürebilir. Sunucu Gezgini bağlanamıyorsanız, işlemi zaman aşımına olabilir. Veriler alınır, ancak Visual Studio'nun diğer bölümlerinde çalışmaya devam edebilirsiniz. Çok uzun sürüyorsa işlemi iptal etmek için seçin **Yenilemeyi Durdur** Sunucu Gezgini araç çubuğunda.

### <a name="to-refresh-blob-container-data"></a>BLOB kapsayıcı verileri yenilemek için

* Seçin **BLOB'lar** düğümün altında **depolama**ve ardından **yenileme** Sunucu Gezgini araç çubuğunda.
* Görüntülenen BLOB'lar listesini yenilemek için seçin **yürütme** düğmesi.

### <a name="to-refresh-table-data"></a>Tablo verileri yenilemek için

* Seçin **tabloları** düğümün altında **depolama**ve ardından **yenileme** Sunucu Gezgini araç çubuğunda.
* Tablo Tasarımcısı'nda görüntülenen varlıklar listesini yenilemek için seçin **yürütme** Tablo Tasarımcısı'nda düğmesi.

### <a name="to-refresh-queue-data"></a>Sıra verileri yenilemek için

Seçin **sıraları** düğümün altında **depolama**ve ardından **yenileme** Sunucu Gezgini araç çubuğunda.

### <a name="to-refresh-all-items-in-a-storage-account"></a>Bir depolama hesabındaki tüm öğeleri yenilemek için

Hesap adı seçin ve ardından **yenileme** Sunucu Gezgini araç çubuğunda.

## <a name="add-storage-accounts-by-using-server-explorer"></a>Sunucu Gezgini kullanarak depolama hesapları ekleme

Sunucu Gezgini kullanarak depolama hesapları eklemek için iki yolu vardır. Azure aboneliğinizde bir depolama hesabı oluşturabilir veya varolan bir depolama hesabı ekleyebilirsiniz.

### <a name="to-create-a-storage-account-by-using-server-explorer"></a>Sunucu Gezgini kullanarak bir depolama hesabı oluşturmak için

1. Server Explorer'da için kısayol menüsünü açın **depolama** düğümünü ve ardından **depolama hesabı oluştur**.

1. İçinde **depolama hesabı oluştur** iletişim kutusunda, seçin veya aşağıdaki bilgileri girin:

   * Depolama hesabı eklemek istediğiniz Azure aboneliği.
   * Yeni depolama hesabı için kullanmak istediğiniz adı.
   * Bölge veya benzeşim grubunda (örneğin, Batı ABD veya Doğu Asya).
   * Depolama hesabı için aşağıdaki gibi kullanmak istediğiniz çoğaltma türünü yerel olarak yedekli.

   ![Azure Storage hesabı oluşturma](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. **Oluştur**’u seçin.

Yeni depolama hesabı görünür **depolama** Çözüm Gezgini'nde listesi.

### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Sunucu Gezgini kullanarak mevcut bir depolama hesabını eklemek için

1. Sunucu Gezgini'nde, Azure için kısayol menüsünü açın **depolama** düğümünü ve ardından **harici depolama ekleme**.

    ![Varolan bir depolama hesabı ekleme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
1. İçinde **depolama hesabı oluştur** iletişim kutusunda, seçin veya aşağıdaki bilgileri girin:

   * Eklemek istediğiniz varolan depolama hesabı adı.
   * Seçilen depolama hesabı anahtarı. Bir depolama hesabı seçtiğinizde bu değer genellikle sizin için sağlanır. Depolama hesap anahtarını anımsa için Visual Studio istiyorsanız seçin **hesap anahtarını anımsa** onay kutusu.
   * Depolama hesabı, örneğin HTTP, HTTPS veya özel bir uç nokta bağlanmak için kullanılacak protokolü. Özel uç noktaları hakkında daha fazla bilgi için bkz: [yapılandırmak bağlantı dizeleri nasıl](https://msdn.microsoft.com/library/azure/ee758697.aspx).

### <a name="to-view-the-secondary-endpoints"></a>İkincil uç noktaları görüntülemek için

Kullanarak bir depolama hesabı oluşturduysanız **okuma erişimli coğrafi olarak yedekli** çoğaltma seçeneği, kendi ikincil uç noktaları hesap adı için kısayol menüsünü açarak görüntüleyebilir ve ardından **özellikleri**.

![Depolama ikincil uç noktaları](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Server Explorer'dan bir depolama hesabını kaldırmak için

Sunucu Gezgini'nde, hesap adı için kısayol menüsünü açın ve ardından **silmek**. 

Bir depolama hesabı silerseniz, bu hesap için kaydedilen tüm anahtar bilgileri de kaldırılır.

Server Explorer'dan bir depolama hesabı silerseniz, depolama hesabınız veya onu içeren herhangi bir veri etkilemez. Server Explorer'dan yalnızca başvuru kaldırır. Bir depolama hesabı kalıcı olarak silmek için kullanın [Azure portal](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

Azure storage Hizmetleri kullanma hakkında daha fazla bilgi için bkz: [Azure depolama hizmetlerine erişilmesi](https://msdn.microsoft.com/library/azure/ee405490.aspx).
