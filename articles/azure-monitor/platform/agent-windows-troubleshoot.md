---
title: Windows için Log Analytics Aracısı sorunlarını giderme | Microsoft Docs
description: Azure İzleyici'de Windows için Log Analytics Aracısı belirtileri, nedenleri ve en sık karşılaşılan sorunlara yönelik çözümler açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2019
ms.author: magoedte
ms.openlocfilehash: afa4483677336e9a887908a8cccf9590eed27af3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120116"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-windows"></a>Windows için Log Analytics Aracısı ile ilgili sorunları giderme 

Bu makalede, Azure İzleyici'de Windows için Log Analytics aracısını ile karşılaşabilirsiniz ve bunların çözülmesine yönelik olası çözümler önerir hatalarını giderme hakkında Yardım sağlar.

Bu adımların hiçbiri işinize yaramazsa aşağıdaki Destek kanallarını da kullanılabilir:

* Avantajları Premier Destek ile bir destek isteği açabilirsiniz [Premier](https://premier.microsoft.com/).
* Azure destek sözleşmeleri olan müşteriler, bir destek isteği açabilirsiniz [Azure portalında](https://manage.windowsazure.com/?getsupport=true).
* Gönderilen fikirleri ve hataları gözden geçirmek için Log Analytics geri bildirim sayfasını ziyaret edin [ https://aka.ms/opinsightsfeedback ](https://aka.ms/opinsightsfeedback) veya yeni bir dosya. 

## <a name="important-troubleshooting-sources"></a>Önemli sorun giderme kaynakları

 Windows için Log Analytics aracısını ilgili sorunları gidermeye yardımcı olmak için aracı olayları Windows olay günlüğüne, özellikle altında kaydeder *uygulama ve Services\Operations Manager*.  

## <a name="connectivity-issues"></a>Bağlantı sorunları

Aracıyı bir proxy sunucusu veya Güvenlik Duvarı iletişim kuruyorsa kısıtlamalar yerde kaynak bilgisayarı ve Azure İzleyici hizmeti iletişimi engelliyor olabilir. İletişim engellenirse, yanlış yapılandırma, ek bir çalışma alanına raporlama yapacak Aracısı Kurulum sonrası yapılandırma aracı yüklemeye çalışmadan ya da sonra başarılı kayıt aracı iletişimi başarısız bir çalışma alanı ile kayıt başarısız olabilir. Bu bölümde, bu tür bir Windows Aracısı ile ilgili sorun giderme yöntemleri açıklar. 

Çift güvenlik duvarı veya proxy aşağıdaki bağlantı noktaları ve URL'ler aşağıdaki tabloda açıklanan izin verecek şekilde yapılandırıldığını denetleyin. Ayrıca, Azure İzleyici ve aracı arasında güvenli bir TLS kanal engelleyebilir gibi HTTP İnceleme web trafiği için etkin değil doğrulayın.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Giden|Evet |  
|*.azure-automation.net |Bağlantı noktası 443 |Giden|Evet |  

Azure kamu için gerekli güvenlik duvarı için bilgi [Azure kamu Yönetimi](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Aracıyı Azure İzleyici ile başarıyla iletişim kurduğu, doğrulayabilirsiniz birkaç yolu vardır.

- Etkinleştirme [Azure Log Analytics aracı sistem durumu değerlendirme](../insights/solution-agenthealth.md) çalışma. Aracı sistem durumu Panosu'nu **yanıt vermeyen aracı sayısı** hızla aracı listelenip listelenmediğini sütunu.  

- Aracı çalışma alanına raporlama yapacak şekilde yapılandırılmış bir sinyal gönderen onaylamak için aşağıdaki sorguyu çalıştırın. Değiştirin <ComputerName> gerçek makine adını.

    ```
    Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer 
    ```

    Bilgisayarı başarıyla hizmetiyle iletişim kurulurken, sorgu bir sonuç döndürmesi gerekir. Sorgu bir sonuç döndürmedi, ilk aracıyı doğru çalışma alanına rapor yapılandırıldığını doğrulayın. Doğru şekilde yapılandırıldıysa, 3. adımına devam etmek ve aracıyı hangi sorunu, Azure İzleyici ile iletişim kurmasını engelleyen günlüğü tanımlamak için Windows olay günlüğü arayın.

- Bir bağlantı sorunu belirlemek üzere başka bir yöntem çalışıyor **TestCloudConnectivity** aracı. Aracın aracıda klasöründe varsayılan olarak yüklü *%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent*. Yükseltilmiş bir komut isteminden klasörüne gidin ve aracı çalıştırın. Aracı vurgular (örneğin, belirli bir bağlantı noktası/engellenen URL ilişkili ise) testin başarısız olduğu ve sonuçları döndürür. 

    ![TestCloudConnection aracı yürütme sonuçları](./media/agent-windows-troubleshoot/output-testcloudconnection-tool-01.png)

- Filtre *Operations Manager* olay günlüğüne göre **olay kaynakları** - *sistem durumu hizmet modülleri*, *HealthService*, ve *Hizmet Bağlayıcısı* ve filtre **olay düzeyi** *uyarı* ve *hata* , olaylarından yazılmış, onaylamak için Aşağıdaki tablo. Böyle bir durumda dahil olası her olay için çözüm adımlarını gözden geçirin.

    |Olay Kimliği |source |Açıklama |Çözüm |
    |---------|-------|------------|-----------|
    |2133 & 2129 |Sistem sağlığı hizmeti |Hizmet Aracısı'ndan bağlantısı başarısız oldu |Aracıyı doğrudan veya Azure İzleyici hizmeti bir güvenlik duvarı/Ara sunucu üzerinden iletişim kurduğunda bu hata oluşabilir. Aracı proxy ayarları veya ağ güvenlik duvarı/proxy hizmeti bilgisayardan gelen TCP trafiğine izin verdiğini doğrulayın.|
    |2138 |Sistem durumu hizmet modülleri |Proxy kimlik doğrulaması gerektiriyor |Aracı proxy ayarlarını yapılandırın ve proxy sunucusu ile kimlik doğrulamak için gereken kullanıcı adı/parola belirtin. |
    |2129 |Sistem durumu hizmet modülleri |Başarısız bağlantı/başarısız SSL anlaşması |Ağ bağdaştırıcısının TCP/IP ayarlarını ve aracı proxy ayarlarını denetleyin.|
    |2127 |Sistem durumu hizmet modülleri |Veri gönderme başarısız oldu, hata kodu alındı |Yalnızca bir gün boyunca düzenli aralıklarla gerçekleşir, yalnızca göz ardı edilebilir rastgele bir anomali olabilir. Ne kadar sıklıkla olduğunu anlamak için izleyin. Gün boyunca sık sık olursa, ağ yapılandırması ve proxy ayarlarını denetleyin. HTTP hata kodu 404 tanım içerir ve aracı hizmete veri göndermeye ilk kez ise, bir iç 404 hata kodlu bir 500 hata içerir. 404 bulunamadı, yeni bir çalışma alanı için depolama alanı yine sağlanıyor gösteren anlamına gelir. Sonraki yeniden deneme sırasında başarıyla çalışma alanına veri, beklenen şekilde yazılacaktır. Bir HTTP Hata 403 izni veya kimlik bilgilerini sorunu gösterebilir. Sorunu gidermeye yardımcı olmak için 403 hatası dahil daha fazla bilgi bulunmaktadır.|
    |4000 |Hizmet Bağlayıcısı |DNS ad çözümlemesi başarısız oldu |Makine, veri hizmetine gönderirken kullanılan Internet adresi çözümlenemedi. DNS Çözümleyicisi ayarları makinenizi, yanlış proxy ayarlarını veya belki de geçici bir DNS sorununun sağlayıcınızla kaynaklanıyor olabilir. Düzenli aralıklarla olursa, geçici bir ağ ile ilgili sorun tarafından kaynaklanabilir.|
    |4001 |Hizmet Bağlayıcısı |Hizmet bağlantısı başarısız oldu. |Aracıyı doğrudan veya Azure İzleyici hizmeti bir güvenlik duvarı/Ara sunucu üzerinden iletişim kurduğunda bu hata oluşabilir. Aracı proxy ayarları veya ağ güvenlik duvarı/proxy hizmeti bilgisayardan gelen TCP trafiğine izin verdiğini doğrulayın.|
    |4002 |Hizmet Bağlayıcısı |Hizmet bir sorguya yanıt olarak HTTP durum kodu 403 döndürdü. Sistem durumu hizmetinin hizmet yöneticisine danışın. Sorgu daha sonra yeniden denenecek. |Bu hata, aracının ilk kayıt aşamasında yazılır ve aşağıdakine benzer bir URL görürsünüz: *https://<workspaceID>.oms.opinsights.azure.com/AgentService.svc/AgentTopologyRequest*. Bir hata kodu 403 anlamına gelir Yasak ve yanlış yazılan çalışma alanı kimliği veya anahtarı tarafından kaynaklanabilir veya veri ve zaman, bu bilgisayarda yanlış. Saati geçerli saatten 15 dakika +/-ise, ardından ekleme başarısız olur. Bunu düzeltmek için tarih ve/veya Windows bilgisayarınız saat dilimini güncelleştirin.|

## <a name="data-collection-issues"></a>Veri toplama sorunları

Aracı yüklendikten sonra raporlar, yapılandırılan çalışma alanına veya çalışma alanları, yapılandırma, toplama veya iletme performansı, günlükleri veya diğer verileri etkin olup olmadığına bağlı olarak hizmete alma ve bilgisayarı hedefleyen durdurabilir. Belirlemek gereklidir:

- Bu, belirli bir veri türüne veya çalışma alanında kullanılabilir olmayan tüm verileri mi?
- Bir çözüm tarafından belirtilen ya da çalışma verisi toplama yapılandırması bir parçası olarak belirtilen veri türü?
- Kaç bilgisayar etkilendi? Bu, tek bir veya birden çok bilgisayar çalışma alanına raporlama mi?
- Çalışır durumda olan ve günün belirli bir zamanda durdu veya toplanan olmamıştı? 
- Kullanmakta olduğunuz günlük arama sorgusu sözdizimsel olarak doğru mu? 
- Aracı, Azure İzleyici'den hiç olmadığı kadar yapılandırmasıyla aldı?

Sorun giderme ilk adımı, bilgisayar bir sinyal olay gönderiyor belirlemektir.

```
Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer
```

Sorgu sonuçları döndürürse, belirli bir veri türüne değil toplanan ve hizmete iletildi olmadığını belirlemeniz gerekir. Bu hizmeti veya aracı normal çalışmasını engelleyen başka bir belirti güncelleştirilmiş yapılandırmayı almak aracısı tarafından kaynaklanabilir. Daha fazla sorun giderme için aşağıdaki adımları gerçekleştirin.

1. Bilgisayarda yükseltilmiş bir komut istemi açın ve yazarak aracı hizmetini yeniden başlatmanız `net stop healthservice && net start healthservice`.
2. Açık *Operations Manager* olay günlüğü ve arama **olay kimlikleri** *7023, 7024, 7025, 7028* ve *1210* gelen **olay Kaynak** *HealthService*.  Bu olaylar, aracıyı Azure İzleyici'deki yapılandırma başarıyla alıyor ve etkin olarak bilgisayarı izlemeye gösterir. Olay Kimliği 1210 son da belirteceksiniz olay açıklamasında tüm çözümler ve aracıda izleme kapsamını dahil edilen İçgörüler satır.  

    ![Olay Kimliği 1210 açıklaması](./media/agent-windows-troubleshoot/event-id-1210-healthservice-01.png)

3. Birkaç dakika sonra beklenen verileri Görselleştirme ve sorgu sonuçlarını görmüyorsanız, eğer bağlı olarak bir çözüm ya da Öngörülere verilerden gelen görüntülediğiniz *Operations Manager* olay günlüğü, arama **olay Kaynakları** *HealthService* ve *sistem durumu hizmet modülleri* ve filtre **olay düzeyi** *uyarı* ve *Hata* olayları aşağıdaki tablodan yazılmış, onaylamak için.

    |Olay Kimliği |source |Açıklama |Çözüm |
    |---------|-------|------------|
    |8000 |HealthService |Bu olay, bir iş akışı ilişkilidir, performans için olay veya toplanan diğer veri türleri için çalışma alanına alımı hizmetini iletemiyor belirteceksiniz. | Olay Kimliği 2136 HealthService bu olay ile birlikte yazılır ve aracıyı belirtebilir kaynağından proxy ve kimlik doğrulama ayarları, ağ kesintisi veya ağ güvenlik duvarı yanlış yapılandırılması nedeniyle muhtemelen hizmetiyle iletişim kuramadı / Proxy hizmetine bilgisayardan gelen TCP trafiğine izin vermez.| 
    |10102 ve 10103 |Sistem durumu hizmet modülleri |İş akışı veri kaynağı çözümlenemedi. |Belirtilen performans sayacı örneği bilgisayarda mevcut değil veya yanlış çalışma alanı veri ayarlarında tanımlanır varsa bu durum oluşabilir. Bu kullanıcı tarafından belirtilen ise [performans sayacı](data-sources-performance-counters.md#configuring-performance-counters), belirtilen bilgileri doğru biçimde aşağıdaki doğrulayın ve hedef bilgisayarlarda bulunmaktadır. |
    |26002 |Sistem durumu hizmet modülleri |İş akışı veri kaynağı çözümlenemedi. |Belirtilen Windows olay günlüğü bilgisayarda mevcut değilse bu durum oluşabilir. Bu kullanıcı tarafından belirtilen, aksi takdirde kayıtlı bu olay günlüğü için bilgisayar görülmüyorsa bu hata güvenle yoksayılabilir [olay günlüğü](data-sources-windows-events.md#configuring-windows-event-logs), belirttiğiniz bilgilerin doğru olduğunu doğrulayın. |

