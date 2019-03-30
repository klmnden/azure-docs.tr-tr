---
title: Azure depolama Güvenlik Kılavuzu | Microsoft Docs
description: Azure depolama, ancak bunlarla sınırlı olmamak RBAC, depolama hizmeti şifrelemesi, istemci tarafı şifreleme, SMB 3.0 ve Azure Disk şifrelemesi için güvenli hale getirme birçok yöntemleri ayrıntılı olarak açıklanmaktadır.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 329782a436924355dbdfbb5db260e88795394697
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650130"
---
# <a name="azure-storage-security-guide"></a>Azure depolama Güvenlik Kılavuzu

Azure depolama, kapsamlı birlikte güvenli uygulamalar oluşturmalarını sağlayan güvenlik özellikleri sağlar:

- Azure Depolama'ya yazılan tüm veriler, kullanılarak otomatik olarak şifrelenir [depolama hizmeti şifrelemesi (SSE)](storage-service-encryption.md). Daha fazla bilgi için [Azure Blobları, dosyalar, tablo ve kuyruk depolama için varsayılan şifreleme Duyurusu](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).
- Azure Active Directory (Azure AD) ve rol tabanlı Access Control (RBAC) desteklenen Azure depolama için kaynak yönetimi işlemleri ve veri işlemleri için şu şekilde:   
    - Güvenlik ilkeleri ve Azure AD kullanım anahtar yönetimi gibi yönetim işlemlerini kaynak yetkilendirmek için depolama hesabına kapsamına RBAC rolleri atayabilirsiniz.
    - Azure AD tümleştirmesi, blob ve kuyruk veri işlemleri için desteklenir. Bir abonelik, kaynak grubu, depolama hesabı veya bir tek kapsayıcı veya bir güvenlik sorumlusu veya Azure kaynakları için yönetilen bir kimlik kuyruğuna kapsamına RBAC rolleri atayabilirsiniz. Daha fazla bilgi için [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md).   
- Veri güvenli bir uygulama ve Azure arasında aktarımda kullanarak [istemci tarafı şifreleme](../storage-client-side-encryption.md), HTTPS ve SMB 3.0.  
- Azure sanal makineler tarafından kullanılan işletim sistemi ve veri diskleri kullanılarak şifrelenebilir [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md). 
- Azure depolama veri nesnelerini temsilci erişimi kullanılarak verilebilir [paylaşılan erişim imzaları](../storage-dotnet-shared-access-signature-part-1.md).

Bu makalede, Azure depolama ile kullanılan bu güvenlik özelliklerin her biri bir bakış sağlar. Bağlantılardır her özelliğin ayrıntılarını bunu sağlayacak makaleler için bunu kolayca yapabilirsiniz sağlanan araştırma her konuyla ilgili daha fazla.

Bu makalede ele alınacak konular şunlardır:

* [Yönetim düzlemi güvenlik](#management-plane-security) – depolama hesabınızın güvenliğini sağlama

  Yönetim düzlemi, depolama hesabınızı yönetmek için kullanılan kaynaklardan oluşur. Bu bölümde, Azure Resource Manager dağıtım modeli ve depolama hesaplarınız erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanmayı kapsar. Depolama hesabı anahtarlarınızı ve bunları yeniden nasıl yönetme yöneliktir.
* [Veri düzlemi güvenlik](#data-plane-security) – verilerinize erişim güvenliğini sağlama

  Bu bölümde, depolama hesabınızdaki BLOB, dosyalar, kuyruklar ve tablolar gibi gerçek veri nesnelerine erişim sağlayan paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanarak göz atacağız. Hizmet düzeyi SAS hem de hesap düzeyinde SAS ele alınacaktır. Ayrıca belirli bir IP adresi (veya IP adresi aralığı) erişimi sınırlamak nasıl ve HTTPS için kullanılan protokol sınırlamak nasıl bir paylaşılan erişim imzası sona tamamlanmasını beklemenize gerek kalmadan iptal etmek için göreceğiz.
* [Aktarım Sırasında Şifreleme](#encryption-in-transit)

  Bu bölümde, Azure depolama içine veya dışına aktardığınızda verilerin güvenliğini sağlamak nasıl ele alınmaktadır. HTTPS ve Azure dosya paylaşımları için SMB 3.0 tarafından kullanılan şifreleme önerilen kullanımı hakkında konuşacağız. Biz de sağlayan bir istemci uygulamasında depolama alanına aktarılır önce verileri şifrelemek ve depolama alanınız aktarıldıktan sonra verilerin şifresini çözmek için istemci tarafı şifreleme göz atın.
* [Bekleme Sırasında Şifreleme](#encryption-at-rest)

  Biz, ilgili depolama hizmeti şifrelemesi (artık otomatik olarak yeni ve var olan depolama hesapları için etkinleştirilmiş olan SSE), konuşur. Ayrıca Azure Disk şifrelemesi kullanın ve temel farklılıklar ve istemci tarafı şifreleme yerine SSE ve Disk şifrelemesi örneklerini keşfedin nasıl atacağız. Kısaca ABD için FIPS uyumluluğunu atacağız Kamu bilgisayarlar.
* Kullanarak [depolama analizi](#storage-analytics) Azure depolama erişimi denetlemek için

  Bu bölümde, bir istek için depolama analizi günlüklerinde bilgi anlatılmaktadır. Biz, günlük verilerinin gerçek depolama analizi göz atın ve depolama hesabınızın anahtarıyla bir paylaşılan erişim imzası bir istekte veya anonim olarak ve olup başarılı veya başarısız olduğunu ayrım öğrenin.
* [CORS kullanarak tarayıcı tabanlı istemcileri etkinleştirme](#cross-origin-resource-sharing-cors)

  Bu bölümde, çıkış noktaları arası kaynak paylaşımı (CORS) izin verecek şekilde hakkında konuşuyor. Etki alanları arası erişim ve Azure Depolama'ya yerleşik CORS özellikleriyle işlemek nasıl hakkında konuşacağız.

## <a name="management-plane-security"></a>Yönetim düzlemi güvenlik
Yönetim düzlemi depolama hesabını etkileyen işlemlerden oluşur. Örneğin, oluşturma veya bir depolama hesabı silebilir, bir Abonelikteki depolama hesaplarının bir listesini alma, depolama hesabı anahtarlarını almak veya depolama hesabı anahtarlarını yeniden oluştur.

Yeni bir depolama hesabı oluşturduğunuzda, Klasik veya Resource Manager dağıtım modeli seçin. Azure kaynakları oluşturma, Klasik modeli yalnızca abonelik ve ardından depolama hesabını zorlamadan erişim sağlar.

Bu kılavuz depolama hesapları oluşturmak için önerilen yöntemdir Resource Manager modeli ele alınmaktadır. Resource Manager depolama hesapları yerine ile tüm abonelik erişimi veren, daha sınırlı düzeyinde rol tabanlı erişim denetimi (RBAC) kullanarak yönetim düzlemine erişimi denetleyebilirsiniz.

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama
RBAC nedir ve nasıl kullanabileceğiniz hakkında konuşalım. Her Azure aboneliği bir Azure Active Directory’ye sahiptir. Kullanıcılara, gruplara veya bu dizine uygulamalarından Azure aboneliğinde Resource Manager dağıtım modelini kullanan kaynakları yönetmek üzere erişim verilebilir. Bu tür bir güvenlik rolü tabanlı erişim denetimi (RBAC) denir. Bu erişimi yönetmek için kullanabileceğiniz [Azure portalında](https://portal.azure.com/), [Azure CLI araçlarını](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), veya [Azure depolama kaynak sağlayıcısı REST API'lerini](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Resource Manager modeli ile depolama hesabı Azure Active Directory'yi kullanarak bu belirli bir depolama hesabı için yönetim düzlemi bir kaynak grubu ve Denetim erişim yerleştirin. Örneğin, belirli kullanıcılar diğer kullanıcılar için depolama hesabıyla ilgili bilgileri görüntüleyebilirsiniz, ancak bu depolama hesabı anahtarlarını erişilemiyor, depolama hesabı anahtarları, erişim olanağı verebilirsiniz.

#### <a name="granting-access"></a>Erişim verme
Kullanıcıları, grupları ve uygulamaları, doğru kapsamda uygun RBAC rolü atanarak erişim verilir. Aboneliğin tümü erişim vermek için abonelik düzeyinde bir rol atayın. Kaynak grubunun kendisine izinleri vererek bir kaynak grubundaki tüm kaynakları için erişim verebilirsiniz. Depolama hesapları gibi belirli kaynaklar belirli roller atayabilirsiniz.

Bir Azure depolama hesabı yönetim işlemlerini erişmek için RBAC kullanma hakkında bilmeniz gereken temel noktalar şunlardır:

* Erişim atadığınızda, erişimine sahip olmasını istediğiniz hesap için temel bir rol atayın. Bu depolama hesabını yönetmek için kullanılan işlemler, ancak hesaptaki veri nesnelerini erişimi denetleyebilirsiniz. Örneğin, (örneğin, artıklık için), depolama hesabının özelliklerini alma izni verebilirsiniz ancak bir kapsayıcı veya Blob Depolama içinde bir kapsayıcı içindeki veriler.
* Birinin veri nesneleri depolama hesabına erişim iznine sahip depolama hesabı anahtarlarını okuma izni verin ve o kullanıcı ardından bloblar, kuyruklar, tablolar ve dosyalar erişmek için bu tuşlarını kullanabilirsiniz.
* Roller, belirli bir kullanıcı hesabı, bir kullanıcı grubu veya belirli bir uygulamaya atanabilir.
* Her rolün Eylemler ve eylemleri değil bir listesi vardır. Örneğin, sanal makine Katılımcısı rolü "Listkeys'i" eylem vardır okumak depolama hesabı anahtarlarını sağlar. Katkıda bulunan erişimi Active Directory Kullanıcıları için güncelleştirilmesi gibi "Değil, Eylemler" vardır.
* Depolama için rolleri dahil (ancak bunlarla sınırlı değildir) aşağıdaki rolleri:

  * Sahip – bunlar erişim dahil her şeyi yönetebilir.
  * Katkıda bulunan – yapabilecekleri herhangi bir şey sahibi erişimi atayın hariç. Birisi bu role sahip görüntüleyebilir ve depolama hesabı anahtarlarını yeniden oluştur. Depolama hesabı anahtarları, bunlar veri nesnelere erişebilir.
  * Okuyucu-gizli dizileri dışında depolama hesabıyla ilgili bilgileri görüntüleyebilirler. Örneğin, depolama hesabındaki okuyucu izinlere sahip bir role birine atarsanız, depolama hesabının özelliklerini görüntüleyebilirler ancak herhangi bir değişiklik özellikleri veya depolama hesabı anahtarlarını görüntülemek.
  * Depolama hesabı Katılımcısı – bunlar depolama hesabını yönetebilir: aboneliğin kaynak grupları ve kaynakları okuyabilirsiniz oluşturmak ve abonelik kaynak grubu dağıtımları yönetin. Bunlar, sırayla veri düzlemine erişebilecekleri anlamına gelir depolama hesabı anahtarlarını da erişebilirsiniz.
  * Kullanıcı erişimi Yöneticisi – bunlar depolama hesabı için kullanıcı erişimini yönetebilir. Örneğin, bunlar belirli bir kullanıcıya okuyucu erişim verebilirsiniz.
  * Sanal makine Katılımcısı-sanal makineler ancak bağlı depolama hesabını değil yönetebilirler. Bu rol, yani bu role atadığınız kullanıcı veri düzlemine güncelleştirebilirsiniz depolama hesabı anahtarlarını listeleyebilirsiniz.

    Bir sanal makine oluşturmak bir kullanıcı için sırada bir depolama hesabında VHD dosyasını karşılık gelen oluşturabilmek sahiptirler. Bunu yapmak için depolama hesabı anahtarınızı alma ve VM oluşturma API'sine geçirin görebilmek gerekir. Bu nedenle, depolama hesabı anahtarlarını Listele, böylece bu izni olmalıdır.
* Özel roller tanımlama yeteneği Azure kaynaklarında gerçekleştirilebilir kullanılabilir eylemler listesinden eylemleri kümesini oluşturan olanak tanıyan bir özelliktir.
* Kullanıcı için bir rol atamak için önce Azure Active Directory'nizde ayarlanması gerekir.
* Kimin izni/ne tür bir erişim buradan Kime ve PowerShell veya Azure CLI kullanarak hangi kapsamda iptal bir rapor oluşturabilirsiniz.

#### <a name="resources"></a>Kaynaklar
* [Azure Active Directory Rol Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md)

  Bu makalede Azure Active Directory Rol Tabanlı Access Control ve nasıl çalıştığı açıklanmaktadır.
* [RBAC: Yerleşik roller](../../role-based-access-control/built-in-roles.md)

  Bu makalede RBAC'de kullanılabilen yerleşik rollerin tümünde ayrıntıları.
* [Resource Manager dağıtımını ve klasik dağıtımı anlama](../../azure-resource-manager/resource-manager-deployment-model.md)

  Bu makalede, Resource Manager dağıtımını ve klasik dağıtım modellerini açıklar ve Resource Manager ve kaynak gruplarını kullanmanın avantajları anlatılmaktadır. Bu, Azure işlem, ağ ve depolama sağlayıcıları Resource Manager modeli altında nasıl çalıştığını açıklar.
* [Rol Tabanlı Erişim Denetimini REST API’si ile Yönetme](../../role-based-access-control/role-assignments-rest.md)

  Bu makalede RBAC yönetimi için REST API’sinin nasıl kullanılacağı gösterilmektedir.
* [Azure Depolama Kaynak Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Bu API Başvurusu, depolama hesabınızın programlı olarak yönetmek için kullanabileceğiniz API'ler açıklar.
* [Aboneliklere erişmek için Kaynak Yöneticisi'ni kullanın kimlik doğrulama API'si](../../azure-resource-manager/resource-manager-api-authentication.md)

  Bu makalede, Resource Manager API'leri kullanarak kimlik doğrulaması yapmayı gösterir.
* [Ignite’tan Microsoft Azure için Rol Tabanlı Erişim Denetimi](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Bu bağlantı 2015 MS Ignite konferansının 9. Kanalındaki videoya aittir. Bu oturumda, Azure’daki erişim yönetimi ve raporlama özellikleri konuşulmakta ve Azure Active Directory'yi kullanarak Azure aboneliklerine erişimin güvenliğini sağlama konusundaki en iyi uygulamalar keşfedilmektedir.

### <a name="managing-your-storage-account-keys"></a>Depolama hesabı anahtarlarınızı yönetme
Depolama hesabı anahtarları, depolama hesabı adı ile birlikte BLOB, tablo ve kuyruk iletileri bir Azure Dosya paylaşımındaki dosyaları içindeki varlıklar, örneğin depolama hesabında depolanan verileri nesnelere erişmek için kullanılan Azure tarafından oluşturulan 512 bit dizelerdir. Depolama hesabı anahtarları denetimleri veri düzlemine erişim bu depolama hesabı için erişimi denetleme.

Her Depolama hesabında "Anahtar 1" ve "2. anahtara" anılacaktır iki anahtar olduğundan [Azure portalında](https://portal.azure.com/) ve PowerShell cmdlet'leri. Bunlar dahil ancak bunlarla sınırlı olmamak kullanarak birkaç yöntemden birini kullanarak el ile yeniden üretilebilir [Azure portalında](https://portal.azure.com/), PowerShell, Azure CLI veya .NET depolama istemci kitaplığı veya Azure Storage Hizmetleri programlı olarak kullanma REST API.

Herhangi bir sayı, depolama hesabı anahtarlarını yeniden nedenleri vardır.

* Bunları düzenli güvenlik nedenleriyle oluşturduğunuzda.
* Birisi bir uygulamaya hack ve sabit kodlanmış veya bir yapılandırma dosyasında kaydedilen anahtarı almak depolama hesabınıza tam erişim vermeden yönetiliyorsa, depolama hesabı anahtarlarını yeniden.
* Takımınız, depolama hesabı anahtarı koruyan bir Depolama Gezgini uygulama kullanıyor ve takım üyelerinden bırakır yeniden anahtar oluşturma işlemi için başka bir durumdur. Uygulamanın çalışması bunlar gittiğinden sonra erişim, depolama hesabınıza vermeden devam eder. Aslında hesap düzeyinde paylaşılan erişim imzaları oluşturdukları birincil nedeni budur: hesap düzeyi SAS erişim anahtarlarını bir yapılandırma dosyasında depolamak yerine kullanabilirsiniz.

#### <a name="key-regeneration-plan"></a>Anahtarı yeniden üretme planı
Yalnızca bazı planlama olmadan kullandığınız anahtarı yeniden oluşturmak istemiyorsanız. Bunu yaparsanız, büyük bir kesintiye neden olabilir, depolama hesabı için tüm erişimi kesin. İki anahtar vardır nedeni budur. Bir kerede bir anahtar.

Anahtarlarınızı yeniden önce tüm depolama hesabına bağlı uygulamalarınızın yanı sıra Azure'da kullandığınız hizmetlerin listesi olduğundan emin olun. Depolama hesabınıza bağlı olan Azure Media Services kullanıyorsanız, anahtarı yeniden oluşturduktan sonra gibi erişim tuşlarını medya hizmetlerinizle yeniden eşitleme gerekir. Depolama Gezgini gibi tüm uygulamaları kullanıyorsanız, de bu uygulamalar için yeni anahtarlarımı vermem gerekir. VHD dosyaları depolama hesabında depolanır Vm'leriniz varsa, bunlar depolama hesabı anahtarlarını yeniden oluşturma tarafından etkilenmez.

Anahtarlarınızı Azure portalında yeniden oluşturabilirsiniz. Anahtarları yeniden sonra bunlar depolama hizmetleri arasında eşitlenmesi 10 dakika kadar sürebilir.

Hazır olduğunuzda, anahtarınızı nasıl değiştiğini gerçekleşen Genel süreç şöyledir. Bu durumda, anahtar 1 kullanmakta olduğunuz ve her şeyi anahtar 2 kullanmayı değiştirmek için oluşturacağınız varsayılır.

1. Anahtarı güvenli olduğundan emin olmak için 2 yeniden oluşturun. Azure portalında, bunu yapabilirsiniz.
2. Tüm uygulamaların depolama anahtarının depolandığı anahtar 2'in yeni bir değer kullanmak için depolama anahtarı değiştirin. Test ve uygulamayı yayımlayın.
3. Tüm uygulamaları ve Hizmetleri hazır ve anahtar 1 yeniden başarıyla çalışır. Bu, herkes Kime açıkça yeni anahtarı verdiğiniz değil artık depolama hesabına erişimi olacağını sağlar.

Anahtar 2 kullanıyorsanız, aynı işlemi kullanır, ancak anahtar adları ters.

Yeni anahtarı kullanmak için her uygulamayı değiştirme ve yayımlama birkaç gün, geçirebilirsiniz. Hepsini tamamladıktan sonra daha sonra geri dönün ve böylece artık çalışır eski anahtarını yeniden.

Depolama hesabı anahtarı yerleştirmek için başka bir seçenek olan bir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) bir gizli dizi olarak sahip uygulamalarınızı almanıza ve anahtar buradan. Anahtarını yeniden oluşturun ve Azure anahtar Kasası'nı güncelleştirme zaman ardından uygulamalara bunlar yeni anahtarı Azure Key Vault'tan otomatik olarak seçer olduğundan dağıtılması gerekli değildir. İhtiyacınız olan her zaman anahtar okumasını olabilir veya bellekte önbelleğe ve kullanırken, başarısız olursa anahtarı Azure Key Vault'tan yeniden almak unutmayın.

Azure Key Vault'u kullanarak başka bir depolama anahtarlarınızı için güvenlik düzeyini ekler. Bu yöntemi kullanırsanız, depolama anahtarı sabit kodlanmış kaldırır. Cadde birisi belirli bir izni olmadan anahtarlarına erişim sağlama, konusu No: bir yapılandırma dosyasında hiç gerekir.

Azure Key Vault'u kullanarak başka bir avantajı, erişimi Azure Active Directory'yi kullanarak anahtarlarınızı denetleyebilirsiniz olmasıdır. Başka bir deyişle, Azure Key Vault'tan anahtarları almak ve diğer uygulamalara erişim anahtarlarını izni özellikle vermeden için bunları mümkün olmayacaktır bilmeniz gereken uygulamalar dizi için erişim verebilirsiniz.

> [!NOTE]
> Microsoft, anahtarlar yalnızca biri tüm uygulamalar aynı anda kullanılmasını önerir. Bazı yerlerde anahtar 1 ve anahtar 2'de başkaları kullanıyorsanız anahtarlarınızın Döndür erişim hakkını kaybetmesini bazı uygulama mümkün olmayacaktır.

#### <a name="resources"></a>Kaynaklar

* [Azure portalında depolama hesabı ayarlarını yönetme](storage-account-manage.md)
* [Azure Depolama Kaynak Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Veri düzlemi güvenliği
Veri düzlemi güvenliği Azure Depolama: bloblar, kuyruklar, tablolar ve dosyalar depolanan veri nesnelerini güvenli hale getirmek için kullanılan yöntemler ifade eder. Güvenlik ve veri veri aktarım sırasında şifrelemek için yöntemleri gördük ancak nesnelere erişimi denetleme hakkında nasıl devam?

Azure depolama, veri nesnelerine erişimi yetkilendirmek için üç seçeneğiniz de dahil olmak üzere:

- Kapsayıcılar ve Kuyruklar erişim yetkisi vermek için Azure AD kullanarak. Azure AD yetkilendirme, kodunuzda gizli dizileri depolamak için gereken kaldırmayı da için diğer yaklaşımlar avantaj sağlar. Daha fazla bilgi için [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md). 
- Paylaşılan anahtar aracılığıyla erişim yetkisi vermek için depolama hesabı anahtarlarını kullanma. Microsoft, Azure AD, bunun yerine, mümkün olduğunda kullanarak önerir. Bu nedenle, depolama hesabı anahtarları, uygulamanızda depolama paylaşılan anahtar ile yetkilendirme gerektirir.
- Belirli bir süre için belirli veri nesnelerini denetimli izinler vermek için paylaşılan erişim imzaları kullanma.

Ayrıca, Blob Depolama için genel erişim bloblarınızın için uygun şekilde blobları tutan kapsayıcı için erişim düzeyi ayarlayarak izin verebilirsiniz. Bir kapsayıcıya Blob veya kapsayıcı için erişim ayarlarsanız, bu kapsayıcıdaki blobları için genel okuma erişimini sağlayacaktır. Başka bir deyişle, kapsayıcıdaki bir bloba işaret eden bir URL olan herkes, bir tarayıcıda kullanarak bir paylaşılan erişim imzası veya depolama hesabı anahtarlarını sahip açabilirsiniz.

Yetkilendirme aracılığıyla erişim sınırlama yanı sıra, ayrıca kullanabileceğiniz [güvenlik duvarları ve sanal ağlar](storage-network-security.md) ağ kurallara göre depolama hesabı erişimini sınırlamak için.  Yalnızca belirli Azure sanal ağları ya da genel internet erişimi vermek için ve genel internet trafiği engellemek bu yaklaşım etkinleştirir erişim IP adresi aralıkları.

### <a name="storage-account-keys"></a>Depolama Hesabı Anahtarları
Depolama hesabı anahtarları, depolama hesabında depolanan verileri nesnelere erişmek için depolama hesabı adı ile birlikte kullanıldığında Azure tarafından oluşturulan 512 bit dizelerdir.

Örneğin, BLOB'ları okuma, kuyruklara yazmak, tablolar oluşturun ve dosyalarını değiştirme. Bu eylemlerin çoğunu Azure portalı üzerinden gerçekleştirilebilir veya birçok depolama Gezgin uygulaması birini kullanarak. Ayrıca, bu işlemleri gerçekleştirmek için REST API veya depolama istemci kitaplıklarından birini kullanmak için kod yazabilirsiniz.

Üzerinde bölümünde açıklandığı gibi [yönetim düzlemi güvenlik](#management-plane-security), Klasik depolama hesabı, Azure aboneliğinize tam erişim vererek verilebilir için erişim için depolama anahtarları. Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager modelini kullanarak bir depolama hesabı için depolama anahtarlarına erişimi denetlenebilir.

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanarak hesabınızdaki nesnelere erişim devretmek nasıl
Paylaşılan erişim imzası, depolama nesnelerine erişim ve izinler ve erişim tarih/saat aralığı gibi kısıtlamaları belirtmek izin veren bir URI için bağlı bir güvenlik belirteci içeren bir dizedir.

Bloblar, kapsayıcılar, kuyruk iletileri, dosyaları ve tablolar için erişim verebilirsiniz. Tablolarla, aslında kullanıcının erişmesini istediğiniz bölüm ve satır anahtarı aralığı belirterek varlık tablosuna erişim izni verebilirsiniz. Coğrafi durumunun bölüm anahtarıyla depolanan verileri varsa, örneğin, birisi size California yalnızca verileri için erişim.

Başka bir örnekte, bir web uygulaması, bir kuyruğa girişler yazmak için sağlayan bir SAS belirteci vermek ve bir çalışan rolü uygulama kuyruktan iletileri almak ve bunları işlemek için bir SAS belirteci vermek. Veya, bir müşteri, Blob depolamadaki bir kapsayıcıda resim yükleyebilirsiniz ve bu resimler okumak için bir web uygulaması izin vermek için bir SAS belirteci verebilirsiniz. Her iki durumda da bir görev ayrımı nettir vardır: her uygulamanın kendi görevi gerçekleştirmek için gereken erişim verilebilir. Bu paylaşılan erişim imzaları genelindeki mümkündür.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Paylaşılan erişim imzaları kullanmak istediğiniz neden
Neden çok daha kolaydır, depolama hesabı anahtarını vermek yerine SAS kullanmak istiyorsunuz? Depolama hesabı anahtarınızı anahtarları, depolama Krallık paylaşımı gibi sağlıyor. Tam erişim verir. Birisi anahtarlarınızı kullanın ve bunların tüm Müzik kitaplığı, depolama hesabınıza yükleyin. Bunlar da dosyalarınızın virüs bulaşmış sürümlerle değiştirin veya verilerinizi çalmak. Hemen sınırsız erişim, depolama hesabınıza hafifçe alınmamalıdır bir şey sağlıyor.

Paylaşılan erişim imzaları ile bir istemci, yalnızca sınırlı bir süre için gerekli izinleri verebilirsiniz. Örneğin, birisi bir blob hesabınıza yükleniyor, bunları blob (blob boyutuna göre doğal bağlı olarak) yüklemek için yeterli zaman yazma erişim verebilirsiniz. Ve fikrinizi değiştirirseniz, bu erişimi iptal edebilirsiniz.

Ayrıca, SAS kullanılarak yapılan istekleri belirli bir IP adresi veya IP adresi aralığı Azure'a dış sınırlı olduğunu belirtebilirsiniz. Ayrıca, belirli bir Protokolü (HTTPS veya HTTP/HTTPS) kullanarak isteklerinin yapılma isteyebilirsiniz. Bu, yalnızca HTTPS trafiğine izin vermek istiyorsanız, yalnızca HTTPS için gerekli Protokolü ayarlayabilirsiniz ve HTTP trafiğini engellenir anlamına gelir.

#### <a name="definition-of-a-shared-access-signature"></a>Paylaşılan erişim imzası tanımı
Paylaşılan erişim imzası bir kaynağa işaret eden URL eklenecek sorgu parametreleri kümesidir.

izin verilen erişim ve kendisi için erişimine izin verilen süreyi hakkında bilgi sağlar. Bir örnek aşağıda verilmiştir; Bu URI, beş dakikalığına blob okuma erişimi sağlar. SAS sorgu parametreleri URL kodlu, iki nokta üst üste (:) için % 3A gibi olması gerektiğini unutmayın veya bir alan için % 20.

```
http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)
```

#### <a name="how-the-shared-access-signature-is-authorized-by-the-azure-storage-service"></a>Paylaşılan erişim imzası Azure depolama hizmeti tarafından nasıl yetkisi
Depolama hizmeti isteği aldığında, giriş sorgu parametrelerini alır ve çağıran program aynı yöntemi kullanarak bir imza oluşturur. Ardından, iki imzalarını karşılaştırır. Kullanıcının kabul etmesi durumunda, depolama hizmeti geçerli olduğundan emin olun, geçerli tarih ve saati belirtilen pencereye olduğunu doğrulamak için istenen erişim yapılan istek, vb. için karşılık gelen emin olun depolama hizmet sürümünü kontrol edebilirsiniz.

URL yerine bir blobu bir dosyaya işaret eden, bir blob için paylaşılan erişim imzası olduğunu belirttiği için örneğin, yukarıdaki bizim URL'si ile bu isteği başarısız olur. Çağrılan REST komutu, bir blob güncelleştirilecek olduysa, paylaşılan erişim imzası yalnızca okuma erişimine izin verildiğini belirtir nedeniyle başarısız olur.

#### <a name="types-of-shared-access-signatures"></a>Paylaşılan erişim imzaları türleri
* Bir hizmet düzeyi SAS, depolama hesabındaki belirli kaynaklara erişmek için kullanılabilir. Bu bazı örnekler bir blob yükleme, bir tablodaki bir varlığı güncelleştirmek, bir kuyruğa ileti ekleyerek veya bir dosya paylaşımına dosya karşıya bir kapsayıcıdaki blobları listesini alıyor.
* Hesap düzeyi SAS, bir hizmet düzeyi SAS için kullanılabilecek herhangi bir şey erişmek için kullanılabilir. Ayrıca, bu seçenekleri kapsayıcıları, tabloları, kuyrukları ve dosya paylaşımları oluşturma olanağı gibi bir hizmet düzeyi SAS ile izin verilmeyen kaynaklara verebilirsiniz. Aynı anda birden çok hizmetlere erişimi de belirtebilirsiniz. Örneğin, birisi verebilir hem BLOB hem de depolama hesabınızda dosyaları erişim.

#### <a name="creating-a-sas-uri"></a>SAS URI'si oluşturma
1. Tüm sorgu parametrelerinin her tanımlama, isteğe bağlı olarak bir URI oluşturabilirsiniz.

   Bu yaklaşım esnektir, ancak her zaman benzer parametreler mantıksal bir dizi varsa, bir depolanmış erişim ilkesini kullanarak daha iyi bir fikir olabilir.
2. Bir kapsayıcının tamamını, dosya paylaşımı, tablo veya kuyruk için depolanan bir erişim ilkesi oluşturabilirsiniz. Ardından, bu temel olarak oluşturduğunuz SAS URI kullanabilirsiniz. İzinleri depolanmış erişim ilkelerine bağlı olarak kolayca iptal edilebilir. Her kapsayıcı, kuyruk, tablo veya dosya paylaşımı üzerinde tanımlanan en fazla beş ilkelere sahip olabilir.

   Örneğin, belirli bir kapsayıcıdaki blobları okuma çoğu kişi için oluşturacağınız, bir depolanmış erişim "okuma erişimi verin" ve her zaman aynı olacaktır herhangi bir ayarı bildiren ilkesi oluşturabilirsiniz. Ardından, sona erme tarihi/saati belirten ve depolanan erişim ilkesi ayarlarını kullanarak bir SAS URI'si oluşturabilirsiniz. Bu avantajı, her zaman tüm sorgu parametreleri belirtmeniz gerekmez ' dir.

#### <a name="revocation"></a>İptal etme
SAS tehlikede veya Kurumsal güvenlik veya yasal uyumluluk gereksinimleri nedeniyle değişiklik yapmak istediğiniz varsayalım. Bu SAS kullanarak bir kaynağa erişimi nasıl iptal? Bu, SAS URI'sini nasıl oluşturulacağını bağlıdır.

Geçici bir URI'leri kullanıyorsanız, üç seçeneğiniz vardır. Kısa bir süre sonu ilkeleri ile SAS belirteçlerini vermek ve SAS sona bekleyin. Yeniden adlandırma veya silme (belirteç tek bir nesne olarak kapsamlı varsayılarak) kaynak. Depolama hesabı anahtarlarını değiştirebilirsiniz. Bu son seçenek, kaç Hizmetleri, depolama hesabı kullanıyorsanız bağlı olarak önemli bir etkisi olabilir ve büyük olasılıkla bazı planlama olmadan yapmak istediğiniz bir şey değildir.

Depolanan bir erişim ilkesi tarafından türetilmiş bir SAS kullanıyorsanız, depolanmış erişim ilkesini iptal ederek erişimi kaldırabilirsiniz: yalnızca zaten doldu ya da tamamen kaldırın değiştirebilirsiniz. Bu durum, hemen geçerli olur ve bu depolanmış erişim ilkesini kullanılarak oluşturulan tüm SAS'ı geçersiz kılar. Güncelleştirme veya depolanmış erişim ilkesini kaldırma etkisi kişiler bu belirli kapsayıcı dosya paylaşımına erişen tablo veya eskisinin geçersiz hale geldiğinde yeni bir SAS istediklerinde bu nedenle istemcilerin yazılır, ancak sıra aracılığıyla SAS, bu düzgün çalışır.

Depolanan bir erişim ilkesi tarafından türetilmiş bir SAS kullanarak bu SAS hemen iptal etme olanağı sağlar, her zaman mümkün olduğunda depolanan erişim ilkelerini kullanmak için önerilen en iyi uygulama olmasıdır.

#### <a name="resources"></a>Kaynaklar
Paylaşılan erişim imzalarını ve depolanan erişim ilkelerini örnekleri ile tam kullanma hakkında daha ayrıntılı bilgi için aşağıdaki makalelere bakın:

* Başvuru makalelerimize şunlardır.

  * [Hizmet SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    Bu makalede, BLOB, kuyruk iletileri, tablo aralıkları ve dosyaları ile hizmet düzeyi SAS kullanarak örnekler sağlar.
  * [Hizmet SAS oluşturma](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Bir hesap SAS oluşturma](https://msdn.microsoft.com/library/mt584140.aspx)
* Paylaşılan erişim imzalarını ve depolanan erişim ilkelerini oluşturmak için .NET istemci kitaplığını kullanma öğreticileri şunlardır.

  * [Paylaşılan erişim imzaları (SAS) kullanma](../storage-dotnet-shared-access-signature-part-1.md)
  * [Paylaşılan erişim imzaları, bölüm 2: Oluşturma ve Blob hizmetiyle SAS kullanma](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Bu makale, SAS Modeli'ni veya paylaşılan erişim imzaları örnekleri bir açıklama içerir ve SAS'ın en iyi yöntem önerileri kullanın. De ele izni iptal olur.

* Authentication

  * [Azure Storage Hizmetleri için kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Paylaşılan erişim imzaları öğretici kullanmaya başlama

  * [SAS başlama Öğreticisi.](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
### <a name="transport-level-encryption--using-https"></a>HTTPS kullanarak aktarım düzeyinde şifreleme:
Azure depolama verilerinizin güvenliğini sağlamak için yapmanız gereken başka bir Azure depolama ve istemci arasında verileri şifrelemek için bir adımdır. İlk öneri her zaman kullanmaktır [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protokolü, genel Internet üzerinden güvenli iletişim sağlar.

Depolama REST API'lerini çağırma veya erişme nesneleri güvenli bir iletişim kanalı sağlamak için her zaman HTTPS kullanmanız gerekir. Ayrıca, **paylaşılan erişim imzaları**, herkes gönderme sağlanarak paylaşılan erişim imzaları kullanırken yalnızca HTTPS protokolünü kullanılabileceğini belirtmek için bir seçenek Azure depolama nesneleri erişimi devretmek için kullanılabilecek SAS bağlantıları dışarı belirteçleri doğru protokolü kullanır.

Depolama hesaplarında etkinleştirerek nesneleri erişmek için REST API'lerini çağırma HTTPS kullanılmasını zorunlu kılabilir [güvenli aktarım gerekli](../storage-require-secure-transfer.md) depolama hesabı için. Bu etkinleştirildikten sonra HTTP kullanarak bağlantı reddedilecek.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Azure dosya paylaşımları ile aktarım sırasında şifreleme kullanma
[Azure dosyaları](../files/storage-files-introduction.md) dosya REST API'sini kullanarak şifreleme aracılığıyla SMB 3.0 ve HTTPS ile destekler. Örneğin, şirket içi veya başka bir Azure bölgesi içinde bulunduğu Azure bölgesinin dışında Azure dosya paylaşımını bağlama, SMB 3.0 şifreleme ile her zaman gereklidir. Varsayılan olarak bağlantıları yalnızca Azure aynı bölgede içinde izin verilir, ancak şifreleme ile SMB 3.0 zorunlu tarafından SMB 2.1 şifrelemeyi desteklemiyor [güvenli aktarım gerektiren](../storage-require-secure-transfer.md) depolama hesabı için.

SMB 3.0 şifreleme ile kullanılabilir [tüm Windows ve Windows Server işletim sistemlerinde desteklenen](../files/storage-how-to-use-files-windows.md) Windows 7 ve Windows Server 2008 R2 dışında yalnızca destekleyen SMB 2.1. Üzerinde de SMB 3.0 desteklenir [macOS](../files/storage-how-to-use-files-mac.md) ve dağıtımlarını [Linux](../files/storage-how-to-use-files-linux.md) Linux çekirdeği 4.11 kullanan ve üstü. SMB 3.0 şifreleme desteği de çeşitli Linux dağıtımları tarafından Linux çekirdeğinin eski sürümlerine yönelik backported, başvurun [anlama SMB istemci gereksinimleri](../files/storage-how-to-use-files-linux.md#smb-client-reqs).

### <a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Depoya gönderdiğiniz verilerin güvenliğini sağlamak için istemci tarafı şifreleme kullanma
Bir istemci uygulaması ve depolama arasında aktarılırken verilerinizin güvenli olduğundan emin olmanıza yardımcı olan başka bir istemci tarafı şifreleme seçenektir. Verileri Azure Depolama'ya aktarılmadan önce şifrelenir. İstemci tarafında alındıktan sonra Azure Depolama'dan veri alma, verilerin şifresi çözülür. Veri bütünlük denetimi, veri bütünlüğünü etkileyen ağ hatalarını giderme Yardımı'nda yerleşik olarak sahip olduğundan kablo giden veriler şifrelenir, ancak aynı zamanda HTTPS kullanmanızı öneririz.

Veriler, şifrelenmiş biçimde depolanır istemci tarafı şifreleme Ayrıca, bekleyen verilerinizi şifrelemek için bir yöntem aynıdır. Bu konuda bölümünde daha ayrıntılı hakkında konuşacağız [bekleyen şifreleme](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Bekleme sırasında şifreleme sağlayan üç Azure özellikleri vardır. Azure Disk şifrelemesi, işletim sistemi ve veri diskleri Iaas sanal Makineler'de şifrelemek için kullanılır. İstemci tarafı şifreleme ve SSE hem de Azure Depolama'da verileri şifrelemek için kullanılır. 

(Bu da şifrelenmiş biçimde depolama alanında depolanır) Aktarımdaki verileri şifrelemek için istemci tarafı şifreleme kullanabilmenize karşın, aktarım sırasında HTTPS kullanıyorsanız ve bu şekilde depolandığında, otomatik olarak şifrelenecek veriler için tercih edebilirsiniz. --Bunu yapmanın iki yolu Azure Disk şifrelemesi ve SSE vardır. Bir doğrudan VM'ler tarafından kullanılan işletim sistemi ve veri diskleri verileri şifrelemek için kullanılır ve diğer Azure Blob depolama alanına yazılan verileri şifrelemek için kullanılır.

### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

SSE tüm depolama hesaplarında etkinleştirilir ve devre dışı bırakılamaz. SSE, Azure depolama alanına yazarken, verilerinizi otomatik olarak şifreler. Azure Depolama'dan veri okuma, döndürülmeden önce Azure Depolama tarafından şifresi çözülür. SSE, kodu değiştirin veya kod tüm uygulamalarınıza eklemek zorunda kalmadan verilerinizin güvenliğini sağlar.

Microsoft tarafından yönetilen anahtarlar ya da kendi özel anahtarları da kullanabilirsiniz. Microsoft yönetilen anahtarlarını oluşturur ve iç Microsoft İlkesi tarafından tanımlandığı gibi normal döndürme yanı sıra kendi güvenli depolama işler. Özel anahtarları kullanma hakkında daha fazla bilgi için bkz. [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).

SSE tüm performans katmanları (Standart ve Premium), tüm dağıtım modelleri (Azure Resource Manager ve Klasik) ve tüm Azure Depolama hizmetlerinde (Blob, Kuyruk, Tablo ve Dosya) verileri otomatik olarak şifreler. 

### <a name="client-side-encryption"></a>İstemci tarafı şifreleme
İstemci tarafı şifreleme Aktarımdaki verilerin şifrelenmesini tartışırken belirttiğimiz. Bu özellik, verilerinizi kablo göndermeden önce bir istemci uygulamasında Azure depolama alanına yazılacak ve programlı olarak da Azure Depolama'dan aldıktan sonra verilerin şifresini çözmek için programlı olarak şifrelemek verir.

Bu aktarım sırasında şifreleme sağlar, ancak Ayrıca, bekleme sırasında şifreleme özelliği sağlar. Veriler aktarım sırasında şifrelenir olsa da, veri bütünlüğünü etkileyen ağ hatalarını giderme Yardımı yerleşik veri bütünlük denetimi yararlanmak için HTTPS kullanmanızı hala öneririz.

Bu burada kullanabileceğinize bir örnek blobları depolar ve BLOB'ları alan bir web uygulamasına sahip ve uygulama ve verilerin olabildiğince güvenli olmasını istediğiniz ' dir. Bu durumda, istemci tarafı şifreleme kullanırsınız. Azure Blob hizmeti ile istemci arasındaki trafiği şifrelenmiş kaynağı içeriyor ve hiç kimse Aktarımdaki verileri yorumlama ve bunu özel bloblarınızın yeniden oluşturmak.

Java ve Azure anahtar kasası uygulamak kolaylaştıran API'leri sırayla kullanın .NET depolama istemci kitaplıkları, istemci tarafı şifreleme yerleşiktir. Şifreleme ve verilerin şifresini çözme işlemini Zarf teknik kullanır ve şifreleme her depolama nesnesi tarafından kullanılan meta verileri depolar. Örneğin, BLOB'ları için bunu depoladığı blob meta verilerini sırada sıralar, bunu, her kuyruk iletisi ekler.

Şifreleme için kendi oluşturabilir ve kendi şifreleme anahtarlarınızı yönetme. Azure depolama istemci kitaplığı tarafından oluşturulan anahtarları da kullanabilirsiniz veya Azure anahtar kasası anahtarlarını sahip olabilir. Şifreleme anahtarlarınızı şirket içi anahtar Depolama'nızda depolamak veya bir Azure anahtar Kasası'nda depolayabilirsiniz. Azure Key Vault gizli dizileri Azure Key vault'ta Azure Active Directory'yi kullanarak belirli kullanıcılar için erişim sağlar. Bu, yalnızca herkes Azure anahtar Kasası'nı okuyun ve böylelikle kullandığınız için istemci tarafı şifreleme anahtarlarını alma anlamına gelir.

#### <a name="resources"></a>Kaynaklar
* [Şifreleme ve şifre çözme Azure anahtar Kasası'nı kullanarak Microsoft Azure depolama BLOB'ları](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  Bu makalede KEK oluşturma ve PowerShell kullanarak depolama da dahil olmak üzere, Azure Key Vault ile istemci tarafı şifreleme kullanmayı gösterir.
* [Microsoft Azure depolama istemci tarafı şifreleme ve Azure anahtar kasası](../storage-client-side-encryption.md)

  Bu makalede, istemci tarafı şifreleme bir açıklama sunar ve şifreleme ve şifre çözme dört depolama hizmetlerinde kaynakları için depolama istemcisi kitaplığı kullanma örnekleri sağlar. Ayrıca, Azure Key Vault hakkında konuşuyor.

### <a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Diskleri, sanal makineler tarafından kullanılan şifreleme için Azure Disk Şifrelemesi'ni kullanma
Azure Disk şifrelemesi yeni bir özelliktir. Bu özellik işletim sistemi diskleri ve veri diskleri bir Iaas sanal makine tarafından kullanılan şifrelemenizi sağlar. Windows için sürücüleri, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için DM-Crypt teknolojisini kullanarak diskleri şifrelenir. Bu, disk şifreleme anahtarlarını yönetmek ve denetlemek izin vermek için Azure anahtar kasası ile tümleştirilmiştir.

Microsoft Azure'da etkinleştirildiğinde çözüm Iaas Vm'leri için aşağıdaki senaryoları destekler:

* Azure Key Vault ile tümleştirme
* Standart katmanı Vm'lerini: [A, D, DS, G, GS ve benzeri serisi Iaas Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Windows ve Linux Iaas Vm'lerinde şifrelemesini etkinleştirme
* Windows Iaas Vm'leri için sürücüler işletim sistemi ve veri şifrelemeyi devre dışı bırakma
* Linux Iaas sanal makineler için veri sürücülerini şifreleme devre dışı bırakma
* Windows istemci işletim sistemi çalıştıran bir Iaas Vm'leri şifrelemesini etkinleştirme
* Bağlama yolu içeren birimlerde şifrelemesini etkinleştirme
* Linux vm'lerinde mdadm kullanarak (RAID) bölümlenerek diskle yapılandırılmış şifrelemesini etkinleştirme
* Veri diskleri için LVM kullanarak Linux vm'lerinde şifrelemesini etkinleştirme
* Depolama alanları kullanılarak yapılandırılan Windows Vm'leri üzerinde şifrelemeyi etkinleştirme
* Tüm genel Azure bölgelerinde desteklenir

Çözüm aşağıdaki senaryoları, özellikleri ve teknoloji sürümde desteklemez:

* Temel katman Iaas Vm'leri
* Linux Iaas Vm'leri için şifreleme bir işletim sistemi sürücüsünde devre dışı bırakma
* Iaas VM'ler, Klasik VM oluşturma yöntemini kullanarak oluşturulur
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme
* Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılan Windows Vm'leri


> [!NOTE]
> Linux işletim sistemi disk şifreleme şu anda aşağıdaki Linux dağıtımlarında desteklenir: RHEL 7.2 CentOS 7.2n ve Ubuntu 16.04.
>
>

Bu özellik, sanal makine disklerindeki tüm veriler şifrelenir, Azure Depolama'daki sağlar.

#### <a name="resources"></a>Kaynaklar
* [Windows ve Linux Iaas sanal makineleri için Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Azure Disk şifrelemesi, SSE ve istemci tarafı şifreleme karşılaştırması

#### <a name="iaas-vms-and-their-vhd-files"></a>Iaas Vm'leri ve bunların VHD dosyaları

Iaas VM'ler tarafından kullanılan veri diskleri için Azure Disk şifrelemesi önerilir. Azure Marketi'nden bir görüntü kullanarak yönetilmeyen disklerle bir VM oluşturursanız, Azure gerçekleştirir bir [yüzeysel kopya](https://en.wikipedia.org/wiki/Object_copying) SSE etkin olsa bile görüntünün depolama ve Azure depolama hesabında şifrelenmez. Sanal makine oluşturur ve görüntü güncelleştiriliyor başlatır, SSE veri şifreleme başlar. Bu nedenle, Azure Disk şifrelemesi tamamen şifreli istiyorsanız görüntüleri Azure Market'te oluşturulan yönetilmeyen diskleri olan Vm'lerde kullanmak en iyisidir. Yönetilen disklerle bir VM oluşturursanız, SSE platform yönetilen anahtarlar kullanılarak varsayılan olarak tüm verileri şifreler. 

Şirket içinden Azure'a önceden şifrelenmiş VM getirirseniz, şifreleme anahtarları Azure Key Vault'a yüklemek ve şirket içinde kullandığınız bu VM için şifreleme kullanmaya devam etmek mümkün olacaktır. Azure Disk şifrelemesi, bu senaryonun işlenmesi için etkinleştirilir.

Şirket içi şifrelenmemiş VHD'den varsa, özel görüntü olarak galeri uygulamasına yükleyin ve bir VM'den sağlayın. Resource Manager şablonları kullanarak bunu, VM'yi önyükleme yaptığında, Azure Disk şifrelemesini etkinleştirmeniz isteyebilirsiniz.

Veri diski ekleme ve VM'de bağlama, bu verileri disk üzerinde Azure Disk şifrelemesini kapatabilirsiniz. Yerel olarak, veri diski önce şifreler ve depolama içeriği şifrelenir, böylece Klasik dağıtım modeli katmanı depolama karşı yavaş yazma sonra ne yapacağını.

#### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme
Geçiş öncesinde verilerin şifrelediğinden istemci tarafı şifreleme, veri şifreleme en güvenli yöntemdir.  Ancak, kod yapmak isteyebilirsiniz depolama kullanarak uygulamalarınıza eklemeniz gerekmez. Bu gibi durumlarda, HTTPS Aktarımdaki verilerinizin güvenliğini sağlamak için kullanabilirsiniz. Azure depolama veri ulaştığında, SSE tarafından şifrelenir.

İstemci tarafı şifreleme ile tablo varlıkları, blobları ve kuyruk iletileri şifreleyebilirsiniz. 

İstemci tarafı şifreleme, uygulama tarafından tamamen yönetilir. Bu en güvenli yaklaşımdır, ancak uygulamanızı programlı değişiklik ve anahtar yönetimi işlemlerini yerleştirdiniz gerektirir. Bu, geçiş sırasında ek güvenlik istediğiniz ve depolanan verilerinizin şifrelenmesini istediğiniz zaman kullanırsınız.

İstemci tarafı şifreleme istemci üzerinde daha fazla yük, ve özellikle, şifreleme ve büyük miktarda veri aktarmak için bu ölçeklenebilirlik planlarınızda hesaba sahip.

#### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

SSE, Azure Depolama tarafından yönetilir. SSE Aktarımdaki verilerin güvenliğini sağlamaz, ancak Azure Depolama'ya yazılan verileri şifreleyin. SSE, Azure Depolama performansını etkilemez.

Her tür veriyi SSE kullanarak depolama hesabının şifreleyebilir (blok blobları, blobları, sayfa blobları, tablo verilerini, sıra veri ve dosya ekleme).

Bir arşiv veya yeni sanal makineler oluşturmak için temel olarak kullanan VHD dosyalarını kitaplığı varsa, yeni bir depolama hesabı oluşturun ve ardından ilgili hesabı VHD dosyalarını karşıya yükleyin. Bu VHD dosyalarını Azure Depolama tarafından şifrelenir.

VM diskleri için Azure Disk şifrelemesini etkinleştirmişseniz, herhangi bir yeni yazılmış veri SSE hem Azure Disk şifrelemesi ile şifrelenir.

## <a name="storage-analytics"></a>Depolama Analizi
### <a name="using-storage-analytics-to-monitor-authorization-type"></a>Yetkilendirme türünü izlemek için depolama analizi kullanma
Her Depolama hesabı için günlük gerçekleştirmek ve ölçüm verilerini depolamak Azure depolama çözümlemeleri etkinleştirebilirsiniz. Bu performans ölçümlerinden birinde bir depolama hesabı denetlemek istediğiniz ya da performans sorunlarınız için bir depolama hesabı gidermeye ihtiyaç duyan kullanmak için harika bir araçtır.

Depolama analizi günlüklerinde gördüğünüz verilerin başka bir parça, depolama eriştiklerinde birisi tarafından kullanılan kimlik doğrulama yöntemidir. Örneğin, Blob Depolama ile paylaşılan erişim imzası veya depolama hesabı anahtarları kullandıysanız ya da erişilen blob genel görebilirsiniz.

Bu, depolama alanına erişimi sıkı bir şekilde kullanılan koruyarak istediğinizde yararlı olabilir. Örneğin, Blob Depolama alanında tüm kapsayıcıları private olarak ayarlayın ve bir SAS hizmet kullanımını uygulamalarınız uygulayın. Ardından bir güvenlik ihlalini gösterebilir, depolama hesabı anahtarlarını kullanarak bloblarınızın erişilen veya bloblar herkese açık, ancak bunlar olmamalıdır düzenli olarak görmek için günlükleri gözden geçirebilirsiniz.

#### <a name="what-do-the-logs-look-like"></a>Günlükleri ne gibi görünüyor?
Sonra depolama hesabı ölçümleri etkinleştirme ve Azure portalı üzerinden günlüğe kaydetme, analiz verilerini birikerek hızla başlar. Günlüğe kaydetme ve ölçümler her hizmet için ayrı; günlüğe kaydetme, yalnızca bir etkinlik olduğunda bu depolama hesabında ölçüm dakikada, saatte veya nasıl yapılandırdığınıza bağlı olarak, her gün günlüğe kaydedilir ancak yazılır.

Günlükler, depolama hesabındaki $logs adlı bir kapsayıcı içinde blok blobları olarak depolanır. Bu kapsayıcı, depolama analizi etkinleştirildiğinde otomatik olarak oluşturulur. Bu kapsayıcı oluşturulduktan sonra içeriği silebilir ancak bunu silemezsiniz.

$Logs kapsayıcısı altında her hizmet için bir klasör bulunur ve ardından vardır alt yıl/ay/gün/saat. Saat altında günlükleri numaralandırılır. Bu dizin yapısı aşağıdaki gibi görünür.

![Günlük dosyalarının görüntüle](./media/storage-security-guide/image1.png)

Azure depolama için her isteği günlüğe kaydedilir. İlk birkaç alanlarını gösteren, bir günlük dosyasının anlık görüntüsünü aşağıda verilmiştir.

![Günlük dosyasının anlık görüntü](./media/storage-security-guide/image2.png)

Günlükleri bir depolama hesabına çağrıları herhangi bir türden izlemek için kullanabileceğiniz görebilirsiniz.

#### <a name="what-are-all-of-those-fields-for"></a>Tüm bu alanlar için nelerdir?
Var olan kaynakları, aşağıda listelenen bir makale günlüklerine ve ne için kullanıldıkları birçok alanların listesini sağlar. Alanları sırada listesi aşağıda verilmiştir:

![Anlık görüntü alan bir günlük dosyasına](./media/storage-security-guide/image3.png)

GetBlob girdilerini ilgilendiğimiz ve yetkileri nasıl kadar ihtiyacımız olan işlem türü "Get-Blob" girdilerini arayın ve istek durumunu denetlemek (Dördüncü</sup> sütun) ve Yetkilendirme türü (sekizinci</sup> sütunu).

Örneğin, yukarıdaki listede ilk birkaç satırı, istek durumu "Başarılı" olan ve Yetkilendirme türü "kimlik doğrulaması". Bu depolama hesabı anahtarını kullanarak istek yetkili olduğu anlamına gelir.

#### <a name="how-is-access-to-my-blobs-being-authorized"></a>Nasıl yetki verilirken my bloblara erişimi var mı?
İlgileniriz üç durumda sahibiz.

1. Blob ortak ve paylaşılan erişim imzası olmadan bir URL kullanarak erişilir. Bu durumda, istek-status "AnonymousSuccess" ve "anonim" Yetkilendirme türü.

   1.0;2015-11-17T02:01:29.0488963Z;GetBlob;**AnonymousSuccess**;200;124;37;**anonymous**;;mystorage…
2. Blob özel olduğu ve paylaşılan erişim imzası ile kullanıldı. Bu durumda, istek-status "SASSuccess" ve "sas" Yetkilendirme türü olan.

   1.0;2015-11-16T18:30:05.6556115Z;GetBlob;**SASSuccess**;200;416;64;**sas**;;mystorage…
3. Blob özel olduğu ve depolama anahtarını erişmek için kullanıldı. Bu durumda, istek durumu olan "**başarı**"ve Yetkilendirme türü"**kimliği doğrulanmış**".

   1.0;2015-11-16T18:32:24.3174537Z;GetBlob;**Success**;206;59;22;**authenticated**;mystorage…

Microsoft ileti Çözümleyicisi'ni görüntülemek ve bu günlükleri analiz etmek için kullanabilirsiniz. Bu, arama ve filtreleme yetenekleri içerir. Örneğin, aramak istediğiniz, diğer bir deyişle, beklediğiniz kullanım ise emin olmak için biri değil eriştiği, depolama hesabınızı uygunsuz bir şekilde görmek için GetBlob örnekleri.

#### <a name="resources"></a>Kaynaklar
* [Depolama Analizi](../storage-analytics.md)

  Bu makalede, depolama analizi ve bunları etkinleştirmek nasıl bir genel bakıştır.
* [Depolama analizi günlük biçimi](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Bu makalede, depolama analizi günlük biçimi gösterir ve istek için kullanılan kimlik doğrulama türünü gösteren kimlik doğrulama-türü de dahil olmak üzere, kullanılabilir alanların ayrıntıları.
* [Azure portalında depolama hesabı izleme](../storage-monitor-storage-account.md)

  Bu makalede, bir depolama hesabına yönelik günlüğe kaydetme ve ölçümlerini izleme nasıl yapılandırılacağı gösterilmektedir.
* [Azure depolama ölçümlerini ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi kullanarak uçtan uca sorun giderme](../storage-e2e-troubleshooting.md)

  Bu makalede, depolama analizi kullanarak sorun giderme hakkında konuşuyor ve Microsoft Message Analyzer'ı kullanmayı gösterir.
* [Microsoft Message Analyzer işletim kılavuzu](https://technet.microsoft.com/library/jj649776.aspx)

  Bu makalede, Microsoft Message Analyzer içindir ve öğretici, hızlı ve özelliklerine ilişkin Özete bağlantılarını içerir.

## <a name="cross-origin-resource-sharing-cors"></a>Çıkış Noktaları Arası Kaynak Paylaşımı (CORS)
### <a name="cross-domain-access-of-resources"></a>Kaynak etki alanları arası erişim
Bir etki alanında çalışan bir web tarayıcısı farklı bir etki alanından bir kaynak için bir HTTP isteği yaptığında, bu çıkış noktaları arası HTTP isteği olarak adlandırılır. Örneğin, contoso.com sunulan bir HTML sayfası fabrikam.blob.core.windows.net üzerinde barındırılan bir jpeg için bir istek gönderir. Güvenlik nedenleriyle, tarayıcılar arası HTTP istekleri JavaScript gibi betiklerin ve başlatılan kısıtlayın. Bu miktar JavaScript kodu bir web sayfasında contoso.com, jpeg fabrikam.blob.core.windows.net üzerinde istediğinde, tarayıcısı istek izin vermeyeceğini anlamına gelir.

Hangi Azure depolama ile yapmak yok? Da JSON veya XML veri dosyaları gibi statik varlıklar depoluyorsanız Blob Depolama alanında Fabrikam adlı bir depolama hesabı kullanarak, etki alanı varlıkları için fabrikam.blob.core.windows.net olur ve contoso.com web uygulaması, bunlara erişmek mümkün olmayacaktır kullanma JavaScript etki alanları farklı olduğundan. Bu da bir Azure depolama hizmetleri: Tablo depolama gibi– çağrılacak çalışıyorsanız, JavaScript istemci tarafından işlenecek JSON verileri döndürmek geçerlidir.

#### <a name="possible-solutions"></a>Olası çözümler
Bu sorunu çözmek için bir yolu, "storage.contoso.com" gibi özel bir etki alanı için fabrikam.blob.core.windows.net atamaktır. Bir depolama hesabına yalnızca bu özel etki alanı atayabilirsiniz sorunudur. Peki varlıkları birden çok depolama hesaplarında depolanır?

Bu sorunu çözmek için başka bir yolu, depolama çağrıları için proxy olarak davranmasına web uygulamasına sahip olmaktır. Bu, bir dosyayı Blob deposuna yükleme, web uygulaması ya da yerel olarak yazın ve ardından Blob depolamaya kopyalama veya tümünün belleğe okuma ve ardından Blob depolamaya yazma, anlamına gelir. Alternatif olarak, dosyaları yerel olarak yükler ve Blob depolama alanına Yazar bir adanmış web uygulaması (örneğin, bir Web API'si) yazabilirsiniz. Her iki durumda da, ölçeklenebilirlik belirleme ihtiyacı olduğunda bu işlev için hesabı gerekir.

#### <a name="how-can-cors-help"></a>CORS nasıl yardımcı olabilir?
Azure depolama, CORS – arası kaynak paylaşımı kaynak etkinleştirmenize izin verir. Her Depolama hesabı için bu depolama hesabındaki kaynaklara erişebilen bir etki alanı belirtebilirsiniz. Örneğin, yukarıda özetlenen Örneğimizde, biz CORS fabrikam.blob.core.windows.net depolama hesabında etkinleştirebilir ve contoso.com için erişime izin verecek şekilde yapılandırın. Daha sonra web uygulaması contoso.com fabrikam.blob.core.windows.net kaynaklara doğrudan erişebilirsiniz.

Unutmayın, CORS erişim sağlar, ancak tüm ortak olmayan erişim için depolama kaynakları için gerekli olan kimlik doğrulaması sağlamaz değildir. Başka bir deyişle, bunlar geneldir veya bir paylaşılan erişim imzası uygun vermiş dahil yalnızca BLOB'lar erişebilirsiniz. Tablolar, kuyruklar ve dosyalar genel erişimi yok ve bir SAS gerektirir.

Varsayılan olarak, CORS tüm hizmetleri devre dışıdır. CORS hizmeti ilkeleri ayarlamak için yöntemlerden birini çağırmak için REST API veya depolama istemci kitaplığı kullanarak etkinleştirebilirsiniz. Bunu yaptığınızda, XML'de bir CORS kuralı içerir. Blob hizmeti için hizmeti özelliklerini ayarla işlemine kullanarak bir depolama hesabı için ayarlanmış bir CORS kuralı örneği aşağıda verilmiştir. Azure depolama için depolama istemcisi kitaplığı veya REST API'leri kullanarak bu işlemi gerçekleştirebilir.

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Her satır anlamı aşağıda verilmiştir:

* **AllowedOrigins** eşleşmeyen hangi etki alanlarının talep edip veri almak ve storage hizmetinden bu bildirir. Bu, veri hem contoso.com ve fabrikam.com belirli bir depolama hesabı için Blob depolama alanından isteyebilir diyor. Bu da bir joker karakter için ayarlayabilirsiniz (\*) erişim isteklerini tüm etki alanları izin vermek için.
* **AllowedMethods** isteği yaparken kullanılan yöntem (HTTP istek fiilleri) listesidir. Bu örnekte, yalnızca GET ve PUT izin verilir. Bunun için joker karakter ayarlayabilirsiniz (\*) kullanılacak tüm yöntemleri izin vermek için.
* **AllowedHeaders** kaynak etki alanı isteği yapılırken belirtebilirsiniz istek üstbilgilerini budur. Bu örnekte, x-ms-meta-hedef ve x-ms-meta-abc, x-ms-meta verileri ile başlangıç tüm meta veri üst bilgileri izin verilir. Joker karakter (\*) Belirtilen önek herhangi üstbilgi başlayarak izin verildiğini gösterir.
* **ExposedHeaders** bu isteği gönderene tarayıcı tarafından hangi yanıt üst bilgilerini kullanıma bildirir. Bu örnekte, ile başlayan herhangi bir üst bilgi "x-ms - meta-" sunulur.
* **Maxageınseconds** bir tarayıcının denetim öncesi seçenekler isteğini önbelleğe alacağı en uzun süreyi budur. (Denetim öncesi isteği hakkında daha fazla bilgi için aşağıdaki ilk makaleye bakın.)

#### <a name="resources"></a>Kaynaklar
CORS ve nasıl etkinleştirileceğini hakkında daha fazla bilgi için bu kaynaklara göz atın.

* [Çıkış noktaları arası kaynak paylaşımı (CORS) desteğiyle Azure.com üzerindeki Azure Storage Hizmetleri için](../storage-cors-support.md)

  Bu makalede, CORS ve farklı depolama hizmetleri için kuralları ayarlama hakkında genel bir bakış sağlar.
* [Çıkış noktaları arası kaynak paylaşımı (CORS) desteği için MSDN'de Azure depolama hizmetleri](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Azure Storage Hizmetleri için CORS desteği için başvuru belgeleri budur. Bu uygulama için her depolama hizmeti, makalelerin bağlantıları vardır ve bir örnek gösterir ve CORS dosyasında her öğe açıklar.
* [Microsoft Azure Depolama: CORS ile tanışın](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Bu ilk blog makalesi CORS ile tanışın ve nasıl kullanılacağını gösteren bir bağlantıdır.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Azure depolama güvenliği hakkında sık sorulan sorular
1. **HTTPS protokolünü kullanamıyorsanız miyim Azure depolama içine veya dışına aktarılması blobları bütünlüğünü doğrulamak ne?**

   HTTP yerine HTTPS ve, kullanmanız gereken herhangi bir nedenle blok blobları ile çalışıyorsanız, aktarılan blobları bütünlüğünü doğrulamak amacıyla MD5 denetimini kullanabilirsiniz. Bu ağ/aktarım katmanı hataları koruması olan ancak mutlaka Ara saldırıları ile yardımcı olur.

   Aktarım düzeyi güvenlik sağlayan HTTPS kullanırsanız sonra MD5 denetimi yedekli ve gereksiz kullanmaktır.

   Daha fazla bilgi için lütfen kullanıma [Azure Blob MD5 genel bakış](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **ABD için FIPS uyumluluğu hakkında Kamu?**

   Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) ABD tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar. Federal hükümeti bilgisayar sistemleri hassas verilerin korunması. FIPS etkinleştirme modu Masaüstü veya Windows server işletim sistemi yalnızca FIPS doğrulanmış şifreleme algoritmaları kullanılması gerektiğini bildirir. Uygulamanın uyumlu algoritmalar kullanıyorsa, uygulamaları çalışmamasına neden olur. With.NET Framework sürüm 4.5.2 veya üzeri uygulama bilgisayarda FIPS modundayken FIPS uyumlu algoritmalar kullanmak için şifreleme algoritmalarını otomatik olarak geçer.

   Microsoft FIPS modundayken etkinleştirip etkinleştirmemeye karar vermek için her müşteri kadar bırakır. Varsayılan olarak FIPS modunu etkinleştirmek için hükümet düzenlemeleri olmayan müşteriler için ilgi çekici bir sebep olmadığı inanıyoruz.

### <a name="resources"></a>Kaynaklar
* [Neden "FIPS modunda" artık öneriyoruz değil](https://blogs.technet.microsoft.com/secguide/2014/04/07/why-were-not-recommending-fips-mode-anymore/)

  Bu blog makalesi, FIPS genel bir bakış sağlar ve bunlar varsayılan olarak FIPS modundayken neden etkinleştirme açıklanmaktadır.
* [FIPS 140 doğrulaması](https://technet.microsoft.com/library/cc750357.aspx)

  Bu makalede nasıl Microsoft ürünleri ve şifreleme modüllerine FIPS standardıyla ABD uyumlu hakkında bilgi sağlar. Federal Hükümet.
* ["Sistem şifrelemesi: Şifreleme, kodlama ve imzalama için FIPS uyumlu algoritmaları kullan"güvenlik ayarları etkileri Windows XP ve sonraki Windows sürümlerinde](https://support.microsoft.com/kb/811833)

  Bu makalede, FIPS modunda daha eski Windows bilgisayarları kullanımı hakkında konuşuyor.
