Sanal makine (VM), çalışan uygulamalar için güvenli tutmak önemlidir. Vm'leriniz güvenli hale getirme, bir veya daha fazla Azure Hizmetleri ve de VM'ler ve güvenli depolama verilerinizin güvenli erişim kapak özellikler içerebilir. Bu makalede, VM ve uygulamalarınızı güvenli kalmasına olanak sağlayan bilgiler sağlar.

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma

Modern tehditlere bulut ortamları için uyumluluk ve güvenlik gereksinimlerini karşılamak için etkili korumasını sürdürmek için baskısı artırma dinamik bir işlemdir. [Azure için Microsoft Antimalware](../articles/security/azure-security-antimalware.md) tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım kaldırma yardımcı olan bir ücretsiz gerçek zamanlı koruma özelliğidir. Uyarılar kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya VM üzerinde çalışan girişiminde bilinen olduğunda sizi bilgilendirmek üzere yapılandırılabilir.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](../articles/security-center/security-center-intro.md) engellemenize, algılamanıza ve Vm'leriniz tehditlerine yanıt yardımcı olur. Güvenlik Merkezi, Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur.

## <a name="encryption"></a>Şifreleme

Gelişmiş için [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) ve [Linux VM](../articles/virtual-machines/linux/encrypt-disks.md) güvenlik ve uyumluluk, azure'da sanal diskler şifrelenir. Windows sanal makineleri sanal disklerde Bitlocker kullanılarak bekleme sırasında şifrelenir. Linux sanal makineleri sanal disklerde dm-crypt kullanılarak bekleme sırasında şifrelenir. 

Azure sanal diskleri şifreleme ücretsizdir. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz. Bir Azure Active Directory Hizmet sorumlusu VM'ler açma ve kapatma gücü gibi bu şifreleme anahtarları verme için güvenli bir mekanizma sağlar.

## <a name="key-vault-and-ssh-keys"></a>Anahtar kasası ve SSH anahtarları

Sertifikaları ve parolaları kullanılabilir kaynaklar olarak modellenir ve tarafından sağlanan [anahtar kasası](../articles/key-vault/key-vault-whatis.md). Azure PowerShell için anahtar kasalarını oluşturmak için kullanabileceğiniz [Windows Vm'lerini](../articles/virtual-machines/windows/key-vault-setup.md) ve Azure CLI için [Linux VM'ler](../articles/virtual-machines/linux/key-vault-setup.md). Şifreleme anahtarları de oluşturabilirsiniz.

Anahtar kasası erişim ilkelerini anahtarları, gizli ve sertifikaları ayrı olarak izinleri. Örneğin, bir kullanıcıya parolalar için herhangi bir izin vermeden yalnızca anahtarlar için erişim verebilirsiniz. Ancak, anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Diğer bir deyişle, [anahtar kasası erişim ilkesi](../articles/key-vault/key-vault-secure-your-key-vault.md) nesne düzeyi izinleri desteklemiyor.

VM'ler için bağlandığınızda, onlara oturum açmak için daha güvenli bir şekilde sağlamak için ortak anahtar şifrelemesini kullanmanız gerekir. Bu işlem kendiniz yerine bir kullanıcı adı ve parola kimlik doğrulaması için güvenli Kabuk (SSH) komutunu kullanarak ortak ve özel bir anahtar değişimi içerir. Parolalar yanılma saldırılarına, özellikle web sunucuları gibi İnternete VM'ler için etkilenir. Güvenli Kabuk (SSH) anahtar çifti ile oluşturduğunuz bir [Linux VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) oturum açmak parola gereksinimini kimlik doğrulaması için SSH anahtarları kullanan. Ayrıca SSH anahtarları bağlanması için kullanabileceğiniz bir [Windows VM](../articles/virtual-machines/linux/ssh-from-windows.md) bir Linux VM.

## <a name="policies"></a>İlkeler

[Azure ilkeleri](../articles/azure-policy/azure-policy-introduction.md) kuruluşunuzun istenen davranışını tanımlamak için kullanılan [Windows Vm'lerini](../articles/virtual-machines/windows/policy.md) ve [Linux VM'ler](../articles/virtual-machines/linux/policy.md). İlkeleri kullanarak, bir kuruluşun çeşitli kuralları ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı zorlama kuruluşun başarısı için katkıda bulunan sırasında risk azaltılmasına yardımcı olur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kullanarak [rol tabanlı erişim denetimi (RBAC)](../articles/active-directory/role-based-access-control-what-is.md), ekibiniz içinde görevleri kurabilmeleri ve sadece erişim miktarını kullanıcıların işlerini yapmak için gereksinim duydukları, VM verin. Herkes vermek yerine izinleri Kısıtlanmamış VM, yalnızca belirli eylemleri izin verebilirsiniz. VM için erişim denetimini yapılandırmak [Azure portal](../articles/active-directory/role-based-access-control-configure.md)kullanarak [Azure CLI](https://docs.microsoft.com/cli/azure/role), veya[Azure PowerShell](../articles/active-directory/role-based-access-control-manage-access-powershell.md).


## <a name="next-steps"></a>Sonraki adımlar
- Azure Güvenlik Merkezi'ni kullanarak sanal makine güvenliği izlemek için izleyeceğiniz adımlarda size yol [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) veya [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md).