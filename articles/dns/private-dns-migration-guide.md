---
title: Eski Azure DNS özel bölgeleri yeni kaynak modeline geçirme
description: Bu kılavuz, eski özel DNS bölgelerini en son kaynak modeline geçiş yapmaya yönelik adım adım yönerge sağlar.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: rohink
ms.openlocfilehash: e7ebbf35cd572601f02a69930b58811686a92c86
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276100"
---
# <a name="migrating-legacy-azure-dns-private-zones-to-new-resource-model"></a>Eski Azure DNS özel bölgelerini yeni kaynak modeline geçirme

Biz, yeni bir API/kaynak modeli Azure DNS özel bölgeleri için Önizleme yenileme yayının parçası olarak birlikte gelir. Önizleme yenileme yeni işlevsellik sağlar ve bazı sınırlamalar ve kısıtlamalar ilk genel Önizleme kaldırır. Ancak, bu avantajlar eski API'si kullanılarak oluşturulan bir özel DNS bölgelerini üzerinde mevcut değildir. Yeni sürümün avantajlarından yararlanabilmek için eski bir özel DNS bölgesi kaynakları yeni kaynak modeline geçirmeniz gerekir. Geçiş işlemi basittir ve bu işlemi otomatikleştirmek için bir PowerShell komut dosyası sağladık. Bu kılavuz, Azure DNS özel bölgelerini yeni kaynak modeline geçirme adım adım yönerge sağlar.

## <a name="prerequisites"></a>Önkoşullar

Azure PowerShell'in en son sürümünü yüklediğinizden emin olun. Daha fazla bilgi için Azure PowerShell (Az) ve nasıl yükleneceği ziyaret edin https://docs.microsoft.com/powershell/azure/new-azureps-module-az

Az.PrivateDns modülü Azure PowerShell'in için açtığınızdan emin olun. Bu modülü yüklemek için yükseltilmiş bir PowerShell penceresi (Yönetici modu) açın ve komutu girin

```powershell
Install-Module -Name Az.PrivateDns -AllowPrerelease
```

>[!IMPORTANT]
>Geçiş işlemi tamamen otomatik ve herhangi bir kesintiye neden olmayan bekleniyor. Ancak, bir kritik üretim ortamında Azure DNS özel bölgeleri (Önizleme) kullanıyorsanız, geçiş işlemi planlı bakım zaman penceresi boyunca çalıştırılmalıdır. Geçiş öncesinde bir betik gerçekleştirdiğinizde yapılandırma veya bir özel DNS bölgelerini kayıt kümelerini değiştirmemeniniz emin olun.

## <a name="installing-the-script"></a>Komut dosyası yükleme

Bir yükseltilmiş bir PowerShell penceresi (Yönetici modu) ve aşağıdaki komutu Çalıştır'ı açın

```powershell
install-script PrivateDnsMigrationScript
```

Komut yüklemek isteyip istemediğiniz sorulduğunda "A" girin

![Komut dosyası yükleme](./media/private-dns-migration-guide/install-migration-script.png)

PowerShell Betiği en son sürümünü el ile de edinebilirsiniz https://www.powershellgallery.com/packages/PrivateDnsMigrationScript

## <a name="running-the-script"></a>Betik çalıştırma

Betiği çalıştırmak için komutu yürütün

```powershell
PrivateDnsMigrationScript.ps1
```

![Betik çalıştırma](./media/private-dns-migration-guide/running-migration-script.png)

### <a name="enter-the-subscription-id-and-sign-in-to-azure"></a>Abonelik Kimliğini girin ve Azure'da oturum açın

Abonelik kimliği, geçirmek istediğiniz özel DNS bölgelerini içeren girmeniz istenir. Azure hesabınızda oturum açmanız istenir. Oturum açma betiği abonelik özel DNS bölgesi kaynaklarına erişebilmesi tamamlayın.

![Azure'da oturum açma](./media/private-dns-migration-guide/login-migration-script.png)

### <a name="select-the-dns-zones-you-want-to-migrate"></a>Geçirmek istediğiniz DNS bölgeleri seçin

Komut dosyasını, Abonelikteki tüm özel DNS bölgelerini listesini almak ve hangilerinin geçirmek istediğiniz onaylamanızı ister. Tüm özel DNS bölgelerini geçirmek için "A" girin. Bu adım gerçekleştirildikten sonra komut yeni kaynak modeli kullanarak yeni özel DNS bölgelerini oluşturup yeni DSN bölgeye verileri kopyalayın. Bu adım, mevcut özel DNS bölgeleri seçemiyorsunuz yine de değiştirmez.

![DNS bölgeleri seçin](./media/private-dns-migration-guide/migratezone-migration-script.png)

### <a name="switching-dns-resolution-to-the-new-dns-zones"></a>DNS çözümlemesi için yeni DNS bölgelerini değiştirme

Bölgeleri ve kayıtları yeni kaynak modeline kopyalandıktan sonra betik yeni DNS bölgeleri için DNS çözümlemesi geçiş yapmanızı ister. Bu adım eski özel DNS bölgelerini ve, sanal ağlar arasındaki ilişkiyi kaldırır. Eski bölge sanal ağdan bağlantısı kaldırıldığında, yukarıda oluşturulan yeni DNS bölgelerini DNS çözümlemesi için bu sanal ağları üzerinden otomatik olarak sürecektir.

Tüm sanal ağları için DNS çözümlemesi geçiş yapmak için ' A' seçeneğini belirleyin.

![Geçiş ad çözümlemesi](./media/private-dns-migration-guide/switchresolution-migration-script.png)

### <a name="verify-the-dns-resolution"></a>DNS çözümlemesini doğrulayın

Devam etmeden önce DNS sunucusu DNS çözümlemesi beklendiği gibi çalıştığını doğrulayın. Azure Vm'lerinizi oturum ve bu DNS çözümlemesini doğrulamak için geçirilen bölgeleri sorunu nslookup sorgusunu çalışmaktadır.

![Ad çözümlemesini doğrulama](./media/private-dns-migration-guide/verifyresolution-migration-script.png)

DNS sorgu çözümleme olmayan bulursanız, birkaç dakika bekleyin ve sorguları yeniden deneyin. DNS sorguları beklendiği gibi çalışıyorsanız, betik özel DNS bölgesi sanal ağ kaldırmanızı ister 'Y' girin.

![Ad çözümlemesi onaylayın](./media/private-dns-migration-guide/confirmresolution-migration-script.png)

>[!IMPORTANT]
>Herhangi bir nedenle DNS nedeniyle geçirilen bölgeleri karşı çözümlemesi beklendiği gibi çalışmıyorsa girin, 'N' sayıda Yukarıdaki adımı ve betik eski bölgelere geri DNS çözümlemesi geçirir. Bir destek bileti oluşturun ve DNS bölgeleriniz yükseltmeye yardımcı olabiliriz.

## <a name="cleanup"></a>Temizleme

Bu adım, eski DNS bölgelerini siler ve yalnızca DNS çözümlemesi beklendiği gibi çalıştığını doğruladıktan sonra yürütülmelidir. Her özel DNS bölgesini silme istenir. 'Y', DNS çözümlemesi, bölgeler için düzgün çalıştığını doğruladıktan sonra her istemine girin.

![Temizleme](./media/private-dns-migration-guide/cleanup-migration-script.png)

## <a name="update-your-automation"></a>Otomasyon güncelleştirme

Şablonlar, PowerShell betikleri veya SDK'sını kullanarak geliştirilen özel kod gibi Otomasyon kullanıyorsanız, yeni kaynak modeli için özel DNS bölgelerini kullanmak için Otomasyon güncelleştirmeniz gerekir. Yeni özel DNS CLI'si/PS/SDK belgelerine bağlantılar aşağıda verilmiştir.
* [Azure DNS özel bölgeleri REST API](https://docs.microsoft.com/rest/api/dns/privatedns/privatezones)
* [Azure DNS özel bölgelerini CLI](https://docs.microsoft.com/cli/azure/ext/privatedns/network/private-dns?view=azure-cli-latest)
* [Azure DNS özel bölgelerini PowerShell](https://docs.microsoft.com/powershell/module/az.privatedns/?view=azps-2.3.2)
* [Azure DNS özel bölgelerini SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/privatedns/management?view=azure-dotnet-preview)

## <a name="need-further-help"></a>Daha fazla yardım gerekiyor

Geçiş işlemi ile daha fazla yardım edilmeli veya herhangi bir nedenden dolayı yukarıda listelenen adımları çalışmaz, bir destek bileti oluşturun. Destek biletiniz ile PowerShell betiği tarafından oluşturulan bir döküm dosyasını dahil edin.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanarak Azure DNS özel bölgesi oluşturmayı öğrenin [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md).

* Bazı ortak hakkında okuyun [özel bölge senaryoları](./private-dns-scenarios.md) Azure DNS'te özel bölgeler ile gerçekleştirilebilecek.

* Yaygın sorular ve yanıtlar hakkında Azure DNS özel bölgeleri için de dahil olmak üzere belirli bir davranışı, belirli türde operasyonlar beklediğiniz, bakın [özel DNS SSS](./dns-faq-private.md).

* DNS bölgeleri ve kayıtları hakkında ederek bilgi [DNS bölgeleri ve kayıtları genel bakış](dns-zones-records.md).

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
