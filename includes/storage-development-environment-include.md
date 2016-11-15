## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma
Ardından, geliştirme ortamınızı Visual Studio’da ayarlayın; böylece bu kılavuzda verilen kod örnekleri denemeye hazır olursunuz.

### <a name="create-a-windows-console-application-project"></a>Windows konsol uygulaması projesi oluşturma
Visual Studio'da yeni bir Windows konsol uygulamasını gösterildiği gibi oluşturun:

![Windows konsol uygulaması oluşturma](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Bu öğreticideki tüm kod örnekleri konsol uygulamanızın `program.cs` içindeki **Main()** yöntemine eklenebilir.

Azure bulut hizmeti, Azure web uygulaması, masaüstü uygulaması veya bir mobil uygulama gibi her tür .NET uygulamasından Azure Storage İstemcisi Kitaplığını kullanabildiğinizi unutmayın. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

### <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma
Bu öğreticiyi tamamlamak amacıyla projenize yüklemek için size gereken iki paket vardır:

* [.NET için Microsoft Azure Storage İstemcisi Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/): Bu paket depolama hesabınızdaki veri kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Yapılandırma Yöneticisi Kitaplığı](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Her iki paketi de almak için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. Çevrimiçi olarak "WindowsAzure.Storage" ifadesini arayın ve Depolama İstemci Kitaplığı’nı ve bağımlılıklarını yüklemek için **Yükle**’ye tıklayın.
3. Çevrimiçi olarak "ConfigurationManager" ifadesini arayın ve Azure Yapılandırma Yöneticisi’ni yüklemek için **Yükle**’ye tıklayın.

> [!NOTE]
> Depolama İstemcisi Kitaplığı paketi [.NET için Azure SDK](https://azure.microsoft.com/downloads/) uygulamasında da bulunur. Ancak, istemci kitaplığının her zaman en son sürümüne sahip olmanızı sağlamak için Depolama istemcisi Kitaplığını NuGet’ten yüklemenizi öneririz.
> 
> .NET için Depolama İstemcisi Kitaplığı’ndaki ODataLib bağımlılıkları, WCF Veri Hizmetleri üzerinde değil de, NuGet üzerinde bulunan ODataLib (sürüm 5.0.2 ve sonraki sürümler) paketlerinde çözümlenir. ODataLib kitaplıkları NuGet aracılığıyla doğrudan indirilebilir veya kod projenizle başvurulabilir. Depolama İstemcisi Kitaplığı tarafından kullanılan belirli ODataLib paketleri [OData](http://nuget.org/packages/Microsoft.Data.OData/5.0.2), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/5.0.2) ve [Spatial](http://nuget.org/packages/System.Spatial/5.0.2) paketleridir. Bu kitaplıklar, Azure Table Storage sınıfları tarafından kullanılırken Depolama İstemcisi Kitaplığı’yla programlama için gerekli bağımlılıklardır.
> 
> 

### <a name="determine-your-target-environment"></a>Hedef ortamınızı saptama
Bu kılavuzdaki örnekleri çalıştırmak için iki ortam seçeneğiniz vardır:

* Kodunuzu buluttaki bir Azure Storage hesabına karşı çalıştırabilirsiniz. 
* Kodunuzu Azure Storage öykünücüsüne karşı çalıştırabilirsiniz. Depolama öykünücüsü, buluttaki Azure Storage hesabına öykünen bir yerel ortamdır. Öykünücü, uygulamanız geliştirildiği sırada kodunuzu test etmek ve hatalarını ayıklamak için bağımsız bir seçenektir. Öykünücü iyi bilinen hesabı ve anahtarı kullanır. Daha fazla ayrıntı için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](../articles/storage/storage-use-emulator.md).

Buluttaki bir depolama hesabını hedefliyorsanız, depolama hesabınız için birincil erişim tuşunu Azure Portal’dan kopyalayın. Daha fazla bilgi için bkz. [Depolama erişim tuşlarını görüntüleme ve kopyalama](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [!NOTE]
> Azure Storage ile ilişkili maliyetlerin oluşmasını önlemek için depolama öykünücüsünü hedefleyebilirsiniz. Ancak, buluttaki bir Azure Storage hesabını hedeflemeyi seçerseniz, bu öğreticiyi gerçekleştirme maliyetleri göz ardı edilecektir.
> 
> 

### <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
.NET için Azure Storage İstemcisi Kitaplığı,depolama hizmetlerine erişilmesi amacıyla uç noktaları ve kimlik bilgilerini yapılandıracak depolama bağlantı dizesinin kullanılmasını destekler. Depolama bağlantı dizenizi korumanın en iyi yolu bir yapılandırma dosyasında tutmaktır. 

Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [Azure Storage Bağlantı Dizesi Yapılandırma](../articles/storage/storage-configure-connection-string.md).

> [!NOTE]
> Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Depolama hesabı anahtarınızı korumak için her zaman özen gösterin. Diğer kullanıcılara dağıtmaktan, sabit kodlamaktan ve başkalarının erişebileceği düz metin dosyasına kaydetmekten kaçının. Anahtarınızın tehlikede olduğunu düşünüyorsanız, Azure Portal'ı kullanarak hesap anahtarınızı yeniden oluşturun.
> 
> 

Bağlantı dizenizi yapılandırmak için, `app.config` dosyasını Visual Studio’daki Çözüm Gezgini'nden açın. `<appSettings>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. `account-name` değerini depolama hesabınızın adıyla ve `account-key` değerini hesabınızın erişim tuşuyla değiştirin:

    <configuration>
        <startup> 
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
        </startup>
          <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
          </appSettings>
    </configuration>

Örneğin, yapılandırma ayarınız şuna benzeyecektir:

    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln6fT7mvY+rxu2iWAEyzPKITGkhM88J8HUoyofvK7C6fHcZc2kRZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />

Depolama öykünücüsünü hedeflemek için iyi bilinen hesap adıyla ve anahtarıyla eşleşen bir kısayolu kullanabilirsiniz. Bu durumda, bağlantı dizesi ayarı şöyle olacaktır:

    <add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />



<!--HONumber=Nov16_HO2-->


