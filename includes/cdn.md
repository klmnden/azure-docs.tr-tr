# <a name="using-cdn-for-azure"></a>Azure için CDN kullanma
Azure içerik teslim ağı (CDN) geliştiriciler BLOB'ları ve işlem örnekleri ABD, Avrupa, Asya, Avustralya ve Güney Amerika fiziksel düğümlerde, statik içeriği önbelleğe alarak yüksek bant genişliği içeriği teslimi için genel bir çözüm sunar. CDN düğümü konumları güncel bir listesi için bkz: [Azure CDN düğümü konumları].

Bu görev aşağıdaki adımları içerir:

* [1. adım: depolama hesabı oluşturma](#Step1)
* [2. adım: depolama hesabınız için yeni bir CDN uç noktası oluşturma](#Step2)
* [3. adım: CDN içeriğinize erişebilirsiniz](#Step3)
* [4. adım: CDN içeriğinizi Kaldır](#Step4)

Azure veriyi önbelleğe almak için CDN kullanmanın avantajları şunlardır:

* Son kullanıcılar için bir içerik kaynağı gölgeden uzak ve burada birçok 'Internet dönüşleri' içerik yüklemek için gerekli uygulamaları kullanarak, daha iyi performans ve kullanıcı deneyimi
* Bir ürün sunumu gibi bir olay başlangıcında daha iyi anlık yüksek düzeyde yükü işlemek için dağıtılmış büyük ölçekli söyleyin

Var olan CDN müşterileri artık Azure CDN'de kullanmak [Klasik Azure portalı]. CDN bir eklenti aboneliğinizde bir özelliktir ve ayrı bir sahip [fatura planı].

<a id="Step1"> </a>

<h2>1. adım: depolama hesabı oluşturma</h2>

Bir Azure aboneliği için yeni bir depolama hesabı oluşturmak için aşağıdaki yordamı kullanın. Bir depolama hesabı için Azure storage hizmetlerine erişmenizi sağlar. Depolama hesabı Azure depolama hizmeti bileşenlerinin her biri erişmek için ad alanı en yüksek düzeyde temsil eder: Blob Hizmetleri, kuyruk Hizmetleri ve Tablo Hizmetleri. Azure depolama hizmetleri hakkında daha fazla bilgi için bkz: [Azure Storage Hizmetleri kullanarak](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Bir depolama hesabı oluşturmak için Hizmet Yöneticisi veya ilişkili abonelik için bir ortak yönetici olması gerekir.

> [!NOTE]
> Azure Hizmet Yönetimi API'sini kullanarak bu işlemi gerçekleştirme hakkında daha fazla bilgi için bkz: [depolama hesabı oluştur](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) başvuru konusu.
> 
> 

**Bir Azure aboneliği için bir depolama hesabı oluşturmak için**

1. [Klasik Azure portalı] oturum açın.
2. Sol alt köşede tıklatın **yeni**. İçinde **yeni** iletişim kutusunda **Veri Hizmetleri**, ardından **depolama**, ardından **hızlı Oluştur**.
   
   **Depolama hesabı oluştur** iletişim kutusu görüntülenir.
   
   ![Depolama hesabı oluşturma][create-new-storage-account]
3. İçinde **URL** alan, bir alt etki alanı adı yazın. Bu giriş, 3-24 küçük harfler ve sayılar içerebilir.
   
    Bu değer aboneliği için Blob, kuyruk veya tablo kaynak adres için kullanılan URI içindeki konak adına dönüşür. Blob hizmetinde kapsayıcı kaynak adres için aşağıdaki biçimde bir URI kullanırsınız. burada  *&lt;StorageAccountLabel&gt;*  , yazdığınız değer başvurduğu **birURLgirin**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Önemli:** URL depolama hesabının URI alt etki alanı forms etiket ve azure'da barındırılan tüm hizmetler arasında benzersiz olması gerekir.
   
    Bu değer, aynı zamanda bu hesabı program aracılığıyla erişirken Portalı'nda veya bu depolama hesabı adı olarak kullanılır.
4. Gelen **bölge/benzeşim grubu** bir bölge veya benzeşim grubunda depolama hesabı için aşağı açılan listesinde seçin. Depolama hizmetlerinizi kullandığınız diğer Windows Azure hizmetleriyle aynı veri merkezinde olmasını istiyorsanız bir bölge yerine bir benzeşim grubu seçin. Performansı geliştirebilir ve çıkışı için hiçbir Ücret tahakkuk eden.  
   
   **Not:** bir benzeşim grubu oluşturmak için açık **ayarları** Yönetim Portalı'nı bölümünü tıklatın **benzeşim grupları**ve ardından ya da **benzeşimgrubuEkle** veya **eklemek**. Ayrıca, oluşturun ve Windows Azure Hizmet Yönetimi API'sini kullanarak benzeşim grupları yönetin. Daha fazla bilgi için [benzeşim gruplarında işlemler] bakın.
5. Gelen **abonelik** aşağı açılan listesinde, depolama hesabı ile kullanılacak aboneliği seçin.
6. **Depolama Hesabı Oluştur**’a tıklayın. Depolama hesabı oluşturma işleminin tamamlanması birkaç dakika sürebilir.
7. Depolama hesabı başarıyla oluşturulduğunu doğrulamak için hesap için listelenen öğeleri göründüğünü doğrulayın **depolama** durumuyla **çevrimiçi**.

<a id="Step2"> </a>

<h2>2. adım: depolama hesabınız için yeni bir CDN uç noktası oluşturma</h2>

Bir depolama hesabı için CDN erişimi etkinleştirmek ya da barındırılan hizmetin sonra genel olarak kullanılabilir tüm nesneler CDN uç önbelleğe alma için uygundur. CDN şu anda önbelleğe alınmış nesneyi değiştirirseniz, yaşam süresi önbelleğe alınan içerik süresi sona erdiğinde, CDN içeriğini yeniler kadar yeni içerik CDN kullanılabilir olmayacaktır.

**Depolama hesabınız için yeni bir CDN uç noktası oluşturmak için**

1. İçinde [Klasik Azure portalı], Gezinti bölmesinde **CDN**.
2. Şerit'te tıklatın **yeni**. İçinde **yeni** iletişim kutusunda **uygulama hizmetleri**, ardından **CDN**, ardından **hızlı Oluştur**.
3. İçinde **kaynak etki alanı** açılan listesinde, depolama birimi seçin önceki bölümde, kullanılabilir depolama hesapları listesinden oluşturduğunuz hesap. 
4. ' I tıklatın **oluşturma** düğmesi yeni uç noktası oluşturun.
5. Uç nokta oluşturulduktan sonra abonelik için uç noktalar listesinde görünür. Liste görünümünde, kaynak etki alanının yanı sıra, önbelleğe alınan içeriğe erişmek için kullanılacak URL gösterilir. 
   
    Kaynak etki alanı içinden CDN içeriği önbelleğe alan konumdur. Kaynak etki alanı, bir depolama hesabı veya bir bulut hizmeti olabilir; bir depolama hesabı bu örnek amacıyla kullanılır. Depolama içeriği belirttiğiniz bir ön bellek denetimi ayarı, veya varsayılan buluşsal yöntemler önbelleğe alma ağın göre uç sunucularına önbelleğe alınır. 

    > [AZURE.NOTE]Uç nokta için oluşturulan yapılandırma hemen kullanılabilir olmaz; kaydın CDN ağ üzerinden yayılması için 60 dakika kadar sürebilir. İçerik CDN kullanılabilir hale gelene kadar hemen CDN etki alanı adını kullanmayı deneyen kullanıcılar durum kodu 400 (Hatalı istek) alabilir.

<a id="Step3"> </a>

<h2>Adım 3: Erişim CDN içerik</h2> 

CDN önbelleğe alınmış içeriğe erişmek için CDN Portal'da sağlanan URL'yi kullanın. Önbelleğe alınan bir blob adresi aşağıdakine benzer olacaktır:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>

<h2>4. adım: içerik CDN Kaldır</h2>

Artık bir nesne içinde Azure içerik teslim ağı (CDN) önbelleğe almak istiyorsanız, aşağıdaki adımlardan birini gerçekleştirebilirsiniz:

* Bir Azure blob'u için ortak kapsayıcıdan blob silebilirsiniz.
* Kapsayıcı yapabileceğiniz ortak yerine özel. Bkz: [kapsayıcılara ve Blob'lara erişimi kısıtla](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) daha fazla bilgi için.
* Yönetim Portalı'nı kullanarak CDN uç noktasını silmek ya da devre dışı bırakın.
* Barındırılan hizmet artık nesne için isteklere yanıt verecek şekilde değiştirebilirsiniz.

Zaten CDN önbelleğe alınmış bir nesne, nesne yaşam süresi boyunca süresi doluncaya kadar önbelleğe alınmış olarak kalır. Yaşam süresi süresi sona erdiğinde, CDN CDN uç noktası hala geçerli olup olmadığını ve hala anonim olarak erişilebilir nesne görmek için kontrol eder. Değilse, ardından nesne artık önbelleğe alınır.

Hemen içerik temizleme olanağı Azure Yönetim Portalı'ndaki şu anda desteklenmiyor. Temasa [Azure Destek](https://azure.microsoft.com/support/options/) hemen içeriği temizlenecek gerekiyorsa. 

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure'da bir benzeşim grubu oluşturma]
* [Nasıl yapılır: Azure aboneliği için depolama hesaplarını yönetme]
* [Hizmet Yönetimi API hakkında]
* [CDN İçeriğini Özel Etki Alanı ile Eşleme]

[Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
[Azure CDN düğümü konumları]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
[Klasik Azure portalı]: https://manage.windowsazure.com/
[fatura planı]: /pricing/calculator/?scenario=full
[Azure'da bir benzeşim grubu oluşturma]: http://msdn.microsoft.com/library/azure/ee460798.aspx
[Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
[Hizmet Yönetimi API hakkında]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
[CDN İçeriğini Özel Etki Alanı ile Eşleme]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
