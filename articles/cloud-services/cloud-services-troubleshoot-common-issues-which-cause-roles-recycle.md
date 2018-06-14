---
title: Bulut hizmeti rolleriyle geri dönüştürme yaygın nedenlerini | Microsoft Docs
description: Aniden geri dönüştürüldüğünde bir bulut hizmet rolü önemli kapalı kalma süresi neden olabilir. Kapalı kalma süresini azaltmanıza yardımcı olabilecek dönüştürülmesi, rollerin neden bazı yaygın sorunlar aşağıda verilmiştir.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 7f513a7d3fe44cbbf06855059c87c10ddceef7c9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
ms.locfileid: "23984295"
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Rollerin geri dönüştürülmesine neden olan yaygın sorunlar
Bu makalede, bazı ortak nedenlerinden biri dağıtım sorunlarını ele alır ve bu sorunları çözmenize yardımcı olacak sorun giderme ipuçları verilmektedir. Bir sorun uygulamayla var. rol örneğini başlatmaya hata verdiğinde veya başlatılırken, meşgul ve durdurma durumları arasında geçiş yapar göstergesidir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Eksik çalışma zamanı bağımlılıkları
Herhangi bir rol, uygulamanızda dayalıysa .NET Framework veya Azure parçası olmayan derleme yönetilen kitaplık, bu derleme uygulama paketinde açıkça eklemeniz gerekir. Diğer Microsoft çerçevelerinin varsayılan olarak Azure üzerinde kullanılabilir olmadığını göz önünde bulundurun. Böyle bir Framework'te rolünüze bağlı ise, bu derlemeler için uygulama paketi eklemeniz gerekir.

Derleme ve uygulamanızı paket önce aşağıdakileri doğrulayın:

* Visual studio kullanarak, emin olun **kopya yerel** özelliği ayarlanmış **True** her başvurulan derleme projenizdeki Azure SDK'sı veya .NET Framework parçası değil.
* Web.config dosyası derleme öğesi kullanılmayan tüm derlemelerde başvurmuyor emin olun.
* **Yapı eylemi** her .cshtml dosya kümesine **içerik**. Bu dosyaları pakette doğru görünür ve pakette görünmesi başvurulan diğer dosyaları etkinleştirir sağlar.

## <a name="assembly-targets-wrong-platform"></a>Derleme hedefleri yanlış platformu
Azure 64-bit ortamıdır. Bu nedenle, .NET derleme için bir 32 bit hedef derlenmiş Azure üzerinde çalışmaz.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol başlatma veya durdurma sırasında işlenmeyen özel durum oluşturur
Yöntemleri tarafından oluşturulan özel durumlar [RoleEntryPoint] içeren sınıf [OnStart], [OnStop], ve [çalıştırmak] yöntemleri, İşlenmeyen özel durumlar şunlardır. Aşağıdaki yöntemlerden birini işlenmeyen bir özel durum oluşursa, rol geri. Rol sürekli geri dönüştürme, işlenmeyen bir özel durum başlamaya her zaman atma.

## <a name="role-returns-from-run-method"></a>Run yöntemi rol döndürür
[çalıştırmak] yöntemi süresiz olarak çalışacak şekilde tasarlanmıştır. Kodunuzu kılıyorsa [çalıştırmak] , onu sleep yöntemi süresiz olarak. Varsa [çalıştırmak] yöntemi döndürür, rol geri dönüştürür.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Yanlış DiagnosticsConnectionString ayarı
Uygulama Azure tanılama kullanıyorsa, hizmet yapılandırma dosyası belirtmeniz gerekir `DiagnosticsConnectionString` yapılandırma ayarı. Bu ayar, Azure depolama hesabınız için bir HTTPS bağlantısı belirtmeniz gerekir.

Emin olmak için `DiagnosticsConnectionString` Azure için uygulama paketini dağıtmadan önce ayarlarının doğru olduğundan, aşağıdakileri doğrulayın:  

* `DiagnosticsConnectionString` Azure içindeki geçerli bir depolama hesabına noktaları ayarlama.  
  Varsayılan olarak, bu ayar, uygulama paketini dağıtmadan önce bu ayarı değiştirmek açıkça gerekir böylece benzetilmiş depolama hesabına işaret eder. Bu ayar değiştirmezseniz rol örneği tanı İzleyicisi başlatmayı denediğinde özel durum oluşur. Bu rol örneği süresiz olarak geri dönüşüm neden olabilir.
* Aşağıda, bağlantı dizesi belirtilen [biçimi](../storage/common/storage-configure-connection-string.md). (Protokolü HTTPS belirtilmesi gerekir.) Değiştir *MyAccountName* depolama hesabınızın adıyla ve *MyAccountKey* erişim anahtarı ile:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Microsoft Visual Studio için Azure araçlarını kullanarak, uygulama geliştiriyorsanız, bu değeri ayarlamak için özellik sayfalarını kullanabilirsiniz.

## <a name="exported-certificate-does-not-include-private-key"></a>Dışarı aktarılan sertifika özel anahtar içermiyor
SSL altında web rolü çalıştırmak için dışa aktarılan yönetim sertifikanızı özel anahtar içerdiğinden emin olmanız gerekir. Kullanırsanız *Windows Sertifika Yöneticisi'ni* sertifikasını dışarı aktarmak için seçtiğinizden emin olun **Evet** için **özel anahtarı dışarı** seçeneği. Sertifika şu anda desteklenen tek biçimi PFX biçiminde dışarı aktarılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi görüntülemek [sorun giderme makalelerini](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) bulut Hizmetleri için.

Daha fazla rol senaryolarında geri dönüştürme görüntülemek [Kevin Williamson'ın blog dizisini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[çalıştırmak]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
