---
title: "Klasik Azure portalı için DPM iş yüklerini yedekleme | Microsoft Docs"
description: "Azure Yedekleme hizmetini kullanarak DPM sunucularını yedekleme için bir giriş"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "System Center Data Protection Manager, veri koruma Yöneticisi, dpm yedekleme"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cwatson
ms.openlocfilehash: b0b6fe4a0b667f4d777bfc5c12589bae3ef58629
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>DPM ile Azure’a iş yüklerini yedeklemeye hazırlama
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup sunucusu (Klasik)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (Klasik)](backup-azure-dpm-introduction-classic.md)
>
>

Bu makalede, System Center Data Protection Manager (DPM) sunucuları ve iş yüklerini korumak için Microsoft Azure Yedekleme kullanılarak giriş bilgileri sağlar. Bunu okuyarak, anlamanız:

* Azure DPM sunucusu yedek nasıl çalışır
* Kesintisiz bir yedekleme deneyimi elde etmek için Önkoşullar
* Tipik hatalarla karşılaşıldı ve bunlarla ilgili Yapılacaklar
* Desteklenen senaryolar

System Center DPM dosya ve uygulama verileri yedekler. DPM için yedeklenen verileri diskte, bantta depolanan veya Microsoft Azure yedekleme ile azure'a yedeklenebilir. DPM Azure Backup ile aşağıdaki gibi etkileşim kurar:

* **Fiziksel sunucu veya şirket içi sanal makine olarak dağıtılan DPM** — varsa DPM fiziksel sunucu olarak veya geri verileri bir Azure yedekleme kasasına disk ve bant yanı sıra bir şirket içi Hyper-V sanal makinesi olarak dağıtılan yedekleme.
* **Azure sanal makinesi olarak dağıtılan DPM** — güncelleştirme 3 ile System Center 2012 R2'den DPM Azure sanal makinesi olarak dağıtılabilir. DPM Azure disklere verileri yedekleyebilirsiniz bir Azure sanal makinesi olarak dağıtılırsa DPM Azure sanal makinesine bağlı veya bir Azure yedekleme kasası kadar yedekleyerek veri depolama boşaltabilir.

## <a name="why-backup-your-dpm-servers"></a>Neden DPM sunucularını yedekleme?
DPM sunucularını yedekleme için Azure Yedekleme'yi kullanarak iş avantajları şunlardır:

* Şirket içi DPM dağıtımı için banda uzun vadeli dağıtım alternatif olarak Azure Yedekleme'yi kullanabilirsiniz.
* Azure'da DPM dağıtımları için Azure yedekleme depolama Azure diskten boşaltmak eski verileri Azure yedekleme ve disk üzerindeki yeni verileri depolayarak ölçekleme yapmanıza olanak sağlar.

## <a name="how-does-dpm-server-backup-work"></a>DPM sunucusu yedek nasıl çalışır?
Bir sanal makineyi yedeklemek için önce bir zaman içinde nokta verilerin anlık görüntüsünü gereklidir. Azure Backup hizmeti yedekleme işi zamanlanan saatte başlatır ve bir anlık görüntüyü almaya yedekleme uzantısını tetikler. Backup uzantısı tutarlılık elde etmek için konuk VSS hizmetiyle düzenler ve tutarlılık sınıra ulaşıldıktan sonra Azure depolama hizmetinin blob anlık görüntü API çağırır. Bu, kapatmak zorunda kalmadan sanal makinenin disklerinin tutarlı bir anlık görüntü almak için gerçekleştirilir.

Anlık görüntü alındıktan sonra verileri Azure yedekleme hizmeti tarafından yedekleme Kasası'na aktarılır. Hizmet tanımlama ve yedeklemelerin depolama ve ağ verimli hale son yedeklemeden değişen blokları aktarma ilgilenir. Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur. Bu kurtarma noktasını Azure Klasik Portalı'nda görülebilir.

> [!NOTE]
> Linux sanal makineleri için yalnızca dosyayla tutarlı yedekleme mümkündür.
>
>

## <a name="prerequisites"></a>Ön koşullar
Azure yedekleme DPM verileri yedeklemek gibi için hazırlayın:

1. **Bir yedekleme kasası oluşturma**. Bir yedekleme kasası, aboneliğinizde oluşturmadıysanız, bu makalenin - Azure portal sürümü bkz [Azure ile DPM iş yüklerini yedeklemek üzere hazırlamak](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
  > Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 30 Kasım 2017 sonra yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz. **30 Kasım 2017 tarafından**:
  >- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
  >- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
  >

2. **Kasa kimlik bilgilerini indirme** — Azure Backup'ta oluşturduğunuz yönetim sertifikasını kasaya yükleyin.
3. **Azure yedekleme Aracısı'nı yükleyin ve sunucuyu kaydetmek** — gelen Azure yedekleme, her DPM sunucusunda aracıyı yükleyin ve DPM sunucusu yedek kasaya kaydedin.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Gereksinimleri (ve sınırlamaları)
* Bir fiziksel sunucuda veya System Center 2012 SP1 veya System Center 2012 R2'de yüklü bir Hyper-V sanal makinesi olarak DPM çalıştırıyor olabilir. System Center 2012 R2 ile en az çalışan Azure sanal makinesi olarak da çalışabilir DPM 2012 R2 güncelleştirme paketi 3 veya System Center 2012 R2 ile en az çalışan vmware'deki Windows sanal makine Güncelleştirme Paketi 5.
* System Center 2012 SP1 ile DPM çalıştırıyorsanız, System Center Data Protection Manager SP1 için Güncelleştirme Paketi 2 yüklemeniz gerekir. Azure yedekleme Aracısı'nı yükleyebilmek için önce bu gereklidir.
* DPM sunucusunda Windows PowerShell ve .net olması Framework 4.5 yüklü.
* DPM çoğu iş yüklerini Azure Yedekleme'de yedekleyebilir. Bkz: desteklenen sahip bir tam listesi için Azure Backup aşağıdaki öğeleri destekler.
* Azure Yedekleme'de depolanan verileri "banda Kopyala" seçeneğiyle kurtarılamıyor.
* Azure Backup özelliği etkinleştirilmiş bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Hakkında bilgi edinin [Azure yedekleme fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/).
* Azure Yedekleme'yi kullanarak Azure Backup aracısının yedeklemek istediğiniz sunucularda yüklü olması gerekir. Her sunucunun yerel boş depolama olarak kullanılabilir yedeklendiğinden verilerin boyutunu % 10'en az olmalıdır. Örneğin, 100 GB veri yedekleme 10 GB boş alan karalama konumda en az gerektirir. En az % 10 olsa da, % 15 boş yerel depolama alanı için önbellek konumu kullanılması önerilir.
* Veriler Azure kasası depolama alanında depolanır. Bir Azure yedekleme kadar kasa veri miktarı için bir sınır yoktur ancak (örneğin sanal makine ya da veritabanı) veri kaynağının boyutunu 54,400 GB aşan döndürmemelidir.

Bu dosya türlerini Azure'a yedekleme için desteklenir:

* Şifrelenmiş (yalnızca tam yedeklemeler)
* Sıkıştırılmış (artımlı yedeklemeler desteklenir)
* Aralıklı (artımlı yedeklemeler desteklenir)
* Sıkıştırılmış ve aralıklı (aralıklı olarak kabul edilir)

Ve bu desteklenmez:

* Büyük küçük harfe duyarlı dosya sistemlerindeki sunucular desteklenmez.
* Sabit bağlantılar (atlanır)
* Yeniden ayrıştırma noktaları (atlanır)
* Şifrelenmiş ve sıkıştırılmış (atlanır)
* Şifrelenmiş ve aralıklı (atlanır)
* Sıkıştırılmış akış
* Aralıklı akış

> [!NOTE]
> Gelen System Center 2012 DPM SP1 ile başlayarak, içinde Microsoft Azure Yedekleme kullanılarak Azure DPM tarafından korunan iş yüklerini yedekleme yapabilir.
>
>
