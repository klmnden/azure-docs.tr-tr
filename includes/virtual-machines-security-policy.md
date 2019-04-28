---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: c44b39effdc6d8fcdc144915ec7b51489e3798cd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405426"
---
Sanal makinenize (VM), çalışan uygulamalar için güvenli tutmak önemlidir. Sanal makinelerinize güvenli hale getirme, bir veya daha fazla Azure Hizmetleri ve VM'ler ve güvenli depolama verilerinizin güvenli erişim kapsayan özellikler içerebilir. Bu makalede, VM ve uygulamalarınızı güvenli tutmak sağlayan bilgiler sağlar.

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma

Modern tehditlere bulut ortamları için uyumluluk ve güvenlik gereksinimlerini karşılamak için etkili korumayı sürdürmek için baskısı artırma dinamik bir işlemdir. [Azure için Microsoft Antimalware](../articles/security/azure-security-antimalware.md) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan ücretsiz gerçek zamanlı koruma özelliğidir. Uyarılar, kötü amaçlı veya istenmeyebilecek yazılımlar sanal makinenizde kurulmayı veya dener bilinen olduğunda sizi bilgilendirmek üzere yapılandırılabilir.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](../articles/security-center/security-center-intro.md) önleyin, algılayın ve vm'lere tehditlere yanıt verin yardımcı olur. Güvenlik Merkezi, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve güvenlik çözümlerinin geniş ekosistemiyle çalışır algılamaya yardımcı olur.

Güvenlik Merkezi'nin tam zamanında erişim Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için VM dağıtımı arasında uygulanabilir. Just-ın-time etkin olduğundan ve bir kullanıcı bir sanal makine erişimine izin istekleri, Güvenlik Merkezi sanal makine için kullanıcının sahip olduğu izinleri denetler. Bunlar doğru izinlere sahip değilse isteğin onaylanacağını ve Güvenlik Merkezi, ağ güvenlik grupları (Nsg'ler) sınırlı bir süre için seçilen bağlantı noktalarına gelen trafiğe izin verecek şekilde otomatik olarak yapılandırır. Süresi dolduktan sonra Güvenlik Merkezi Nsg'ler önceki durumlarına geri yükler. 

## <a name="encryption"></a>Şifreleme

Gelişmiş için [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) ve [Linux VM](../articles/virtual-machines/linux/encrypt-disks.md) güvenlik ve uyumluluk, azure'da sanal diskler şifrelenebilir. Windows vm'lerinde sanal diskler, BitLocker kullanılarak, bekleme sırasında şifrelenir. Linux vm'lerinde sanal diskler dm-crypt kullanan bekleme durumundayken şifrelenir. 

Azure'da sanal diskler şifrelemek için ücret alınmaz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz. Bir Azure Active Directory Hizmet sorumlusu, Vm'leri açıp desteklenir gibi veren bu şifreleme anahtarları için güvenli bir mekanizma sağlar.

## <a name="key-vault-and-ssh-keys"></a>Key Vault ile SSH anahtarları

Gizli diziler ve sertifikalar kullanılabilir kaynaklar olarak modellenir ve tarafından sağlanan [Key Vault](../articles/key-vault/key-vault-whatis.md). Azure PowerShell için anahtar kasası oluşturmak için kullanabileceğiniz [Windows Vm'leri](../articles/virtual-machines/windows/key-vault-setup.md) ve Azure CLI için [Linux Vm'leri](../articles/virtual-machines/linux/key-vault-setup.md). Şifreleme anahtarları da oluşturabilirsiniz.

Key vault erişim ilkeleri anahtarlara, parolalara ve sertifikalara ayrı ayrı izinler. Örneğin, bir kullanıcıya parolalar için herhangi bir izin vermeden yalnızca anahtarlar için erişim verebilirsiniz. Ancak, anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Diğer bir deyişle, [anahtar kasası erişim ilkesi](../articles/key-vault/key-vault-secure-your-key-vault.md) nesne düzeyinde izinleri desteklemez.

Vm'lere bağlanma, oturum için daha güvenli bir yol sağlamak üzere ortak anahtar şifrelemesi kullanmanız gerekir. Bu işlem, kendiniz yerine bir kullanıcı adı ve parola kimlik doğrulaması için güvenli Kabuk (SSH) komutunu kullanarak genel ve özel bir anahtar değişimi içerir. Parolalar için kaba kuvvet saldırıları, web sunucuları gibi İnternete vm'lerde özellikle etkilenir. Güvenli Kabuk (SSH) anahtar çifti ile oluşturduğunuz bir [Linux VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) kimlik doğrulaması, oturum açmak için parolalara gereksinimi ortadan kaldırmak için SSH anahtarları kullanan. Bağlanmak için SSH anahtarları kullanabilirsiniz bir [Windows VM](../articles/virtual-machines/linux/ssh-from-windows.md) bir Linux VM.

## <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler

Bulut uygulamaları oluştururken yaygın olarak karşılaşılan bir zorluk, bulut hizmetlerinde kimlik doğrulaması yapmak için kodunuzda bulunan kimlik bilgilerinin yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olan kimlik bilgilerinin geliştirici iş istasyonlarında asla gösterilmemesi ve kaynak denetimine kaydedilmemesidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. 

Azure Active Directory (Azure AD) hizmetindeki Azure kaynakları yönetilen hizmetleri bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz.  Bir VM'de çalışan kodunuz yalnızca VM içinden erişilebilir iki uç noktalarından bir belirteç isteğinde bulunabilirsiniz. Bu hizmet hakkında daha ayrıntılı bilgi için gözden [kimliklerini Azure kaynakları için yönetilen](../articles/active-directory/managed-identities-azure-resources/overview.md) genel bakış sayfası.   

## <a name="policies"></a>İlkeler

[Azure ilkeleri](../articles/azure-policy/azure-policy-introduction.md) kuruluşunuzun istenen davranışını tanımlamak için kullanılan [Windows Vm'leri](../articles/virtual-machines/windows/policy.md) ve [Linux Vm'leri](../articles/virtual-machines/linux/policy.md). İlkeleri kullanarak, bir kuruluşun çeşitli kurallar ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı uygulanması, kuruluşun başarısı için katkıda bulunurken risk azaltmaya Yardım olabilir.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kullanarak [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md), ayırabilir, takım içinde ve sadece erişim miktarını kullanıcılara işlerini gerçekleştirmek için ihtiyaç duydukları VM'NİZDE verme. İzinleri vermek yerine herkese sınırsız sanal makinede yalnızca belirli eylemleri izin verebilirsiniz. Erişim denetimi içinde VM için yapılandırabileceğiniz [Azure portalında](../articles/role-based-access-control/role-assignments-portal.md)kullanarak [Azure CLI](https://docs.microsoft.com/cli/azure/role), veya[Azure PowerShell](../articles/role-based-access-control/role-assignments-powershell.md).


## <a name="next-steps"></a>Sonraki adımlar
- Sanal Makine güvenlik için Azure Güvenlik Merkezi'ni kullanarak izlemek için adımlarında yol [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) veya [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md).