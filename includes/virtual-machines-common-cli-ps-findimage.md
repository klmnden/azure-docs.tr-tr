

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Önizleme)

[Azure CLI 2.0 (Önizleme) aracını yükledikten](https://docs.microsoft.com/cli/azure/install-az-cli2) sonra, popüler sanal makine görüntülerinin önbelleğe alınmış bir listesini görmek için `az vm image list` komutunu kullanın. Örneğin, `az vm image list -o table` komutunun aşağıdaki örneği şunu görüntüler:

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               14.04.4-LTS         Canonical:UbuntuServer:14.04.4-LTS:latest                       UbuntuLTS            latest
CentOS         OpenLogic               7.2                 OpenLogic:CentOS:7.2:latest                                     CentOS               latest
openSUSE       SUSE                    13.2                SUSE:openSUSE:13.2:latest                                       openSUSE             latest
RHEL           RedHat                  7.2                 RedHat:RHEL:7.2:latest                                          RHEL                 latest
SLES           SUSE                    12-SP1              SUSE:SLES:12-SP1:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

### <a name="finding-all-current-images"></a>Tüm geçerli görüntüleri bulma

Tüm görüntülerin güncel listesini edinmek için `az vm image list` komutunu `--all` seçeneğiyle kullanın. Azure CLI 1.0 komutlarından farklı olarak, belirli bir `--location` bağımsız değişkeni belirlemediğiniz sürece `az vm image list --all` komutu varsayılan olarak **westus** bölgesindeki tüm görüntüleri döndürdüğünden, `--all` komutunun tamamlanması biraz zaman alır. Etkileşimli bir şekilde araştırmanız gerekiyorsa Azure’da sunulmakta olan görüntülerin bir listesini döndüren ve bunu yerel kullanıma yönelik bir dosya olarak depolayan `az vm image list --all > allImages.json` komutunu kullanın. 

Zaten aklınızda bir veya daha fazla ölçüt varsa birkaç seçenekten birini belirleyerek aramanızı belirli bir konum, teklif, yayımcı veya SKU ile kısıtlayabilirsiniz. Bir konum belirtmezseniz **westus** değerleri döndürülür.

### <a name="find-specific-images"></a>Belirli görüntüleri bulma

Belirli bilgileri bulmak için `az vm image list` komutunu bir [JMESPATH sorgu filtresi](https://docs.microsoft.com/cli/azure/query-az-cli2) ile birlikte kullanın. Örneğin, aşağıda **Debian** için kullanılabilecek **SKU**’lar görüntülenmektedir (`--all` anahtarı kullanılmazsa yalnızca yerel önbellekteki popüler görüntülerin arandığını unutmayın):

```azurecli
az vm image list --query '[?contains(offer,`Debian`)]' -o table --all
```

Çıktı aşağıdakine benzer: 
```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
  Sku  Publisher    Offer    Urn                       Version    UrnAlias
-----  -----------  -------  ------------------------  ---------  ----------
    8  credativ     Debian   credativ:Debian:8:latest  latest     Debian

<list shortened for the example>
```

Nereye dağıtacağınızı biliyorsanız genel görüntü arama sonuçlarını `az vm image list-skus`, `az vm image list-offers` ve `az vm image list-publishers` komutlarıyla kullanarak tam olarak aradığınız şeyi ve nerede dağıtılabileceğini bulabilirsiniz. Örneğin, bir önceki örnekte `credativ`‘in bir Debian teklifi olduğunu biliyorsanız, tam olarak aradığınız şeyi bulmak için `--location` ve diğer seçenekleri kullanabilirsiniz. Aşağıdaki örnekte **westeurope** bölgesinde bir Debian 8 görüntüsü aranır:

```azurecli 
az vm image show -l westeurope -f debian -p credativ --skus 8 --version 8.0.201701180
```

Çıktı şu şekildedir:

```json
{
  "dataDiskImages": [],
  "id": "/Subscriptions/<guid>/Providers/Microsoft.Compute/Locations/westeurope/Publishers/credativ/ArtifactTypes/VMImage/Offers/debian/Skus/8/Versions/8.0.201701180",
  "location": "westeurope",
  "name": "8.0.201701180",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

## <a name="azure-cli-10"></a>Azure CLI 1.0 

> [!NOTE]
> Bu makalede Azure Resource Manager dağıtım modelini destekleyen bir Azure CLI 1.0 ya da Azure PowerShell yüklemesi kullanarak sanal makine görüntülerine gitme ve görüntü seçme işlemleri açıklanır. Bir önkoşul olarak Resource Manager moduna geçin. Azure CLI’de `azure config mode arm` yazarak bu moda girebilirsiniz. 
> 

Bir görüntüyü bulmanın en hızlı yolu, `azure vm image list` komutunu arayıp konumu, yayımcı adını (büyük-küçük harfe duyarlı değildir!) ve biliyorsanız teklifi geçirmektir. Örneğin, "Canonical"’ın bir "UbuntuServer" teklifi yayımcısı olduğunu biliyorsanız, aşağıdaki kısa örnek listeyi (çoğu liste bayağı uzundur) edinebilirsiniz.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

**Urn** sütunu `azure vm quick-create` komutuna geçireceğiniz formdur.

Bununla birlikte, çoğu zaman henüz hangi tekliflerin sunulduğunu bilemezsiniz. Bu durumda, `azure vm image list-publishers` komutunu kullanıp istendiğinde bir veri merkezi belirterek görüntülerde gezinebilirsiniz. Örneğin, aşağıdaki listede Batı ABD konumundaki tüm görüntü yayımcıları listelenmiştir (konum bağımsız değişkenini, standart konumları küçük harfe dönüştürüp boşlukları atarak geçirin)

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Bu listeler genelde çok uzun olduğundan, yukarıdaki örnek listeyi küçük bir örnek olarak görebilirsiniz. Diyelim ki Canonical’ın gerçekten de Batı ABD konumunda bir görüntü yayımlayıcısı olduğunu fark ettim. Artık aşağıdaki örnekte olduğu gibi `azure vm image list-offers` komutunu arayıp istendiğinde konum ve yayımcıyı geçirerek sundukları teklifleri bulabilirsiniz:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Artık Batı ABD bölgesinde Canonical’ın Azure’da **UbuntuServer** teklifini yayımladığını biliyoruz. Peki bunların SKU’su ne? Bu değerleri öğrenmek için `azure vm image list-skus` komutunu arar ve istendiğinde bulduğunuz konum, yayımcı ve teklifi sağlarsınız.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

Bu bilgilerle artık üst taraftaki özgün çağrıyı arayarak tam olarak istediğiniz görüntüyü bulabilirsiniz.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

Artık tam olarak kullanmak istediğiniz görüntüyü seçebilirsiniz. Az önce bulduğunuz URN bilgilerini kullanarak ya da bu URN bilgilerini içeren bir şablonu kullanarak hızla bir sanal makine oluşturmak için bkz. [Mac, Linux ve Windows için Azure CLI’yi Azure Resource Manager ile kullanma](../articles/xplat-cli-azure-resource-manager.md).

## <a name="powershell"></a>PowerShell
> [!NOTE]
> [En son Azure PowerShell sürümünü](/powershell/azureps-cmdlets-docs) yükleyin ve yapılandırın. Sürümü 1.0’dan düşük olan Azure PowerShell modüllerini kullanıyorsanız yine aşağıdaki komutları kullanırsınız, ancak önce `Switch-AzureMode AzureResourceManager`. 
> 
> 

Azure Resource Manager ile yeni bir sanal makine oluştururken bazı durumlarda aşağıdaki görüntü özelliklerinin birleşimine sahip bir görüntü belirtmeniz gerekir:

* Yayımcı
* Sunduğu
* SKU

Örneğin, `Set-AzureRMVMSourceImage` PowerShell cmdlet’i için veya oluşturulacak sanal makine türünü belirtmeniz gereken bir kaynak grubu şablon dosyasında bu değerler gerekir.

Bu değerleri belirlemeniz gerekiyorsa görüntülere giderek bu değerleri belirleyebilirsiniz:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

İlk olarak aşağıdaki komutlarla yayımcıları listeleyin:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Seçtiğiniz yayımcı adını girin ve aşağıdaki komutları çalıştırın:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Seçtiğiniz teklif adını girin ve aşağıdaki komutları çalıştırın:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

`Get-AzureRMVMImageSku` komutunun ekranından yeni bir sanal makine için kullanılacak görüntüyü belirlemek için ihtiyacınız olan tüm bilgileri edinebilirsiniz.

Aşağıda tam bir örnek gösterilmiştir:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

"MicrosoftWindowsServer" yayımcısı için:

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

"WindowsServer" teklifi için:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Bu listeden seçtiğiniz SKU adını kopyaladığınızda, `Set-AzureRMVMSourceImage` PowerShell cmdlet’i veya bir kaynak grubu şablonu için ihtiyacınız olan tüm bilgilere sahip olursunuz.

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/

<!--HONumber=Feb17_HO3-->


