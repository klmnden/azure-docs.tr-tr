---
title: Azure yedekleme sunucusu ne yedekleme yapabilirsiniz | Microsoft Docs
description: "Bu makalede, tüm iş yükleri, veri türleri ve yüklemeleri Azure yedekleme sunucusu v2 koruyan listeleyen bir destek matrisi sağlar."
services: backup
documentation center: 
author: markgalioto
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
keywords: 
ms.date: 11/28/2017
ms.topic: article
ms.author: markgal,masaran
manager: carmonm
ms.openlocfilehash: 45e3e7e1288c9c468619bd553963cfd018298c32
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi

Bu makalede, çeşitli sunucular ve Azure yedekleme sunucusu ile Koruyabileceğiniz iş yükleri listelenmektedir. Aşağıdaki matris ne Azure yedekleme sunucusu v1 ve v2 ile korunabilir listeler.

## <a name="protection-support-matrix"></a>Koruması destek matrisi

|İş yükü|Sürüm|Azure Backup Sunucusu</br> installation|Azure Backup</br> Sunucu v2|Azure Backup</br> Sunucu v1 |Koruma ve kurtarma|
|------------|-----------|--------------------|--------------------------------------------|--------------------------------|---------------------------|
|System Center VMM|VMM 2016<br/>VMM 2012 SP1, R2|Fiziksel sunucu<br /><br />Hyper-V sanal makine|E|E|Tüm dağıtım senaryolarında: veritabanı|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 10|Fiziksel sunucu<br /><br />Hyper-V sanal makine<br /><br />VMware sanal makinesi|E|E|Dosyalar<br /><br />Korumalı birimler NTFS olmalıdır. FAT ve FAT32 desteklenmez.<br /><br />Birimler, en az 1 GB olmalıdır. DPM, veri anlık görüntüsünü almak için Birim Gölge Kopyası Hizmeti (VSS) kullanır ve anlık görüntü yalnızca birim en az 1 GB ise çalışır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 8.1|Fiziksel sunucu<br /><br />Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS olmalıdır. FAT ve FAT32 desteklenmez.<br /><br />Birimler, en az 1 GB olmalıdır. DPM, veri anlık görüntüsünü almak için Birim Gölge Kopyası Hizmeti (VSS) kullanır ve anlık görüntü yalnızca birim en az 1 GB ise çalışır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 8.1|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 8|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 8|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 7|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows 7|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows Vista SP2|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows Vista SP1|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows Vista|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Dosyalar<br /><br />Korumalı birimler NTFS ile en az 1 GB olmalıdır.|
|İstemci bilgisayarlar (64-bit ve 32-bit)|Windows Vista|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma), yinelenenleri kaldırılan birimler|
|Sunucular (32 bit ve 64 bit)|Windows Server 2016|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi<br /><br />Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak<br /><br />Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E<br /><br />Nano server|N|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma), yinelenenleri kaldırılan birimler|
|Sunucular (32 bit ve 64 bit)|Windows Server 2012 R2 - Datacenter ve Standard|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E |Birim, paylaşım, klasör, dosya<br /><br />DPM çalıştırmalıdır en az Windows Server 2012 korumak için Windows Server 2012 R2 yinelenenleri kaldırılan birimler.|
|Sunucular (32 bit ve 64 bit)|Windows Server 2012 R2 - Datacenter ve Standard|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma)<br /><br />DPM, yinelenenleri kaldırılan Windows Server 2012 birimlerinin korunması için Windows Server 2012 veya 2012 R2'de çalışmalıdır.|
|Sunucular (32 bit ve 64 bit)|Windows Server 2012/2012 SP1 - Datacenter ve Standard|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma<br /><br />DPM çalıştırmalıdır en az Windows Server 2012 korumak için Windows Server 2012 R2 yinelenenleri kaldırılan birimler.|
|Sunucular (32 bit ve 64 bit)|Windows Server 2012/2012 SP1 - Datacenter ve Standard|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E|Birim, paylaşım, klasör, dosya<br /><br />DPM çalıştırmalıdır en az Windows Server 2012 korumak için Windows Server 2012 R2 yinelenenleri kaldırılan birimler.|
|Sunucular (32 bit ve 64 bit)|Windows Server 2012/2012 SP1 - Datacenter ve Standard|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma<br /><br />DPM çalıştırmalıdır en az Windows Server 2012 korumak için Windows Server 2012 R2 yinelenenleri kaldırılan birimler.|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2 SP1 - Standard ve Enterprise|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E<br /><br />SP1 ve yükleme çalışıyor olması gereken [Windows Management çerçeve 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2 SP1 - Standard ve Enterprise|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E<br /><br />SP1 ve yükleme çalışıyor olması gereken [Windows Management çerçeve 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|E |Birim, paylaşım, klasör, dosya|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2 SP1 - Standard ve Enterprise|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E<br /><br />SP1 ve yükleme çalışıyor olması gereken [Windows Management çerçeve 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|E |Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|N|E|Birim, paylaşım, klasör, dosya|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008 R2|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|N|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|N|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Server 2008|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|Sunucular (32 bit ve 64 bit)|Windows Storage Server 2008|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Birim, paylaşım, klasör, dosya, sistem durumu/tam kurtarma|
|SQL Server|SQL Server 2016|Fiziksel sunucu <br /><br /> Şirket içi Hyper-V sanal makine <br /> <br /> Azure sanal makinesi <br /><br /> Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E |N|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2014|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E |Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2014|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012 SP2|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E |Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012 SP2|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012 SP2|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012, SQL Server 2012 SP1|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012, SQL Server 2012 SP1|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2012, SQL Server 2012 SP1|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008 R2|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008 R2|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008 R2|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E|Tüm dağıtım senaryolarında: veritabanı|
|SQL Server|SQL Server 2008|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|Tüm dağıtım senaryolarında: veritabanı|
|Exchange|Exchange 2016|Fiziksel sunucu<br/><br/> Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2016|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2013|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2013|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2010|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2010|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |(Tüm dağıtım senaryolarında) koruma: tek başına Exchange server, veritabanı bir veritabanı kullanılabilirlik grubu (DAG) altındaki<br /><br />(Tüm dağıtım senaryolarında) kurtarma: posta kutusu, DAG altındaki posta kutusu veritabanları|
|Exchange|Exchange 2007|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: depolama grubu<br /><br />(Tüm dağıtım senaryolarında) kurtarma: depolama grubu, veritabanı, posta kutusu|
|Exchange|Exchange 2007|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |(Tüm dağıtım senaryolarında) koruma: depolama grubu<br /><br />(Tüm dağıtım senaryolarında) kurtarma: depolama grubu, veritabanı, posta kutusu|
|SharePoint|SharePoint 2016|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine<br /><br />(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi<br /><br />Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E |N|(Tüm dağıtım senaryolarında) koruma: Grup, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu<br /><br />İçerik veritabanları için SQL Server 2012 AlwaysOn özelliğini kullanarak bir SharePoint grubunu korumanın desteklenmediğini unutmayın.|
|SharePoint|SharePoint 2013|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu<br /><br />İçerik veritabanları için SQL Server 2012 AlwaysOn özelliğini kullanarak bir SharePoint grubunu korumanın desteklenmediğini unutmayın.|
|SharePoint|SharePoint 2013|Azure sanal (iş yükü Azure sanal makinesi olarak çalışırken) makine - DPM 2012 R2 güncelleştirme paketi 3 veya sonraki sürümleri|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu<br /><br />İçerik veritabanları için SQL Server 2012 AlwaysOn özelliğini kullanarak bir SharePoint grubunu korumanın desteklenmediğini unutmayın.|
|SharePoint|SharePoint 2013|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E |(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu<br /><br />İçerik veritabanları için SQL Server 2012 AlwaysOn özelliğini kullanarak bir SharePoint grubunu korumanın desteklenmediğini unutmayın.|
|SharePoint|SharePoint 2010|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu|
|SharePoint|SharePoint 2010|(İş yükü Azure sanal makinesi olarak çalışırken) Azure sanal makinesi|E|E |(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu|
|SharePoint|SharePoint 2010|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu|
|SharePoint|SharePoint 2007|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu|
|SharePoint|SharePoint 2007|Windows sanal makinesi (vmware'deki Windows sanal makinede çalışan iş yüklerini korur) olarak|E|E|(Tüm dağıtım senaryolarında) koruma: Grup, SharePoint arama, ön uç web sunucusu içeriği<br /><br />(Tüm dağıtım senaryolarında) kurtarma: Grup, veritabanı, web uygulaması, dosya veya liste öğesi, SharePoint arama, ön uç web sunucusu|
|Hyper-V konağı - Hyper-V konak sunucusu, küme veya VM üzerindeki DPM koruma Aracısı|Windows Server 2016|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|N|Koruma: Hyper-V bilgisayarları, Küme Paylaşılan birimleri (CSV)<br /><br />Kurtarma: sanal makine, dosya ve klasör, birimler, sanal sabit disk sürücüler öğe düzeyinde kurtarma|
|Hyper-V konağı - Hyper-V konak sunucusu, küme veya VM üzerindeki DPM koruma Aracısı|Windows Server 2012 R2 - Datacenter ve Standard|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Koruma: Hyper-V bilgisayarları, Küme Paylaşılan birimleri (CSV)<br /><br />Kurtarma: sanal makine, dosya ve klasör, birimler, sanal sabit disk sürücüler öğe düzeyinde kurtarma|
|Hyper-V konağı - Hyper-V konak sunucusu, küme veya VM üzerindeki DPM koruma Aracısı|Windows Server 2012 - Datacenter ve Standard|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Koruma: Hyper-V bilgisayarları, Küme Paylaşılan birimleri (CSV)<br /><br />Kurtarma: sanal makine, dosya ve klasör, birimler, sanal sabit disk sürücüler öğe düzeyinde kurtarma|
|Hyper-V konağı - Hyper-V konak sunucusu, küme veya VM üzerindeki DPM koruma Aracısı|Windows Server 2008 R2 SP1 - Enterprise ve Standard|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|E|E|Koruma: Hyper-V bilgisayarları, Küme Paylaşılan birimleri (CSV)<br /><br />Kurtarma: sanal makine, dosya ve klasör, birimler, sanal sabit disk sürücüler öğe düzeyinde kurtarma|
|Hyper-V konağı - Hyper-V konak sunucusu, küme veya VM üzerindeki DPM koruma Aracısı|Windows Server 2008|Fiziksel sunucu<br /><br />Şirket içi Hyper-V sanal makine|N|N|Koruma: Hyper-V bilgisayarları, Küme Paylaşılan birimleri (CSV)<br /><br />Kurtarma: sanal makine, dosya ve klasör, birimler, sanal sabit disk sürücüler öğe düzeyinde kurtarma|
|VMware Sanal Makineleri|VMware server 5.5 veya 6.0 veya 6.5 |Şirket içi Hyper-V sanal makine|E|Y (UR1 ile)|Küme Paylaşılan birimleri (CSV), NFS VMware Vm'lerinde ve SAN depolama alanı<br /> Dosya ve klasörlerin yalnızca Windows için kullanılabilir öğe düzeyinde kurtarma<br /> VMware Vapps'yi desteklenmiyor|
|Linux|Hyper-V veya VMware Konuğu olarak çalıştırılan Linux|Şirket içi Hyper-V sanal makine|E|E|Hyper-V, Windows Server 2012 R2 veya Windows Server 2016 üzerinde çalışmalıdır. Koru: Tüm sanal makine<br /><br />Kurtarma: Tüm sanal makine <br/><br/> Makale desteklenen Linux dağıtımları ve sürümleri tam listesi için bkz: [tarafından Azure destekli dağıtımlar Linux'ta](../virtual-machines/linux/endorsed-distros.md).|

## <a name="cluster-support"></a>Küme desteği
Azure yedekleme sunucusu aşağıdaki kümelenmiş uygulamalarda verileri koruyabilir:

-   Dosya sunucuları

-   SQL Server

-   Hyper-ölçeklendirilmiş DPM koruması kullanan bir Hyper-V kümesi korursanız V -, korunan Hyper-V iş yükleri için ikincil koruma ekleyemezsiniz.

    Windows Server 2008 R2'de Hyper-V çalıştırmanız gerekiyorsa KB açıklanan güncelleştirmeyi yüklediğinizden emin olun [975354](https://support.microsoft.com/en-us/kb/975354).
    Bir küme yapılandırmasında Windows Server 2008 R2'de Hyper-V çalıştırırsanız, SP2 ve KB yüklediğinizden emin olun [971394](https://support.microsoft.com/en-us/kb/971394).

-   Exchange Server - Azure yedekleme sunucusu desteklenen Exchange Server sürümleri (küme sürekli çoğaltması) için paylaşılmayan disk kümelerini koruyabilir ve ayrıca yerel sürekli çoğaltma için yapılandırılmış Exchange Server'a koruyabilir.

-   SQL Server - Küme Paylaşılan birimlerinde (CSV) barındırılan SQL Server veritabanlarını yedeklemek Azure yedekleme sunucusu desteklemiyor.

Azure yedekleme sunucusu DPM sunucusuyla aynı etki alanında ve bir alt veya güvenilen etki alanında bulunan küme iş yüklerini koruyabilir. Güvenilmeyen etki alanlarındaki veya çalışma gruplarındaki veri kaynaklarını korumak istiyorsanız, yalnızca bir küme için tek bir sunucu için NTLM veya sertifika kimlik doğrulaması veya sertifika kimlik doğrulaması kullanın.
