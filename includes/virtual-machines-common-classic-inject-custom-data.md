


Bu makalede nasıl yapılır:

* Bunu sağlanan zaman veri bir Azure sanal makine (VM) yerleştirir.
* Windows ve Linux için alın.
* Özel araçlar kullanılabilir bazı sistemlerinde algılayabilir ve özel verileri otomatik olarak işlemek için kullanın.

> [!NOTE]
> Bu makalede nasıl özel veri Klasik dağıtım modeli kullanılarak oluşturulmuş bir VM kullanarak yerleştirilebilir. Azure kaynak yönetimi API kullanılması hakkında bilgi için bkz: [örnek şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Azure sanal makineniz özel veri injecting
Bu özellik yalnızca desteklenmekte [Azure komut satırı arabirimi](https://github.com/Azure/azure-xplat-cli). Burada oluşturuyoruz bir `custom-data.txt` bizim veri içeren dosyayı ardından Ekle, VM'ye sağlama sırasında. İçin seçeneklerden birini kullanabilir ancak `azure vm create` komutu, aşağıdaki örnek, bir temel yaklaşım gösterir:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Sanal makineyi özel veri kullanma
* Windows tabanlı bir VM, Azure VM, ardından özel veri dosyası kaydedilir `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Yeni VM yerel bilgisayardan aktarmak için Base64 ile kodlanmış olmasına rağmen otomatik olarak kodu çözülmüş ve açılabilen veya hemen kullanılır.
  
  > [!NOTE]
  > Dosya varsa üzerine yazılır. Dizin üzerinde güvenlik kümesine **sistem: Tam Denetim** ve **Administrators: Tam Denetim**.
  > 
  > 
* Azure VM Linux tabanlı bir VM ise, özel veri dosyası, distro bağlı olarak aşağıdaki konumlardan birinde bulunur. Verileri ilk kod çözme gerekebilir böylece veriler Base64 ile kodlanmış olabilir:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Azure üzerinde bulut başlatma
Azure VM Ubuntu veya CoreOS görüntüden ise, bir bulut-config bulut başlatma göndermek için CustomData kullanabilirsiniz. Veya özel veri dosyanızın bir komut dosyası ise, sonra bulut init onu çalıştırabilirsiniz.

### <a name="ubuntu-cloud-images"></a>Ubuntu bulut görüntüleri
Çoğu Azure Linux görüntülerinde düzenlemek istiyorum "/ etc/waagent.conf" geçici kaynak disk yapılandırma ve dosya değiştirme. Bkz: [Azure Linux Aracısı Kullanıcı Kılavuzu](../articles/virtual-machines/extensions/agent-linux.md) daha fazla bilgi için.

Ancak, Ubuntu bulut görüntülerinde bulut init kaynak disk (diğer bir deyişle, "kısa ömürlü" disk) yapılandırmak için kullanmanız gerekir ve takas bölüm. Daha fazla ayrıntı için Ubuntu Wiki'de aşağıdaki sayfaya bakın: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Sonraki adımlar: Bulut init kullanma
Daha fazla bilgi için bkz: [Ubuntu bulut init belgelerine](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Rolü Hizmet Yönetimi REST API Başvurusu Ekle](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure komut satırı arabirimi](https://github.com/Azure/azure-xplat-cli)

