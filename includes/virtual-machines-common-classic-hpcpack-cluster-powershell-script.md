---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 6f0d2d59ed50c743adb19027c404bfa83a1886f1
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484872"
---
Ortamı ve Seçenekler bağlı olarak, Azure sanal ağ, depolama hesapları, bulut Hizmetleri, etki alanı denetleyicisi, uzak veya yerel SQL veritabanları, baş düğüm ve ek küme düğümlerini dahil olmak üzere tüm küme altyapısı, komut dosyası oluşturabilirsiniz. Alternatif olarak, betik önceden var olan Azure altyapısını kullanır ve yalnızca HPC küme düğümleri oluşturabilir.

Bir HPC Pack kümesinde planlama hakkında arka plan bilgileri için bkz. [ürün değerlendirme ve planlama](https://technet.microsoft.com/library/jj899596.aspx) ve [Başlarken](https://technet.microsoft.com/library/jj899590.aspx) HPC Pack 2012 R2 TechNet Kitaplığı'nda içeriği.

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği**: Bir aboneliği Azure Global veya Azure China hizmetinde kullanabilirsiniz. Abonelik limitlerinizi sayısını ve türünü dağıtabileceğiniz küme düğümlerinin etkiler. Bilgi için [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../articles/azure-subscription-service-limits.md).
* **Azure PowerShell 0.8.10 ile Windows istemci bilgisayarı veya üzeri yüklü ve yapılandırılmış**: Bkz: [Azure PowerShell'i kullanmaya başlama](/powershell/azureps-cmdlets-docs) yükleme yönergeleri ve adımları Azure aboneliğinize bağlanmak için.
* **HPC Pack Iaas dağıtım betiği**: Karşıdan yükle ve en son sürümünü betikten paket [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü denetleyin `New-HPCIaaSCluster.ps1 –Version`. Bu makalede, betik sürüm 4.5.2 temel alır.
* **Yapılandırma betiği**: HPC kümesini yapılandırmak için komut dosyası kullanan bir XML dosyası oluşturun. Bilgi ve örnekler için bu makalede ve dağıtım betiğini eşlik eden Manual.rtf dosyasını devamındaki bölümlerin bakın.

## <a name="syntax"></a>Sözdizimi
```powershell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Betiği, bir yönetici olarak çalıştırın.
> 
> 

### <a name="parameters"></a>Parametreler
* **ConfigFile**: HPC kümesi tanımlamak için yapılandırma dosyasının dosya yolunu belirtir. Yapılandırma dosyası bu konuda daha fazla bilgi Bkz veya ' % s'dosyasında Manual.rtf klasöründe komut dosyasını içeren.
* **AdminUserName**: Kullanıcı adını belirtir. Etki alanı ormanı komut dosyası tarafından oluşturduysanız, tüm sanal makineler için yerel yönetici kullanıcı adı ve etki alanı yönetici adı olur. Etki alanı orman zaten varsa, bu etki alanı kullanıcısı HPC paketi yüklemek için yerel yönetici kullanıcı adı olarak belirtir.
* **AdminPassword**: Yönetici parolasını belirtir. Komut satırında belirtilmezse, betik parolasını girmenizi ister.
* **HPCImageName** (isteğe bağlı): HPC Kümesi dağıtmak için kullanılan HPC Pack VM görüntü adını belirtir. Azure Market'ten bir HPC Pack Microsoft tarafından sağlanan görüntüsü olmalıdır. Belirtilmezse, (genellikle önerilir), kodun en son yayımlanan seçer [HPC Pack 2012 R2 görüntünüzü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). En yeni görüntüyü Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 güncelleştirme 3 ile temel alır.
  
  > [!NOTE]
  > Geçerli bir HPC Pack görüntüsü belirtmezseniz dağıtımı başarısız olur.
  > 
  > 
* **Günlük dosyası** (isteğe bağlı): Dağıtım günlük dosyası yolu belirtir. Belirtilmezse, betik betik çalıştıran bilgisayarın temp dizininde bir günlük dosyası oluşturur.
* **Zorla** (isteğe bağlı): Tüm onay istemlerini bastırır.
* **NoCleanOnFailure** (isteğe bağlı): Başarıyla dağıtılan değil Azure Vm'lerini kaldırılmaz belirtir. Bu Vm'lere dağıtım devam etmek için komut dosyasını yeniden çalıştırmadan önce el ile kaldırın veya dağıtım başarısız olabilir.
* **PSSessionSkipCACheck** (isteğe bağlı): Bu komut dosyası tarafından dağıtılan Vm'leri, her bulut hizmeti için otomatik olarak imzalanan bir sertifika, Azure tarafından otomatik olarak oluşturulur ve bulut hizmetindeki tüm Vm'leri Bu sertifika varsayılan Windows Uzaktan Yönetim (WinRM) sertifikası olarak kullanın. HPC özellikleri bu Azure sanal makinelerinde dağıtmak için komut dosyası varsayılan olarak geçici olarak bu sertifikaları yerel bilgisayara yükler\\"güvenilir CA" güvenlik hatası bastırmak için istemci bilgisayarın güvenilen kök sertifika yetkilileri deposuna betik yürütme sırasında. Betik tamamlandığında sertifikalar kaldırılır. Bu parametre belirtilmezse, istemci bilgisayar sertifikaları yüklü değil ve güvenlik uyarı bastırılır.
  
  > [!IMPORTANT]
  > Bu parametre, üretim dağıtımları için önerilmez.
  > 
  > 

### <a name="example"></a>Örnek
Aşağıdaki örnek yapılandırma dosyası kullanarak bir HPC Pack kümesi oluşturur *MyConfigFile.xml*, Küme yükleme için yönetici kimlik bilgilerini belirtir.

```powershell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Diğer konular
* Betik, isteğe bağlı olarak HPC paketi web portalı veya HPC Pack REST API'si ile iş gönderme etkinleştirebilirsiniz.
* Ek yazılımlar yüklemek veya diğer ayarları yapılandırmak istiyorsanız betik öncesi ve sonrası yapılandırma özel komut dosyaları baş düğüm üzerinde isteğe bağlı olarak çalıştırabilirsiniz.

## <a name="configuration-file"></a>Yapılandırma dosyası
Dağıtım betiği için yapılandırma dosyasını bir XML dosyasıdır. Şema dosyası HPCIaaSClusterConfig.xsd HPC Pack Iaas dağıtım betiği klasöründe bulunur. **IaaSClusterConfig** ayrıntılı dağıtım komut dosyası klasörü Manual.rtf dosyasında açıklanan alt öğelerini içeren yapılandırma dosyasının kök öğesidir.

