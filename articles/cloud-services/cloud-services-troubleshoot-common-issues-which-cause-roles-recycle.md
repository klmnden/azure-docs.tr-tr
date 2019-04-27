---
title: Bulut hizmeti rollerinin geri dönüşüme uğramasının yaygın nedenleri | Microsoft Docs
description: Aniden geri dönüştüren bir bulut hizmeti rolü önemli kapalı kalma süresi yaşanabilir. Kapalı kalma sürelerini azaltmaya yardımcı olabilecek dönüştürülmesi, rollerin neden bazı yaygın sorunlar aşağıda verilmiştir.
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
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 2a9214b918883e493ebe5c93fc7f56e7ce9c77ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60652222"
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Rollerin geri dönüştürülmesine neden olan yaygın sorunlar
Bu makalede, bazı yaygın nedenleri dağıtım sorunlarını ele alır ve bu sorunları çözmenize yardımcı olacak sorun giderme ipuçları sağlar. Bir uygulama ile bir sorun olduğunu rol örneği başlatılamıyor veya başlatılıyor, meşgul ve durdurma durumlar arasında döngüleri göstergesidir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Eksik çalışma zamanı bağımlılıkları
Herhangi bir rolü, uygulamanızda dayanıyorsa .NET Framework veya Azure parçası değil derleme yönetilen kitaplığı, bu derleme uygulama paketinde açıkça eklemeniz gerekir. Diğer Microsoft çerçeveleri varsayılan olarak Azure üzerinde kullanılabilir olmadığını aklınızda bulundurun. Rolünüz bu tür bir framework dayanıyorsa, bu derlemeler için uygulama paketi eklemeniz gerekir.

Derleme ve uygulamanızı paketlemek için önce aşağıdakileri doğrulayın:

* Visual studio kullanıyorsanız, emin **Yereli Kopyala** özelliği **True** için Azure SDK'sı veya .NET Framework parçası değil, projenizdeki her başvurulan derleme.
* Derleme ögesi kullanılmayan tüm derlemelerde web.config dosyasına başvurmuyor emin olun.
* **Derleme eylemi** her bir .cshtml dosya kümesine **içerik**. Bu dosyaları pakette doğru görünür ve diğer başvurulan dosyaları pakette görüntülenmesini etkinleştirir sağlar.

## <a name="assembly-targets-wrong-platform"></a>Derleme hedefleri yanlış platform
Azure, bir 64-bit ortamıdır. Bu nedenle, bir 32 bit hedef için derlenmiş .NET derlemelerini, Azure üzerinde çalışmaz.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Başlatma veya durdurma sırasında rol yakalanamayan özel durum oluşturur
Yöntemleri tarafından oluşturulan özel durumları [RoleEntryPoint] içeren sınıf [OnStart], [OnStop], ve [çalıştırma] yöntemler, İşlenmeyen özel durumları olan. Aşağıdaki yöntemlerden birini işlenmeyen bir özel durum oluşursa, rol geri dönüşümü. Rol sürekli geri dönüştürüldüğünde, başlamaya her zaman işlenmeyen özel durum atma.

## <a name="role-returns-from-run-method"></a>Rol Run yönteminden döndürülüyor
[Çalıştırma] yöntemi süresiz olarak çalıştırmak için tasarlanmıştır. Kodunuzu kılıyorsa [çalıştırma] yöntemi, uyku süresiz olarak. Varsa [çalıştırma] yöntemi döndürür, rol geri dönüştürülür.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Yanlış DiagnosticsConnectionString ayarı
Uygulama Azure Tanılama'yı kullanıyorsa, hizmet yapılandırma dosyasını belirtmelisiniz `DiagnosticsConnectionString` yapılandırma ayarı. Bu ayar Azure depolama hesabınıza bir HTTPS bağlantısı belirtmeniz gerekir.

Emin olmak için `DiagnosticsConnectionString` uygulama paketinizi Azure'a dağıtmadan önce ayarlarının doğru olduğundan, aşağıdakileri doğrulayın:  

* `DiagnosticsConnectionString` Geçerli bir depolama hesabına azure'da noktaları ayarlama.  
  Varsayılan olarak, bu ayar, uygulama paketini dağıtmadan önce açıkça bu ayarı değiştirmelisiniz şekilde benzetilmiş bir depolama hesabına işaret eder. Bu ayarın değiştirilmemesi, rol örneği tanı İzleyicisi çalıştığında bir özel durum oluşturulur. Bu rol örneği süresiz olarak geri dönüştürülmesine neden olabilir.
* Bağlantı dizesi, aşağıda belirtilen [biçimi](../storage/common/storage-configure-connection-string.md). (Protokolü HTTPS belirtilmesi gerekir.) Değiştirin *MyAccountName* depolama hesabınızın adıyla ve *MyAccountKey* erişim anahtarınızı:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Microsoft Visual Studio için Azure araçları kullanarak uygulama geliştiriyorsanız, bu değeri ayarlamak için özellik sayfalarını kullanabilirsiniz.

## <a name="exported-certificate-does-not-include-private-key"></a>Dışarı aktarılan sertifika özel anahtar içermiyor
Bir web rolü SSL altında çalıştırmak için dışarı aktarılan yönetim sertifikası özel anahtar içerdiğinden emin olmanız gerekir. Kullanırsanız *Windows Sertifika Yöneticisi* sertifikasını dışarı aktarmak için seçtiğinizden emin olun **Evet** için **özel anahtarı dışarı** seçeneği. Sertifika şu anda desteklenen tek biçim PFX biçiminde dışarı aktarılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazlasını görüntüle [sorun giderme makaleleri](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) bulut Hizmetleri için.

Daha fazla rol senaryolarında geri dönüştürme görüntülemek [Kevin Williamson'ın blog dizisini](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Çalıştırma]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
