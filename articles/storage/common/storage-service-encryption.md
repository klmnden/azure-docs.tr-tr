---
title: "Rest verileri için Azure Storage hizmeti şifreleme | Microsoft Docs"
description: "Veri alınırken şifresini çözmek ve Azure depolama hizmeti şifrelemesi özelliği hizmet tarafında Azure Blob Depolama veri depolarken şifrelemek için kullanın."
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: tamram
ms.openlocfilehash: d04752c92218d17b4e90bb03a2ed2ac5a0a11b93
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Bekleyen Veri için Azure Storage Hizmeti Şifreleme
Azure Storage hizmeti şifreleme (SSE) bekleyen veri için korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olur. Bu özellik ile Azure Depolama, verilerinizi depolama alanında kalıcı hale gelmeden önce otomatik olarak şifreler ve alınmadan önce bunların şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi, kullanıcılara tamamen şeffaf bir şekilde sunulur.

Aşağıdaki bölümlerde, depolama hizmeti şifrelemesi özelliklerin yanı sıra desteklenen senaryolar ve kullanıcı nasıl kullanılacağı hakkında ayrıntılı yönergeler deneyimleri sunar.

## <a name="overview"></a>Genel Bakış
Azure depolama birlikte geliştiricilerin güvenli uygulamalar oluşturmalarını sağlayan güvenlik özellikleri kapsamlı bir kümesini sağlar. Veri güvenli bir uygulama ile Azure arasında aktarımda kullanarak [istemci tarafı şifreleme](../storage-client-side-encryption.md), HTTPs veya SMB 3.0. Depolama hizmeti şifrelemesi, bekleyen şifreleme, işleme şifreleme, şifre çözme ve tamamen saydam bir şekilde anahtar yönetimi sağlar. Tüm veriler, 256 bit kullanılarak şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

Azure depolama alanına yazılır ve Azure Blob, dosya, tablo ve kuyruk depolama için kullanılan verileri şifreleyerek SSE çalışır. Şunlar için geçerlidir:

* Standart Depolama: Dosya, tablo ve kuyruk depolama ve Blob storage hesapları Blobları için genel amaçlı depolama hesapları
* Premium depolama 
* Klasik ve ARM depolama hesapları için varsayılan olarak etkin 
* Tüm artıklık düzeyleri (LRS, ZRS, GRS, RA-GRS)
* Azure Resource Manager depolama hesapları (ancak klasik değil) 
* Tüm bölgeler.

Daha fazla bilgi için SSS Bölümüne bakın.

Bir depolama hesabı için depolama hizmeti şifrelemesi ayarlarını görüntülemek için oturum açın [Azure portal](https://portal.azure.com) ve bir depolama hesabı seçin. Ayarlar dikey penceresinde bu ekran görüntüsünde gösterildiği gibi için Blob hizmeti bölümüne bakın ve şifreleme'yi tıklatın.

![Portal gösteren ekran görüntüsü şifreleme seçeneği](./media/storage-service-encryption/image1.png)
<br/>*Şekil 1: (1. adım) Blob hizmeti için SSE etkinleştir*

![Portal gösteren ekran görüntüsü şifreleme seçeneği](./media/storage-service-encryption/image3.png)
<br/>*Şekil 2: Dosya hizmeti (1. adım) SSE etkinleştir*

Şifreleme ayarı tıkladıktan sonra etkinleştirme veya depolama hizmeti şifrelemesi devre dışı bırakabilirsiniz.

![Portal gösteren ekran görüntüsü şifreleme özellikleri](./media/storage-service-encryption/image2.png)
<br/>*Şekil 3: SSE için Blob etkinleştirin ve dosya hizmeti (Adım2)*

## <a name="encryption-scenarios"></a>Şifreleme senaryosu
Depolama hizmeti şifrelemesi, bir depolama hesabı düzeyinde etkinleştirilir. Tüm hizmetler için varsayılan etkindir. Aşağıdaki müşteri senaryoları destekler:

* Klasik ve Resource Manager hesapları için şifreleme Blob, dosya, tablo ve kuyruk depolama.

SSE aşağıdaki sınırlamalara sahiptir:

* Şifreleme etkinleştirildikten sonra varolan verilerin - SSE yalnızca yeni oluşturulan verileri şifreler. Örneğin yeni bir Resource Manager depolama hesabı oluşturun ancak şifreleme kapatmayın ve ardından BLOB veya arşivlenmiş VHD'ler bu depolama hesabı için karşıya yükleme ve SSE kapatma varsa, yeniden yazılmıştır veya kopyaladığınız sürece bu BLOB'lar şifrelenmez.
* Market desteği - VMs etkinleştir şifrelenmesi oluşturulan Market kullanımından [Azure portal](https://portal.azure.com), PowerShell ve Azure CLI. VHD temel görüntü şifrelenmemiş kalacak; Ancak, VM çalışmaya başlar sonra yapılan tüm yazma şifrelenir.

## <a name="getting-started"></a>Başlarken
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>1. adım: [yeni depolama hesabı oluşturma](../storage-create-storage-account.md).
### <a name="step-2-verify-encryption"></a>2. adım: şifreleme doğrulayın.
Şifreleme kullanarak doğrulayabilirsiniz [Azure portal](https://portal.azure.com).

> [!NOTE]
> Program aracılığıyla bir depolama hesabında depolama hizmeti şifrelemesi doğrulamak istiyorsanız, kullanabileceğiniz [Azure depolama kaynak sağlayıcısı REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx), [.NET için depolama kaynak sağlayıcısı istemci Kitaplığı](https://msdn.microsoft.com/library/azure/mt131037.aspx) , [Azure PowerShell](/powershell/azureps-cmdlets-docs), veya [Azure CLI](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>3. adım: veri depolama alanına kopyalanmaya
SSE için bir depolama hesabı etkinleştirildikten sonra bu depolama hesabına yazılan veri şifrelenir. Bunlar yazılır kadar zaten bu depolama hesabında bulunan tüm veriler şifrelenmez. Veri önceki verilerin şifrelendiğinden emin olmak için başka bir bir kapsayıcıdan kopyalayabilirsiniz. Bunu gerçekleştirmek için aşağıdaki araçlardan birini kullanabilirsiniz. Dosya, tablo ve kuyruk depolama için de aynı davranışı budur.

#### <a name="using-azcopy"></a>AzCopy kullanma
AzCopy en uygun performans ile basit komutları kullanarak Microsoft Azure Blob, dosya ve tablo depolama gelen ve giden veri kopyalamak için tasarlanmış bir Windows komut satırı yardımcı programıdır. Bu BLOB veya dosyaları bir depolama hesabından SSE etkin olan başka bir kopyalamak için kullanabilirsiniz. 

Daha fazla bilgi için lütfen ziyaret [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

#### <a name="using-smb"></a>SMB kullanma
Azure dosyaları standart SMB protokolü kullanılarak bulutta dosya paylaşımları sunar. Şirket içinde veya Azure bir istemciden bir dosya paylaşımı bağlayabilir. Takılı sonra Azure dosya paylaşımları üzerinden dosyaları kopyalamak için Robocopy gibi araçlar kullanılabilir. Daha fazla bilgi için bkz: [Windows Azure dosya paylaşımı nasıl](../files/storage-how-to-use-files-windows.md) ve [Linux'ta Azure dosya paylaşımının nasıl](../files/storage-how-to-use-files-linux.md).


#### <a name="using-the-storage-client-libraries"></a>Depolama istemcisi kitaplıklarını kullanma
Blob veya dosya verilerini ve blob depolamadan veya depolama istemci kitaplığı .NET, C++, Java, Android, Node.js, PHP, Python ve Ruby gibi zengin bizim kümesini kullanarak depolama hesapları arasında kopyalayabilirsiniz.

Daha fazla bilgi için lütfen ziyaret bizim [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Depolama Gezgini'ni kullanma
Depolama hesapları oluşturmak, karşıya yükleme ve veri indirin, BLOB'lar içeriğini görüntülemek ve dizinleri gidin için bir Depolama Gezgini kullanabilirsiniz. BLOB Depolama hesabınıza şifreleme ile karşıya yüklemek için bunlardan birini kullanabilirsiniz. Bazı depolama gezginleri ile ayrıca veri varolan blob depolama alanından depolama hesabı ya da etkin SSE sahip yeni bir depolama hesabında farklı bir kapsayıcıya kopyalayabilirsiniz.

Daha fazla bilgi için lütfen ziyaret [Azure depolama gezginleri](../storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>4. adım: şifrelenmiş verileri durumunu sorgulama
Depolama istemcisi kitaplıklarını güncelleştirilmiş bir sürümünü veya şifrelenmiş varsa belirlemek için bir nesnenin durumunu sorgulamak izin veren dağıtıldı. Bu şu anda yalnızca Blob Depolama için kullanılabilir. Yol Haritası üzerinde dosya depolama desteğidir. 

Bu arada, çağırabilirsiniz [hesap özellikleri Al](https://msdn.microsoft.com/library/azure/mt163553.aspx) depolama hesabı şifreleme etkin olduğunu doğrulayın veya Azure Portal'da depolama hesabı özellikleri görüntülemek için.

## <a name="encryption-and-decryption-workflow"></a>Şifreleme ve şifre çözme iş akışı
Aşağıda, şifreleme/şifre çözme iş akışı kısa bir açıklaması verilmiştir:

* Şifreleme için bir depolama hesabı etkinleştirilir.
* Müşteri yeni verileri (PUT Blob, YERLEŞTİRME blok YERLEŞTİRME sayfası, PUT dosya vb.) Blob veya dosya depolama alanına yazdığında; Her yazma 256 bit kullanılarak şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.
* Müşteri verilerini (Blob alma, vb.) erişim gerektiğinde veriler kullanıcıya geri dönmeden önce otomatik olarak çözülür.
* Şifreleme devre dışı bırakılırsa, yeni yazma artık şifrelenir ve kullanıcı tarafından yeniden yazılmıştır kadar mevcut şifrelenmiş veriler şifrelenmiş kalır. Şifreleme etkinken Blob veya dosya depolama yazma işlemlerini şifrelenir. Etkinleştirme/depolama hesabı için şifrelemeyi devre dışı bırakma arasında geçiş kullanıcı veri durumunu değiştirmez.
* Tüm şifreleme anahtarlarını şifreli, saklanır ve Microsoft tarafından yönetilir.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Sık sorulan bekleyen veri için depolama hizmeti şifrelemesi hakkında sorular
**S: Klasik depolama hesabınız sahibim. SSE üzerinde etkinleştirebilirim?**

Y: SSE tüm depolama hesapları (Klasik ve Resource Manager depolama hesapları) için varsayılan olarak etkindir.

**S: nasıl ı veri Klasik depolama Hesabımı şifreleyebilir mi?**

A: varsayılan olarak etkin şifreleme ile yeni veri depolama hizmeti tarafından otomatik olarak şifrelenir. 

Ayrıca yeni bir Resource Manager depolama hesabı oluşturun ve tüm kullanarak verilerinizi kopyalayın [AzCopy](storage-use-azcopy.md) hesabınızdan varolan Klasik depolama, yeni oluşturulan Resource Manager depolama hesabı. 

Klasik depolama hesabınıza bir Resource Manager depolama hesabına geçirmek seçebilirsiniz. Bu işlem anlık, hesabınızı türünü değiştirir ancak mevcut verilerinizi etkilemez. Yalnızca şifreleme etkinleştirildikten sonra herhangi bir yeni yazılmış veri şifrelenir. Daha fazla bilgi için bkz: [Platform desteklenen geçiş Iaas kaynak Klasik'ten Kaynak Yöneticisi](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Bu yalnızca Blob ve Dosya Hizmetleri için desteklendiğini unutmayın.

**S: mevcut bir Resource Manager depolama hesabını sahibim. SSE üzerinde etkinleştirebilirim?**

Y: SSE varolan tüm Resource Manager depolama hesaplarıyla üzerinde varsayılan olarak etkindir. Bu BLOB'lar, tablolar, dosya ve Kuyruk için desteklenen depolama. 

**S: var olan bir Resource Manager depolama hesabı geçerli verileri şifrelemek ister misiniz?**

Y: ile SSE varsayılan olarak tüm depolama hesaplarının - Klasik ve Resource Manager depolama hesapları etkin. Ancak, zaten mevcut veri şifrelenmez. Var olan verileri şifrelemek için başka bir ad veya başka bir kapsayıcı kopyalayın ve ardından şifrelenmemiş sürümlerini kaldırın. 

**S: Premium depolama kullanıyorum; SSE kullanabilir miyim?**

A: Evet SSE hem standart depolama hem de Premium depolama desteklenir.  Premium depolama dosya hizmeti için desteklenmiyor.

**S: ı yeni bir depolama hesabı etkin SSE ile oluşturursanız, depolama hesabı VM'im şifrelenir, ortalama mu kullanarak yeni bir VM oluşturulsun mu?**

Y: Evet. SSE etkinleştirildikten sonra oluşturulan sürece yeni depolama hesabı kullanmak oluşturulmuş diskler şifrelenir. VM Azure markette kullanılarak oluşturulduysa, VHD temel görüntü şifrelenmemiş kalacak; Ancak, VM çalışmaya başlar sonra yapılan tüm yazma şifrelenir.

**S: yeni depolama hesapları ile Azure PowerShell ve Azure CLI kullanarak etkin SSE oluşturabilir miyim?**

Y: SSE herhangi bir depolama hesabı (Klasik ve Resource Manager hesapları) oluşturma sırasında varsayılan olarak etkindir. Azure PowerShell ve Azure CLI kullanarak hesap özelliklerini doğrulayabilirsiniz.

**S: SSE etkinse, daha fazlasını nasıl Azure depolama maliyeti?**

Y: yoktur ek ücret ödemeden.

**S: şifreleme anahtarları yöneten?**

Y: anahtarları Microsoft tarafından yönetilir.

**S: kendi şifreleme anahtarları kullanmak?**

Y: müşterilerin kendi şifreleme anahtarlarını getirmek özelliklerinin sağlanması üzerinde çalışıyoruz.

**S: şifreleme anahtarlarının erişimi iptal?**

Şu anda A:; anahtarları tam olarak Microsoft tarafından yönetilir.

**S: yeni bir depolama hesabı oluşturduğunuzda, varsayılan olarak etkin SSE mi?**

A: Evet SSE Microsoft Managed tuşlarını kullanarak, tüm depolama hesapları - Azure Resource Manager ve klasik depolama hesapları için varsayılan olarak etkindir. Tüm hizmetleri de - Blob, tablo, kuyruk ve dosya depolama alanı için etkinleştirilir.

**S: nasıl bu Azure Disk Şifrelemesi ' farklı mı?**

Y: Bu özellik, Azure Blob Depolama birimindeki verileri şifrelemek için kullanılır. Azure Disk şifrelemesi, işletim sistemi ve veri diskleri Iaas Vm'leri de şifrelemek için kullanılır. Daha fazla ayrıntı için lütfen ziyaret bizim [depolama Güvenlik Kılavuzu](../storage-security-guide.md).

**S: Peki SSE etkin ve ardından Azure Disk şifrelemesi disklerde etkinleştirilsin mi?**

Y: Bu sorunsuz bir şekilde çalışır. Her iki yöntemle veri şifrelenir.

**S: depolama Hesabımı coğrafi nedenle çoğaltılması için ayarlanır. SSE etkinleştirilirse, my yedek kopya de şifrelenir mi?**

A: Evet depolama hesabını tüm kopyalarını şifrelenir ve tüm artıklık seçenekleri – yerel olarak yedekli depolama (LRS), bölge olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) – desteklenir.

**S: şifreleme depolama Hesabımı devre dışı bırakılamıyor.**

Y: şifreleme varsayılan olarak etkindir ve depolama hesabınız için şifrelemeyi devre dışı bırakmak için hiçbir sağlama yok. 

**S: SSE yalnızca belirli bölgelerine izin?**

Y: SSE tüm hizmetler için tüm bölgelerde kullanılabilir. 

**S: herhangi bir sorununuz veya geri bildirim sağlamak istiyorsanız nasıl ı biriyle iletişim?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) depolama hizmeti şifrelemesi ile ilgili sorunlar için.

## <a name="next-steps"></a>Sonraki Adımlar
Azure depolama birlikte geliştiricilerin güvenli uygulamalar oluşturmasını sağlama güvenlik özellikleri kapsamlı bir kümesini sağlar. Daha fazla ayrıntı için ziyaret [depolama Güvenlik Kılavuzu](../storage-security-guide.md).
