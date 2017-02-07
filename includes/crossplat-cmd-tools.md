# <a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Mac ve Linux için Azure Komut Satırı Araçlarını kullanma
Bu kılavuzda Azure’da hizmet oluşturup yönetmek amacıyla Mac ve Linux için Azure Komut Satırı Araçlarını kullanma işlemi açıklanmaktadır. **Araçları yükleme**, **yayımlama ayarlarını içeri aktarma**, **Azure Web Siteleri oluşturma ve yönetme** ile **Azure Sanal Makineleri oluşturma yönetme** senaryoları ele alınmaktadır. Kapsamlı başvuru belgeleri için bkz. [Mac ve Linux için Azure komut satırı aracı belgeleri][reference-docs]. 

## <a name="table-of-contents"></a>İçindekiler
* [Mac ve Linux için Azure Komut Satırı Araçları nelerdir](#Overview)
* [Mac ve Linux için Azure Komut Satırı Araçlarını yükleme](#Download)
* [Azure hesabı oluşturma](#CreateAccount)
* [Yayımlama ayarlarını indirme ve içeri aktarma](#Account)
* [Azure Web Sitesi oluşturma ve yönetme](#WebSites)
* [Azure Sanal Makinesi oluşturma ve yönetme](#VMs)

<h2><a id="Overview"></a>Mac ve Linux için Azure Komut Satırı Araçları nelerdir</h2>

Mac ve Linux için Azure Komut Satırı Araçları, Azure hizmetlerinin dağıtımına ve yönetimine yönelik bir dizi komut satırı aracıdır.

Aşağıdaki görevler desteklenir:

* Yayımlama ayarlarını içeri aktarma.
* Azure Web Siteleri oluşturma ve yönetme.
* Azure Sanal Makineler oluşturma ve yönetme.

Desteklenen komutların tam listesi için araçları yükledikten sonra komut satırına `azure -help` yazın veya [başvuru belgelerine][reference-docs] bakın.

<h2><a id="Download">Mac ve Linux için Azure Komut Satırı Araçlarını yükleme</a></h2>

Aşağıdaki liste, işletim sisteminize bağlı olarak komut satırı araçlarını yüklemeye yönelik bilgiler içerir:

* **Mac**: [Azure SDK Yükleyicisi][mac-installer]’ni indirin. İndirilen .pkg dosyasını açıp, istenen şekilde yükleme adımlarını tamamlayın.
* **Linux**: En son [Node.js][nodejs-org] sürümünü yükleyin (bkz. [Paket Yöneticisi ile Node.js yükleme][install-node-linux]), ardından aşağıdaki komutu çalıştırın:
  
        npm install azure-cli -g
  
    **Not**: Bu komutu yükseltilmiş ayrıcalıklarla çalıştırmanız gerekebilir:
  
        sudo npm install azure-cli -g
* **Windows**: Winows yükleyicisini (.msi dosyası) çalıştırın. Şurada bulabilirsiniz: [Azure Komut Satırı Araçları][windows-installer].

Yüklemeyi test etmek için komut istemine `azure` yazın. Yükleme başarılı olduysa, kullanılabilir tüm `azure` komutlarının listesini görürsünüz.

<h2><a id="CreateAccount"></a>Azure hesabı oluşturma</h2>

Mac ve Linux için Azure Komut Satırı Araçlarını kullanmak istiyorsanız bir Azure hesabı gereklidir.

Bir web tarayıcısı açıp [http://www.windowsazure.com][windowsazuredotcom] adresine gidin ve sağ üst köşedeki **ücretsiz hesap** öğesine tıklayın.

![Azure Web Sitesi][Azure Web Site]

Bir hesap oluşturmak için yönergeleri izleyin.

<h2><a id="Account"></a>Yayımlama ayarlarını indirme ve içeri aktarma</h2>

Başlamak için ilk olarak yayımlama ayarlarınızı indirip içeri aktarmanız gerekir. Bunun yapılması, Azure Hizmetleri oluşturup yönetmeye yönelik araçlar kullanmanıza olanak tanır. Yayımlama ayarlarınızı indirmek için `account download` komutunu kullanın:

    azure account download

Bu işlem varsayılan tarayıcınızı açar ve Yönetim Portalı’nda oturum açmanızı ister. Oturum açtıktan sonra `.publishsettings` dosyanız indirilir. Bu dosyanın kaydedildiği yeri not edin.

Ardından, aşağıdaki komutta `{path to .publishsettings file}` ifadesini `.publishsettings` dosyanızın yoluyla değiştirip komutu çalıştırarak `.publishsettings` dosyasını içeri aktarın:

    azure account import {path to .publishsettings file}

<code>import</code> komutu tarafından depolanan tüm bilgileri <code>account clear</code> komutunu kullanarak kaldırabilirsiniz:

    azure account clear

`account` komutlarına ilişkin seçeneklerin listesini görmek için `-help` seçeneğini kullanın:

    azure account -help

Yayımlama ayarlarını içeri aktardıktan sonra güvenlik nedeniyle `.publishsettings` dosyasını silmeniz gerekir.

> [!NOTE]
> Yayımlama ayarlarınızı içeri aktarırken, Azure aboneliğinize erişmeye ilişkin kimlik bilgileri `user` klasörünüzün içine depolanır. `user` klasörünüz işletim sisteminiz tarafından korunur. Ancak, `user` klasörünüzü şifrelemek için ek adımlar uygulamanız önerilir. Bunu aşağıdaki yöntemlerle yapabilirsiniz:    
> 
> * Windows üzerinde klasör özelliklerini değiştirin ya da BitLocker'ı kullanın.
> * Mac üzerinde klasörün FileVault özelliğini açın.
> * Ubuntu’da Şifreli Giriş dizini özelliğini kullanın. Diğer Linux dağıtımları eşdeğer özellikler sunar.
> 
> 

Azure Web Siteleri ve Azure Sanal Makineleri oluşturmak ve yönetmek için artık hazırsınız.  

<h2><a id="WebSites"></a>Azure Web Sitesi oluşturma ve yönetme</h2>

### <a name="create-a-website"></a>Web sitesi oluşturma
Bir Azure web sitesi oluşturmak için ilk olarak `MySite` adlı boş bir dizin oluşturun ve bu dizine göz atın.

Ardından şu komutu çalıştırın:

    azure site create MySite --git

Bu komutun çıktısı, yeni oluşturulan web sitesi için varsayılan URL'yi içerir. `--git` seçeneği yerel uygulama dizininizde ve web sitenizin veri merkezinde git depoları oluşturarak web sitenizde git ile yayımlama yapmanıza olanak tanır. Yerel klasörünüz zaten bir git deposu ise bu komut var olan depoya web sitenizin veri merkezindeki depoyu işaret eden yeni bir uzak öğe ekler.

`azure site create` komutunu aşağıdaki seçeneklerden herhangi biriyle yürütebilirsiniz:

* `--location [location name]`. Bu seçenek, web sitenizin oluşturulduğu veri merkezinin konumunu belirtmenize olanak tanır (örn. "Batı ABD"). Bu seçeneği kullanmazsanız bir konum seçmeniz istenir.
* `--hostname [custom host name]`. Bu seçenek web siteniz için özel bir ana bilgisayar adı belirtmenize olanak tanır.

Bundan sonra web sitenizin dizinine içerik ekleyebilirsiniz. İçeriğinizi kaydetmek için normal git akışını (`git add`, `git commit`) kullanın. Web sitenizin içeriğini Azure'a göndermek için aşağıdaki git komutunu kullanın: 

    git push azure master

### <a name="set-up-publishing-from-github"></a>GitHub'dan yayımlamayı ayarlama
Bir GitHub deposundan sürekli yayımlamayı ayarlamak için bir site oluştururken `--GitHub` seçeneğini kullanın:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

GitHub deposunun yerel bir kopyasına sahipseniz veya GitHub deposuna tek bir uzak başvurusu olan deponuz varsa, bu komut GitHub deposundaki kodu sitenizde otomatik olarak yayımlar. Bu noktadan sonra, GitHub deposuna gönderilen tüm değişiklikler sitenizde otomatik olarak yayımlanır.

GitHub’dan yayımlamayı ayarladığınızda kullanılan varsayılan dal, ana daldır. Farklı bir dal belirtmek için yerel deponuzdan aşağıdaki komutu yürütün:

    azure site repository <branch name>

### <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma
Uygulama ayarları, çalışma zamanında uygulamanız için kullanılabilen anahtar-değer çiftleridir. Bir Azure Web Sitesi için belirlediğiniz uygulama ayar değerleri, sitenizin Web.config dosyasında tanımlanmış aynı anahtara sahip ayarları geçersiz kılar. Node.js ve PHP uygulamaları için uygulama ayarları, ortam değişkenleri olarak kullanılabilir. Aşağıdaki örnekte bir anahtar-değer çiftinin nasıl ayarlandığı gösterilmektedir:

    azure site config add <key>=<value> 

Tüm anahtar/değer çiftlerinin listesini görmek için aşağıdakileri kullanın:

    azure site config list 

Veya anahtarı biliyor ve değeri görmek istiyorsanız şunu kullanabilirsiniz:

    azure site config get <key> 

Mevcut bir anahtarın değerini değiştirmek isterseniz öncelikle var olan anahtarı silip yeniden eklemeniz gerekir. Temizleme komutu şu şekildedir:

    azure site config clear <key> 

### <a name="list-and-show-sites"></a>Siteleri listeleme ve görüntüleme
Web sitelerinizi listelemek için aşağıdaki komutu kullanın:

    azure site list

Bir siteyle ilgili ayrıntılı bilgi almak için `site show` komutunu kullanın. Aşağıdaki örnekte `MySite` için ayrıntılar gösterilmiştir:

    azure site show MySite

### <a name="stop-start-or-restart-a-site"></a>Siteyi durdurma, başlatma veya yeniden başlatma
Bir siteyi `site stop`, `site start` ya da `site restart` komutlarıyla durdurabilir, başlatabilir veya yeniden başlatabilirsiniz:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

### <a name="delete-a-site"></a>Bir siteyi silme
Son olarak, bir siteyi `site delete` komutu ile silebilirsiniz:

    azure site delete MySite

Yukarıdaki komutların herhangi birini `site create` öğesini çalıştırdığınız klasörün içinden çalıştırıyorsanız, `MySite` site adını son parametre olarak belirtmeniz gerekmez.

`site` komutlarının tam listesini görmek için `-help` seçeneğini kullanın:

    azure site -help 

<h2><a id="VMs"></a>Azure Sanal Makinesi oluşturma ve yönetme</h2>

Azure Sanal Makinesi sizin belirttiğiniz veya Görüntü Galerisinde mevcut olan bir sanal makine görüntüsünden (.vhd dosyası) oluşturulur. Kullanılabilir görüntüleri görmek için `vm image list` komutunu kullanın:

    azure vm image list

Bir sanal makineyi `vm create` komutu ile kullanılabilir görüntülerden birinden sağlayıp başlatabilirsiniz. Aşağıdaki örnekte Görüntü Galerisi (CentOS 6.2) içindeki bir görüntüden Linux sanal makinesi (`myVM` adlı) oluşturma işlemi gösterilmektedir. Sanal makinenin kök kullanıcı adı ve parolası sırasıyla `myusername` ve `Mypassw0rd` şeklindedir. (`--location` parametresi sanal makinenin oluşturulduğu veri merkezini belirtir. `--location` parametresini kullanmazsanız bir konum seçmeniz istenir.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Yeni oluşturulan sanal makineye uzaktan bağlantıları etkinleştirmek için `vm create` konumuna `--ssh` bayrağını (Linux) veya `--rdp` bayrağını (Windows) geçirmeyi düşünebilirsiniz.

Sanal makineyi özel bir görüntüden sağlarsanız, `vm image create` komutuyla bir .vhd dosyasından görüntü oluşturabilir, ardından `vm create` komutunu kullanarak sanal makineyi sağlayabilirsiniz. Aşağıdaki örnekte yerel bir .vhd dosyasından Linux görüntüsü (`myImage` adlı) oluşturma işlemi gösterilmiştir. (`--location` parametresi, görüntünün depolandığı verileri belirtir.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Yerel bir .vhd dosyasından görüntü oluşturuyorsanız, Azure Blob Depolama’ya kaydedilmiş bir .vhd dosyasından görüntü oluşturabilirsiniz. Bunu `blob-url` parametresiyle yapabilirsiniz:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Bir görüntü oluşturduktan sonra `vm create` kullanarak görüntüden sanal makine sağlayabilirsiniz. Aşağıdaki komut yukarıda oluşturduğunuz görüntüden (`myImage`) `myVM` adlı bir sanal makine oluşturur.

    azure vm create myVM myImage myusername --location "West US"

Bir sanal makineyi sağladıktan sonra sanal makinenize uzaktan erişime izin vermek için uç noktalar oluşturmak isteyebilirsiniz (örneğin). Aşağıdaki örnekte `myVM` üzerinde dış bağlantı noktası 22 ve yerel bağlantı noktası 22’yi açmak için `vm create endpoint` komutu kullanılmıştır:

    azure vm endpoint create myVM 22 22

Bir sanal makineyle ilgili ayrıntılı bilgileri (IP adresi, DNS adı ve uç nokta bilgileri) `vm show` komutuyla alabilirsiniz:

    azure vm show myVM

Sanal makineyi kapatmak, başlatmak veya yeniden başlatmak için aşağıdaki komutlardan birini kullanın:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Son olarak, VM’yi silmek için `vm delete` komutunu kullanın:

    azure vm delete myVM

Sanal makine oluşturup yönetmeye yönelik komutların tam listesi için `-h` seçeneğini kullanın:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com



<!--HONumber=Jan17_HO5-->


