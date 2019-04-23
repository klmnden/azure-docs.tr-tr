---
title: İş akışı Windows Azure VM mimarisi | Microsoft Docs
description: Bu makalede, bir hizmet dağıttığınızda iş akışı işlemlerini genel bakış sağlar.
services: cloud-services
documentationcenter: ''
author: genlin
manager: Willchen
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/08/2019
ms.author: kwill
ms.openlocfilehash: 7c8459a6694663a49203b6ec21a760d3e6bd60c3
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60150529"
---
#    <a name="workflow-of-windows-azure-classic-vm-architecture"></a>İş akışı Windows Azure Klasik VM mimarisi 
Bu makalede, iş akışı işlemlerini dağıttığınızda veya bir Azure kaynağı bir sanal makine gibi güncelleştirme oluşan genel bir bakış sağlar. 

> [!NOTE]
>Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir.

Aşağıdaki diyagram, Azure kaynaklarını mimarisi sunar.

![Azure iş akışı](./media/cloud-services-workflow-process/workflow.jpg)

## <a name="workflow-basics"></a>İş akışı temel bilgileri
   
**BİR**. RDFE / FFE kullanıcı dokusuna iletişim yolu. RDFE (RedDog ön uç) Yönetim Portalı ön ucu genel olarak sunulan API ve Visual Studio, Azure MMC ve benzeri gibi hizmet yönetimi API ' dir.  Kullanıcıdan gelen tüm istekleri RDFE gidin. FFE (Fabric ön uç) fabric komutları içine RDFE gelen istekleri çevirir katmanıdır. RDFE gelen tüm istekleri, yapı denetleyicilerine ulaşamıyoruz FFE gidin.

**B**. Yapı denetleyicisi, koruma ve veri merkezinde tüm kaynakları izleme sorumludur. Konuk işletim sistemi sürümü, hizmet paketi, hizmet yapılandırmasını ve hizmet durumunu gibi işletim sistemi gönderen bilgileri fabric'te fabric konak aracılarıyla iletişim kurar.

**C**. Konak aracısı üzerinde konak OSsystem yaşar ve konuk işletim sistemi ayarlama sorumludur ve istenen hedef durumu doğru rolü güncelleştirir ve sinyal yapmak için konuk Aracısı (WindowsAzureGuestAgent) ile iletişim kurulurken Konuk aracıyla denetler. Konak Aracısı, konak Aracısı 10 dakika için sinyal yanıt almazsa, konuk işletim Sisteminin yeniden başlatır.

**C2**. Yükleme, yapılandırma ve WindowsAzureGuestAgent.exe güncelleştirme için WaAppAgent sorumludur.

**D**.  WindowsAzureGuestAgent aşağıdakilerden sorumlu olmaz:

1. Konuk işletim sistemi güvenlik duvarı, ACL'ler, LocalStorage kaynaklar, hizmet paketi ve yapılandırması ve sertifikalar dahil olmak üzere yapılandırma. Rol, altında çalıştırılacağı bir kullanıcı hesabı için SID ayarlama.
2. Rol durumu dokuya iletişim.
3. WaHostBootstrapper başlatma ve onu emin olmak için izleme rolü hedef durumda değil.

**E**. WaHostBootstrapper sorumludur:

1. Rol yapılandırmasını okuma ve tüm uygun görevleri ve işlemleri yapılandırıp rolünü çalıştırmak için başlatılıyor.
2. Tüm alt işlemleri izleme.
3. Rolü ana bilgisayar işlemi StatusCheck olayı oluşturma.

**F**. Rolü (bunu SDK 1.2 HWC rolleri için çalışmaz) tam IIS web rolü olarak yapılandırılmışsa IISConfigurator çalıştırır. Sorumlu olduğu:

1. Standart IIS hizmetleri başlatılıyor
2. Yeniden yazma modülü web yapılandırma
3. Hizmet modelinde yapılandırılmış rolü için AppPool ayarlama
4. IIS günlüğe kaydetmeyi DiagnosticStore LocalStorage klasöre işaret edecek şekilde ayarlama
5. İzin ve ACL'ler yapılandırma
6. Web sitesi % roleroot%:\sitesroot\0 ve IIS çalıştırmak için bu konuma apppool noktaları bulunur. 

**G**. Başlangıç görevleri rol modeli tarafından tanımlanan ve WaHostBootstrapper tarafından başlatıldı. Başlangıç görevleri arka planda zaman uyumsuz olarak çalıştırmak için yapılandırılabilir ve konak önyükleyici başlangıç görevini başlatmak ve diğer başlangıç görevleri devam edin. Başlangıç görevleri, sonlanması ve sonraki başlangıç görevine eklenmesi devam etmeden önce bir başarı (0) çıkış kodu dönmek başlangıç görevi, konak önyükleyici bekler (varsayılan) basit modda çalıştırmak için de yapılandırılabilir.

**H**. Bu görevler, SDK'sının bir parçasıdır ve eklentileri rolün hizmet tanımı (.csdef) olarak tanımlanır. Başlangıç görevleri genişletildiğinde **DiagnosticsAgent** ve **RemoteAccessAgent** her iki başlangıç görevi, bir normal ve sahip bir tanımlayın, benzersiz olan bir **/blockStartup** parametresi. Böylece rol çalışırken arka planda çalıştırabilirsiniz normal başlangıç görevinin başlangıç arka plan görevi olarak tanımlanır. **/BlockStartup** başlangıç görevi, basit bir başlangıç görevi olarak tanımlanır, böylece WaHostBootstrapper devam etmeden önce çıkmak için bekler. **/BlockStartup** görev başlatma son normal görevin tamamlanmasını bekler ve ardından çıkış yapar ve devam etmek konak önyükleyici izin. Tanılama ve RDP erişimine (Bu /blockStartup görev gerçekleştirilir) rol işlemler başlamadan önce yapılandırılabilir, böylece bu yapılır. Bu da tanılama ve RDP erişimini konak önyükleyici (Bu, Normal bir görev gerçekleştirilir) başlangıç görevleri tamamladıktan sonra çalışmaya devam etmesini sağlar.

**BEN**. WaWorkerHost normal çalışan rolleri için standart bir ana bilgisayar işlemidir. Bu ana bilgisayar işlemi rolün tüm DLL'ler ve OnStart ve çalıştırma gibi giriş noktası kod barındırır.

**J**. SDK'sı 1.2 uyumlu barındırılabilir Web Çekirdeği (HWC) kullanmak için yapılandırılmışsa WaWebHost web rolleri için standart bir ana bilgisayar işlemidir. Rolleri, hizmet tanımı (.csdef) öğe kaldırarak HWC modunu etkinleştirebilirsiniz. Bu modda, hizmetin tüm kod ve DLL'leri WaWebHost işlemden çalıştırın. IIS (w3wp) kullanılmaz ve IIS Yöneticisi'nde IIS WaWebHost.exe içinde barındırıldığı için yapılandırılmış hiçbir AppPools vardır.

**K**. WaIISHost tam IIS kullanan web rolleri için rol giriş noktası kod ana bilgisayar işlemidir. Bu işlemi kullanan bulunan ilk DLL'yi yükler **RoleEntryPoint** sınıfı ve bu sınıftan (OnStart, çalıştırma, OnStop) kodu yürütür. Tüm **RoleEnvironment** RoleEntryPoint sınıfında oluşturulan olaylar (örneğin, StatusCheck ve değiştirilen) Bu işlemde ortaya çıkar.

**M**. W3wp rol tam IIS kullanmak üzere yapılandırılmışsa, kullanılan standart IIS çalışan işlemi var. Bu, IISConfigurator yapılandırılmış AppPool çalıştırır. Burada oluşturulan tüm RoleEnvironment olaylar (örneğin, StatusCheck ve değiştirilen) Bu işlemde oluşturulur. Her iki işlem olaylara abone olursanız RoleEnvironment olayları (WaIISHost ve w3wp.exe) her iki konumda ateşlenir unutmayın.

## <a name="workflow-processes"></a>İş akışı süreçleri

1. Bir kullanıcı .cspkg ve .cscfg dosyaları yükleniyor, durdurmak için bir kaynak belirten veya bir yapılandırma değişikliği yapma gibi bir isteği yapar ve benzeri. Bu, Azure portalı ya da Visual Studio Yayımlama özelliği gibi hizmet yönetimi API'sini kullanan bir araç aracılığıyla yapılabilir. Bu istek yapmak için RDFE tüm abonelikle ilişkili çalışma ve ardından FFE isteği iletişim gider. İş akışı adımları geri kalanını olan yeni bir paket dağıtma ve başlatın.
2. FFE (Müşteri giriş gibi benzeşim grubu veya coğrafi konum artı makine kullanılabilirlik gibi doku girişten temelinde) doğru makine havuzu bulur ve bu makine havuzundaki ana yapı denetleyicisi ile iletişim kurar.
3. Yapı denetleyicisi kullanılabilir CPU çekirdeği olan bir konak bulur (veya dönüş en yeni bir ana bilgisayar). Yapılandırma ve hizmet paketi ana bilgisayara kopyalanır ve yapı denetleyicisi konak işletim sistemi paketini dağıtmak için konak Aracısı ile iletişim kurar (Dıps, bağlantı noktaları, konuk işletim sistemi ve benzeri yapılandırma).
4. Konak Aracısı, konuk işletim sistemini başlatır ve Konuk Aracısı (WindowsAzureGuestAgent) ile iletişim kurar. Ana bilgisayar rolü hedef durumunu doğru çalıştığından emin olmak için konuk sinyal gönderir.
5. WindowsAzureGuestAgent konuk işletim sistemi (Güvenlik Duvarı, ACL'ler, LocalStorage ve benzeri) ' ayarlar, yeni bir XML yapılandırma dosyası için c:\Config kopyalar ve ardından WaHostBootstrapper işlemini başlatır.
6. Tam IIS web rolleri için WaHostBootstrapper IISConfigurator başlar ve IIS web rolü için mevcut tüm AppPools silmek açıklanmaktadır.
7. WaHostBootstrapper okur **başlangıç** görevleri E:\RoleModel.xml ve başlangıç görevleri yürütme başlar. Tüm basit başlangıç görevleri tamamlandı ve "başarılı" iletisi döndürülen kadar WaHostBootstrapper bekler.
8. WaHostBootstrapper tam IIS web rolleri için IIS uygulama havuzu yapılandırmak için IISConfigurator bildirir ve sitesine yönelen `E:\Sitesroot\<index>`burada `<index>` sayısı 0 tabanlı bir dizine olan <Sites> hizmet için tanımlanan öğe.
9. WaHostBootstrapper rolü türüne bağlı olarak ana bilgisayar işlemi başlatılır:
    1. **Çalışan rolü**: WaWorkerHost.exe başlatılır. WaHostBootstrapper OnStart() yöntemini yürütür. Döndürür sonra WaHostBootstrapper uygulamasındaki Run() yönteminde yürütülmeye başlamadan ve ardından aynı anda rol hazır olarak işaretler ve (InputEndpoints tanımlanmışsa) yük dengeleyici rotasyonuna koyar. WaHostBootsrapper, ardından rol durumu denetimi bir döngüye giriyor.
    1. **SDK'sı 1.2 HWC Web rolü**: WaWebHost başlatılır. WaHostBootstrapper OnStart() yöntemini yürütür. Döndürür sonra WaHostBootstrapper uygulamasındaki Run() yönteminde yürütülecek başlar ve aynı anda rol hazır olarak işaretler ve yük dengeleyici rotasyonuna yerleştirir. WaWebHost Isınma isteğini (GET /do.rd_runtime_init) verir. Tüm web istekleri için WaWebHost.exe gönderilir. WaHostBootsrapper, ardından rol durumu denetimi bir döngüye giriyor.
    1. **Tam IIS Web rolü**: aIISHost başlatılır. WaHostBootstrapper OnStart() yöntemini yürütür. Bunu döndükten sonra uygulamasındaki Run() yönteminde yürütülmeye başlamadan aynı anda rol hazır olarak işaretler ve yük dengeleyici rotasyonuna yerleştirir. WaHostBootsrapper, ardından rol durumu denetimi bir döngüye giriyor.
10. Gelen web isteklerini tam IIS rolü Tetikleyicileri W3WP sürecini başlatmak ve istek aynı şirket içi IIS ortamında olduğu gibi hizmet için IIS web.

## <a name="log-file-locations"></a>Günlük dosyası konumları

**WindowsAzureGuestAgent**

- C:\Logs\AppAgentRuntime.log.  
Bu günlük başlatır, durdurur ve yeni yapılandırmaları gibi hizmet değişiklikleri içerir. Hizmet değişmezse, bu günlük dosyasında büyük zaman aralıkları görmeyi bekleyebilirsiniz.
- C:\Logs\WaAppAgent.log.  
Bu günlük durum güncelleştirmeleri ve sinyal bildirimler içeriyor ve 2-3 saniyede bir güncelleştirilir.  Bu günlük, örneğinin durumunu bir geçmiş görünümünü içerir ve ne zaman örneği hazır durumda değildi size bildirir.
 
**WaHostBootstrapper**

C:\Resources\Directory\<deploymentID>.<role>.DiagnosticStore\WaHostBootstrapper.log
 
**WaWebHost**

C:\Resources\Directory\<guid>.<role>\WaWebHost.log
 
**WaIISHost**

C:\Resources\Directory\<deploymentID>.<role>\WaIISHost.log
 
**IISConfigurator**

C:\Resources\Directory\<deploymentID>.<role>\IISConfigurator.log
 
**IIS günlükleri**

C:\Resources\Directory\<guid>.<role>.DiagnosticStore\LogFiles\W3SVC1
 
**Windows olay günlükleri**

D:\Windows\System32\Winevt\Logs
 



