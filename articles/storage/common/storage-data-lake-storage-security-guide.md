---
title: Azure Data Lake depolama Gen2 depolama Güvenlik Kılavuzu | Microsoft Docs
description: Ayrıntılar dahil olmak üzere ancak bunlarla sınırlı olmamak RBAC ve depolama hizmeti şifrelemesi, Azure depolama güvenlik altına almanın birçok yöntemi
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: ce01301455c7abcd26006e622fcfbb8127e1c511
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372497"
---
# <a name="azure-data-lake-storage-gen2-security-guide"></a>Azure Data Lake depolama Gen2 Güvenlik Kılavuzu

Azure Data Lake depolama Gen2, Azure depolama hesaplarında özellikler kümesidir. Bu nedenle, bu makaledeki tüm başvuruları bir Azure depolama hesabı için etkinleştirilmiş hiyerarşik ad alanı ile (Data Lake depolama Gen2 Özellikler) yöneliktir.

- Azure Depolama'ya yazılan tüm veriler, kullanılarak otomatik olarak şifrelenir [depolama hizmeti şifrelemesi (SSE)](storage-service-encryption.md). Daha fazla bilgi için [Azure Blobları, dosyalar, tablolar ve kuyruk depolama için varsayılan şifreleme Duyurusu](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).
- Azure Active Directory (Azure AD) ve rol tabanlı Access Control (RBAC) desteklenen Azure depolama için kaynak yönetimi işlemleri ve veri işlemleri için şu şekilde:
    - Güvenlik ilkeleri ve Azure AD kullanım anahtar yönetimi gibi yönetim işlemlerini kaynak yetkilendirmek için depolama hesabına kapsamına RBAC rolleri atayabilirsiniz.
    - Azure AD tümleştirmesi, Azure Depolama'da veri işlemleri için desteklenir. Bir abonelik, kaynak grubu, depolama hesabı veya bir güvenlik sorumlusu veya Azure kaynakları için yönetilen bir kimlik için tek bir dosya sistemi kapsamı RBAC rolleri atayabilirsiniz. Daha fazla bilgi için [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md).
- Azure depolama veri nesnelerini temsilci erişimi kullanılarak verilebilir [paylaşılan erişim imzaları](../storage-dotnet-shared-access-signature-part-1.md).

Bu makalede, Azure depolama ile kullanılan bu güvenlik özelliklerin her biri bir bakış sağlar. Bağlantılardır her özelliğin ayrıntılarını bunu sağlayacak makaleler için bunu kolayca yapabilirsiniz sağlanan araştırma her konuyla ilgili daha fazla.

Bu makalede ele alınacak konular şunlardır:

* [Yönetim düzlemi güvenlik](#management-plane-security) – depolama hesabınızın güvenliğini sağlama

  Yönetim düzlemi, depolama hesabınızı yönetmek için kullanılan kaynaklardan oluşur. Bu bölümde, Azure Resource Manager dağıtım modeli ve depolama hesaplarınız erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanmayı kapsar. Depolama hesabı anahtarlarınızı ve bunları yeniden nasıl yönetme yöneliktir.
* [Veri düzlemi güvenlik](#data-plane-security) – verilerinize erişim güvenliğini sağlama

  Bu bölümde, depolama hesabınızdaki, yani dosyaları ve dizinleri gerçek veri nesnelerine erişim izin vermeyi paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanarak göz atacağız. Hizmet düzeyi SAS hem de hesap düzeyinde SAS ele alınacaktır. Ayrıca belirli bir IP adresi (veya IP adresi aralığı) erişimi sınırlamak nasıl ve HTTPS için kullanılan protokol sınırlamak nasıl bir paylaşılan erişim imzası sona tamamlanmasını beklemenize gerek kalmadan iptal etmek için göreceğiz.
* [Aktarım Sırasında Şifreleme](#encryption-in-transit)

Bu bölümde, bunu içine veya dışına bir depolama hesabı ile Data Lake depolama etkin Gen2 aktardığınızda verilerin güvenliğini sağlamak nasıl ele alınmaktadır. HTTPS önerilen kullanımı hakkında konuşacağız.

## <a name="management-plane-security"></a>Yönetim düzlemi güvenlik

Yönetim düzlemi depolama hesabını etkileyen işlemlerden oluşur. Örneğin, oluşturma veya bir depolama hesabı silebilir, bir Abonelikteki depolama hesaplarının bir listesini alma, depolama hesabı anahtarlarını almak veya depolama hesabı anahtarlarını yeniden oluştur.

Bu kılavuzda, depolama hesapları ile Data Lake depolama Gen2 özellikleri oluşturmaya yönelik araçlar dağıtımı Resource Manager modelini odaklanır. Resource Manager depolama hesapları yerine ile tüm abonelik erişimi veren, daha sınırlı düzeyinde rol tabanlı erişim denetimi (RBAC) kullanarak yönetim düzlemine erişimi denetleyebilirsiniz.

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama

RBAC nedir ve nasıl kullanabileceğiniz hakkında konuşalım. Her Azure aboneliği bir Azure Active Directory’ye sahiptir. Kullanıcılara, gruplara veya bu dizine uygulamalarından Azure aboneliğinde Resource Manager dağıtım modelini kullanan kaynakları yönetmek üzere erişim verilebilir. Bu tür bir güvenlik rolü tabanlı erişim denetimi (RBAC) denir. Bu erişimi yönetmek için **erişim denetimi (IAM)** Azure portalında.

Resource Manager modeli ile depolama hesabı Azure Active Directory'yi kullanarak bu belirli bir depolama hesabı için yönetim düzlemi bir kaynak grubu ve Denetim erişim yerleştirin. Örneğin, belirli kullanıcılar diğer kullanıcılar için depolama hesabıyla ilgili bilgileri görüntüleyebilirsiniz, ancak bu depolama hesabı anahtarlarını erişilemiyor, depolama hesabı anahtarları, erişim olanağı verebilirsiniz.

#### <a name="granting-access"></a>Erişim verme

Kullanıcıları, grupları ve uygulamaları, doğru kapsamda uygun RBAC rolü atanarak erişim verilir. Aboneliğin tümü erişim vermek için abonelik düzeyinde bir rol atayın. Kaynak grubunun kendisine izinleri vererek bir kaynak grubundaki tüm kaynakları için erişim verebilirsiniz. Depolama hesapları gibi belirli kaynaklar belirli roller atayabilirsiniz.

Bir Azure depolama hesabı yönetim işlemlerini erişmek için RBAC kullanma hakkında bilmeniz gereken temel noktalar şunlardır:

* Birinin veri nesneleri depolama hesabına erişim iznine sahip depolama hesabı anahtarlarını okuma izni verin ve o kullanıcı ardından dosyaların ve dizinlerin erişmek için bu tuşlarını kullanabilirsiniz.
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

Depolama hesabı anahtarı yerleştirmek için başka bir seçenek olan bir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) bir gizli dizi olarak sahip uygulamalarınızı almanıza ve anahtar buradan. Anahtarını yeniden oluşturun ve Azure anahtar Kasası'nı güncelleştirme zaman ardından uygulamalara bunlar yeni anahtarı Azure Key Vault'tan otomatik olarak seçer olduğundan dağıtılması gerekli değildir. İhtiyacınız olan her zaman anahtar okumasını olabilir veya bellekte önbelleğe ve kullanırken, başarısız olursa anahtarı Azure Key Vault'tan yeniden almak.

Azure Key Vault'u kullanarak başka bir depolama anahtarlarınızı için güvenlik düzeyini ekler. Bu yöntemi kullanırsanız, depolama anahtarı sabit kodlanmış kaldırır. Cadde birisi belirli bir izni olmadan anahtarlarına erişim sağlama, konusu No: bir yapılandırma dosyasında hiç gerekir.

Azure Key Vault'u kullanarak başka bir avantajı, erişimi Azure Active Directory'yi kullanarak anahtarlarınızı denetleyebilirsiniz olmasıdır. Başka bir deyişle, Azure Key Vault'tan anahtarları almak ve diğer uygulamalara erişim anahtarlarını izni özellikle vermeden için bunları mümkün olmayacaktır bilmeniz gereken uygulamalar dizi için erişim verebilirsiniz.

> [!NOTE]
> Microsoft, anahtarlar yalnızca biri tüm uygulamalar aynı anda kullanılmasını önerir. Bazı yerlerde anahtar 1 ve anahtar 2'de başkaları kullanıyorsanız anahtarlarınızın Döndür erişim hakkını kaybetmesini bazı uygulama mümkün olmayacaktır.

#### <a name="resources"></a>Kaynaklar

* [Azure portalında depolama hesabı ayarlarını yönetme](storage-account-manage.md)
* [Azure Depolama Kaynak Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Veri düzlemi güvenliği
Veri düzlemi güvenliği Azure Depolama'da depolanan veri nesnelerini güvenli hale getirmek için kullanılan yöntemler ifade eder. Güvenlik ve veri veri aktarım sırasında şifrelemek için yöntemleri gördük ancak nesnelere erişimi denetleme hakkında nasıl devam?

Azure depolama, veri nesnelerine erişimi yetkilendirmek için üç seçeneğiniz de dahil olmak üzere:

- Dosya sistemleri ve kuyruklara erişim yetkisi vermek için Azure AD kullanarak. Azure AD yetkilendirme, kodunuzda gizli dizileri depolamak için gereken kaldırmayı da için diğer yaklaşımlar avantaj sağlar. Daha fazla bilgi için [erişim için Azure depolama, Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md). 
- Paylaşılan anahtar aracılığıyla erişim yetkisi vermek için depolama hesabı anahtarlarını kullanma. Microsoft, Azure AD, bunun yerine, mümkün olduğunda kullanarak önerir. Bu nedenle, depolama hesabı anahtarları, uygulamanızda depolama paylaşılan anahtar ile yetkilendirme gerektirir.
- Belirli bir süre için belirli veri nesnelerini denetimli izinler vermek için paylaşılan erişim imzaları kullanma.

Yetkilendirme aracılığıyla erişim sınırlama yanı sıra, ayrıca kullanabileceğiniz [güvenlik duvarları ve sanal ağlar](storage-network-security.md) ağ kurallara göre depolama hesabı erişimini sınırlamak için.  Yalnızca belirli Azure sanal ağları ya da genel internet erişimi vermek için ve genel internet trafiği engellemek bu yaklaşım etkinleştirir erişim IP adresi aralıkları.

### <a name="storage-account-keys"></a>Depolama Hesabı Anahtarları

Depolama hesabı anahtarları, depolama hesabında depolanan verileri nesnelere erişmek için depolama hesabı adı ile birlikte kullanıldığında Azure tarafından oluşturulan 512 bit dizelerdir.

Örneğin, dosyaları okuyabilir. Bu eylemlerin çoğunu Azure portalı üzerinden gerçekleştirilebilir veya birçok depolama Gezgin uygulaması birini kullanarak. Ayrıca, bu işlemleri gerçekleştirmek için REST API kullanma için kod yazabilirsiniz.

Üzerinde bölümünde açıklandığı gibi [yönetim düzlemi güvenlik](#management-plane-security), erişim için depolama anahtarları için rol tabanlı erişim denetimi (RBAC) Azure Resource Manager modelini kullanarak bir depolama hesabı denetlenebilir.

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanarak hesabınızdaki nesnelere erişim devretmek nasıl

Paylaşılan erişim imzası, depolama nesnelerine erişim ve izinler ve erişim tarih/saat aralığı gibi kısıtlamaları belirtmek izin veren bir URI için bağlı bir güvenlik belirteci içeren bir dizedir.

Dosyaları veya dizinleri için erişim verebilirsiniz.

Bunlar Blob depolama alanındaki bir dosya sistemi resim yükleyebilirsiniz ve bu resimlerin okumak için bir web uygulaması izin vermek için bir SAS belirteci bir müşteri verebilirsiniz. Her iki durumda da bir görev ayrımı nettir vardır: her uygulamanın kendi görevi gerçekleştirmek için gereken erişim verilebilir. Bu paylaşılan erişim imzaları genelindeki mümkündür.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Paylaşılan erişim imzaları kullanmak istediğiniz neden

Neden çok daha kolaydır, depolama hesabı anahtarını vermek yerine SAS kullanmak istiyorsunuz? Depolama hesabı anahtarınızı anahtarları, depolama Krallık paylaşımı gibi sağlıyor. Tam erişim verir. Birisi anahtarlarınızı kullanın ve bunların tüm Müzik kitaplığı, depolama hesabınıza yükleyin. Bunlar da dosyalarınızın virüs bulaşmış sürümlerle değiştirin veya verilerinizi çalmak. Hemen sınırsız erişim, depolama hesabınıza hafifçe alınmamalıdır bir şey sağlıyor.

Paylaşılan erişim imzaları ile bir istemci, yalnızca sınırlı bir süre için gerekli izinleri verebilirsiniz. Örneğin, birisi bir dosyayı hesabınıza yükleniyor, bunları (dosya boyutuna Elbette bağlı olarak) dosyasını karşıya yüklemek yeterli zaman yazma erişim verebilirsiniz. Ve fikrinizi değiştirirseniz, bu erişimi iptal edebilirsiniz.

Ayrıca, SAS kullanılarak yapılan istekleri belirli bir IP adresi veya IP adresi aralığı Azure'a dış sınırlı olduğunu belirtebilirsiniz. Ayrıca, belirli bir Protokolü (HTTPS veya HTTP/HTTPS) kullanarak isteklerinin yapılma isteyebilirsiniz. Bu, yalnızca HTTPS trafiğine izin vermek istiyorsanız, yalnızca HTTPS için gerekli Protokolü ayarlayabilirsiniz ve HTTP trafiğini engellenir anlamına gelir.

#### <a name="definition-of-a-shared-access-signature"></a>Paylaşılan erişim imzası tanımı

Paylaşılan erişim imzası bir kaynağa işaret eden URL eklenecek sorgu parametreleri kümesidir.

izin verilen erişim ve kendisi için erişimine izin verilen süreyi hakkında bilgi sağlar. Bir örnek aşağıda verilmiştir; Bu URI, beş dakikalığına blob okuma erişimi sağlar. SAS sorgu parametreleri URL kodlu, iki nokta üst üste (:) için % 3A gibi olmalıdır veya bir alan için % 20.

```
http://mystorage.dfs.core.windows.net/myfilesystem/myfile.txt (URL to the file)
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

* Bir hizmet düzeyi SAS, depolama hesabındaki belirli kaynaklara erişmek için kullanılabilir. Bu bazı örnekler filesystem dosyaların bir listesini almak veya dosya indirme.
* Hesap düzeyi SAS, bir hizmet düzeyi SAS için kullanılabilecek herhangi bir şey erişmek için kullanılabilir. Ayrıca, bu dosya sistemleri oluşturma olanağı gibi bir hizmet düzeyi SAS ile izin verilmeyen kaynaklara seçenekleri verebilirsiniz.

#### <a name="creating-a-sas-uri"></a>SAS URI'si oluşturma

1. Tüm sorgu parametrelerinin her tanımlama, isteğe bağlı olarak bir URI oluşturabilirsiniz.

   Bu yaklaşım esnektir, ancak her zaman benzer parametreler mantıksal bir dizi varsa, bir depolanmış erişim ilkesini kullanarak daha iyi bir fikir olabilir.
2. Tüm dosya sistemi, dosya paylaşımı, tablo veya kuyruk için depolanan bir erişim ilkesi oluşturabilirsiniz. Ardından, bu temel olarak oluşturduğunuz SAS URI kullanabilirsiniz. İzinleri depolanmış erişim ilkelerine bağlı olarak kolayca iptal edilebilir. Her dosya, kuyruk, tablo veya dosya paylaşımı tanımlanan en fazla beş ilkelere sahip olabilir.

   Örneğin, belirli bir dosya sistemi BLOB'ları okuma çoğu kişi için oluşturacağınız, bir depolanmış erişim "okuma erişimi verin" ve her zaman aynı olacaktır herhangi bir ayarı bildiren ilkesi oluşturabilirsiniz. Ardından, sona erme tarihi/saati belirten ve depolanan erişim ilkesi ayarlarını kullanarak bir SAS URI'si oluşturabilirsiniz. Bu avantajı, her zaman tüm sorgu parametreleri belirtmeniz gerekmez ' dir.

#### <a name="revocation"></a>İptal etme

SAS tehlikede veya Kurumsal güvenlik veya yasal uyumluluk gereksinimleri nedeniyle değişiklik yapmak istediğiniz varsayalım. Bu SAS kullanarak bir kaynağa erişimi nasıl iptal? Bu, SAS URI'sini nasıl oluşturulacağını bağlıdır.

Geçici bir URI'leri kullanıyorsanız, üç seçeneğiniz vardır. Kısa bir süre sonu ilkeleri ile SAS belirteçlerini vermek ve SAS sona bekleyin. Yeniden adlandırma veya silme (belirteç tek bir nesne olarak kapsamlı varsayılarak) kaynak. Depolama hesabı anahtarlarını değiştirebilirsiniz. Bu son seçenek, kaç Hizmetleri, depolama hesabı kullanıyorsanız bağlı olarak önemli bir etkisi olabilir ve büyük olasılıkla bazı planlama olmadan yapmak istediğiniz bir şey değildir.

Depolanan bir erişim ilkesi tarafından türetilmiş bir SAS kullanıyorsanız, depolanmış erişim ilkesini iptal ederek erişimi kaldırabilirsiniz: yalnızca zaten doldu ya da tamamen kaldırın değiştirebilirsiniz. Bu durum, hemen geçerli olur ve bu depolanmış erişim ilkesini kullanılarak oluşturulan tüm SAS'ı geçersiz kılar. Güncelleştirme veya depolanmış erişim ilkesini kaldırma etkisi kişiler, belirli dosya sistemi, dosya paylaşımına erişen tablo veya eskisinin geçersiz hale geldiğinde yeni bir SAS istediklerinde bu nedenle istemcilerin yazılır, ancak sıra aracılığıyla SAS, bu düzgün çalışır.

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

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

SSE tüm depolama hesaplarında etkinleştirilir ve devre dışı bırakılamaz. SSE, Azure depolama alanına yazarken, verilerinizi otomatik olarak şifreler. Azure Depolama'dan veri okuma, döndürülmeden önce Azure Depolama tarafından şifresi çözülür. SSE, kodu değiştirin veya kod tüm uygulamalarınıza eklemek zorunda kalmadan verilerinizin güvenliğini sağlar.

Microsoft tarafından yönetilen anahtarlar ya da kendi özel anahtarları da kullanabilirsiniz. Microsoft yönetilen anahtarlarını oluşturur ve iç Microsoft İlkesi tarafından tanımlandığı gibi normal döndürme yanı sıra kendi güvenli depolama işler. Özel anahtarları kullanma hakkında daha fazla bilgi için bkz. [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).
