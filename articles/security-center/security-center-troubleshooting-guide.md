---
title: "Azure Güvenlik Merkezi Sorun Giderme Kılavuzu | Microsoft Belgeleri"
description: "Bu belge Azure Güvenlik Merkezi’ndeki sorunları gidermenize yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: b9f4a8b185f9fb06f8991b6da35a5d8c94689367
ms.openlocfilehash: dbbec729c14d0d9dc5781e7a88a1db3f66f7df97
ms.lasthandoff: 02/16/2017


---
# <a name="azure-security-center-troubleshooting-guide"></a>Azure Güvenlik Merkezi Sorun Giderme Kılavuzu
Bu kılavuz, kuruluşları Azure Güvenlik Merkezi'ni kullanmayı planlayan ve Güvenlik Merkezi ile ilgili sorunları gidermeye ihtiyaç duyan bilgi teknolojisi (BT) uzmanları, bilgi güvenlik analizi uzmanları ve bulut yöneticileri içindir.

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu
Bu kılavuzda Güvenlik Merkezi ile ilgili sorunların nasıl giderildiği açıklanmaktadır. Güvenlik Merkezi’nde yapılan sorun giderme işlemlerinin büyük bölümü, başarısız bileşen için ilk olarak [Denetim Günlüğü](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) kayıtlarına bakılarak yapılır. Denetim günlükleri ile aşağıdakileri belirleyebilirsiniz:

* Hangi işlemlerin gerçekleştirildiği
* İşlemi kimin başlattığı
* İşlemin ne zaman oluştuğu
* İşlemin durumu
* İşlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri

Denetim günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir, ancak okuma işlemlerini (GET) içermez.

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Windows’ta izleme aracısı yükleme sorunlarını giderme
Güvenlik Merkezi izleme aracısı, veri toplamayı gerçekleştirmek için kullanılır. Veri toplama etkinleştirildikten ve aracı hedef makineye doğru yüklendikten sonra şu işlemler yürütülmelidir:

* ASMAgentLauncher.exe - Azure İzleme Aracısı 
* ASMMonitoringAgent.exe - Azure Güvenlik İzleme uzantısı
* ASMSoftwareScanner.exe – Azure Tarama Yöneticisi

Azure Security Monitoring uzantısı, güvenlikle ilgili çeşitli yapılandırmaları tarar ve güvenlik günlüklerini sanal makineden toplar. Tarama yöneticisi bir düzeltme eki tarayıcısı olarak kullanılır.

Yükleme başarıyla gerçekleştirilirse hedef sanal makinenin Denetim Günlüklerinde aşağıdakine benzer bir giriş görmeniz gerekir:

![Denetim Günlükleri](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

*%systemdrive%\windowsazure\logs* (örnek: C:\WindowsAzure\Logs) konumunda bulunan aracı günlüklerini okuyarak yükleme işlemi hakkında daha fazla bilgi elde edebilirsiniz.

> [!NOTE]
> Azure Güvenlik Merkezi Aracısı hatalı davranıyorsa aracıyı durdurup başlatmaya yönelik bir komut olmadığından hedef sanal makineyi yeniden başlatmanız gerekir.


Veri toplamayla ilgili sorun yaşamaya devam ediyorsanız, aşağıdaki adımları izleyerek aracıyı kaldırabilirsiniz:

1. **Azure Portal**’dan veri toplama sorunları yaşayan sanal makineyi seçin ve **Uzantılar**’a tıklayın.
2. **Microsoft.Azure.Security.Monitoring** öğesine sağ tıklayıp **Kaldır**’a tıklayın.

![Aracıyı kaldırma](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig4.png)

Azure Güvenlik İzleme uzantısı birkaç dakika içinde otomatik olarak yeniden yüklenir.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Linux’ta izleme aracısı yükleme sorunlarını giderme
Bir Linux sisteminde Sanal Makine Aracısı yükleme sorunlarını giderirken uzantının /var/lib/waagent/ dizinine indirildiğinden emin olmanız gerekir. Yüklendiğini doğrulamak için aşağıdaki komutu çalıştırabilirsiniz:

`cat /var/log/waagent.log` 

Sorun giderme amacıyla gözden geçirebileceğiniz diğer günlük dosyaları şunlardır: 

* /var/log/mdsd.err
* /var/log/azure/

Çalışan bir sistemde TCP 29130 üzerinde mdsd işlemiyle bir bağlantı görmeniz gerekir. Bu bağlantı mdsd işlemiyle iletişim kuran syslog dosyasıdır. Aşağıdaki komutu çalıştırarak bu davranışı doğrulayabilirsiniz:

`netstat -plantu | grep 29130`

## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Uç nokta korumasıyla ilgili sorunları giderme işlemi düzgün çalışmıyor

Konuk aracı, [Microsoft Kötü Amaçlı Yazılımdan Koruma](../security/azure-security-antimalware.md) uzantısının gerçekleştirdiği tüm işlemlerin üst işlemidir. Konuk aracı işleminin başarısız olması durumunda konuk aracının alt işlemi olarak çalışan Microsoft Kötü Amaçlı Yazılımdan Koruma da başarısız olabilir.  Bu gibi senaryolarda aşağıdaki seçenekleri doğrulamanız önerilir:

- Hedef sanal makine özel bir görüntü mü ve sanal makineyi oluşturan kişi konuk aracısını hiç yüklememiş mi?
- Hedef bir Windows sanal makinesi değil de Linux sanal makinesiyse, kötü amaçlı yazılımdan koruma uzantısının Windows sürümünün Linux sanal makinesine yüklenmesi başarısız olur. Linux konuk aracısının işletim sistemi sürümü ve gerekli paketler açısından belirli gereksinimleri vardır ve bu gereksinimler karşılanmazsa sanal makine aracısı burada da çalışmaz. 
- Sanal makine konuk aracısının eski bir sürümüyle mi oluşturulmuş? Bu durumda, bazı eski aracıların kendini otomatik olarak daha yeni bir sürüme güncelleştiremediğini ve bu soruna yol açabileceğini unutmamanız gerekir. Kendi görüntülerinizi oluşturuyorsanız konuk aracısının her zaman en son sürümünü kullanın.
- Bazı üçüncü taraf yönetim yazılımları konuk aracısını devre dışı bırakabilir ya da aracının belirli dosya konumlarına erişmesini engelleyebilir. Sanal makinenizde üçüncü taraf yazılım yüklüyse aracının dışlama listesinde olduğundan emin olun.
- Belirli güvenlik duvarı ayarları ya da Ağ Güvenlik Grubu (NSG), konuk aracısı için giden ve gelen trafiği engelleyebilir.
- Belirli bir Erişim Denetimi Listesi (ACL) disk erişimini engelleyebilir.
- Disk alanının yetersiz olması konuk aracısının düzgün çalışmasını engelleyebilir. 

Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi varsayılan olarak devre dışıdır; gerektiğinde bu arabirimi nasıl etkinleştirebileceğiniz hakkında daha fazla bilgi edinmek için [Azure Resource Manager Sanal Makinelerinde Dağıtımdan Sonra Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi’ni Etkinleştirme](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) konusunu okuyun.

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Pano yükleme sorunlarını giderme

Güvenlik Merkezi panosunu yüklemeyle ilgili sorun yaşıyorsanız, aboneliği Güvenlik Merkezi’ne kaydeden kullanıcı (Güvenlik Merkezi’ni bu abonelikle ilk kez açan kullanıcı) ile veri toplamayı etkinleştirmek isteyen kullanıcının abonelikte *Sahip* veya *Katkıda Bulunan* rolüne sahip olduğundan emin olun. O andan itibaren, abonelikte *Okuyucu* rolüne sahip kullanıcılar da pano/uyarılar/öneri/ilke sayfasını görebilir.

## <a name="contacting-microsoft-support"></a>Microsoft Destek ile iletişim kurma
Bazı sorunlar bu makalede verilen yönergeler kullanılarak tanımlanabilirken, bazılarını Güvenlik Merkezi genel [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)’nda da bulabilirsiniz. Ancak daha fazla sorun giderme adımı gerekirse aşağıda gösterildiği gibi **Azure Portal**’ı kullanarak yeni bir destek isteği açabilirsiniz: 

![Microsoft Destek](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz


