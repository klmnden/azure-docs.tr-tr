---
title: Azure günlük Tümleştirmesi ile ilgili SSS | Microsoft Docs
description: Bu makalede, Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 9f7d482b088003e3800debb2db9f6f26bda1672a
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298181"
---
# <a name="azure-log-integration-faq"></a>Azure günlük tümleştirmesi hakkında SSS

Bu makalede, Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS) yanıtlarını.

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanımdan kaldırılacaktır. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirmesi, Azure kaynaklarınızı ham günlüklerinden, şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleriyle tümleştirmek için kullanabileceğiniz bir Windows işletim sistemi hizmetidir. Bu tümleştirme, tüm varlıklarınız için şirket içinde veya bulutta birleştirilmiş bir Pano sağlar. Ardından toplayın, ilişkilendirin, çözümleyebilir ve uygulamalarınızla ilişkili güvenlik olayları için uyarı.

SIEM satıcınızın Azure İzleyici Bağlayıcısı'nı kullanarak ve bunlar aşağıdaki Azure günlük tümleştirme için tercih edilen yöntem olan [yönergeleri](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Azure İzleyici için bir bağlayıcı SIEM satıcınıza sağlamıyorsa (sıem sistemlerinizden alınabileceği gibi Azure günlük tümleştirmesi tarafından destekleniyorsa) bu tür bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük tümleştirmesi geçici bir çözüm kullanmanız mümkün olabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="is-the-azure-log-integration-software-free"></a>Azure günlük tümleştirmesi yazılım ücretsiz mi?

Evet. Azure günlük tümleştirmesi yazılımı için ek ücret yoktur.

## <a name="where-is-azure-log-integration-available"></a>Azure günlük tümleştirmesi nerede kullanılabilir?

Azure ticari ve Azure kamu şu anda kullanılabilir ve Çin ya da Almanya kullanılabilir değil.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Azure günlük tümleştirmesi Azure VM günlüklerini çekme depolama hesaplarını nasıl görebilirim?

Komutunu çalıştırın **AzLog kaynağı listesi**.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Azure günlük tümleştirmesi günlükleri arasındadır hangi aboneliği nasıl anlayabilirim?

Yerleştirilir denetim günlüklerini söz konusu olduğunda **AzureResourcemanagerJson** dizinleri, abonelik kimliği: günlük dosyası adında. Bu ayrıca günlükler için geçerlidir **AzureSecurityCenterJson** klasör. Örneğin:

20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Azure Active Directory denetim günlüklerini adının bir parçası olarak Kiracı Kimliğini içerir.

Bir olay hub'ından okumak tanılama günlükleri, abonelik kimliği adının bir parçası olarak içermez. Bunun yerine, olay hub'ı kaynağı oluşturmanın bir parçası belirtilen kolay ad içerirler. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Proxy yapılandırması'nı nasıl güncelleştirebilirim?

Proxy ayarı Azure depolama erişimi doğrudan sağlamıyorsa açın **AZLOG. EXE. CONFIG** dosyası **c:\Program Files\Microsoft Azure günlük tümleştirmesi**. Dosyasını içerecek şekilde güncelleştirmek **defaultProxy** proxy adresine sahip bir bölüm, kurumunuzun. Güncelleştirme tamamlandıktan sonra durdurup hizmet komutlarını kullanarak **net stop AzLog** ve **net Başlat AzLog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress="http://127.0.0.1:8888"
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Windows olayları abonelik bilgileri nasıl görebilirim?

Abonelik kimliği, kaynak eklenirken bir hata için kolay ad ekleyin:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
Abonelik kimliği de dahil olmak üzere aşağıdaki meta verileri XML olay vardır:

![Event XML][1]

## <a name="error-messages"></a>Hata iletileri
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Komutu çalıştırdığınızda miyim ```AzLog createazureid```, neden şu hatayı alıyorum?

Hata:

  *AAD uygulaması - oluşturulamadı Kiracı 72f988bf-86f1-41af-91ab-2d7cd011db37-nedeni 'Yasak' - ileti = 'işlemi tamamlamak için yeterli ayrıcalık yok.' =*

**Azlog createazureid** Azure oturum açma erişimi olan abonelikler için tüm Azure AD kiracılarıyla bir hizmet sorumlusu oluşturmak komut çalışır. Yalnızca Konuk kullanıcı, Azure AD kiracısı olarak Azure giriş bilgileriniz ise "işlemi tamamlamak için yeterli ayrıcalıklara." komutu başarısız Kiracı Yöneticisi olarak kiracıda bir kullanıcı hesabınızı eklemek için isteyin.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>Komutu çalıştırdığınızda miyim **azlog yetkilendirmek**, neden şu hatayı alıyorum?

Hata:

  *Uyarı: rol ataması - AuthorizationFailed oluşturuluyor İstemci janedo\@microsoft.com' nesne kimliği 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' kapsamı üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok. ' /subscriptions/ 70d 95299 d689 4c 97 b971 0d8ff0000000'.*

**Azlog yetkilendirmek** komutu için Azure AD hizmet sorumlusu okuyucu rolüne atar (ile oluşturulan **azlog createazureid**) sağlanan abonelikler. Azure oturum açma, bir ortak yönetici veya abonelik sahibi değil, bir "Yetkilendirme başarısız oldu" hata iletisiyle başarısız olur. Azure rol tabanlı erişim denetimi (RBAC) ortak yönetici veya sahibi bu eylemi tamamlamak için gereklidir.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Denetim günlüğünde özelliklerini tanımını nerede bulabilirim?

Bkz.

* [Azure Resource Manager ile işlemleri denetleme](../azure-resource-manager/resource-group-audit.md)
* [Bir abonelikte Azure İzleyici REST API Yönetimi olayları listeler](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarılarını ayrıntıları nerede bulabilirim?

Bkz: [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Nasıl VM Tanılama ile toplanan değişiklik yapabilirsiniz?

Nasıl alınacağına yönelik ayrıntılar değiştirebilir ve Azure Tanılama yapılandırmasını ayarlamak için bkz: [PowerShell kullanarak Windows çalıştıran bir sanal makine Azure tanılamayı etkinleştirerek](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aşağıdaki örnek, Azure Tanılama yapılandırmasını alır:

    Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Aşağıdaki örnek, Azure Tanılama yapılandırmasını değiştirir. Bu yapılandırmada güvenlik olay günlüğüne olay kimliği 4624 ve olay kimliği 4625 toplanır. Microsoft Azure etkinlikleri Antimalware sistem olay günlüğüne toplanmadı. XPath ifadeleri kullanımı hakkında ayrıntılı bilgi için bkz. [Consuming Events](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Aşağıdaki örnek, Azure Tanılama yapılandırmasını ayarlar:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Değişiklikleri yaptıktan sonra depolama hesabının doğru olayları toplanır emin olmak için kontrol edin.

Yükleme ve yapılandırma sırasında herhangi bir sorun varsa, lütfen açık bir [destek isteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Seçin **günlük tümleştirmesi** destek isteme hizmet olarak.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Ağ İzleyicisi günlükleri my SIEM ile tümleştirme için Azure günlük tümleştirmesi kullanabilir miyim?

Azure Ağ İzleyicisi, büyük miktarlarda günlük bilgileri oluşturur. Bu günlükler için bir SIEM gönderilmek üzere tasarlanmamıştır. Yalnızca desteklenen hedef Ağ İzleyicisi günlükleri için bir depolama hesabıdır. Azure günlük tümleştirmesi, bu günlükleri okuyan ve bir SIEM için kullanılabilir hale getirme desteklemez.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
