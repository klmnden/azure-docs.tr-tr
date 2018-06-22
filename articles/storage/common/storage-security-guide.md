---
title: Azure depolama Güvenlik Kılavuzu | Microsoft Docs
description: Azure Storage, ancak bunlarla sınırlı olmamak RBAC, depolama hizmeti şifrelemesi, istemci tarafı şifreleme, SMB 3.0 ve Azure Disk şifrelemesi dahil olmak üzere güvenlik altına almanın birçok yöntem ayrıntıları verilmektedir.
services: storage
author: craigshoemaker
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/31/2018
ms.author: cshoe
ms.openlocfilehash: ba008a86f76a526967bb9dab6ba37043a85f5cf3
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304531"
---
# <a name="azure-storage-security-guide"></a>Azure depolama Güvenlik Kılavuzu

Azure depolama kapsamlı bir araya geliştiricilerin güvenli uygulamalar oluşturmasını sağlama güvenlik özellikleri sağlar:

- Tüm verileri Azure depolama alanına yazılır, kullanarak otomatik olarak şifrelenir [depolama hizmeti şifreleme (SSE)](storage-service-encryption.md). Daha fazla bilgi için bkz: [Azure BLOB'ları, dosyalar, tablo ve kuyruk depolama varsayılan şifreleme Duyurusu](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).
- Azure Active Directory (Azure AD) ve rol tabanlı erişim denetimi (RBAC) desteklenen Azure depolama için kaynak yönetimi işlemleri ve veri işlemleri için aşağıdaki gibi:   
    - Güvenlik ilkeleri ve kullanım Azure AD anahtar yönetimi gibi kaynak yönetimi işlemleri yetkilendirmek için depolama hesabına kapsamına RBAC roller atayabilirsiniz.
    - Azure AD tümleştirme Blob ve kuyruk Hizmetleri veri işlemleri için Önizleme'de desteklenir. Bir abonelik, kaynak grubu, depolama hesabı ya da bir bireysel kapsayıcı veya bir güvenlik sorumlusu ya da yönetilen hizmet kimliği kuyruğuna kapsamına RBAC roller atayabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md).   
- Veri güvenli bir uygulama ile Azure arasında aktarımda kullanarak [istemci tarafı şifreleme](../storage-client-side-encryption.md), HTTPS veya SMB 3.0.  
- Azure sanal makineler tarafından kullanılan işletim sistemi ve veri diskleri kullanılarak şifrelenir [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md). 
- Azure storage'da veri nesneleri yetkilendirilmiş erişim olanağı verilir kullanarak [paylaşılan erişim imzaları](../storage-dotnet-shared-access-signature-part-1.md).

Bu makalede Azure Storage ile kullanılabilmesi için bu güvenlik özelliklerin her biri bir bakış sağlar. Bağlantılardır kolayca yapabilirsiniz her bir özelliğin ayrıntılarını bunu verecektir makaleler için sağlanan daha fazla araştırma her konuda.

Bu makalede ele alınacak konular şunlardır:

* [Yönetim düzlemi güvenlik](#management-plane-security) – depolama hesabınızın güvenliğini sağlama

  Yönetim düzeyi, depolama hesabınızı yönetmek için kullanılan kaynakları oluşur. Bu bölüm, Azure Resource Manager dağıtım modeli ve depolama hesaplarınızı erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanmayı kapsar. Ayrıca, depolama hesabı anahtarlarını ve bunları yeniden nasıl yönetme giderir.
* [Veri düzlemine güvenlik](#data-plane-security) – verilerinize erişimin güvenliğini sağlama

  Bu bölümde, biz gerçek veri nesnelerine erişimi depolama hesabınızdaki BLOB'lar, dosyalar, kuyruklar ve tablolar gibi izin vermeyi paylaşılan erişim imzaları ve depolanan erişim ilkeleri kullanarak göreceğiz. Hizmet düzeyi SAS ve hesap düzeyinde SAS ele alınacaktır. Ayrıca belirli bir IP adresi (veya IP adresi aralığı) erişimi sınırlamak nasıl, HTTPS için kullanılan protokol sınırlamak nasıl ve paylaşılan erişim imzası sona tamamlanmasını beklemeden iptal etme göreceğiz.
* [Aktarım Sırasında Şifreleme](#encryption-in-transit)

  Bu bölümde içine veya dışına Azure Storage aktardığınızda verilerin güvenliğini sağlamak nasıl açıklanmaktadır. HTTPS ve Azure dosya paylaşımları için SMB 3.0 tarafından kullanılan şifreleme önerilen kullanımıyla ilgili konuşun. Biz de ve bu sayede istemci uygulamasında depolama alanına aktarılır önce verilerin şifrelenmesi ve depolama alanı biterse aktarıldıktan sonra verilerin şifresini çözmek için istemci tarafı şifreleme göz atın.
* [Bekleme Sırasında Şifreleme](#encryption-at-rest)

  Biz hakkında depolama hizmeti şifreleme (artık otomatik olarak yeni ve var olan depolama hesapları için etkinleştirilmiş olan SSE), konuşur. Ayrıca Azure Disk şifrelemesi kullanın ve temel farklar ve istemci tarafı şifreleme karşı SSE karşı Disk şifrelemesi örneklerini keşfedin nasıl ele alacağız. Kısaca ABD için FIPS uyumluluk ele alacağız Kamu bilgisayarlar.
* Kullanarak [depolama çözümlemeleri](#storage-analytics) Azure Storage erişimi denetlemek için

  Bu bölümde, bir istek için depolama analytics günlüklerde bilgiye anlatılmaktadır. Biz, günlük verilerinin gerçek depolama çözümlemeleri göz atın ve depolama hesabınızın anahtarıyla bir paylaşılan erişim imzası içeren bir istek yapıldığında veya anonim olarak ve olup başarılı veya başarısız olup olmadığını keşfedilir konusuna bakın.
* [Tarayıcı tabanlı istemcileri CORS kullanarak etkinleştirme](#Cross-Origin-Resource-Sharing-CORS)

  Bu bölümde çıkış noktaları arası kaynak paylaşımı (CORS) izin hakkında alınmaktadır. Etki alanları arası erişimi ve Azure depolama alanına yerleşik CORS özelliklerini işlemeye nasıl hakkında konuşun.

## <a name="management-plane-security"></a>Yönetim düzlemi güvenliği
Yönetim düzeyi, depolama hesabı etkileyen işlemden oluşur. Örneğin, oluşturmak veya bir depolama hesabı silebilir, depolama hesaplarının bir listesiyle bir abonelik almak, depolama hesabı anahtarlarını almak veya depolama hesabı anahtarlarını yeniden.

Yeni bir depolama hesabı oluşturduğunuzda, Klasik veya Resource Manager dağıtım modelini seçin. Azure kaynakları oluşturma Klasik modeli yalnızca abonelik ve ardından, depolama hesabı ya hep ya hiç erişim sağlar.

Bu kılavuz, depolama hesapları oluşturmak için önerilen yöntemdir Resource Manager modeli odaklanır. Resource Manager depolama hesaplarıyla yerine ile tüm abonelik erişimi vermiş, rol tabanlı erişim denetimi (RBAC) kullanarak yönetim düzlemi daha sınırlı bir düzeye erişimi denetleyebilirsiniz.

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama
Şimdi RBAC nedir ve nasıl kullanabileceğiniz hakkında konuşun. Her Azure aboneliği bir Azure Active Directory’ye sahiptir. Kullanıcılar, gruplar ve uygulamalar bu dizinden Resource Manager dağıtım modeli kullanan Azure aboneliği kaynakları yönetmek için erişim hakkı verilebilir. Bu tür güvenlik rol tabanlı erişim denetimi (RBAC) denir. Bu erişimi yönetmek için kullanabileceğiniz [Azure portal](https://portal.azure.com/), [Azure CLI araçlarını](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), veya [Azure depolama kaynak sağlayıcısı REST API'lerini](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Resource Manager modeli ile depolama hesabı Azure Active Directory'yi kullanarak, belirli bir depolama hesabı Yönetim düzeyi için bir kaynak grubu ve Denetim erişim yerleştirin. Örneğin, belirli kullanıcılar diğer kullanıcıların depolama hesabıyla ilgili bilgileri görüntüleyebilirsiniz, ancak depolama hesabı anahtarlarını erişemiyor depolama hesabı anahtarlarını erişim olanağı verebilirsiniz.

#### <a name="granting-access"></a>Erişim izni verme
Kullanıcılar, gruplar ve doğru kapsamda uygulamalara uygun RBAC rolü atayarak erişimi verilir. Tüm abonelik erişim vermek için abonelik düzeyinde bir rol atayın. Kaynak grubunun kendisine izin vererek tüm kaynak grubundaki kaynaklar için erişim izni verebilir. Depolama hesapları gibi belirli kaynaklar belirli roller atayabilirsiniz.

Bir Azure Storage hesabı yönetim işlemlerini erişmek için RBAC kullanarak hakkında bilmeniz gereken temel noktalar şunlardır:

* Erişim atadığınızda, erişimi istediğiniz hesap için temel olarak bir rol atayın. Bu depolama hesabı yönetmek için kullanılan işlemleri ancak hesaptaki veri nesneleri için erişimi denetleyebilirsiniz. Örneğin, (örneğin, artıklık), depolama hesabının özelliklerini alma izni verebilirsiniz, ancak bir kapsayıcı veya Blob Storage içindeki bir kapsayıcı içindeki veri.
* Veri nesneleri depolama hesabındaki erişim iznine sahip birine için depolama hesabı anahtarlarını okuma izni verin ve o kullanıcı ardından BLOB, kuyruklar, tablolar ve dosyaları erişmek için bu tuşlarını kullanabilirsiniz.
* Roller, belirli bir kullanıcı hesabı, bir kullanıcı grubuna veya belirli bir uygulamaya atanabilir.
* Her rol Eylemler ve değil eylemlerinin bir listesi vardır. Örneğin, "listKeys" eylemini sanal makine Katılımcısı rolüne sahip okumak depolama hesabı anahtarları sağlar. Katkıda bulunan kullanıcılar Active Directory erişimi güncelleştirme gibi "Değil eylem" yok.
* Depolama için rolleri dahil (ancak bunlarla sınırlı değildir) aşağıdaki rolleri:

  * Sahibi – bunlar erişim dahil her şeyi yönetebilir.
  * Katkıda bulunan – yapabilecekleri şeylerin herhangi bir şey sahibi erişimi atayın dışındaki. Bu role sahip biri görüntülemek ve depolama hesabı anahtarlarını yeniden kullanabilirsiniz. Depolama hesabı anahtarları ile bunların veri nesneleri erişebilir.
  * Okuyucu – gizli dışında depolama hesabıyla ilgili bilgileri görüntüleyebilirler. Örneğin, depolama hesabı okuyucu izinlerine sahip bir rol birine atarsanız, depolama hesabının özelliklerini görüntüleyebilirsiniz, ancak herhangi bir değişiklik özelliklerine veya depolama hesabı anahtarlarını görüntülemek.
  * Depolama hesabı katkıda bulunan – bunlar depolama hesabını yönetebilir – abonelik kaynak grupları ve kaynakları okuyabilir oluşturmak ve abonelik kaynak grubu dağıtımlarını yönetin. Bunlar, sırayla veri düzlemi erişebilecekleri anlamına gelir depolama hesabı anahtarlarını da erişebilirsiniz.
  * Kullanıcı erişimi Yöneticisi – bunlar depolama hesabı için kullanıcı erişimini yönetebilirsiniz. Örneğin, bunlar belirli bir kullanıcıya okuyucu erişim verebilirsiniz.
  * Sanal makine Katılımcısı – bunlar sanal makineler ancak bağlı depolama hesabı değil yönetebilirsiniz. Bu rol bu rolü atadığınız kullanıcı veri düzlemi güncelleştirebilirsiniz başka bir deyişle, depolama hesabı anahtarlarını listeleyebilirsiniz.

    Bir kullanıcının bir sanal makine oluşturmak, bunlar bir depolama hesabında karşılık gelen VHD dosyası oluşturmak olması. Bunu yapmak için bunlar depolama hesabı anahtarı almak ve VM oluşturma API geçmesi gerekir. Bu nedenle, depolama hesabı anahtarlarını listeleyebilirsiniz şekilde bu izni olmalıdır.
* Özel roller tanımlama yeteneği Azure kaynakları gerçekleştirilebilir kullanılabilir eylemler listesinden Eylemler kümesi oluşturmak izin veren bir özelliktir.
* Kullanıcının bir rol için bunları atamadan önce Azure Active Directory'yi ayarlanması gerekir.
* Kimin verilen/grafikten kim ve PowerShell veya Azure CLI kullanarak hangi kapsamında erişim türüne iptal bir rapor oluşturabilirsiniz.

#### <a name="resources"></a>Kaynaklar
* [Azure Active Directory Rol Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md)

  Bu makalede Azure Active Directory Rol Tabanlı Access Control ve nasıl çalıştığı açıklanmaktadır.
* [RBAC: Yerleşik Roller](../../role-based-access-control/built-in-roles.md)

  Bu makalede tüm RBAC'de kullanılabilen yerleşik rollerin ayrıntılarını verir.
* [Resource Manager dağıtımını ve klasik dağıtımı anlama](../../azure-resource-manager/resource-manager-deployment-model.md)

  Bu makalede, Resource Manager dağıtımını ve klasik dağıtım modellerine açıklar ve Resource Manager ve kaynak grupları kullanmanın avantajları açıklanmıştır. Bu, Azure işlem, ağ ve depolama sağlayıcıları Resource Manager modeli altında nasıl çalıştığı açıklanmıştır.
* [Rol Tabanlı Erişim Denetimini REST API’si ile Yönetme](../../role-based-access-control/role-assignments-rest.md)

  Bu makalede RBAC yönetimi için REST API’sinin nasıl kullanılacağı gösterilmektedir.
* [Azure depolama kaynak sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Bu API Başvurusu depolama hesabınız programlı olarak yönetmek için kullanabileceğiniz API'ları açıklar.
* [Erişim abonelikler için kaynak yöneticisi kimlik doğrulaması API'sini kullanın](../../azure-resource-manager/resource-manager-api-authentication.md)

  Bu makalede Resource Manager API'leri kullanılarak kimlik doğrulaması yapmayı gösterir.
* [Ignite’tan Microsoft Azure için Rol Tabanlı Erişim Denetimi](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Bu bağlantı 2015 MS Ignite konferansının 9. Kanalındaki videoya aittir. Bu oturumda, Azure’daki erişim yönetimi ve raporlama özellikleri konuşulmakta ve Azure Active Directory'yi kullanarak Azure aboneliklerine erişimin güvenliğini sağlama konusundaki en iyi uygulamalar keşfedilmektedir.

### <a name="managing-your-storage-account-keys"></a>Depolama hesabı anahtarlarını yönetme
Depolama hesabı anahtarları, depolama hesabı adı ile birlikte depolama hesabında örneğin depolanan veri nesneleri erişmek için kullanılan, BLOB, tablo, kuyruk iletileri ve bir Azure dosya paylaşımında varlıkları Azure tarafından oluşturulan 512 bit dizelerdir. Depolama hesabı anahtarları denetimleri erişimi veri düzlemi bu depolama hesabı için erişimi denetleme.

Her Depolama hesabı "Anahtar 1" ve "2 anahtar" başvurulan iki anahtarlara sahip [Azure portal](http://portal.azure.com/) ve PowerShell cmdlet'leri. Bunlar, ancak bunlarla sınırlı olmamak kullanarak da dahil olmak üzere birkaç yöntemden birini kullanarak el ile yeniden üretilebilir [Azure portal](https://portal.azure.com/), PowerShell, Azure CLI veya .NET depolama istemci kitaplığı veya Azure Storage Hizmetleri program aracılığıyla kullanarak REST API.

Herhangi bir sayı, depolama hesabı anahtarlarını yeniden nedenleri vardır.

* Bunları düzenli güvenlik nedenleriyle yeniden.
* Birisi bir uygulamaya korsan saldırılarına ve sabit kodlanmış veya bir yapılandırma dosyasında kaydedilen anahtarı almak depolama hesabınıza tam erişim onların yönetiliyorsa, depolama hesabı anahtarlarını yeniden.
* Ekibinizin depolama hesabı anahtarı koruyan bir Depolama Gezgini uygulama kullanıyor ve ekip üyelerinin birini bırakır anahtarını yeniden üretme işlemi için başka bir durumdur. Uygulama çalışmak kaldırılmıştır sonra bunları erişim depolama hesabınıza verip devam eder. Bu hesap düzeyinde paylaşılan erişim imzaları oluşturulan gerçekte birincil nedeni – yapılandırma dosyasındaki erişim anahtarlarını depolamak yerine bir hesap düzeyinde SAS kullanabilirsiniz.

#### <a name="key-regeneration-plan"></a>Anahtarını yeniden üretme planı
Yalnızca bazı planlama olmadan kullanıyorsanız anahtarını yeniden oluşturmak istemiyorsanız. Bunu yaparsanız önemli kesintiye neden olabilir, depolama hesabı için tüm erişimi devre dışı Kes. İki anahtar vardır nedeni budur. Aynı anda bir anahtar anahtarını yeniden oluşturmalısınız.

Anahtarlarınızı yeniden önce tüm depolama hesabında bağımlı olan uygulamalar, aynı zamanda Azure'da kullandığınız hizmetlerin listesini sahip olduğunuzdan emin olun. Depolama hesabınıza bağlı olan bir Azure Media Services kullanıyorsanız, anahtarı yeniden oluşturduktan sonra gibi erişim tuşlarını medya hizmetlerinizle yeniden eşitleme gerekir. Bir Depolama Gezgini gibi herhangi bir uygulama kullanıyorsanız, bu uygulamalara da yeni anahtarlar sağlamanız gerekir. VHD dosyaları depolama hesabında depolanır Vm'leriniz varsa, bunlar depolama hesabı anahtarlarını yeniden oluşturma tarafından etkilenmez.

Azure portalında anahtarlarınızı yeniden oluşturabilirsiniz. Anahtarları yeniden sonra bunlar depolama hizmetleri arasında eşitlenmesi 10 dakika kadar sürebilir.

Hazır olduğunuzda, anahtarınızı nasıl değiştiğini ayrıntılı genel süreç şöyledir. Bu durumda, anahtar 1 kullanmakta olduğunuz ve anahtar 2 kullanmayı her şeyi değiştirmek zorunda kalacaklarını varsayılır.

1. Anahtar güvenliğini sağlamaya yönelik 2 yeniden oluşturun. Azure portalında bunu yapabilirsiniz.
2. Tüm depolama anahtarı depolandığı uygulamalar, depolama anahtarı anahtar 2'in yeni değeri değiştirin. Test edin ve uygulamayı yayımlayın.
3. Tüm uygulamalar ve hizmetler hazır olduğunuzda, başarılı bir şekilde çalışan, yeniden anahtar 1. Bu herkes Kime açıkça yeni anahtarı verdiğiniz değil artık depolama hesabı erişmesini sağlar.

Anahtar 2 kullanıyorsanız, aynı işlemi kullanır, ancak anahtar adları ters çevir.

Yeni anahtarı kullanmak üzere her bir uygulama değiştirme ve yayımlamadan gün, birkaç geçirebilirsiniz. Hepsini tamamladıktan sonra daha sonra geri dönün ve böylece artık çalışır eski anahtarını yeniden gerekir.

Depolama hesabı anahtarı yerleştirmek için başka bir seçenektir bir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) gizli olarak sahip uygulamalarınızı almanıza ve anahtarı buradan. Anahtarı yeniden oluşturmak ve Azure anahtar kasası güncelleştirin, sonra uygulamaları bunlar yeni anahtarı Azure anahtar Kasası'nı otomatik olarak seçer çünkü dağıtılması gerekmez. Anahtar ihtiyacınız okuyamayacağı uygulamanın olabilir veya bellekte önbelleğe ve, kullanırken başarısız olursa anahtarı yeniden Azure anahtar Kasası'alıp unutmayın.

Azure anahtar kasası kullanarak başka bir depolama anahtarları için güvenlik düzeyi ekler. Bu yöntemi kullanırsanız, birisi belirli izniniz olmadan anahtarlarına erişim sağlama, o avenue kaldırır bir yapılandırma dosyasında depolama anahtar sabit kodlanmış hiçbir zaman gerekir.

Azure anahtar kasası kullanarak başka bir avantajı, erişimi Azure Active Directory'yi kullanarak anahtarlarınızı denetleyebilirsiniz olmasıdır. Başka bir deyişle, anahtarları Azure anahtar Kasası'almak ve diğer uygulamalar izni özellikle vermeden için bunları erişim anahtarları mümkün olmaz bilmeniz gereken uygulamalar sayıda erişim izni verebilir.

Not: yalnızca anahtarların tüm uygulamalar aynı anda kullanmak için önerilir. Bazı yerlerde anahtar 1 ve diğer anahtar 2 kullanıyorsanız, erişimi kaybetmenize bazı uygulama anahtarlarınızı döndürme mümkün olmaz.

#### <a name="resources"></a>Kaynaklar
* [Azure Storage hesapları hakkında](storage-create-storage-account.md#regenerate-storage-access-keys)

  Bu makalede, depolama hesapları genel bir bakış sağlar ve görüntüleme, kopyalama ve depolama erişim anahtarlarını yeniden anlatılmaktadır.
* [Azure depolama kaynak sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/mt163683.aspx)

  Bu makale, REST API kullanarak bir Azure hesabı için depolama hesabı anahtarlarını yeniden oluşturma ve depolama hesabı anahtarlarını alma hakkında belirli makalelerinin bağlantıları içerir. Not: Resource Manager depolama hesapları için budur.
* [Depolama hesapları işlemleri](https://msdn.microsoft.com/library/ee460790.aspx)

  Bu makalede depolama Service Manager REST API Başvurusu alma ve REST API kullanarak depolama hesabı anahtarlarını yeniden oluşturma belirli makalelerinin bağlantıları içerir. Not: Klasik depolama hesapları için budur.
* [Yönetim anahtarı – Azure AD kullanarak Azure Storage veri erişimi yönetmek için veda](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

  Bu makalede, Azure Storage anahtarları Azure anahtar Kasası'nda erişimi denetlemek için Active Directory kullanmayı gösterir. Ayrıca, bir Azure Otomasyonu işini anahtarları saatlik düzenli olarak yeniden oluşturmak için nasıl kullanılacağını gösterir.

## <a name="data-plane-security"></a>Veri düzlemi güvenliği
Veri düzlemi güvenliği Azure Storage – BLOB, kuyruklar, tablolar ve dosyaları depolanan veri nesneleri güvenli hale getirmek için kullanılan yöntemleri gösterir. Veri ve güvenlik veriler aktarım sırasında şifrelemek için yöntemleri gördük ancak nesnelere erişimi denetleme hakkında olduğunu nasıl gittiğiniz?

Azure storage'da veri nesnelere erişimi yetkilendirmek için üç seçeneğiniz de dahil olmak üzere:

- Kapsayıcılar ve Kuyruklar (Önizleme) erişim yetkisi vermek için Azure AD kullanma. Azure AD yetkilendirme, kodunuzda parolaları depolamak için gereken kaldırma dahil olmak üzere diğer yaklaşımları avantaj sağlar. Daha fazla bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md). 
- Paylaşılan anahtar aracılığıyla erişim yetkisi vermek için depolama hesabı anahtarlarını kullanıyor. Paylaşılan anahtar yetkilendirme Microsoft Azure AD, bunun yerine, mümkün olduğunda kullanılmasını önerir şekilde depolama hesabı anahtarları, uygulamanızda saklanması gerekir. Üretim uygulamaları için ya da Azure tabloları ve dosyalarına erişimi yetkilendirmek için Azure AD tümleştirme önizlemesinde paylaşılan anahtar kullanarak devam edin.
- Belirli bir süre için belirli veri nesneleri denetimli izinleri için paylaşılan erişim imzaları kullanma.

Ayrıca, Blob Depolama için genel erişim bloblarınızın için uygun şekilde BLOB tutan kapsayıcı için erişim düzeyi ayarlayarak izin verebilirsiniz. Blob veya kapsayıcı için bir kapsayıcı erişim ayarlarsanız bu kapsayıcıda BLOB'lar için herkese okuma erişimi sağlar. Bu, kapsayıcı bir blob'a işaret eden bir URL kimseyle bir tarayıcıda paylaşılan erişim imzası kullanarak veya depolama hesabı anahtarlarını sahip açmadan anlamına gelir.

Yetkilendirme aracılığıyla erişimi sınırlayan ek olarak da kullanabilirsiniz [güvenlik duvarları ve sanal ağlar](storage-network-security.md) ağ kurallara göre depolama hesabına erişimi sınırlamak için.  Ortak Internet trafiği ve vermek için erişimi engellemek bu yaklaşım etkinleştirir yalnızca belirli Azure sanal ağları veya genel internet erişimi IP adresi aralıkları.

### <a name="storage-account-keys"></a>Depolama Hesabı Anahtarları
Depolama hesabı anahtarları, depolama hesabı adı ile birlikte depolama hesabında depolanan verileri nesnelere erişmek için kullanılan Azure tarafından oluşturulan 512 bit dizelerdir.

Örneğin, BLOB'lar okuyun, sıralara yazma, tablolar oluşturmak ve dosyalarda değişiklik. Bu eylemler çoğunu Azure Portalı aracılığıyla gerçekleştirilebilir veya çok sayıda Depolama Gezgini uygulamalardan birini kullanma. Ayrıca bu işlemleri gerçekleştirmek için REST API veya depolama istemci kitaplıklarından birini kullanmak üzere kod yazabilirsiniz.

Üzerinde bölümünde açıklandığı gibi [yönetim düzlemi güvenlik](#management-plane-security), Klasik depolama hesabı için Azure aboneliği tam erişim vererek verilebilir için depolama anahtarlarına erişim. Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager modelini kullanarak bir depolama hesabı için depolama anahtarlarına erişimi denetlenebilir.

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Paylaşılan erişim imzaları ve depolanan erişim ilkeleri kullanarak hesabınızda nesnelere erişimi nasıl
Paylaşılan erişim imzası temsilci depolama nesnelere erişimi ve izinler ve erişim tarih aralığını gibi kısıtlamaları belirtmek izin veren bir URI iliştirilmiş bir güvenlik belirteci içeren bir dizedir.

BLOB'ları, kapsayıcıları, iletileri kuyruğa, dosyaları ve tabloları erişim izni verebilir. Tablolarla, aslında kullanıcının erişmesini istediğiniz bölüm ve satır anahtarı aralıkları belirterek varlıklar tablosundaki bir dizi erişim izni verebilirsiniz. Coğrafi durumu ile bir bölüm anahtarı depolanan verileri varsa, örneğin, birisi size California yalnızca verileri için erişim.

Başka bir örnekte, bir web uygulaması, onu bir sıraya girişler yazmak sağlayan bir SAS belirteci vermek ve bir çalışan rolü uygulama kuyruktan iletileri almak ve bunları işlemek için bir SAS belirteci vermek. Ya da bunlar bir kapsayıcıda Blob Storage resimleri yüklemek için kullanın ve bu resimlerinizi okumak için bir web uygulaması izin vermek için bir SAS belirteci bir müşteri sunabilir. Her iki durumda da sorunları ayrılması yoktur – her uygulamanın kendi görev gerçekleştirmek için ihtiyaç erişim verilebilir. Bu paylaşılan erişim imzaları kullanımı ile mümkün olur.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Paylaşılan erişim imzaları kullanmak istediğiniz neden
Neden çok daha kolaydır, depolama hesabı anahtarınızı vermek yerine bir SAS kullanmak istiyor? Depolama hesabı anahtarınızı vermiş, depolama Krallık anahtarları gibi paylaşımıdır. Tam erişim verir. Birisi anahtarlarınızı kullanın ve bunların tüm Müzik Kitaplığı depolama hesabınıza yükleyin. Bunlar de virüs bulaşmış sürümleri ile dosyalarınızı değiştirmek veya verilerinizi çalabilir. Uzakta depolama hesabınıza sınırsız erişimi vermiş, hafifçe alınmamalıdır şeydir.

Paylaşılan erişim imzaları ile bir istemci yalnızca sınırlı bir süre için gerekli izinleri verebilirsiniz. Örneğin, birisi bir blob hesabınıza yükleniyor, bunları yazma erişimine (blob boyutuna Elbette bağlı olarak) blob karşıya yüklemek yeterli süre için verebilirsiniz. Ve fikrinizi değiştirirseniz, o erişimi iptal edebilirsiniz.

Ayrıca, SAS kullanılarak yapılan istekleri belirli bir IP adresi veya IP adresi aralığı için Azure dış sınırlı olduğunu belirtebilirsiniz. Ayrıca, belirli bir Protokolü (HTTPS veya HTTP/HTTPS) kullanarak isteklerinin yapılma isteyebilirsiniz. Bu, yalnızca HTTPS trafiğine izin vermek istiyorsanız, yalnızca HTTPS için gerekli Protokolü ayarlayabilirsiniz ve HTTP trafiği engellenir anlamına gelir.

#### <a name="definition-of-a-shared-access-signature"></a>Paylaşılan erişim imzası tanımı
Paylaşılan erişim imzası bir kaynağa işaret eden URL eklenen sorgu parametreleri kümesidir.

erişimine izin verilir ve erişimine izin verilen süreyi hakkında bilgi sağlar. Örnek aşağıda verilmiştir; Bu URI beş dakika boyunca bir blob okuma erişimi sağlar. SAS parametreleri sorgu Not URL kodlanmış, iki nokta üst üste (:) veya bir alan için % %20 3A gibi olması gerekir.

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

#### <a name="how-the-shared-access-signature-is-authorized-by-the-azure-storage-service"></a>Paylaşılan erişim imzası Azure depolama hizmeti tarafından nasıl yetkilendirilir
Depolama hizmet isteği aldığında, giriş sorgu parametrelerini alır ve çağıran program yöntemin aynısı kullanılarak bir imza oluşturur. Ardından, iki imzaları karşılaştırır. Kullanıcının kabul etmesi durumunda, depolama birimi hizmeti geçerli olduğundan emin olun, geçerli tarih ve saat içinde belirtilen pencere olduğunu doğrulayın, istenen erişim karşılık gelen yapılan istek, vb. emin olmak için depolama hizmet sürümü kontrol edebilirsiniz.

URL yerine bir blobu bir dosyaya işaret eden paylaşılan erişim imzası blob için olduğunu belirtir örneğin, yukarıdaki bizim URL ile bu isteği başarısız. Çağrılan REST komutunu blob güncelleştirmek için paylaşılan erişim imzası yalnızca okuma erişimi verilip belirttiğinden başarısız olur.

#### <a name="types-of-shared-access-signatures"></a>Paylaşılan erişim imzaları türleri
* Bir hizmet düzeyi SAS depolama hesabının belirli kaynaklara erişim için kullanılabilir. Bu, bazı örnekler BLOB'ları bir kapsayıcıda blob yükleme, bir tablodaki bir varlık güncelleştirme, bir kuyruk iletileri ekleme veya bir dosya paylaşımı için bir dosyayı karşıya yüklemeyi listesini alıyor.
* Bir hesap düzeyinde SAS bir hizmet düzeyi SAS kullanılabilir herhangi bir şey erişmek için kullanılabilir. Ayrıca, bu seçenekleri kapsayıcılar, tablolar, kuyruklar ve dosya paylaşımları oluşturma yeteneği gibi bir hizmet düzeyi SAS ile izin verilmiyor kaynaklara verebilirsiniz. Aynı anda birden çok hizmetlerine erişim de belirtebilirsiniz. Örneğin, birisi verebilir erişim hem BLOB, hem de depolama hesabındaki dosyaları.

#### <a name="creating-a-sas-uri"></a>SAS URI'si oluşturma
1. Tüm sorgu parametrelerinin her tanımlama, isteğe bağlı bir URI oluşturabilirsiniz.

   Bu yaklaşım esnek, ancak bir mantıksal parametrelerinin her zaman benzer varsa, depolanan bir Erişim İlkesi'ni kullanarak daha iyi bir fikir.
2. Bir kapsayıcının tamamı, dosya paylaşımı, tablo veya kuyruğu için depolanan bir erişim ilkesi oluşturabilirsiniz. Ardından, bu temel olarak SAS oluşturduğunuz URI'ler için kullanabilirsiniz. Depolanan erişim ilkelerine bağlı olarak izinleri kolayca iptal edilebilir. Her kapsayıcısı, kuyruk, tablo veya dosya paylaşımı üzerinde tanımlanan en fazla beş ilkeleri olabilir.

   Örneğin, belirli bir kapsayıcıda BLOB'ları okuma birçok kişi sağlamak için giderek, bir depolanan erişim "okuma erişimi verin" ve her zaman aynı olacaktır herhangi bir ayarı bildiren ilkesi oluşturabilirsiniz. Daha sonra bir SAS depolanan erişim ilkesi ayarları kullanılarak ve süre sonu tarihi/saati belirterek URI oluşturabilirsiniz. Bunun avantajı, tüm sorgu parametrelerinin her zaman belirtmeniz gerekmez ' dir.

#### <a name="revocation"></a>İptal etme
Kurumsal güvenlik veya yasal uyumluluk gereksinimleri nedeniyle değiştirmek istediğiniz veya, SAS tehlikeye varsayalım. Bu SAS kullanarak bir kaynağa erişimi nasıl iptal edilsin mi? Bu, SAS URI'sini nasıl oluşturulacağını bağlıdır.

Geçici URI'ler kullanıyorsanız, üç seçeneğiniz vardır. Kısa süre sonu ilkeleriyle SAS belirteçleri ve SAS süresi dolacak şekilde bekleyin. Yeniden adlandırma veya (belirteç tek bir nesneye kapsamlı varsayılarak) kaynak silin. Depolama hesabı anahtarlarını değiştirebilirsiniz. Bu son seçeneği kaç Hizmetleri bu depolama hesabı kullanıyorsanız bağlı olarak önemli bir etkisi olabilir ve büyük olasılıkla bazı planlama olmadan yapmak istediğiniz bir şey değil.

Depolanan bir erişim ilkesi tarafından türetilmiş bir SAS kullanıyorsanız, depolanan erişim ilkesi iptal ederek erişimi kaldırabilirsiniz – yalnızca süresi dolmuş veya tamamen kaldırmak için değiştirebilirsiniz. Bu hemen etkili olur ve bu depolanmış erişim ilkesi kullanılarak oluşturulan her SAS geçersiz kılar. Güncelleştirme veya depolanan erişim ilkesi kaldırma etkisi kişiler, belirli bir kapsayıcıya, dosya paylaşımı erişimi tablo veya eskisinin geçersiz hale geldiğinde yeni bir SAS istediklerinde böylece istemciler yazılır, ancak sıra SAS, aracılığıyla bu düzgün çalışır.

Depolanan bir erişim ilkesi tarafından türetilmiş SAS kullanarak bu SAS hemen iptal etme olanağını verdiğinden, bu her zaman mümkün olduğunda depolanan erişim ilkelerini kullanmak için önerilen en iyi uygulamadır.

#### <a name="resources"></a>Kaynaklar
Paylaşılan erişim imzaları ve depolanan erişim ilkeleri, örnekler, tam kullanma hakkında daha ayrıntılı bilgi için aşağıdaki makalelere bakın:

* Başvuru makaleleri bunlar.

  * [Hizmet SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    Bu makale bir hizmet düzeyi SAS BLOB'lar, iletileri kuyruğa, tablo aralıkları ve dosyaları kullanma örnekleri sağlar.
  * [Hizmet SAS oluşturma](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Hesap SAS oluşturma](https://msdn.microsoft.com/library/mt584140.aspx)
* Paylaşılan erişim imzaları ve depolanan erişim ilkeleri oluşturmak için .NET istemci kitaplığını kullanma öğreticileri bunlar.

  * [Paylaşılan erişim imzaları (SAS) kullanma](../storage-dotnet-shared-access-signature-part-1.md)
  * [Paylaşılan erişim imzası, bölüm 2: Oluşturma ve Blob hizmetiyle SAS kullanma](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Bu makale, bir açıklama SAS modelinin paylaşılan erişim imzaları örnekleri içerir ve en iyi uygulama önerileri SAS kullanın. Ayrıca ele alınan izni iptal olur.

* Kimlik Doğrulaması

  * [Azure Storage Hizmetleri için kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Paylaşılan erişim imzası öğretici Başlarken

  * [SAS öğretici Başlarken](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
### <a name="transport-level-encryption--using-https"></a>Aktarım düzeyinde şifreleme – HTTPS kullanarak
Azure Storage verilerinizin güvenliğini sağlamak için atılması gereken başka bir Azure Storage ve istemci arasında verileri şifrelemek için bir adımdır. Her zaman kullanmak için ilk önerilir [HTTPS](https://en.wikipedia.org/wiki/HTTPS) bir protokol olan genel Internet üzerinden güvenli iletişim sağlar.

REST API'larını çağırma veya erişme depolama nesneleri güvenli bir iletişim kanalı sağlamak için her zaman HTTPS kullanmalıdır. Ayrıca, **paylaşılan erişim imzaları**, yalnızca HTTPS protokolü herkes gönderme sağlanarak paylaşılan erişim imzaları kullanırken kullanılabilir belirtmek için bir seçenek içeriyor, Azure Storage nesnelere erişimi temsilci için kullanılabilir SAS bağlantılarıyla çıkışı belirteçleri doğru protokolünü kullanır.

Depolama hesaplarında etkinleştirerek nesneleri erişmek için REST API'larını çağırma HTTPS kullanılmasını zorunlu kılabilir [güvenli aktarımı gerekli](../storage-require-secure-transfer.md) depolama hesabı için. Bu özellik etkinleştirildiğinde, HTTP kullanarak bağlantı reddedilecek.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Azure dosya paylaşımları ile aktarım sırasında şifreleme kullanma
[Azure dosyaları](../files/storage-files-introduction.md) şifreleme SMB 3.0 üzerinden ve HTTPS ile dosya REST API'si kullanılırken destekler. Azure dosya paylaşımı Azure bölgesi dışında takma içinde şirket içi gibi veya başka bir Azure bölgesindeki bulunduğunda, SMB 3.0 şifrelemesi ile her zaman gereklidir. Varsayılan olarak bağlantıları yalnızca Azure aynı bölgede içinde izin verilir ancak şifrelemesi ile SMB 3.0 tarafından zorlanır SMB 2.1 şifrelemeyi desteklemiyor [güvenli aktarımı gerektiren](../storage-require-secure-transfer.md) depolama hesabı için.

SMB 3.0 şifrelemesi ile sağlanmıştır [tüm desteklenen Windows ve Windows Server işletim sistemlerini](../files/storage-how-to-use-files-windows.md) Windows 7 ve Windows Server 2008 R2 dışında yalnızca destekleyen SMB 2.1. SMB 3.0 da desteklenir [macOS](../files/storage-how-to-use-files-mac.md) ve dağıtımları [Linux](../files/storage-how-to-use-files-linux.md) Linux çekirdeği 4.11 kullanan ve üstü. SMB 3.0 şifreleme desteği backported birkaç Linux dağıtımları Linux çekirdekten eski sürümleri için de açıldı, başvurun [anlama SMB istemci gereksinimleri](../files/storage-how-to-use-files-linux.md#smb-client-reqs).

### <a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Depolama birimine gönderdiğiniz verilerin güvenliğini sağlamak için istemci tarafı şifreleme kullanma
Bir istemci uygulaması ve depolama arasında aktarılırken verilerinizin güvenli olduğundan emin olun yardımcı olan başka bir istemci tarafı şifreleme seçenektir. Verileri Azure depolama alanına aktarılmadan önce şifrelenir. İstemci tarafında alındıktan sonra verileri Azure depolama biriminden alırken, verilerin şifresi çözülür. Hangi verilerin bütünlüğünü etkileyen ağ hataları azaltmaya yardımcı olmak içinde yerleşik veri bütünlüğü denetimlerini olduğu gibi kablo giderek veri şifrelenir olsa da, aynı zamanda HTTPS kullanmanızı öneririz.

Veriler şifrelenmiş biçimde depolanır istemci tarafı şifreleme Ayrıca, rest, verileri şifrelemek için bir yöntem aynıdır. Biz bu konuda bölümünde daha ayrıntılı üzerinde konuşun [bekleyen şifreleme](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Bekleyen şifreleme
Bekleyen şifreleme sağlayan üç Azure özellikleri vardır. Azure Disk şifrelemesi, işletim sistemi ve veri diskleri olarak Iaas sanal makineleri şifrelemek için kullanılır. İstemci tarafı şifreleme ve SSE ikisi de Azure depolama birimindeki verileri şifrelemek için kullanılır. 

(Bu da şifrelenmiş biçimde depolama alanında depolanır) Aktarımdaki verileri şifrelemek için istemci tarafı şifreleme kullanabilmenize karşın, aktarım sırasında HTTPS kullanmak ve depolandığında otomatik olarak şifrelenecek veriler için herhangi bir şekilde sağlamak tercih edebilirsiniz. --Bunu iki şekilde Azure Disk şifrelemesi ve SSE vardır. Bir doğrudan VM'ler tarafından kullanılan işletim sistemi ve veri disklerdeki verileri şifrelemek için kullanılır ve diğer Azure Blob depolama alanına yazılan verileri şifrelemek için kullanılır.

### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

SSE tüm depolama hesapları için etkin ve devre dışı bırakılamaz. SSE verilerinizi Azure depolama alanına yazma sırasında otomatik olarak şifreler. Verileri Azure depolama alanından okuyun, Azure Storage tarafından döndürülen önce şifresi çözülür. SSE kodu değiştirin veya herhangi bir uygulama kodu eklemek zorunda kalmadan verilerinizin güvenliğini sağlar.

Microsoft tarafından yönetilen anahtarlar veya kendi özel anahtarları kullanabilirsiniz. Microsoft, yönetilen anahtarlar oluşturur ve iç Microsoft İlkesi tarafından tanımlanan normal kendi dönüş yanı sıra, güvenli depolama işler. Özel anahtarları kullanma hakkında daha fazla bilgi için bkz: [depolama hizmeti şifrelemesi müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak](storage-service-encryption-customer-managed-keys.md).

SSE tüm performans katmanları (Standart ve Premium), tüm dağıtım modelleri (Azure Resource Manager ve Klasik) ve tüm Azure Depolama hizmetlerinde (Blob, Kuyruk, Tablo ve Dosya) verileri otomatik olarak şifreler. 

### <a name="client-side-encryption"></a>İstemci tarafı şifreleme
İstemci tarafı şifreleme Aktarımdaki verilerin şifrelenmesini ele alırken belirtiliyor. Bu özellik ağ üzerinden göndermeden önce bir istemci uygulaması Azure depolama alanına yazılmasını ve program aracılığıyla Azure depolama biriminden aldıktan sonra verilerin şifresini çözmek verilerinizdeki programlı olarak şifrelenecek sağlar.

Bu şifreleme Aktarımdaki sağlar, ancak de bekleyen şifreleme özelliği sağlar. Veriler aktarım sırasında şifrelenir, ancak ağ hataları verilerin bütünlüğünü etkileyen azaltmaya yardımcı olmak yerleşik veri bütünlüğü denetimlerini yararlanmak için HTTPS kullanarak hala öneririz.

Bu burada kullanabileceğinize bir örnek, BLOB'ları depolayan ve BLOB'ları alır bir web uygulamasına sahip ve uygulama ve veri olarak güvenli olacak şekilde istediğiniz ' dir. Bu durumda, istemci tarafı şifreleme kullanırsınız. Azure Blob hizmeti ile istemci arasındaki trafiği şifrelenmiş kaynak içerir ve hiç kimse Aktarımdaki verileri yorumlama ve özel bloblarınızın yeniden oluşturma.

İstemci tarafı şifreleme Java ve sırayla Azure anahtar kasası uygulamak kolaylaşır API'lerini kullanan .NET depolama istemcisi kitaplıklarını yerleşik olarak bulunur. Şifreleme ve verilerin şifresini çözme işlemi Zarf tekniği kullanır ve her depolama nesnesindeki şifreleme tarafından kullanılan meta verileri depolar. Örneğin, BLOB'ları için bunu depoladığı blob meta verilerde sırada sıralar, bunu, her kuyruk iletisi ekler.

Şifreleme için kendisini oluşturmak ve kendi şifreleme anahtarlarınızı yönetin. Anahtarları oluştur Azure anahtar kasası olabilir veya Azure Storage istemci kitaplığı tarafından üretilen anahtarlar da kullanabilirsiniz. Şirket içi anahtar depolama şifreleme anahtarlarınızı depolayabilir veya bir Azure anahtar kasası depolayabilirsiniz. Azure anahtar kasası gizli anahtarları Azure anahtar kasası Azure Active Directory'yi kullanarak belirli kullanıcılara erişim sağlar. Bu, yalnızca herkes Azure anahtar kasası okuma ve böylelikle, istemci tarafı şifreleme için kullanmakta olduğunuz anahtarlarını alma anlamına gelir.

#### <a name="resources"></a>Kaynaklar
* [Şifrelemek ve şifresini çözmek Azure anahtar kasası kullanılarak Microsoft Azure Storage blobları](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  Bu makalede KEK oluşturmak ve PowerShell kullanarak kasaya depolamak nasıl dahil olmak üzere Azure anahtar kasası ile istemci tarafı şifreleme kullanmayı gösterir.
* [Microsoft Azure depolama için istemci tarafı şifreleme ve Azure anahtar kasası](../storage-client-side-encryption.md)

  Bu makalede, bir istemci tarafı şifreleme açıklamasını verir ve şifrelemek ve dört depolama hizmetleri kaynaklardan şifresini çözmek için depolama istemci kitaplığı kullanma örnekleri sağlar. Ayrıca, Azure anahtar kasası hakkında alınmaktadır.

### <a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Sanal makineler tarafından kullanılan diskler şifrelemek için Azure Disk şifrelemesi kullanma
Azure Disk şifrelemesi yeni bir özelliktir. Bu özellik, işletim sistemi ve bir Iaas sanal makine tarafından kullanılan veri disklerle şifrelemek sağlar. Windows için sürücüleri, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için DM-Crypt teknolojisini kullanan diskleri şifrelenir. Bu, denetlemek ve disk şifreleme anahtarlarını yönetmek izin vermek için Azure anahtar kasası ile tümleşiktir.

Microsoft Azure'da etkinleştirildiğinde çözümü Iaas VM'ler için aşağıdaki senaryoları destekler:

* Azure anahtar kasası ile tümleştirme
* Standart katmanı VMs: [A, D, DS, G, GS ve benzeri serisi Iaas VM'ler](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Windows ve Linux Iaas VM'ler üzerinde şifrelemeyi etkinleştirme
* Windows Iaas VM'ler için sürücüler işletim sistemi ve veri şifreleme devre dışı bırakma
* Veri sürücülerini şifreleme Linux Iaas VM'ler için devre dışı bırakma
* Windows istemci işletim sistemi çalıştıran bir Iaas Vm'leri üzerinde şifrelemeyi etkinleştirme
* Bağlama yolları birimlerle şifrelemeyi etkinleştirme
* Linux (RAID) mdadm kullanarak bölümlemesine diskle yapılandırılmış sanal makineleri üzerinde şifrelemeyi etkinleştirme
* Veri diskleri için LVM kullanarak Linux VM'ler üzerinde şifrelemeyi etkinleştirme
* Depolama alanları kullanılarak yapılandırılan Windows VM'ler üzerinde şifrelemeyi etkinleştirme
* Tüm Azure genel bölgeleri desteklenir

Çözüm aşağıdaki senaryolar, özellikler ve teknoloji sürümde desteklemez:

* Temel katman Iaas VM'ler
* Bir işletim sistemi sürücüsünde Linux Iaas VM'ler için şifrelemeyi devre dışı bırakma
* Klasik VM oluşturma yöntemini kullanarak oluşturduğunuz Iaas VM'ler
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme
* Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılmış Windows VM'ler


> [!NOTE]
> Linux işletim sistemi disk şifrelemesi aşağıdaki Linux dağıtımları üzerinde şu anda desteklenmiyor: RHEL 7.2, CentOS 7.2n ve Ubuntu 16.04.
>
>

Bu özellik, sanal makine disklerdeki tüm veriler şifrelenir, Azure Storage REST sağlar.

#### <a name="resources"></a>Kaynaklar
* [Windows ve Linux Iaas VM'ler için Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Azure Disk şifrelemesi, SSE ve istemci tarafı şifreleme karşılaştırması

#### <a name="iaas-vms-and-their-vhd-files"></a>Iaas Vm'leri ve bunların VHD dosyaları

Iaas VM'ler tarafından kullanılan veri diskleri için Azure Disk şifrelemesi önerilir. Azure Market'teki bir görüntü kullanarak yönetilmeyen disklerle VM oluşturursanız, Azure gerçekleştiren bir [kopyalama yüzeysel](https://en.wikipedia.org/wiki/Object_copying) SSE etkin olsa bile, depolama resmi ve Azure depolama hesabında şifrelenmez. Bu VM oluşturur ve görüntüsünü güncelleştirme başladıktan sonra verileri şifrelemek SSE başlar. Bu nedenle, bunları tam olarak şifrelenmiş istiyorsanız Azure Market görüntülerini oluşturulan yönetilmeyen disklerle Vm'lerinde Azure Disk şifrelemesi kullanmak en iyisidir. Yönetilen disklerle VM oluşturursanız, SSE yönetilen platform anahtarlar kullanılarak varsayılan olarak tüm verileri şifreler. 

Şirket içinden Azure'a önceden şifrelenmiş VM getirirseniz, şifreleme anahtarları Azure anahtar Kasası'na karşıya yükleyin ve şirket içi kullanmakta olduğunuz o VM için şifreleme kullanmaya devam edebilirsiniz. Bu senaryo işlemek için Azure Disk şifrelemesi etkin.

Şirket içi şifrelenmemiş VHD'den varsa, özel bir görüntü olarak Galerisi içine yüklemek ve bir VM'den sağlayın. Resource Manager şablonları kullanarak bunu yaparsanız, VM'yi yedekleme yüklediğinde Azure Disk şifrelemesi etkinleştirmeniz sorabilirsiniz.

Bir veri diski ekleyin ve VM bağlamak, bu veri diskte Azure Disk şifrelemesi kapatabilirsiniz. Bu veri diski yerel olarak ilk şifreler ve depolama içeriğin şifreli biçimde Klasik dağıtım modeli katmanı geç yazma depolama karşı sonra ne yapacağını.

#### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme
İstemci tarafı şifreleme, verileri şifrelemek için en güvenli yöntem nedeni önce Aktarımdaki verileri şifreler.  Ancak, kodu yapmak isteyebilirsiniz depolama kullanarak uygulamalarınızı ekleyin gerektirir. Bu durumlarda, Aktarımdaki verileri korumak için HTTPS kullanabilirsiniz. Verileri Azure Storage ulaştığında, SSE tarafından şifrelenir.

İstemci tarafı şifreleme ile tablo varlıkları, iletileri kuyruğa ve blobları şifreleyebilirsiniz. 

İstemci tarafı şifreleme tamamen uygulama tarafından yönetilir. Bu en güvenli yaklaşım, ancak uygulamanıza programlı değişiklikler yapabilir ve anahtar yönetimi işlemleri yerleştirdiniz gerektirir. Bu aktarım sırasında ek güvenlik istediğiniz ve depolanan verilerin şifrelenmesi için istediğinizde kullanırsınız.

İstemci tarafı şifreleme istemci üzerinde daha fazla yük ve özellikle, şifreleme ve büyük miktarda veri aktarma ölçeklenebilirlik planlarınızı bu hesaba sahip.

#### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

SSE Azure Storage tarafından yönetilir. SSE Aktarımdaki verileri güvenlik sağlamaz, ancak Azure depolama alanına yazılır gibi verileri şifreliyor. SSE, Azure Depolama performansını etkilemez.

Her türlü veri SSE kullanarak depolama hesabının şifreleyebilir (blok blobları, ekleme blobları, sayfa BLOB'ları, tablo verileri, kuyruk verileri ve dosyaları).

Bir arşiv veya kitaplık yeni sanal makineler oluşturmak için temel olarak kullanmak VHD dosyalarının varsa, yeni bir depolama hesabı oluşturun ve bu hesaba VHD dosyalarını karşıya yükleyin. Bu VHD dosyalarını Azure Storage tarafından şifrelenir.

Azure Disk şifrelemesi VM diskleri için etkin varsa, yeni yazılmış veri SSE hem Azure Disk Şifrelemesi tarafından şifrelenir.

## <a name="storage-analytics"></a>Depolama Analizi
### <a name="using-storage-analytics-to-monitor-authorization-type"></a>Storage Analytics Yetkilendirme türü izlemek için kullanma
Her Depolama hesabı için günlük kaydı gerçekleştirmek ve ölçüm verilerini depolamak Azure Storage Analytics etkinleştirebilirsiniz. Bu, bir depolama hesabı performans ölçümlerini denetlemek istediğiniz veya performans sorunları yaşıyor olduğundan bir depolama hesabı sorun giderme gerektiğinde kullanmak için harika bir araçtır.

Veri depolama analytics günlüklerde görebilirsiniz başka bir parçası, depolama eriştiklerinde birisi tarafından kullanılan kimlik doğrulama yöntemidir. Örneğin, paylaşılan erişim imzası veya depolama hesabı anahtarlarını kullandıysanız ya da erişilen blob ortak ' da, bir Blob Storage'ı görebilirsiniz.

Bu depolama alanına erişimi sıkı bir şekilde kullanılan koruyarak durumunda yararlı olabilir. Örneğin, Blob depolama alanına tüm kapsayıcıların özel ayarlayın ve uygulamalarınızı bir SAS hizmet kullanımını uygulayın. Ardından, düzenli olarak bloblarınızın bir güvenlik açığını gösterebilir, depolama hesabı anahtarları kullanılarak erişilir mı yoksa BLOB ortak ancak bunlar olmamalıdır görmek için günlüklere denetleyebilirsiniz.

#### <a name="what-do-the-logs-look-like"></a>Günlükleri ne gibi görünüyor?
Sonra depolama hesabı ölçümlerini etkinleştirmeniz ve Azure portalı üzerinden günlüğü, analiz verileri hızlı bir şekilde birikmesine başlar. Günlüğe kaydetme ve ölçümleri her hizmet için ayrı; günlüğe kaydetme olduğunda etkinlik bu depolama hesabında ölçümleri dakikada, her saat veya nasıl yapılandırdığınıza bağlı olarak, her gün kaydedilir ancak yalnızca yazılır.

Günlükleri blok blobları depolama hesabındaki $logs adlı bir kapsayıcıda depolanır. Storage Analytics etkinleştirilmişse, bu kapsayıcı otomatik olarak oluşturulur. Bu kapsayıcı oluşturulduktan sonra içeriği silebilir ancak bunu silemezsiniz.

$Logs kapsayıcısı altında her hizmet için bir klasör yoktur ve ardından vardır alt yıl/ay/gün/saattir. Saat altında günlükleri numaralandırılır. Dizin yapısı nasıl görüneceğini şudur:

![Günlük dosyalarını görüntüle](./media/storage-security-guide/image1.png)

Her istek için Azure Storage günlüğe kaydedilir. Aşağıda, bir anlık görüntü ilk birkaç alanları gösteren bir günlük dosyası verilmiştir.

![Anlık görüntü bir günlük dosyası](./media/storage-security-guide/image2.png)

Günlükleri herhangi bir tür bir depolama hesabı çağrı izlemek için kullanabileceğiniz görebilirsiniz.

#### <a name="what-are-all-of-those-fields-for"></a>İçin bu alanların tümünü nelerdir?
Var olan kaynaklar, aşağıda listelenen bir makale günlüklerine ve ne için kullanıldıkları birçok alanların bir listesini sağlar. Alanları sırada listesi aşağıdadır:

![Bir günlük dosyası alanların anlık görüntü](./media/storage-security-guide/image3.png)

GetBlob girişlerinde ilginizi çalışıyoruz ve yetkileri nasıl böylece ihtiyacımız işlemi türü "Get-Blob" girdilerini arayın ve istek durumunu denetlemek (Dördüncü</sup> sütun) ve Yetkilendirme türü (sekizinci</sup> sütun).

Örneğin, ilk birkaç satırı yukarıdaki listede istek durumu "Başarılı" olur ve Yetkilendirme türü "kimlik doğrulaması". Bu istek depolama hesabı anahtarı kullanılarak yetkilendirildi anlamına gelir.

#### <a name="how-is-access-to-my-blobs-being-authorized"></a>Nasıl yetkilendirilmekte my BLOB'lar erişimi var mı?
Biz ilgilendiğiniz üç durumda sunuyoruz.

1. Blob geneldir ve paylaşılan erişim imzası olmadan bir URL kullanılarak erişilir. Bu durumda, istek status "AnonymousSuccess" ve Yetkilendirme türü "anonim".

   1.0;2015-11-17T02:01:29.0488963Z;GetBlob;**AnonymousSuccess**;200;124;37;**anonymous**;;mystorage…
2. Blob özeldir ve paylaşılan erişim imzası ile kullanıldı. Bu durumda, istek durumunu "SASSuccess" ve Yetkilendirme türü "sa".

   1.0;2015-11-16T18:30:05.6556115Z;GetBlob;**SASSuccess**;200;416;64;**sas**;;mystorage…
3. Blob özeldir ve depolama anahtarı erişmek için kullanıldı. Bu durumda, istek durumunu olan "**başarı**"ve Yetkilendirme türü"**kimliği doğrulanmış**".

   1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Başarı**; 206 59; 22; **Kimliği doğrulanmış**; mystorage...

Microsoft Message Analyzer görüntülemek ve bu günlükler çözümlemek için kullanabilirsiniz. Arama ve filtreleme yetenekleri içerir. Örneğin, aramak istediğiniz, diğer bir deyişle, beklediğiniz kullanım ise emin olmak için birisi değil eriştiği depolama hesabınız açamayacağı görmek için GetBlob örnekleri.

#### <a name="resources"></a>Kaynaklar
* [Depolama Analizi](../storage-analytics.md)

  Bu makalede depolama çözümlemeleri ve bunları etkinleştirmek nasıl bir genel bakıştır.
* [Storage Analytics günlük biçimi](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Bu makalede, depolama Analytics günlük biçimi gösterir ve istek için kullanılan kimlik doğrulama türünü gösteren kimlik doğrulama-türü de dahil olmak üzere kullanılabilir alanlar, ayrıntıları.
* [Azure portalında bir depolama hesabını izleme](../storage-monitor-storage-account.md)

  Bu makalede ölçümlerini izleme ve günlüğe kaydetme için bir depolama hesabı nasıl yapılandırılacağı gösterilmektedir.
* [Azure Storage ölçümleri ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi'ni kullanarak uçtan uca sorun giderme](../storage-e2e-troubleshooting.md)

  Bu makalede Storage Analytics kullanarak sorun giderme hakkında konuşur ve Microsoft Message Analyzer'ı kullanmayı gösterir.
* [Microsoft Message Analyzer işletim kılavuzu](https://technet.microsoft.com/library/jj649776.aspx)

  Bu makalede Microsoft Message Analyzer başvurusu ve öğretici, hızlı başlangıç ve Özellik Özeti bağlantılar içerir.

## <a name="cross-origin-resource-sharing-cors"></a>Çıkış Noktaları Arası Kaynak Paylaşımı (CORS)
### <a name="cross-domain-access-of-resources"></a>Kaynak etki alanları arası erişimi
Bir etki alanında çalışan bir web tarayıcısı farklı bir etki alanından bir kaynak için bir HTTP istek yaptığında, bu çıkış noktaları arası HTTP isteği çağrılır. Örneğin, contoso.com sunulan HTML sayfası fabrikam.blob.core.windows.net üzerinde barındırılan bir JPEG dosyası için istekte bulunur. Güvenlik nedenleriyle, tarayıcılar JavaScript gibi komut dosyaları ve başlatılan cross-origin HTTP istekleri kısıtlayın. Bu, bazı JavaScript kodları contoso.com web sayfasında bu jpeg fabrikam.blob.core.windows.net üzerinde istediğinde, tarayıcı istek vermez anlamına gelir.

Ne bu Azure Storage ile yapmak var mı? İyi, JSON veya XML veri dosyaları gibi statik varlıklar depolanıyorsa Blob depolama alanına Fabrikam adlı bir depolama hesabı kullanarak, etki alanı varlıklar için fabrikam.blob.core.windows.net olur ve contoso.com web uygulaması bunlara erişmek mümkün olmaz kullanma JavaScript etki alanları farklı olduğundan. Bu aynı zamanda bir Azure Storage Hizmetleri – Table Storage gibi– çağırmak çalışıyorsanız, JavaScript istemci tarafından işlenmek üzere JSON verileri döndürmek geçerlidir.

#### <a name="possible-solutions"></a>Olası çözümler
Bu sorunu çözmek için bir "storage.contoso.com" gibi özel bir etki alanı için fabrikam.blob.core.windows.net atamak için yoludur. Bir depolama hesabının özel etki yalnızca atayabilirsiniz sorunudur. Ne varlıkları birden çok depolama hesaplarında depolanır?

Bu sorunu çözmek için başka bir depolama çağrıları için proxy olarak davranmasına web uygulaması için yoludur. Bu Blob depolama alanına bir dosya yüklemek, web uygulaması ya da yerel olarak yazma ve Blob depolama alanına kopyalama veya tamamını belleğe okuma ve Blob depolama alanına yazma varsa anlamına gelir. Alternatif olarak, dosyaları yerel olarak yükleyen ve Blob depolama alanına yazan bir adanmış web uygulaması (örneğin, bir Web API) yazabilirsiniz. Her iki durumda da ölçeklenebilirlik belirleme ihtiyacı olduğunda bu işlev için hesabı gerekir.

#### <a name="how-can-cors-help"></a>CORS nasıl yardımcı olabilir?
Azure depolama arası kaynak paylaşımı kaynak CORS – etkinleştirmenize olanak sağlar. Her Depolama hesabı için bu depolama hesabındaki kaynaklara erişebilir etki alanları belirtebilirsiniz. Örneğin, yukarıda özetlenen Örneğimizde, biz CORS fabrikam.blob.core.windows.net depolama hesabında etkinleştirin ve contoso.com erişmesine izin vermek üzere yapılandırın. Daha sonra web uygulama contoso.com fabrikam.blob.core.windows.net kaynaklarında doğrudan erişebilirsiniz.

Not etmek için bir CORS erişim verir, ancak tüm olmayan ortak erişim için depolama kaynakları gerekli olan kimlik doğrulaması sağlamaz şeydir. Bu, yalnızca ortak veya paylaşılan erişim imzası uygun izni vermesini dahil BLOB'lar erişebilir anlamına gelir. Tabloları, kuyrukları ve dosya genel erişiminiz yok ve bir SAS gerektirir.

Varsayılan olarak, CORS tüm hizmetleri devre dışıdır. CORS hizmeti ilkeleri ayarlamak için yöntemi çağırmak için REST API veya depolama istemci kitaplığı kullanarak etkinleştirebilirsiniz. Bunu yaptığınızda, XML biçiminde bir CORS kuralı içerir. Blob hizmeti için hizmet özelliklerini ayarlama işlemi için bir depolama hesabı kullanılarak ayarlanmış bir CORS kuralı bir örneği burada verilmiştir. Azure Storage için depolama istemci kitaplığı veya REST API'lerini kullanarak bu işlemi gerçekleştirebilir.

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

Her satır anlamı şudur:

* **AllowedOrigins** bu hangi eşleşmeyen etki alanları istemek ve storage hizmetinden veri alma bildirir. Bu, veri contoso.com ve fabrikam.com belirli bir depolama hesabı için Blob depolama alanından isteyebilir söyler. Bu aynı zamanda bir joker karakter ayarlayabilirsiniz (\*) erişim isteklerini tüm etki alanları izin vermek için.
* **AllowedMethods** isteği yaparken kullanılan yöntem (HTTP isteği fiiller) listesidir. Bu örnekte, yalnızca GET ve PUT izin verilir. Bu bir joker karakter ayarlayabilirsiniz (\*) kullanılacak tüm yöntemleri izin vermek için.
* **AllowedHeaders** kaynak etki alanı isteği yapılırken belirtebilirsiniz istek üstbilgileri budur. Bu örnekte, x-ms-meta-hedef ve x-ms-meta-abc, x-ms-meta veri ile başlangıç tüm meta veri üstbilgileri izin verilir. Joker karakter (\*) Belirtilen önek herhangi üstbilgi başlayarak izin verildiğini gösterir.
* **ExposedHeaders** bu hangi yanıt üstbilgilerini isteği veren tarayıcıya tarafından açılmamalıdır bildirir. Bu örnekte, başlayarak başlığı "x-ms - meta-" gösterilir.
* **MaxAgeInSeconds** bir tarayıcı denetim öncesi seçenekleri isteği önbelleğe alır en uzun süreyi budur. (Denetim öncesi isteği hakkında daha fazla bilgi için aşağıdaki ilk makalesine bakın.)

#### <a name="resources"></a>Kaynaklar
CORS ve etkinleştirmek hakkında daha fazla bilgi için aşağıdaki kaynaklara gözatın.

* [Çıkış noktaları arası kaynak paylaşımı (CORS) Azure.com üzerindeki Azure Storage Hizmetleri desteği](../storage-cors-support.md)

  Bu makalede CORS ve farklı depolama hizmetleri için kuralları ayarlama hakkında genel bir bakış sağlar.
* [Çıkış noktaları arası kaynak paylaşımı (CORS) MSDN'deki Azure Storage Hizmetleri desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Azure Storage Hizmetleri için CORS desteği için başvuru belgeleri budur. Bu her depolama hizmeti için uygulama makalelerinin bağlantıları vardır ve bir örneği gösterir ve her bir öğesinde CORS dosya açıklar.
* [Microsoft Azure Storage: CORS Tanıtımı](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  CORS tanışın ve nasıl kullanılacağını gösteren ilk blog makaleyi bağlantıdır.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Azure Storage güvenliği hakkında sık sorulan sorular
1. **HTTPS protokolünü kullanamıyorsanız ı içine veya dışına Azure Storage aktarma BLOB'ları bütünlüğünü doğrulamak ne?**

   HTTPS ve yerine HTTP kullanmanız gereken herhangi bir nedenle blok blobları ile çalışıyorsanız, aktarılan BLOB'ları bütünlüğünü doğrulamak için MD5 denetimi'ı kullanabilirsiniz. Bu ağ/aktarım katmanı hataları korumadan, ancak mutlaka Ara saldırılarla yardımcı olur.

   Aktarım düzeyi güvenlik sağlayan HTTPS kullanırsanız, MD5 denetimi kullanarak yedekli ve gereksiz.

   Daha fazla bilgi için lütfen kullanıma [Azure Blob MD5 genel bakış](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **ABD için FIPS uyumluluğu hakkında Kamu?**

   Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) kullanım için ABD tarafından onaylanan şifreleme algoritmalarını tanımlar Önemli verilerin korunması için Federal hükümeti bilgisayar sistemleri. FIPS etkinleştirme yalnızca FIPS doğrulamalı şifreleme algoritmaları kullanılması gereken işletim sistemi Windows server veya masaüstünde modu söyler. Uygulamanın uyumlu algoritmaları kullanıyorsa, uygulamalar çalışmamasına neden olur. With.NET Framework sürüm 4.5.2 veya üzeri, uygulama bilgisayar FIPS modundayken FIPS uyumlu algoritmaları kullanmak için şifreleme algoritmalarını otomatik olarak geçer.

   Microsoft FIPS modunda etkinleştirmek karar vermek için her bir müşteri kadar bırakır. Varsayılan olarak FIPS modunda etkinleştirmek için kamu düzenlemeleri tabi olmayan müşteriler ilgi çekici bir neden yoktur inanıyoruz.

### <a name="resources"></a>Kaynaklar
* [Neden biz "FIPS modunda" artık öneren değil](https://blogs.technet.microsoft.com/secguide/2014/04/07/why-were-not-recommending-fips-mode-anymore/)

  Bu Web günlüğü makalesini FIPS genel bir bakış sağlar ve bunlar varsayılan olarak FIPS modunda neden etkinleştirmezseniz açıklanmaktadır.
* [FIPS 140 doğrulaması](https://technet.microsoft.com/library/cc750357.aspx)

  Bu makalede nasıl Microsoft ürünleri ve şifreleme modülleri FIPS Standart ABD için uyumlu hakkında bilgiler sağlanmaktadır. Federal Hükümet.
* ["Sistem şifrelemesi: kullanım FIPS uyumlu algoritmaları şifreleme, kodlama ve imzalama için" güvenlik ayarları etkilerini Windows XP ve Windows'un sonraki sürümleri](https://support.microsoft.com/kb/811833)

  Bu makalede, FIPS modunda daha eski Windows bilgisayarları kullanımıyla ilgili alınmaktadır.