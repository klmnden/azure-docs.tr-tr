



Ortamınız ve Seçenekler bağlı olarak, Azure sanal ağı, depolama hesapları, bulut Hizmetleri, etki alanı denetleyicisi, uzak veya yerel SQL veritabanları, baş düğüm ve ek küme düğümlerini de dahil olmak üzere tüm küme altyapısı, komut dosyası oluşturabilirsiniz. Alternatif olarak, komut dosyasını önceden var olan Azure altyapısını kullanır ve yalnızca HPC küme düğümleri oluşturun.

HPC Pack küme planlama hakkında arka plan bilgileri için bkz: [ürün değerlendirme ve planlama](https://technet.microsoft.com/library/jj899596.aspx) ve [Başlarken](https://technet.microsoft.com/library/jj899590.aspx) HPC Pack 2012 R2 TechNet Kitaplığı'nda içeriği.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**: Azure genel veya Azure Çin hizmetinde bir aboneliği kullanabilirsiniz. Abonelik sınırlarınızı sayısı ve türü dağıtabilmeniz için küme düğümlerinin etkiler. Bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../articles/azure-subscription-service-limits.md).
* **Azure PowerShell 0.8.10 ile Windows istemci bilgisayarı veya sonrası yüklü ve yapılandırılmış**: bkz [Azure PowerShell ile çalışmaya başlama](/powershell/azureps-cmdlets-docs) yükleme yönergeleri ve adımlar Azure aboneliğinize bağlanmak için.
* **HPC Pack Iaas dağıtım betiği**: karşıdan yükle ve en son sürümünü betikten paket [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyası sürümünü çalıştırarak denetleme `New-HPCIaaSCluster.ps1 –Version`. Bu makalede, komut dosyası sürüm 4.5.2 temel alır.
* **Yapılandırma betiği**: HPC küme yapılandırmak için komut dosyası kullanan bir XML dosyası oluşturun. Bilgi ve örnekler için bu makalede ve dağıtım betiği eşlik Manual.rtf dosya devamındaki bölümlere bakın.

## <a name="syntax"></a>Sözdizimi
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Komut dosyasını bir yönetici olarak çalıştırın.
> 
> 

### <a name="parameters"></a>Parametreler
* **ConfigFile**: HPC küme açıklamak için yapılandırma dosyasının dosya yolunu belirtir. Bu konuda yapılandırma dosyasında hakkında daha fazla bakın veya Manual.rtf klasöründeki dosyasındaki komut dosyasını içeren.
* **AdminUserName**: kullanıcı adını belirtir. Etki alanı ormanı komut dosyası tarafından oluşturduysanız, bu tüm VM'ler için yerel yönetici kullanıcı adı ve etki alanı yöneticisi adı olur. Etki alanı ormanı zaten varsa, bu etki alanı kullanıcısının HPC paketini yüklemek için yerel yönetici kullanıcı adı olarak belirtir.
* **Admınpassword**: Yönetici parolasını belirtir. Komut satırında belirtilmezse, betik parola girmesini ister.
* **HPCImageName** (isteğe bağlı): HPC küme dağıtmak için kullanılan HPC Pack VM görüntü adı belirtir. Azure Marketi'nden Microsoft tarafından sağlanan HPC Pack bir görüntü olmalıdır. Belirtilmezse, (genellikle önerilir), komut dosyasının en son yayımlanan seçer [HPC Pack 2012 R2 görüntüsünü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). En son görüntünün Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 güncelleştirme 3 ile temel alır.
  
  > [!NOTE]
  > Geçerli bir HPC Pack görüntüsü belirtmezseniz dağıtımı başarısız olur.
  > 
  > 
* **Günlük dosyası** (isteğe bağlı): dağıtım günlük dosyası yolunu belirtir. Belirtilmezse, komut dosyası komut dosyasını çalıştıran bilgisayar temp dizininde bir günlük dosyası oluşturur.
* **Zorla** (isteğe bağlı): tüm onay komut istemlerini bastırır.
* **NoCleanOnFailure** (isteğe bağlı): değil başarıyla dağıtılan Azure sanal makinelerini kaldırılmıyor belirtir. Dağıtım devam etmek için komut dosyası yeniden çalıştırmadan önce bu Vm'lere el ile kaldırın veya dağıtım başarısız.
* **PSSessionSkipCACheck** (isteğe bağlı): Bu komut dosyası tarafından dağıtılan VM'ler ile her bulut hizmeti için otomatik olarak imzalanan bir sertifika Azure tarafından otomatik olarak oluşturulur ve bulut hizmetindeki tüm sanal makineleri bu sertifika varsayılan olarak Windows Uzaktan kullanın. Yönetim (WinRM) sertifikası. Bu Azure VM'de HPC özellikler dağıtmak için varsayılan komut dosyası geçici olarak bu sertifikaları yerel bilgisayara yükler\\"güvenilmedi CA" güvenlik hatası gizlemek için istemci bilgisayarın güvenilir kök sertifika yetkilileri deposuna komut dosyası yürütme sırasında. Betik tamamlandığında sertifikaları kaldırılır. Bu parametre belirtilmezse, istemci bilgisayar sertifikaları yüklü değil ve güvenlik uyarısı engellenir.
  
  > [!IMPORTANT]
  > Bu parametre, üretim dağıtımları için önerilmez.
  > 
  > 

### <a name="example"></a>Örnek
Aşağıdaki örnek yapılandırma dosyası kullanarak bir HPC Pack kümesini oluşturur *MyConfigFile.xml*, Küme yükleme için yönetici kimlik bilgilerini belirtir.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Diğer konular
* Komut dosyası iş gönderme HPC paketi web portalı üzerinden veya HPC Pack REST API isteğe bağlı olarak etkinleştirebilirsiniz.
* Ek yazılım yüklemesi veya diğer ayarları yapılandırmak istiyorsanız, komut dosyası baş düğümünde özel öncesi ve sonrası yapılandırma komut dosyaları isteğe bağlı olarak çalıştırabilirsiniz.

## <a name="configuration-file"></a>Yapılandırma dosyası
Dağıtım komut dosyası için yapılandırma dosyasını bir XML dosyasıdır. Şema dosyası HPCIaaSClusterConfig.xsd HPC Pack Iaas dağıtım komut dosyası klasöründe bulunur. **IaaSClusterConfig** ayrıntılı dağıtım komut dosyası klasörü Manual.rtf dosyasında açıklanan alt öğelerini içeren yapılandırma dosyasının kök öğedir.

