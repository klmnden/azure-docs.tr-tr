<properties
    pageTitle="Klasik Azure Portalı’nda depolama hesabı oluşturma, yönetme veya silme | Microsoft Azure"
    description="Azure Portal’da yeni bir depolama hesabı oluşturun, hesap erişim tuşlarınızı yönetin veya bir depolama hesabını silin. Standart ve premium depolama hesapları hakkında bilgi edinin."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="micurd;robinsh"/>



# Azure Storage hesapları hakkında

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## Genel Bakış

Azure Storage hesabı Azure Storage Azure Blob, Kuyruk, Tablo ve Dosya hizmetlere erişmenizi sağlar. Depolama hesabınız Azure Storage veri nesneleriniz için benzersiz ad alanı sağlar. Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir.

İki tür depolama hesabı vardır:

- Standart depolama hesabı Blob, Tablo, Kuyruk ve File Storage içerir.
- Bir premium depolama hesabı şu anda yalnızca Azure Virtual Machine disklerini destekler. Premium Storage için ayrıntılı genel bakış için bkz. [Premium Storage: Azure Virtual Machines İş Yükleri için Yüksek Performanslı Depolama](storage-premium-storage.md).

## Depolama hesabı faturalama

Depolama hesabınıza bağlı olarak Azure Storage kullanımınıza göre faturalandırılırsınız. Depolama maliyetleri dört etkene bağlıdır: depolama kapasitesi, çoğaltma düzeni, depolama işlemleri ve veri çıkışı.

- Depolama kapasitesi, veri depolamak için kullandığınız depolama hesabı payınızın kullandığınız kısmını belirtir. Verinizi depolamanızın maliyeti, depoladığınız veri miktarı ve çoğaltılma şekline bağlıdır.
- Çoğaltma, verilerinizin aynı anda tutulan kopya sayısına ve tutulduğu konumu belirtir.
- İşlemler, Azure Storage’a yapılan tüm okuma ve yazma işlemlerini ifade eder.
- Veri çıkışı bir Azure bölgesinin dışına aktarılan verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalıştırılmayan bir uygulama eriştiğinde, uygulamanın bir bulut hizmeti veya farklı bir uygulama olmasına bakılmaksızın veri çıkışı için ücretlendirilirsiniz. (Azure hizmetleri için, veri çıkış ücretlerini azaltmak veya ortadan kaldırmak için veri ve hizmetlerinizi aynı veri merkezlerinde gruplamak üzere adım atabilirsiniz.)  

[Azure Storage Fiyatlama](https://azure.microsoft.com/pricing/details/storage) sayfası depolama kapasitesi, çoğaltma ve işlemler için ayrıntılı fiyatlandırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/) sayfası veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar.

Depolama hesabının kapasite ve performans hedefleri hakkında ayrıntılar için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](storage-scalability-targets.md).

> [AZURE.NOTE] Bir Azure Virtual Machine oluşturduğunuzda, ilgili konumda bir depolama hesabınız yoksa, depolama konumunda otomatik olarak bir depolama hesabı oluşturulur. Bu nedenle, sanal makine diskleriniz için bir depolama hesabı oluşturmak üzere aşağıdaki adımları izlemeniz gerekli değildir. Depolama hesabı adı sanal makine adına dayalı olarak belirlenir. Daha fazla ayrıntı için bkz. [Azure Virtual Machines belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

## Depolama hesabı oluşturma

1. [Klasik Azure Portalı](https://manage.windowsazure.com)’nda oturum açın.

2. Sayfanın altındaki görev çubuğunda **Yeni**’ye tıklayın. **Veri Hizmetleri** | **Depolama**, ve ardından **Hızlı Oluştur**’u seçin.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. **URL**’ye depolama hesabınız için bir ad girin.

    > [AZURE.NOTE] Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
    >  
    > Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. Seçtiğiniz depolama hesabı adı alınmışsa Klasik Azure Portalı bildirecektir.

    Depolama hesabınızın adının Azure Storage’da nesnelerinizin ele alınması için nasıl kullanılacağı hakkında ayrıntılar için bkz. [Depolama hesabı uç noktaları](#storage-account-endpoints).

4. **Konum/Benzeşim Grubu** içinde depolama hesabınız için size veya müşterilerinize yakın bir konum seçin. Depolama hesabınızdaki verilere Azure Virtual Machine veya bulut hizmeti gibi başka bir Azure hizmeti erişecek olursa, performansı artırmak ve maliyetleri düşürmek üzere depolama hesabınızı kullandığınız diğer Azure hizmetleriyle aynı veri merkezinde gruplamak üzere listeden bir benzeşim grubu seçebilirsiniz.

    Depolama hesabınız oluşturulduğunda bir benzeşim grubu seçmeniz gerektiğini unutmayın. Mevcut bir hesabı bir benzeşim grubuna taşıyamazsınız. Benzeşim grupları hakkında daha fazla bilgi için bkz. [Benzeşim grubu ile hizmeti aynı konuma yerleştirme](#service-co-location-with-an-affinity-group).

    >[AZURE.IMPORTANT] Aboneliğiniz için hangi konumların kullanılabilir olduğunu belirlemek üzere [Tüm kaynak sağlayıcıları listele](https://msdn.microsoft.com/library/azure/dn790524.aspx) işlemini çağırabilirsiniz. Sağlayıcıları PowerShell üzerinden listelemek için [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx)’ı kullanın .NET üzerinden ProviderOperationsExtensions sınıfının [Listeleme](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) yöntemini kullanın.
    >
    >Ayrıca hangi bölgede hangi hizmetin sağlandığına dair daha fazla bilgi için bkz.[Azure Bölgeleri](https://azure.microsoft.com/regions/#services).


5. Birden fazla Azure aboneliğiniz varsa **Abonelik** alanı görüntülenir. **Abonelik** alanında depolama hesabı ile kullanmak istediğiniz Azure aboneliğini girin.

6. **Çoğaltma** alanında depolama hesabınız için istenilen çoğaltma seviyesini belirleyin. Verileriniz için en üst düzeyde dayanıklılık sağlayan coğrafi bölgede yedekli çoğalma seçeneği önerilir. Azure Storage çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).

6. **Depolama Hesabı Oluştur**’a tıklayın.

    Depolama hesabınızı oluşturmak birkaç dakika sürebilir. Durumu denetlemek için Klasik Azure Portalı’nın altındaki bildirimlere göz atabilirsiniz. Depolama hesabı oluşturulduktan sonra yeni depolama hesabınızın durumu **Çevrimiçi** olur ve kullanıma hazır hale gelir.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### Depolama hesabı uç noktaları

Azure Storage’da depoladığınız her nesnenin benzersiz bir URL adresi vardır. Depolama hesabı adı o adresin alt etki alanı adını oluşturur. Her hizmete özel alt etki alanı ve etki alanı birleşimi depolama hesabınız için bir *uç nokta* oluşturur.

Örneğin depolama hesabınızın adı *mystorageaccount* ise, depolama hesabınız için varsayılan uç noktalar şunlardır:

- Blob hizmeti: http://*mystorageaccount*.blob.core.windows.net

- Tablo hizmeti: http://*mystorageaccount*.table.core.windows.net

- Kuyruk hizmeti: http://*mystorageaccount*.queue.core.windows.net

- Dosya hizmeti: http://*mystorageaccount*.file.core.windows.net

Hesap oluşturulduktan sonra [Klasik Azure Portalı](https://manage.windowsazure.com)’nda depolama panosunda depolama hesabınızın uç noktalarını görebilirsiniz.

Bir depolama hesabındaki bir nesneye erişmek için gerekli URL, nesnenin depolama hesabındaki konumunun uç noktaya eklenmesiyle oluşturulur. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca depolama hesabınız ile birlikte kullanmak üzere özel bir etki alanı adı yapılandırabilirsiniz. Ayrıntılar için bkz. [Blob Storage uç noktanız için özel bir etki alanı adı yapılandırma](storage-custom-domain-name.md).

### Hizmeti benzeşim grubu ile birlikte bulundurma

*Benzeşim grubu*, Azure hizmetlerinizi ve VM’lerinizi Azure Storage hesabınızla coğrafi olarak gruplamaktır. Bir benzeşim grubu bilgisayar iş yüklerini aynı veri merkezinde veya hedef kullanıcının hedef kitlesine yakın bir konuma yerleştirerek hizmet performansını iyileştirebilir. Bunun yanında depolama hesabındaki bir veriye aynı benzeşim grubunun bir parçası olan başka bir hizmet eriştiğinde veri çıkışı ücretlendirmesi yapılmaz.

> [AZURE.NOTE]  Benzeşim grubu oluşturmak için [Klasik Azure Portalı](https://manage.windowsazure.com)’nın içindeki <b>Ayarlar</b> alanını açın , <b>Benzeşim Grupları</b>’na tıklayın ve <b>Benzeşim grubu ekle</b> veya <b>Ekle</b> düğmesine tıklayın. Ayrıca Azure Hizmet Yönetim API’sini kullanarak benzeşim grupları oluşturabilir ve bunları yönetebilirsiniz. Daha fazla bilgi için bkz. <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Benzeşim gruplarında işlemler</a>.

## Depolama erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma

Bir depolama hesabı oluşturduğunuzda Azure, depolama hesabına erişim sağlandığında kimlik doğrulama için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sağlayarak AAzure Storage izmetinizde herhangi bir kesinti olmadan veya ilgili hizmete erişim sağlamaya gerek kalmadan anahtarları yeniden oluşturmanızı sağlar.

> [AZURE.NOTE] Depolama erişim tuşlarınızı başkalarıyla paylaşmaktan kaçınmanızı öneririz. Erişim tuşlarınızı vermeden depolama kaynaklarına erişim izni vermek için bir *paylaşılan erişim imzası* kullanabilirsiniz. Paylaşılan erişim imzası, hesabınızdaki bir kaynağa belirlediğiniz zaman diliminde belirlediğiniz izinlerle erişilmesini sağlar. Daha fazla bilgi edinmek için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).

Blob, Tablo ve Kuyruk hizmetlerine erişmek üzere kullanılarak depolama erişim tuşlarını görüntülemek, kopyalamak ve yeniden oluşturmak için [Klasik Azure Portalı](https://manage.windowsazure.com)’nda panoda **Anahtarları Yönet**’i veya **Depolama** sayfasını kullanın.

### Depolama erişim tuşu kopyalama  

Bir bağlantı dizesinde kullanmak üzere depolama erişim tuşunu kopyalamak için **Anahtarları Yönet**’i kullanabilirsiniz. Bağlantı dizesi, kimlik doğrulamada kullanılmak üzere depolama hesabı adı ve bir anahtar kullanır. Azure Storage hizmetlerine erişmek için bağlantı dizelerini yapılandırma hakkında daha fazla bilgi için bkz. [Azure Storage Bağlantı Dizelerini Yapılandırma](storage-configure-connection-string.md).

1. [Klasik Azure Portalı](https://manage.windowsazure.com)’nda **Depolama**’ya tıklayın, ardından panoyu açmak için depolama hesabının adına tıklayın.

2. **Anahtarları Yönet**’i tıklayın.

    **Erişim Tuşlarını Yönet** açılır.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Depolama erişim tuşunu kopyalamak için anahtar metnini seçin. Ardından sağ tıklayarak **Kopyala**’ya tıklayın.

### Depolama erişim tuşlarını yeniden oluşturma
Depolama bağlantılarınızı güvenli tutmaya yardımcı olmak üzere depolama hesabınıza erişim tuşlarını düzenli aralıklarla değiştirmenizi öneririz. Bir erişim tuşunu kullanarak depolama hesabına bağlantıları sağlamak ve diğer anahtarı yeniden oluşturmak üzere kullanmanız için iki erişim tuşu atanır.

> [AZURE.WARNING] Erişim tuşlarınızı yeniden oluşturmak Azure’daki hizmetleri ve depolama hesabına bağlı kendi uygulamalarınızı etkileyebilir. Depolama hesabına erişmek için erişim tuşunu kullanan tüm istemciler yeni anahtarı kullanmak üzere güncelleştirilmelidir.

**Media Services** - Depolama hesabınıza bağlı medya hizmetleri varsa, anahtarları yeniden oluşturduktan sonra erişim tuşlarını medya hizmetlerinizle yeniden eşitlemeniz gerekir.

**Uygulamalar** - Depolama hesabını kullanan web uygulamalarınız veya bulut hizmetleriniz varsa, yeniden anahtar oluşturmanız durumunda, anahtarları toplamazsanız bağlantıları kaybedeceksiniz. 

**Depolama Gezginleri** - Herhangi bir [depolama gezgin uygulaması](storage-explorers.md) kullanıyorsanız büyük olasılıkla söz konusu uygulamaların kullandığı depolama anahtarını güncelleştirmeniz gerekir.

Depolama erişim tuşlarınızı şu şekilde döndürebilirsiniz:

1. Depolama hesabının ikinci erişim tuşunu referans olarak göstermek için uygulama kodunuzdaki bağlantı dizinlerini güncelleştirin.

2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. [Klasik Azure Portalı](https://manage.windowsazure.com)’nda panodan veya **Yapılandır** sayfasından **Anahtarları Yönet**’e tıklayın. Birincil erişim tuşunun altından **Yeniden Oluştur**’u seçin ve yeni bir anahtar oluşturmak istediğinizi onaylamak için **Evet**’e tıklayın.

3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.

4. İkincil erişim tuşunu yeniden oluşturun.

## Bir depolama hesabını silme

Artık kullanmadığınız bir depolama hesabını kaldırmak için panoda**Sil** ‘i veya **Yapılandır** sayfasını kullanın. **Sil**, hesaptaki bloblar, tablolar ve kuyruklar dahil olmak üzere tüm depolama hesabının tamamını siler.

> [AZURE.WARNING] Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.
>
> Depolama hesabınız VHD dosyaları veya bir Azure Virtual Machine içeriyorsa, depolama hesabını silmeden önce bu VHD dosyalarını kullanan tüm görüntü ve diskleri silmeniz gerekir. İlk olarak, sanal makine çalışıyorsa durdurun ve silin. Diskleri silmek için **Diskler** sekmesine gidin ve orada görüntülenen tüm diskleri silin. Görüntüleri silmek için **Görüntüler** sekmesine gidin ve hesapta yer alan tüm görüntüleri silin.

1. [Klasik Azure Portalı](https://manage.windowsazure.com)’nda **Depolama**’ya tıklayın.

2. Ad alanı dışında depolama hesabı girişinde herhangi bir yere tıklayın ve ardından**Sil**’e tıklayın.

     -Veya-

    Panoyu açmak için depolama hesabının adına tıklayın ve ardından **Sil**’e tıklayın.

3. Depolama hesabını silme isteğinizi onaylamak için **Evet**’e tıklayın.

## Sonraki adımlar

- Azure Storage hakkında daha fazla bilgi için bkz. [Azure Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/).
- [Azure Storage ekip blogunu](http://blogs.msdn.com/b/windowsazurestorage/) ziyaret edin.
- [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)



<!--HONumber=Sep16_HO3-->


