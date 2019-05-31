---
title: FSLogix profili kapsayıcılar ve Windows sanal masaüstü - Azure, Azure dosyaları
description: Bu makalede, Windows sanal masaüstü ve Azure dosyaları FSLogix profili kapsayıcılara açıklanır.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: v-chjenk
ms.openlocfilehash: c3f31e8d260ea5e462e8782fadd9f61f34d03add
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307280"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>FSLogix profil kapsayıcıları ve Azure dosyaları

Windows sanal masaüstü Önizleme hizmeti bir kullanıcı profili çözümü FSLogix profili kapsayıcıları önerir. Windows sanal masaüstü gibi uzak bilgi işlem ortamlarının profillerinde gerekmeden FSLogix tasarlanmıştır. Bu, tek bir kapsayıcıda tam kullanıcı profili depolar. Oturum sırasında kapsayıcı yerel, Konuk Sanal Sabit Disk (VHD) ve Hyper-V sanal sabit disk (VHDX) Microsoft hizmetlerini kullanarak bilgi işlem ortamınız için dinamik olarak eklenir. Kullanıcı profili hemen kullanılabilir ve sistemi tam olarak bir yerel kullanıcı profili gibi görünür.

Bu makalede, Azure dosyaları ile kullanılan FSLogix profili kapsayıcıları açıklayacağız. Windows sanal masaüstü, bağlamında bilgilerdir [3/21'duyurulan](https://www.microsoft.com/microsoft-365/blog/2019/03/21/windows-virtual-desktop-public-preview/).

## <a name="user-profiles"></a>Kullanıcı profilleri

Kullanıcı profili hakkında masaüstü ayarlarını kalıcı ağ bağlantıları ve uygulama ayarları gibi yapılandırma bilgilerini de dahil olmak üzere bireysel veri öğeleri içerir. Varsayılan olarak, Windows işletim sistemiyle tümleşiktir bir yerel kullanıcı profili oluşturur.

Kullanıcı verileri ve işletim sistemi arasında bir bölümü bir uzak kullanıcı profili sağlar. İşletim sisteminin değiştirilmesi veya kullanıcı verilerini etkileyen olmadan değiştirilen sağlar. Uzak Masaüstü oturumu ana bilgisayarı (RDSH) ve sanal masaüstü altyapıları (VDI), işletim sistemi aşağıdaki nedenlerden dolayı değiştirilebilir:

- İşletim sistemi yükseltmesi
- Bir mevcut sanal makine (VM) değiştirme
- Bir havuza alınmış (kalıcı) RDSH veya VDI ortamının bir parçası olan bir kullanıcı

Microsoft ürünleri, uzak kullanıcı profilleri, bu teknolojiler de dahil olmak üzere çeşitli teknolojiler ile çalışır:
- Gezici kullanıcı profilleri (RUP'ye)
- Kullanıcı profili diskleri (UDP)
- Kurumsal durumda dolaşım (havale)

UPD ve RUP'ye kullanıcı profillerini Uzak Masaüstü oturumu ana bilgisayarı (RDSH) ve Sanal Sabit Disk (VHD) ortamlarda en yaygın olarak kullanılan teknolojileridir.

### <a name="challenges-with-previous-user-profile-technologies"></a>Önceki kullanıcı profili teknolojileriyle zorlukları

Mevcut ve eski Microsoft çözümleri kullanıcı profilleri için çeşitli zorluklar geldi. Önceki çözümü RDSH veya VDI ortamıyla gelen tüm kullanıcı profili ihtiyaçlarınızı işlenir. Örneğin, UPD büyük OST dosyalar işlenemez ve RUP'ye modern ayarları devam etmez.

#### <a name="functionality"></a>İşlevi

Aşağıdaki tablo, avantajları ve sınırlamaları önceki kullanıcı profili teknolojilerinin gösterir.

| Teknoloji | Modern ayarları | Win32 ayarları | İşletim sistemi ayarları | Kullanıcı verileri | Sunucu SKU'SUNDA desteklenmiyor | Azure'da arka uç depolama alanı | Arka uç depolama şirket içi | Sürüm desteği | Sonraki oturum açma zamanı |Notlar|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Kullanıcı profili diskleri (UDP)** | Evet | Evet | Evet | Evet | Evet | Hayır | Evet | 7 + win | Evet | |
| **Bakım modu gezici kullanıcı profili (RUP'ye)** | Hayır | Evet | Evet | Evet | Evet| Hayır | Evet | 7 + win | Hayır | |
| **Kurumsal durumda dolaşım (havale)** | Evet | Hayır | Evet | Hayır | Notlara bakın | Evet | Hayır | Win 10 | Hayır | Sunucu SKU ancak destekleyen herhangi bir kullanıcı arabirimi işlevleri |
| **Kullanıcı deneyimi sanallaştırma (UE-V)** | Evet | Evet | Evet | Hayır | Evet | Hayır | Evet | 7 + win | Hayır |  |
| **OneDrive bulut dosyaları** | Hayır | Hayır | Hayır | Evet | Notlara bakın | Notlara bakın  | Notlara bakın | Win 10 RS3 | Hayır | Sunucu SKU'SUNDA test yok. Azure'da arka uç depolama eşitleme istemcide bağlıdır. Arka uç depolama şirket içi eşitleme istemcisi gerekir. |

#### <a name="performance"></a>Performans

UPD gerektirir [depolama alanları doğrudan (S2D)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) adres performans gereksinimleri için. UPD sunucu ileti bloğu (SMB) protokolünü kullanır. Bu profili hangi kullanıcının oturumu VM'ye kopyalar. S2D ile UPD RDS takım hizmetinin önizlemesi sırasında sanal Windows Masaüstü için önerilen çözümü oldu.  

#### <a name="cost"></a>Maliyet

S2D kümeleri gerekli performansı elde ederken Kurumsal müşteriler için pahalı, ancak özellikle küçük ve orta ölçekli işletme (SMB) müşteriler için pahalı maliyetidir. Bu çözüm için işletmelerin premium depolama diskleri, bir paylaşım için diskleri kullanan sanal makinelerinin maliyeti için ödeme yaparsınız.

#### <a name="administrative-overhead"></a>Yönetim yükü

S2D kümeleri yama, güncelleştirilen ve güvenli bir durumda tutulan bir işletim sistemi gerektirir. Bu işlemler ve S2D olağanüstü durum kurtarma ayarlama karmaşıklığı S2D yalnızca adanmış bir BT personeli ile kuruluşlar için uygun hale getirir.

## <a name="fslogix-profile-containers"></a>FSLogix profili kapsayıcıları

19 Kasım 2018 [Microsoft alınan FSLogix](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). Birçok profil kapsayıcı zorlukları, bunlar arasında anahtar FSLogix adresler şunlardır:

- **Performans:** [FSLogix profili kapsayıcıları](https://fslogix.com/products/profile-containers) , yüksek performanslı, ve geçmişe yönelik olarak engellediğiniz performans sorunları çözüp önbelleğe alınmış exchange modu.
- **OneDrive:** FSLogix profili kapsayıcılar, OneDrive iş kalıcı olmayan RDSH veya VDI ortamlarında desteklenmez. [OneDrive iş ve FSLogix en iyi uygulamaları](https://fslogix.com/products/technical-faqs/284-onedrive-for-business-and-fslogix-best-practices) nasıl etkileşim kurduklarını açıklar. Daha fazla bilgi için [eşitleme istemcisi sanal masaüstlerini kullanma](https://docs.microsoft.com/deployoffice/rds-onedrive-business-vdi).
- **Ek klasörler:** FSLogix ek klasörleri eklemek için kullanıcı profillerini genişletme olanağı sağlar.

Alım UPD gibi mevcut kullanıcı profili çözümlerini FSLogix profili kapsayıcılar ile değiştirerek Microsoft başlatıldı.

## <a name="azure-files-integration-with-azure-active-directory"></a>Azure Active Directory ile Azure dosyaları tümleştirmesi

FSLogix profili kapsayıcı performansı ve özellikleri, bulutun avantajından yararlanın. 24 Eylül 2018'de Microsoft Azure dosyaları genel önizlemesi Duyuruldu [Azure Active Directory kimlik doğrulamasını destekleyen Azure dosyaları](https://azure.microsoft.com/blog/azure-active-directory-integration-for-smb-access-now-in-public-preview/). Hem maliyeti hem de yönetim yükünü yönlendirerek, Azure Active Directory kimlik doğrulaması ile Azure dosyaları bir premium yeni Windows sanal masaüstü hizmetindeki kullanıcı profilleri için çözümüdür.

## <a name="best-practices-for-windows-virtual-desktop"></a>Windows sanal masaüstü için en iyi uygulamalar

Windows sanal masaüstü boyutu, türü ve müşteriler tarafından kullanılan VM sayısı üzerinde tam denetim sağlar. Daha fazla bilgi için [Windows sanal masaüstü önizlemesi nedir?](https://docs.microsoft.com/azure/virtual-desktop/overview).

Windows sanal masaüstü emin olmak için en iyi ortam aşağıdaki gibidir:

- Azure dosya depolama hesabında oturum ana bilgisayarının Vm'leri aynı bölgede olması gerekir.
- Azure dosyaları izinleri açıklanan izinleri eşleşmelidir [gereksinimleri - profili kapsayıcıları](https://docs.fslogix.com/display/20170529/Requirements+-+Profile+Containers).
- Her konak havuz aynı türde ve aynı ana görüntü temel alan VM boyutu oluşturulmalıdır.
- Her konak havuzu sanal makinesi, ölçeklendirme ve güncelleştirme yönetimi, yardımcı olmak için aynı kaynak grubunda olmalıdır.
- En iyi performans için depolama çözümü ve profili kapsayıcı içindeki aynı verilere olmalıdır FSLogix konumu ortalayın.
- Ana görüntü içeren depolama hesabını aynı bölge ve abonelik Vm'leri burada sağlanan olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bir Windows sanal masaüstü ortamını ayarlamak için aşağıdaki yönergeleri kullanın.

- Masaüstü Sanallaştırma çözümünüzü oluşturmaya başlamak için bkz [Windows sanal masaüstü bir kiracı oluşturmanız](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory).
- Windows sanal masaüstü kiracınız içindeki bir konak havuzu oluşturmak için bkz: [Azure Marketi ile konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace).
- Tam olarak yönetilen ayarlamak için bulutta dosya paylaşımları için bkz: [Azure Dosya Paylaşımı'kurmak](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable).
- FSLogix profili kapsayıcıları yapılandırmak için bkz [ana makine havuzu için bir kullanıcı profili paylaşımını ayarlama](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile).
- Kullanıcılar bir ana havuzuna atamak için bkz: [sanal Windows Masaüstü için uygulama gruplarını yönetme](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups).
- Bir web tarayıcısından Windows sanal masaüstü kaynaklarınıza erişmek için bkz: [Windows sanal masaüstüne bağlanma](https://docs.microsoft.com/azure/virtual-desktop/connect-web).
