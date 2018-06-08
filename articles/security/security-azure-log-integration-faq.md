---
title: Azure günlük tümleştirme ile ilgili SSS | Microsoft Docs
description: Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
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
ms.date: 06/06/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 31524f24eea97082c8b148ba0a65bbeb24717039
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839330"
---
# <a name="azure-log-integration-faq"></a>Azure günlük tümleştirme hakkında SSS

Bu makalede Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS) yanıtlar.

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 01/06/2019 tarafından kullanım dışı kalacaktır.  AzLog yüklemeleri 27 Haz 2018 tarafından devre dışı bırakılacak. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında yönergeler için [SIEM araçları ile tümleştirmek için kullanım Azure İzleyicisi](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/preview/?cdn=disable) 

Azure günlük tümleştirme, şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlere Azure kaynaklarınızı ham günlüklerinden tümleştirmek için kullanabileceğiniz bir Windows işletim sistemi hizmetidir. Bu tümleştirme, tüm varlıklarınızı, şirket içi veya bulutta birleştirilmiş bir Pano sağlar. Ardından toplama, bağıntılı, çözümleyebilir ve uygulamalarınız ile ilişkili güvenlik olayları için uyarı.

SIEM satıcınızın Azure İzleyici Bağlayıcısı'nı kullanarak ve bunlar aşağıdaki Azure günlükleri tümleştirmek için tercih edilen yöntem olduğu [yönergeleri](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). SIEM satıcınıza Azure İzleyici için bir bağlayıcı sağlamıyorsa (SIEM'iniz Azure günlük tümleştirme tarafından destekleniyorsa) bu tür bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük tümleştirme geçici bir çözüm olarak kullanmak mümkün olabilir.

## <a name="is-the-azure-log-integration-software-free"></a>Azure günlük tümleştirme yazılım ücretsiz mi?

Evet. Azure günlük tümleştirme yazılım hiçbir ücret yoktur.

## <a name="where-is-azure-log-integration-available"></a>Burada Azure günlük tümleştirmesi var mı?

Azure ticari ve Azure kamu şu anda kullanılabilir değil ve Çin veya Almanya kullanılabilir değil.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Azure günlük tümleştirme Azure VM günlükleri çekme depolama hesapları nasıl görebilirim?

Komutu çalıştırın **AzLog kaynağı listesi**.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Azure günlük tümleştirme günlükleri arasındadır hangi aboneliğin nasıl anlayabilirim?

Yerleştirilir denetim günlüklerini durumunda **AzureResourcemanagerJson** dizinleri, abonelik kimliği: günlük dosyası adı. Bu, aynı zamanda günlükleri için geçerlidir **AzureSecurityCenterJson** klasör. Örneğin:

20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Azure Active Directory denetim günlüklerini Kiracı kimliği adının bir parçası olarak içerir.

Bir event hub'ından okuma tanılama günlükleri, abonelik kimliği adının bir parçası olarak dahil etmeyin. Bunun yerine, olay hub'ı kaynağı oluşturmanın bir parçası belirtilen kolay ad içerirler. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Proxy yapılandırmasını nasıl güncelleştirebilir miyim?

Proxy ayarı Azure depolama erişime doğrudan izin vermiyorsa, açmak **AZLOG. EXE. CONFIG** dosyasını **c:\Program Files\Microsoft Azure günlük tümleştirme**. Dosyasını içerecek şekilde güncelleştirmek **defaultProxy** proxy adresine sahip bir bölüm, kuruluşunuzun. Güncelleştirme tamamlandıktan sonra durdurmak ve komutları kullanarak hizmeti başlatmak **net Durdur AzLog** ve **net Başlat AzLog**.

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

Abonelik kimliği için kolay ad kaynağı eklenirken ekleyin:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
Abonelik kimliği de dahil olmak üzere aşağıdaki meta verileri XML olay vardır:

![Olay XML][1]

## <a name="error-messages"></a>Hata iletileri
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Komutu çalıştırdığınızda ı ```AzLog createazureid```, aşağıdaki hata neden alabilirim?

Hata:

  *AAD uygulama - oluşturulamadı 72f988bf-86f1-41af-91ab-2d7cd011db37-neden Kiracı = ileti 'Yasak' - ' işlemi tamamlamak için yeterli ayrıcalığa sahip.' =*

**Azlog createazureid** komut çalışır Azure oturum açma erişimi abonelikler için tüm Azure AD kiracılarıyla bir hizmet sorumlusu oluşturmak. Azure oturum açma bilgilerinizi yalnızca Konuk kullanıcı olarak Azure AD Kiracı ise, komut "işlemi tamamlamak için yeterli ayrıcalıklara." başarısız olur. Kiracı yönetici kullanıcı Kiracı olarak hesabınızı eklemek için isteyin.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>Komutu çalıştırdığınızda ı **azlog yetkilendirmek**, aşağıdaki hata neden alabilirim?

Hata:

  *Uyarı rol ataması - AuthorizationFailed oluşturma: istemci janedo@microsoft.com' olan nesne kimliği 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' kapsamı üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok ' / Abonelikleri / 70d 95299 d689 4c 97-b971-0d8ff0000000'.*

**Azlog yetkilendirmek** komut Azure AD hizmet sorumlusu okuyucu rolüne atar (ile oluşturulan **azlog createazureid**) sağlanan aboneliklere. Azure oturum açma bir ortak yönetici veya abonelik sahibi değilse, bir "Yetkilendirme başarısız oldu" hata iletisiyle başarısız olur. Azure rol tabanlı erişim denetimi (RBAC) ortak yönetici veya sahibi bu eylemi tamamlamak için gereklidir.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Denetim günlüğüne özellikleri tanımını nereden bulabilirim?

Bkz:

* [Azure Resource Manager ile işlemlerini denetleme](../azure-resource-manager/resource-group-audit.md)
* [Bir abonelikte Azure İzleyici REST API management olayları listeler](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarılarını ayrıntıları nereden bulabilirim?

Bkz: [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Nasıl VM Tanılama ile toplanan değişiklik yapabilirsiniz?

Ayrıntıları almak nasıl değiştirmek ve Azure tanılama yapılandırması ayarlamak için bkz: [kullanım Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için PowerShell](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aşağıdaki örnek, Azure Tanılama yapılandırmasını alır:

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Aşağıdaki örnek, Azure Tanılama yapılandırmasını değiştirir. Bu yapılandırmada, yalnızca olay kimliği 4624 ve olay kimliği 4625 güvenlik olay günlüğü ' toplanır. Microsoft Antimalware Azure olayları için sistem olay günlüğü ' toplanır. XPath ifadeleri kullanımı hakkında daha fazla bilgi için bkz: [tüketen olayları](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Aşağıdaki örnek, Azure tanılama yapılandırması ayarlar:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Değişiklikleri yaptıktan sonra doğru olayları toplanır emin olmak için depolama hesabı denetleyin.

Yükleme ve yapılandırma sırasında herhangi bir sorun varsa, lütfen açık bir [destek isteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Seçin **günlük tümleştirme** destek isteyen hizmet olarak.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Ağ İzleyicisi günlüklerini my SIEM tümleştirmek için Azure günlük tümleştirme kullanabilir miyim?

Azure Ağ İzleyicisi günlük bilgileri büyük miktarlarda oluşturur. Bu günlükler için bir SIEM gönderilmek üzere düşünülmemiştir. Yalnızca desteklenen hedef Ağ İzleyicisi günlükleri için bir depolama hesabıdır. Bu günlükler okuma ve bir SIEM için kullanılabilir hale getirme Azure günlük tümleştirme desteklemez.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
