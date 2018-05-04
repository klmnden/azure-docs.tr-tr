# <a name="use-infrastructure-automation-tools-with-virtual-machines-in-azure"></a>Azure'daki sanal makinelerle altyapı Otomasyon araçları kullanın
Oluşturmak ve tutarlı bir şekilde ölçekli Azure sanal makineleri (VM'ler) yönetmek için tür Otomasyon genellikle istendiğini. Birçok araçları ve tam Azure altyapı dağıtımı ve Yönetimi yaşam döngüsü otomatikleştirmek izin çözümleri vardır. Bu makalede Azure içinde kullanabileceğiniz altyapı Otomasyon araçları bazıları tanıtılır. Bu araçlar genellikle aşağıdaki yaklaşımlardan biri için uygun:

- Sanal makineleri yapılandırmasını otomatik hale getirme
    - Araçlar [Ansible](#ansible), [Chef](#chef), ve [Puppet](#puppet).
    - VM özelleştirme belirli araçlarda [bulut init](#cloud-init) Linux VM'ler için [PowerShell istenen durum yapılandırması (DSC)](#powershell-dsc)ve [Azure özel betik uzantısının](#azure-custom-script-extension) tüm Azure VM'ler.
 
- Altyapı Yönetimi otomatik hale getirme
    - Araçlar [Packer](#packer) özel VM otomatikleştirmek için görüntü yapılar ve [Terraform](#terraform) otomatikleştirmek için altyapı yapı işlemi.
    - [Azure Otomasyonu](#azure-automation) Azure ve şirket içi altyapınız genelinde eylemleri gerçekleştirebilirsiniz.

- Uygulama dağıtımı ve teslimat otomatikleştirme
    - Örnekler [Visual Studio Team Services](#visual-studio-team-services) ve [Jenkins](#jenkins).


## <a name="ansible"></a>Ansible
[Ansible](https://www.ansible.com/) yapılandırma yönetimi, VM oluşturma veya uygulama dağıtımı için automation motorudur. Ansible kimlik doğrulaması ve hedef makinelere yönetmek için SSH anahtarları ile genellikle bir aracı daha az modelini kullanır. Yapılandırma görevleri playbooks belirli görevleri gerçekleştirmek kullanılabilir Ansible modül sayısı ile tanımlanır. Daha fazla bilgi için bkz: [nasıl Ansible çalışır](https://www.ansible.com/how-ansible-works).

Şunları nasıl yapacağınızı öğrenin:

- [Yükleme ve Azure ile kullanmak için Linux'ta Ansible yapılandırma](../articles/virtual-machines/linux/ansible-install-configure.md).
- [Temel VM oluşturma](../articles/virtual-machines/linux/ansible-create-vm.md).
- [Kaynakları destek dahil olmak üzere tam bir VM ortam oluşturmak](../articles/virtual-machines/linux/ansible-create-complete-vm.md).


## <a name="chef"></a>Chef
[Chef](https://www.chef.io/) altyapınızı nasıl yapılandırıldığını, tanımlamasına yardımcı olur bir Otomasyon platformudur dağıtılan ve yönetilir. Uygulama yaşam döngüsü Otomasyon altyapısı yerine Chef Habitat ek bileşenler dahil ve Chef yardımcı olan InSpec otomatikleştirmek güvenlik ve ilke gereksinimleri ile uyumluluk. Bir veya daha fazla merkezi Chef depolamak ve yapılandırmalarını yönetme sunucuları ile hedef makinelerde Chef istemciler yüklenir. Daha fazla bilgi için bkz: [bir genel bakış Chef](https://docs.chef.io/chef_overview.html).

Şunları nasıl yapacağınızı öğrenin:

- [Dağıtma Chef Azure Marketi'nden otomatikleştirmek](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview).
- [Windows üzerinde Chef yükleme ve Azure sanal makinelerini oluşturma](../articles/virtual-machines/windows/chef-automation.md).


## <a name="puppet"></a>Puppet
[Puppet](https://www.puppet.com) uygulama sunma ve dağıtım işlemini gerçekleştirir bir kurumsal kullanıma hazır Otomasyon platformudur. Aracılar Puppet Azure altyapısının istenen yapılandırma tanımlamak bildirimleri çalıştırmak için ana ve VM'ler izin vermek için hedef makinelerde yüklü. Puppet Jenkins ve GitHub gibi diğer çözümleri için geliştirilmiş devops iş akışı ile tümleştirebilirsiniz. Daha fazla bilgi için bkz: [nasıl Puppet çalışır](https://puppet.com/product/how-puppet-works).

Şunları nasıl yapacağınızı öğrenin:

- [Azure Marketi'nden Puppet dağıtmak](https://azuremarketplace.microsoft.com/marketplace/apps/puppet.puppet-enterprise-2016-1?tab=Overview).


## <a name="cloud-init"></a>Cloud-init
[Cloud-init](https://cloudinit.readthedocs.io), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.  Doğru biçim hakkında daha fazla bilgi için `#cloud-config` dosyaları görmek [bulut init belgeleri sitesi](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` dosyaları base64 ile kodlanmış metin dosyalarıdır.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

 Etkin olarak ile doğrulanan Linux distro ortaklarımızın Azure marketi'ndeki bulut init etkin görüntüleri kullanılabilir olması için çalışıyoruz. Bu görüntüleri bulut init dağıtımlarınızı yapın ve yapılandırmaları VM'ler ve sanal makine ölçek kümeleri ile sorunsuz çalışır. Aşağıdaki tabloda Azure platformu geçerli bulut init etkin görüntüleri kullanılabilirliğine özetlenmektedir:

| Yayımcı | Sunduğu | SKU | Sürüm | Bulut init hazır
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04-LTS |en son |evet | 
|Canonical |UbuntuServer |14.04.5-LTS |en son |evet |
|CoreOS |CoreOS |Dengeli |en son |evet |
|OpenLogic |CentOS |7-CI |en son |önizleme |
|RedHat |RHEL |7-RAW-CI |en son |önizleme |

Azure üzerinde bulut başlatma hakkında daha fazla ayrıntı öğrenin:

- [Linux sanal makineleri Azure için bulut init destekler.](../articles/virtual-machines/linux/using-cloud-init.md)
- [Bulut init kullanarak otomatikleştirilmiş VM yapılandırmasına bir öğretici deneyin](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md).


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell istenen durum yapılandırması (DSC)](https://msdn.microsoft.com/powershell/dsc/overview) hedef makinelere yapılandırmasını tanımlamak için bir yönetim platformu. DSC de kullanılabilir aracılığıyla Linux'ta [açık Yönetim Altyapısı (OMI) sunucu](https://collaboration.opengroup.org/omi/).

DSC yapılandırmaları ne bir makineye yüklemek ve ana bilgisayar yapılandırma tanımlayın. İstenen eylemlerini basılmış yapılandırmalarını temel alan işler her bir hedef düğümde yerel Configuration Manager (LCM'yi) altyapısı çalışır. Bir çekme sunucusuna DSC yapılandırmaları ve ilişkili kaynakları depolamak için merkezi bir ana bilgisayarda çalışan bir web hizmetidir. Çekme sunucunun hakkında rapor ve gerekli yapılandırmaları sağlamak için her hedef ana bilgisayarda LCM'yi altyapısı ile iletişim kurar.

Şunları nasıl yapacağınızı öğrenin:

- [Temel bir DSC yapılandırması oluşturma](https://msdn.microsoft.com/powershell/dsc/quickstart).
- [Bir DSC çekme sunucusuna yapılandırma](https://msdn.microsoft.com/powershell/dsc/pullserver).
- [Linux için DSC kullanmak](https://msdn.microsoft.com/powershell/dsc/lnxgettingstarted).


## <a name="azure-custom-script-extension"></a>Azure özel betik uzantısı
Azure özel betik uzantısı [Linux](../articles/virtual-machines/linux/extensions-customscript.md) veya [Windows](../articles/virtual-machines/windows/extensions-customscript.md) indirir ve Azure Vm'lerinde komut dosyalarını çalıştırır. Bir VM oluşturun veya VM sonra her zaman kullanımda olduğunda uzantısı kullanabilirsiniz. 

Komut dosyaları, Azure depolama veya GitHub deposunu gibi herhangi bir ortak konuma indirilebilir. Özel betik uzantısı ile kaynak VM çalıştıran herhangi bir dil komut dosyalarını yazabilirsiniz. Bu komut dosyaları, uygulamaları yüklemek veya VM istendiği gibi yapılandırmak için kullanılabilir. Güvenli kimlik bilgileri için korumalı bir yapılandırmada parolalar gibi hassas bilgileri depolanabilir. Bu kimlik bilgileri yalnızca VM içinde şifresi çözülür.

Şunları nasıl yapacağınızı öğrenin:

- [Azure CLI ile bir Linux VM oluşturma ve özel betik uzantısı kullanma](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json).
- [Azure PowerShell ile bir Windows VM oluşturma ve özel betik uzantısı kullanma](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json).


## <a name="packer"></a>Packer
[Packer](https://www.packer.io) , özel bir VM görüntüsü oluştururken derleme işlemini otomatikleştirir. İşletim sistemi tanımlamak ve VM, özel gereksinimlerinizi karşılamak için özelleştirmek yapılandırma sonrası komut dosyalarını çalıştırmak için Packer kullanın. Yapılandırdıktan sonra VM sonra yönetilen Disk görüntü olarak yakalanır. Kaynak VM, ağ ve depolama kaynakları oluşturma, yapılandırma komut dosyalarını çalıştırmak ve VM görüntüsü oluşturma işlemi packer otomatikleştirir.

Şunları nasıl yapacağınızı öğrenin:

- [Azure'da bir Linux VM görüntüsü oluşturmak için Packer kullanmak](../articles/virtual-machines/linux/build-image-with-packer.md).
- [Bir Windows VM görüntüsü oluşturmak için Packer kullanın](../articles/virtual-machines/windows/build-image-with-packer.md).


## <a name="terraform"></a>Terraform
[Terraform](https://www.terraform.io) tanımlayın ve tüm Azure altyapısının bir tek şablon biçimi dili ile - HashiCorp yapılandırma dil'donanım (Uyumluluk) oluşturmanıza olanak sağlayan bir Otomasyon aracıdır. Terraform ile ağ, depolama ve VM kaynakları verilen uygulama çözüm oluşturma işlemini otomatikleştirmek şablonlarını tanımlayın. Varolan Terraform şablonlarınız tutarlılığı sağlamak ve bir Azure Resource Manager şablonu dönüştürmek gerek kalmadan altyapı dağıtımı basitleştirmek için Azure ile diğer platformlar için kullanabilirsiniz.

Şunları nasıl yapacağınızı öğrenin:

- [Yükleme ve Azure ile Terraform yapılandırma](../articles/virtual-machines/linux/terraform-install-configure.md).
- [Azure altyapı ile Terraform oluşturma](../articles/virtual-machines/linux/terraform-create-complete-vm.md).


## <a name="azure-automation"></a>Azure Otomasyonu
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) bir dizi görevi hedef VM'ler işlemek için runbook'lar kullanır. Azure Otomasyonu, var olan sanal makineleri yönetmek için yerine bir altyapı oluşturmak için kullanılır. Azure Otomasyonu hem Linux ve Windows VM'ler, hem de bir karma runbook çalışanı ile şirket içi sanal veya fiziksel makineler arasında çalıştırabilirsiniz. Runbook'ları GitHub gibi bir kaynak denetimi deponuza depolanabilir. Bu runbook'lar sonra el ile veya tanımlı bir zamanlamaya göre çalıştırabilirsiniz.

Azure Otomasyonu de Vm'leri belirli bir dizi nasıl yapılandırılmalıdır tanımları oluşturmanıza olanak tanıyan bir istenen durum Yapılandırması'nı (DSC) hizmeti sağlar. DSC sonra gerekli yapılandırma uygulanır ve VM tutarlı kalmasını sağlar. Azure Otomasyonu DSC hem Windows hem de Linux makinelerde çalıştırır.

Şunları nasıl yapacağınızı öğrenin:

- [PowerShell runbook oluşturma](../articles/automation/automation-first-runbook-textual-powershell.md).
- [Karma Runbook çalışanı şirket içi kaynakları yönetmek için kullandığınız](../articles/automation/automation-hybrid-runbook-worker.md).
- [Azure Otomasyonu DSC kullanmak](../articles/automation/automation-dsc-getting-started.md).


## <a name="visual-studio-team-services"></a>Visual Studio Team Services
[Team Services](https://www.visualstudio.com/team-services/) paylaşım ve kod, otomatik yapılarda kullanın ve tam sürekli tümleştirme ve geliştirme (CI/CD) ardışık düzen oluşturma İzle yardımcı olan araçlar paketidir. Team Services kullanım basitleştirmek için Visual Studio ve diğer düzenleyicileri ile tümleşir. Team Services oluşturabilir ve Azure Vm'leri yapılandırın ve ardından kod onlara dağıtın.

Şunları nasıl yapacağınızı öğrenin:

- [Team Services ile sürekli tümleştirme işlem hattı oluşturma](../articles/virtual-machines/windows/tutorial-vsts-iis-cicd.md).


## <a name="jenkins"></a>Jenkins
[Jenkins](https://www.jenkins.io) dağıtmak ve test uygulamaları ve kod teslimi için otomatik işlem hatlarını oluşturmak yardımcı olan bir sürekli tümleştirme sunucusudur. Çekirdek Jenkins platform genişletmek için eklentileri yüzlerce vardır ve diğer birçok ürünleri ve çözümleri arasında kancalarını ile tümleştirebilirsiniz. El ile bir Azure VM'ye Jenkins yüklemeniz, Docker kapsayıcısı içinde Jenkins gelen çalıştırmak veya önceden oluşturulmuş Azure Market görüntüsü kullanın.

Şunları nasıl yapacağınızı öğrenin:

- [Bir geliştirme altyapısı Jenkins, GitHub ve Docker ile azure'da bir Linux VM oluşturma](../articles/virtual-machines/linux/tutorial-jenkins-github-docker-cicd.md).


## <a name="next-steps"></a>Sonraki adımlar
Azure'da altyapı Otomasyon araçları kullanmak için çok sayıda farklı seçeneğiniz vardır. Gereksinim ve ortamınıza en uygun çözümü kullanmak için serbestçe. Başlama ve bazı Azure içinde yerleşik araçlar denemek için özelleştirmesini otomatikleştirmek bkz. bir [Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) veya [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md) VM.
