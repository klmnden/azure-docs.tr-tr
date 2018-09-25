


Bu makalede nasıl yapılır:

* Bunu sağlanıyor, verileri bir Azure sanal makinesine (VM) ekleyin.
* Bu, hem Windows hem de Linux için alın.
* Özel araçlar kullanılabilir bazı sistemlerde algılamak ve özel verileri otomatik olarak işlemek için kullanın.

> [!NOTE]
> Nasıl özel verileri bu makalede Klasik dağıtım modeliyle oluşturulan bir VM'yi kullanarak yerleştirilebilir. Azure Resource Management API'si kullanma hakkında bilgi için bkz: [örnek şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Azure sanal makinesine özel veri ekleme

[!INCLUDE [classic-cli](contains-classic-cli-content.md)]

Bu özellik şu anda yalnızca desteklenmektedir [Azure CLI](https://github.com/Azure/azure-xplat-cli). Burada oluştururuz bir `custom-data.txt` bizim verilerini içeren dosyanın sonra ekleme, VM sağlama sırasında. İçin seçeneklerden birini kullanabilir ancak `azure vm create` komutunu aşağıdaki örnek, bir temel yaklaşım gösterir:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Sanal makinenin özel veri kullanma
* Azure VM bir Windows tabanlı bir vm'dir sonra özel verileri dosya kaydedilmiş kalır `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Yerel bilgisayardan yeni VM'ye aktarmak için base64 kodlamalı olmasına rağmen otomatik olarak kodu çözülen ve açılabilen veya hemen kullanılır.
  
  > [!NOTE]
  > Dosya varsa, üzerine yazılır. Dizin üzerindeki güvenlik ayarlanır **sistem: Tam Denetim** ve **Administrators: Tam Denetim**.
  > 
  > 
* Azure VM için Linux tabanlı bir VM ise, özel veri dosyası, distro bağlı olarak aşağıdaki konumlardan birinde bulunur. Verilerin ilk kod çözme gerekebilir, böylece verileri Base64 ile kodlanmış olabilir:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Azure'da cloud-init
Azure VM, Ubuntu veya CoreOS görüntüden ise, bir bulut yapılandırma için cloud-init göndermek için CustomData kullanabilirsiniz. Veya özel veri dosyanızın bir komut dosyası ise, ardından cloud-init yürütebilirsiniz.

### <a name="ubuntu-cloud-images"></a>Ubuntu bulut görüntüleri
Çoğu Azure Linux görüntüleri düzenlemeniz "/ etc/waagent.conf" geçici kaynak disk yapılandırmak ve takas için. Bkz: [Azure Linux Aracısı Kullanım Kılavuzu](../articles/virtual-machines/extensions/agent-linux.md) daha fazla bilgi için.

Ancak, Ubuntu bulut görüntülerinde cloud-init kaynak disk (diğer bir deyişle, "kısa ömürlü" disk) yapılandırmak için kullanmanız gerekir ve takas bölümü. Ubuntu Wiki daha fazla ayrıntı için şu sayfaya bakın: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Sonraki adımlar: cloud-init kullanma
Daha fazla bilgi için [Ubuntu için cloud-init belgeleri](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Rolü Hizmet Yönetimi REST API Başvurusu Ekle](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure komut satırı arabirimi](https://github.com/Azure/azure-xplat-cli)

