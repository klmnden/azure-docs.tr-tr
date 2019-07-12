---
title: Azure geçişi Gereci mimarisi | Microsoft Docs
description: Azure geçişi Gereci genel bir bakış sağlar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: c2c9ca3082aa9c2067a63f8d6304e8a229dac14a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811459"
---
# <a name="azure-migrate-appliance"></a>Azure geçişi Gereci

Bu makalede, Azure geçişi Gereci açıklanır. Keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için Azure geçişi değerlendirmesi ve Geçiş Araçları'nı kullandığınızda, gereç dağıtın. 

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve private/public bulut Vm'lerinden azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.



## <a name="appliance-overview"></a>Gereç genel bakış

Azure geçişi Gereci türleri ve kullanım aşağıdaki gibidir.

**Olarak dağıtılan** | **İçin kullanılan** | **Ayrıntılar**
--- | --- |  ---
VMware VM | Azure geçişi değerlendirmesi aracı ile VMware VM değerlendirmesi.<br/><br/> Azure geçişi Server Geçiş Aracı ile VMware VM aracısız geçiş | OVA şablonu indirin ve vCenter Server'a VM Gereci oluşturmak için içeri aktarın.
Hyper-V VM | Azure geçişi değerlendirmesi aracı ile Hyper-V VM değerlendirmesi. | Sıkıştırılmış VHD'yi indirin ve Hyper-VM Gereci oluşturmak için V içeri aktarın.

## <a name="appliance-access"></a>Gereç erişim

Gereç yapılandırdıktan sonra VM Gereci uzaktan TCP bağlantı noktası 3389 erişebilirsiniz. Web yönetimi uygulaması gerecinde 44368 URL'siyle bağlantı noktası için uzaktan da erişebilirsiniz: ``` https://<appliance-ip-or-name>:44368 ```.

## <a name="appliance-license"></a>Gereç lisans
Gereç, 180 gün boyunca geçerli bir Windows Server 2016 değerlendirme lisansı ile birlikte gelir. Değerlendirme süresi sona erme yakın ise, indirin ve yeni Gereci dağıtmadan veya gerecinin VM işletim sistemi lisans etkinleştirmenizi öneririz.

## <a name="appliance-agents"></a>Gereç aracıları
Gereç, bu aracıları yüklü sahiptir.

**Aracı** | **Ayrıntılar**
--- | ---
Keşif Aracısı | Şirket içi Vm'lerden yapılandırma verilerini toplar.
Değerlendirme Aracı | VM performans verilerini toplamak için profil şirket içi Ortamı'nı tıklatın.
Geçiş ağ bağdaştırıcısı | VM çoğaltmayı yönetir ve VM'ler ile Azure arasındaki iletişimi düzenler.
Geçiş ağ geçidi | Azure'a çoğaltılan VM verileri gönderir.


## <a name="appliance-deployment-requirements"></a>Gereç dağıtım gereksinimleri

- [Gözden geçirme](migrate-support-matrix-vmware.md#assessment-appliance-requirements) VMware Gereci ve gerecin erişmesi gereken URL'leri dağıtım gereksinimleri.
- [Gözden geçirme](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) bir Hyper-V Gereci ve gerecin erişmesi gereken URL'leri ilişkin dağıtım gereksinimleri.


## <a name="collected-performance-data-vmware"></a>Toplanan performans verileri-VMware

Gereç toplar ve Azure'a gönderilmeden VMware VM performans verileri aşağıda verilmiştir.

**Veri** | **Counter** | **Etki değerlendirmesi**
--- | --- | ---
CPU kullanımı | cpu.usage.average | Önerilen boyut/maliyet VM
Bellek kullanımı | mem.Usage.average | Önerilen boyut/maliyet VM
Disk okuma aktarım hızı (MB / saniye) | virtualDisk.read.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
Disk yazma üretimi (MB / saniye) | virtualDisk.write.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
Disk okuma işlemi / saniye | virtualDisk.numberReadAveraged.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
Disk yazma işlemi / saniye | virtualDisk.numberWriteAveraged.average  | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
NIC okuma aktarım hızı (MB / saniye) | NET.Received.average | VM boyutu için hesaplama
NIC yazma aktarım hızı (MB / saniye) | NET.transmitted.average  |VM boyutu için hesaplama


## <a name="collected-metadata-vmware"></a>Toplanan meta verileri VMware

Gereç toplayan ve Azure'a gönderilmeden VMware VM meta verilerini tam listesi sunulmaktadır.

**Veri** | **Counter**
--- | --- 
**Makine ayrıntıları** | 
VM Kimliği | VM. Config.InstanceUuid 
VM adı | VM. Config.Name
vCenter sunucusu kimliği | VMwareClient.Instance.Uuid
VM açıklaması | VM. Summary.Config.Annotation
Lisans ürün adı | VM. Client.ServiceContent.About.LicenseProductName
İşletim sistemi türü | vm.SummaryConfig.GuestFullName
Önyükleme türü | VM. Config.Firmware
Çekirdek sayısı | vm.Config.Hardware.NumCPU
Bellek (MB) | VM. Config.Hardware.MemoryMB
Disk sayısı | VM. Config.Hardware.Device.ToList(). FindAll(x => is VirtualDisk).count
Disk boyutu listesi | VM. Config.Hardware.Device.ToList(). FindAll (x = > VirtualDisk olan)
Ağ bağdaştırıcılarının listesi | VM. Config.Hardware.Device.ToList(). FindAll(x => is VirtualEthernet).count
CPU kullanımı | cpu.usage.average
Bellek kullanımı |mem.Usage.average
**Disk ayrıntıları** | 
Disk anahtar değeri | disk. Anahtarı
Dikunit numarası | disk.UnitNumber
Disk denetleyici anahtar değeri | disk. ControllerKey.Value
Sağlanan gigabayt | virtualDisk.DeviceInfo.Summary
Disk adı | Disk kullanılarak oluşturulan değeri. UnitNumber, disk. Anahtar, disk. ControllerKey.VAlue
Saniye başına okuma işlemleri | virtualDisk.numberReadAveraged.average
Yazma işlemi / saniye | virtualDisk.numberWriteAveraged.average
Okuma aktarım hızı (MB / saniye) | virtualDisk.read.average
Aktarım hızı (MB / saniye) yazma | virtualDisk.write.average
**NIC başına ayrıntıları** | 
Ağ bağdaştırıcısı adı | NIC Anahtarı
MAC adresi | ((VirtualEthernetCard)nic).MacAddress
IPv4 adresleri | vm.Guest.Net
IPv6 adresleri | vm.Guest.Net
Okuma aktarım hızı (MB / saniye) | NET.Received.average
Aktarım hızı (MB / saniye) yazma | NET.transmitted.average
**Envanteri yolu ayrıntıları** | 
Ad | kapsayıcı. GetType(). Adı
Alt nesne türü | kapsayıcı. ChildType
Başvuru ayrıntıları | kapsayıcı. MoRef
Üst ayrıntıları | Container.Parent
VM başına klasör ayrıntıları | ((Klasör) kapsayıcısı). ChildEntity.Type
VM başına veri merkezi ayrıntıları | ((Veri merkezi) kapsayıcısı). VmFolder
Ana klasörü başına veri merkezi ayrıntıları | ((Veri merkezi) kapsayıcısı). HostFolder
Konak başına küme ayrıntıları | ((ClusterComputeResource) kapsayıcısı). Ana bilgisayar
VM başına ana bilgisayar ayrıntıları | ((HostSystem) kapsayıcısı). VM



## <a name="collected-performance-data-hyper-v"></a>Toplanan performans verileri Hyper-V

Gereç toplar ve Azure'a gönderilmeden VMware VM performans verileri aşağıda verilmiştir.

**Performans sayacı sınıfı** | **Counter** | **Etki değerlendirmesi**
--- | --- | ---
Hyper-V hiper yönetici sanal işlemci | % Konuk çalışma zamanı | Önerilen boyut/maliyet VM
Hyper-V dinamik belleği VM | Geçerli baskı (%)<br/> Konuk görünür fiziksel bellek (MB) | Önerilen boyut/maliyet VM
Hyper-V sanal depolama aygıtı | Okuma Bayt/saniye | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
Hyper-V sanal depolama aygıtı | Yazma Bayt/saniye | Disk boyutu, depolama maliyeti, VM boyutunu hesaplama
Hyper-V sanal ağ bağdaştırıcısı | Saniye başına alınan bayt | VM boyutu için hesaplama
Hyper-V sanal ağ bağdaştırıcısı | Saniye başına gönderilen bayt | VM boyutu için hesaplama

- CPU kullanımı, bir VM'ye bağlı tüm sanal işlemciler için tüm kullanımlar toplamıdır.
- Bellek kullanımı (geçerli baskı * Konuk görünür fiziksel bellek) / 100.
- Disk ve ağ kullanımı değerleri listelenen Hyper-V performans sayaçlarını toplanır.

## <a name="collected-metadata-hyper-v"></a>Toplanan meta verileri Hyper-V

Gereç toplayan ve Azure'a gönderilmeden Hyper-V VM meta verilerini tam listesi sunulmaktadır.

**Veri** | **WMI sınıfı** | **WMI sınıf özelliği**
--- | --- | ---
**Makine ayrıntıları** | 
BIOS _ Msvm_BIOSElement seri numarası | BIOSSerialNumber
VM türü (Gen 1 veya 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
VM görüntü adı | Msvm_VirtualSystemSettingData | ElementName
VM sürümü | Msvm_ProcessorSettingData | VirtualQuantity
Bellek (bayt) | Msvm_MemorySettingData | VirtualQuantity
VM tarafından kullanılabilecek maksimum bellek miktarı | Msvm_MemorySettingData | Sınır
Dinamik bellek etkin | Msvm_MemorySettingData | DynamicMemoryEnabled
İşletim sistemi adı/sürümü/FQDN | Msvm_KvpExchangeComponent | GuestIntrinsciExchangeItems Name Data
Sanal Makinenin güç durumu | Msvm_ComputerSystem | EnabledState
**Disk ayrıntıları** | 
Disk tanımlayıcısı | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Sanal sabit disk türü | Msvm_VirtualHardDiskSettingData | Type
Sanal sabit disk boyutu | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Üst sanal sabit disk | Msvm_VirtualHardDiskSettingData | ParentPath
**NIC başına ayrıntıları** | 
IP adresleri (yapay NIC) | Msvm_GuestNetworkAdapterConfiguration | IP adresi içermiyor
DHCP etkin (yapay NIC) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
NIC kimliği (yapay NIC) | Msvm_SyntheticEthernetPortSettingData | InstanceId
NIC MAC adresi (yapay NIC) | Msvm_SyntheticEthernetPortSettingData | Adres
NIC kimliği (eski NIC) | MsvmEmulatedEthernetPortSetting Data | InstanceId
NIC MAC kimliği (eski NIC) | MsvmEmulatedEthernetPortSetting Data | Adres




## <a name="discovery-and-collection-process"></a>Bulma ve toplama işlemi

Gereci vCenter sunucuları ve Hyper-V konak/küme aşağıdaki işlemi kullanarak iletişim kurar.


1. **Bulmayı Başlat**:
    - Hyper-V gerecinde bulma başlattığınızda, Hyper-V konaklarda WinRM bağlantı noktası 5985'tir (HTTP) ve 5986 (HTTPS) ile iletişim kurar.
    - VMware gerecinde bulma başlattığınızda, varsayılan olarak TCP bağlantı noktası 443 üzerinden vCenter sunucusu ile iletişim kurar. VCenter sunucusu farklı bir bağlantı noktasında DİNLİYORSA, gereç web uygulamasında yapılandırabilirsiniz.
2. **Meta veri ve performans verilerini toplamak**:
    - Gereç, Hyper-V VM veri 5985'tir ve 5986 bağlantı noktaları Hyper-V ana bilgisayarından toplamak için bir ortak bilgi modeli (CIM) oturumu kullanır.
    - Gereç, vCenter Server VMware VM veri toplamak üzere varsayılan olarak 443 numaralı bağlantı noktası ile iletişim kurar.
3. **Veri Gönder**: Gereç toplanan verileri Azure geçişi Server değerlendirmesi ve Azure geçişi sunucusu geçişi için SSL bağlantı noktası 443 üzerinden gönderir.
    - Performans verileri için gereç gerçek zamanlı kullanım verileri toplar.
        - Performans verileri, her 20 saniyede VMware ve Hyper-v, her 30 saniyede her performans ölçümü için toplanır.
        - Tek bir veri noktasının on dakika boyunca oluşturmak için toplanan verileri toplanır.
        - En yüksek kullanımı değerinin 20 tümünden seçili/30 saniye veri noktaları ve değerlendirme hesaplama için gönderilen.
        - Yüzdelik dilim değeri (50. / 90'ıncı / 95/99th) değerlendirme özelliklerinde belirtilen bağlı olarak, on dakikalık puanları artan düzende sıralanır ve uygun yüzdelik dilim değeri değerlendirmeyi hesaplamak için kullanılır
    - Sunucu geçiş için Gereci VM veri toplamaya başlar ve Azure'a çoğaltır.
4. **Değerlendirmek ve geçirmek**: Şimdi, Azure geçişi Server değerlendirmesi'ni kullanarak Gereci tarafından toplanan meta verilerden değerlendirmeler oluşturabilirsiniz. Ayrıca, aracısız VM çoğaltmayı düzenlemek için Azure geçişi sunucusu geçişi kullanan VMware Vm'lerini geçiş başlatabilirsiniz.


![Mimari](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç üzerinde çalışan Azure geçişi aracılarını güncelleştirildikçe gereç yükseltilir.

- Otomatik güncelleştirme gerecinde varsayılan olarak etkin olmadığından bu otomatik olarak gerçekleşir.
- Aracıları el ile güncelleştirmek için bu varsayılan ayarı değiştirebilirsiniz.
- Otomatik Güncelleştirmeler devre dışı bırakmak için HKLM\SOFTWAREMicrosoft\Azure Gereci otomatik güncelleştirmeler, kayıt defteri anahtarı ayarlayın.

### <a name="set-agent-updates-to-manual"></a>Aracı güncelleştirmeleri el ile olarak ayarlandı.

El ile güncelleştirmeler için aynı anda aygıtınızdaki tüm aracıları güncelleştir emin kullanarak **güncelleştirme** gereçte güncel olmayan her aracı için düğmesi. Güncelleştirme ayarlarını geri otomatik güncelleştirmeler dilediğiniz zaman geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](tutorial-assess-vmware.md#set-up-the-appliance-vm) VMware için gerecini ayarlamak için.
[Bilgi nasıl](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) gerecini ayarlamak için Hyper-V için.

