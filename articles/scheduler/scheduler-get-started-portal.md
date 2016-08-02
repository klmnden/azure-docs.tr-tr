<properties
 pageTitle="Azure portalda Azure Scheduler kullanmaya başlama | Microsoft Azure"
 description="Azure portalda Azure Scheduler kullanmaya başlama"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="03/09/2016"
 ms.author="krisragh"/>

# Azure portalda Azure Scheduler kullanmaya başlama

Azure Scheduler’da zamanlanmış işler oluşturmak kolaydır. Bu öğreticide bir iş oluşturmayı öğreneceksiniz: Ayrıca Scheduler’ın izleme ve yönetim özelliklerini öğreneceksiniz.

## Bir iş oluşturma

1.  [Azure portalda](https://portal.azure.com/) oturum açın.  

2.  **+ Yeni**’ye tıklayın arama kutusuna > _Scheduler_ yazın > sonuçlarda **Scheduler**’ı seçin > **Oluştur**’a tıklayın.

   ![][marketplace-create]

3.  Şimdi bir GET isteğiyle http://www.microsoft.com/ adresine işaret eden bir iş oluşturalım. **Scheduler İşi** ekranına, aşağıdaki bilgileri girin:

    1.  **Ad:** `getmicrosoft`  

    2.  **Abonelik:** Azure aboneliğiniz   

    3.  **İş Koleksiyonu:** Mevcut bir iş koleksiyonu seçin veya tıklatın **Yeni Oluştur**’a tıklayın > bir ad girin.

4.  Sonra, **Eylem Ayarları**’nda, aşağıdaki değerleri tanımlayın:

    1.  **Eylem Türü:** ` HTTP`  

    2.  **Yöntem:** `GET`  

    3.  **URL:** ` http://www.microsoft.com`  

   ![][action-settings]

5.  Son olarak, şimdi bir zamanlama tanımlayalım. İş bir kerelik iş olarak tanımlanabilir, ancak bir yineleme zamanlaması seçelim.

    1. **Yineleme**: `Recurring`

    2. **Başlat**: Bugünün tarihi

    3. **Yineleme sıklığı**: `12 Hours`

    4. **Bitiş tarihi**: Bugünden itibaren iki gün  

   ![][recurrence-schedule]

6.  **Oluştur**'a tıklayın

## İşleri yönetme ve izleme

Bir işi oluşturulduktan sonra, ana Azure panosunda görünür. İşe tıkladığınızda aşağıdaki sekmeleri içeren yeni bir pencere açılır:

1.  Özellikler  

2.  Eylem Ayarları  

3.  Zamanlama  

4.  Geçmiş

5.  Kullanıcılar

   ![][job-overview]

### Özellikler

Bu salt okunur özellikler Scheduler işi için yönetim meta verilerini açıklar.

   ![][job-properties]


### Eylem ayarları

**İşler** ekranındaki bir işe tıklamak bu işi yapılandırmanıza olanak tanır. Bu, bunları hızlı oluşturma sihirbazında yapılandırmadıysanız, gelişmiş ayarları yapılandırmanızı sağlar.

Tüm eylem türleri için, yeniden deneme ilkesini ve hata eylemini değiştirebilirsiniz.

HTTP ve HTTPS iş eylemi türleri için, izin verilen bir HTTP fiiline ilişkin yöntemi değiştirebilirsiniz. Ayrıca başlık ve temel kimlik doğrulama bilgileri ekleyebilir, silebilir veya değiştirebilirsiniz.

Depolama kuyruğu eylem türleri için, depolama hesabı, kuyruk adı, SAS belirteci ve gövdeyi değiştirebilirsiniz.

Hizmet veri yolu eylemi türleri için, ad alanı, konu/kuyruk yolu, kimlik doğrulama ayarları, aktarım türü, ileti özellikleri ve ileti gövdesini değiştirebilirsiniz.

   ![][job-action-settings]

### Zamanlama

Bu, hızlı oluşturma sihirbazında oluşturduğunuz zamanlamayı değiştirmek istediğinizde, zamanlamayı yeniden yapılandırmanızı sağlar.

Bu, [işinizde karmaşık zamanlamalar ve gelişmiş yineleme](scheduler-advanced-complexity.md) oluşturma fırsatıdır.

Başlangıç tarihini ve saatini, yineleme zamanlamasını ve bitiş tarihini ve saatini değiştirebilirsiniz (iş yineleniyorsa).

   ![][job-schedule]


### Geçmiş

**Geçmişi** sekmesi seçili iş için sistemdeki her iş yürütme için seçilen ölçümleri görüntüler. Bu ölçümler Scheduler sistem durumunuz ile ilgili gerçek zamanlı değerleri belirtir:

1.  Durum  

2.  Ayrıntılar  

3.  Yeniden deneme sayısı

4.  Oluşma 1., 2., 3., vs.

5.  Yürütme başlangıç saati  

6.  Yürütme bitiş saati

   ![][job-history]

Her yürütmeye ilişkin tüm yanıtlar dahil **Geçmiş Ayrıntıları**’nı görüntülemek için bir yürütmeye tıklayabilirsiniz. Bu iletişim kutusu yanıtı panoya kopyalamanızı da sağlar.

   ![][job-history-details]

### Kullanıcılar

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure Scheduler için ayrıntılı erişim yönetimi sağlar. Kullanıcılar sekmesini kullanmayı öğrenmek için, bkz. [Azure Rol Tabanlı Erişim Denetimi](../active-directory/role-based-access-control-configure.md)


## Ayrıca bkz.

 [Scheduler nedir?](scheduler-intro.md)

 [Scheduler kavramları ve terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler ile karmaşık zamanlamalar ve gelişmiş yineleme oluşturma](scheduler-advanced-complexity.md)

 [Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png



<!---HONumber=Jun16_HO2-->


