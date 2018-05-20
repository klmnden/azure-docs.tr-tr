---
title: Windows güvenliği kullanarak Windows çalıştıran bir kümeye güvenli | Microsoft Docs
description: Windows güvenliği kullanarak Windows üzerinde çalışan tek başına kümedeki düğüm düğümü ve istemci düğümü güvenlik yapılandırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 0f0df7883b25344560514491c08af3eadf872ffb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Windows tek başına bir kümede Windows güvenliği kullanarak güvenli hale getirme
Bir Service Fabric kümesi yetkisiz erişimi önlemek için küme güvenlik altına almanız gerekir. Küme üretim iş yükleri çalıştığında güvenlik özellikle önemlidir. Bu makalede Windows güvenliği kullanarak düğümü düğümü ve istemci düğümü güvenliği yapılandırmak nasıl *ClusterConfig.JSON* dosya.  İşleme için yapılandırma güvenlik adımı, karşılık gelen [Windows üzerinde çalışan tek başına küme oluşturmak](service-fabric-cluster-creation-for-windows-server.md). Service Fabric Windows güvenliği nasıl kullandığı hakkında daha fazla bilgi için bkz: [küme güvenlik senaryoları](service-fabric-cluster-security.md).

> [!NOTE]
> Bir güvenlik seçim diğerine hiçbir Küme yükseltme olduğundan düğümü düğümü güvenlik seçimini dikkatle düşünmelisiniz. Güvenlik seçimini değiştirmek için tam küme yeniden gerekir.
>
>

## <a name="configure-windows-security-using-gmsa"></a>GMSA kullanarak Windows güvenliği yapılandırma  
Örnek *ClusterConfig.gMSA.Windows.MultiMachine.JSON* yapılandırma dosyası ile indirilen [Microsoft.Azure.ServiceFabric.WindowsServer.<version> .zip](http://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme paketi içeren Windows güvenliği kullanarak yapılandırmak için bir şablon [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```  
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "[gMSA Identity]", 
                "ClusterSPN": "[Registered SPN for the gMSA account]",
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Kümesine *Windows* Windows güvenliği düğümler iletişimi etkinleştirmek için.  | 
| ServerCredentialType |Kümesine *Windows* Windows güvenliği istemcisi düğümü iletişimi etkinleştirmek için. |  
| WindowsIdentities |Küme ve istemci kimliklerini içerir. |  
| ClustergMSAIdentity |Düğümü düğümü güvenliğini yapılandırır. Bir grup yönetilen hizmet hesabı. |  
| ClusterSPN |GMSA hesabı için kayıtlı SPN|  
| ClientIdentities |İstemcisi düğümü güvenliğini yapılandırır. İstemci kullanıcı hesapları dizisi. | 
| Kimlik |Etki alanı kullanıcısı, istemci kimliği için etki alanı\kullanıcı adı ekleyin. |  
| IsAdmin |Etki alanı kullanıcısı yönetici istemci erişimi ya da kullanıcı istemci erişimi için yanlış olduğunu belirtmek için true olarak ayarlanır. |  

[Düğüm güvenlik düğüme](service-fabric-cluster-security.md#node-to-node-security) ayarlayarak yapılandırılmış **ClustergMSAIdentity** service fabric gerektiği zaman gMSA altında çalıştırmak. Düğümler arasındaki güven ilişkileri oluşturmak için bunlar birbirinden haberdar olmanız gerekir. Bu iki farklı yolla gerçekleştirilebilir: Grup yönetilen hizmet kümedeki tüm düğümleri içeren hesabı veya kümedeki tüm düğümleri içeren etki alanı makine grubu belirtin. Kullanmanızı öneririz [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) yaklaşım, özellikle büyük kümeler (10'dan fazla düğüm) veya büyütür veya küçültür olasılığı kümeleri.  
Bu yaklaşım eklemek ve üyeleri kaldırmak için erişim haklarını küme yöneticileri verilmiş bir etki alanı grubu oluşturulmasını gerektirmez. Bu hesaplar, otomatik parola yönetimi için de yararlıdır. Daha fazla bilgi için bkz: [Grup yönetilen hizmet hesapları ile çalışmaya başlama](http://technet.microsoft.com/library/jj128431.aspx).  
 
[Düğüm güvenlik istemciye](service-fabric-cluster-security.md#client-to-node-security) kullanılarak yapılandırılmış **ClientIdentities**. Bir istemci ve küme arasında güven sağlamak için hangi istemci, güvenilir kimlikleri bilmeniz küme yapılandırmanız gerekir. Bu iki farklı şekillerde yapılabilir: bağlanın veya bağlanabilmesi için etki alanı düğümü kullanıcıları belirtmek etki alanı grubu kullanıcıları belirtin. Service Fabric Service Fabric kümeye bağlı istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Erişim denetimi, belirli türde bir küme işlemleri farklı küme daha güvenli hale getirme kullanıcı grupları için erişimi sınırlamak Küme Yöneticisi yeteneği sağlar.  Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır. Erişim denetimleri hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).  
 
Aşağıdaki örnek **güvenlik** bölüm Windows güvenliği kullanarak gMSA yapılandırır ve belirten makinelerinizde *ServiceFabric.clusterA.contoso.com* gMSA küme ve o parçasıolan *CONTOSO\usera* yönetici istemci erişimi vardır:  
  
```  
"security": {
    "ClusterCredentialType": "Windows",            
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a>Makine grubu kullanarak Windows güvenliği yapılandırma  
Bu model kullanım dışıdır. Yukarıdaki ayrıntılı olarak gMSA kullanmak için önerilir. Örnek *ClusterConfig.Windows.MultiMachine.JSON* yapılandırma dosyası ile indirilen [Microsoft.Azure.ServiceFabric.WindowsServer.<version> .zip](http://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme paketi, Windows güvenliği yapılandırmak için bir şablonu içerir.  Windows güvenliği yapılandırılmıştır **özellikleri** bölümü: 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Kümesine *Windows* Windows güvenliği düğümler iletişimi etkinleştirmek için.  | 
| ServerCredentialType |Kümesine *Windows* Windows güvenliği istemcisi düğümü iletişimi etkinleştirmek için. |  
| WindowsIdentities |Küme ve istemci kimliklerini içerir. |  
| ClusterIdentity |Makine grubu adı, domain\machinegroup, düğümü düğümü güvenlik yapılandırmak için kullanın. |  
| ClientIdentities |İstemcisi düğümü güvenliğini yapılandırır. İstemci kullanıcı hesapları dizisi. |  
| Kimlik |Etki alanı kullanıcısı, istemci kimliği için etki alanı\kullanıcı adı ekleyin. |  
| IsAdmin |Etki alanı kullanıcısı yönetici istemci erişimi ya da kullanıcı istemci erişimi için yanlış olduğunu belirtmek için true olarak ayarlanır. |  

[Düğüm güvenlik düğüme](service-fabric-cluster-security.md#node-to-node-security) ayarı kullanılarak yapılandırılır **ClusterIdentity** bir Active Directory etki alanı içinde bir makine grubu kullanmak istiyorsanız. Daha fazla bilgi için bkz: [Active Directory'de bir makine grubu oluştur](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[İstemcisi düğümü güvenlik](service-fabric-cluster-security.md#client-to-node-security) kullanılarak yapılandırılan **ClientIdentities**. Bir istemci ve küme arasında güven sağlamak için kümenin küme güvenebileceği kimlikleri istemci bilmeniz için yapılandırmanız gerekir. İki farklı yolla güven kurabilir:

- Bağlanabilmesi için etki alanı grubu kullanıcıları belirtin.
- Bağlanabilmesi için etki alanı düğümü kullanıcıları belirtin.

Service Fabric Service Fabric kümeye bağlı istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Erişim denetimi, belirli türde bir küme işlemleri için farklı kullanıcı grupları, küme daha güvenli kılan erişimi sınırlamak Küme Yöneticisi sağlar.  Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır.  

Aşağıdaki örnek **güvenlik** bölüm Windows güvenliği yapılandırır, belirleyen makinelerinizde *ServiceFabric/clusterA.contoso.com* kümesinin parçası olan ve bu belirtir*CONTOSO\usera* yönetici istemci erişimi vardır:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric bir etki alanı denetleyicisinde dağıtılmalıdır değil. ClusterConfig.json etki alanı denetleyicisinin IP adresine bir makine grubu kullanırken içermez ve Grup yönetilen hizmet hesabı (gMSA) olduğundan emin olun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Windows Güvenlik yapılandırdıktan sonra *ClusterConfig.JSON* dosya, küme oluşturma işlemine devam [Windows üzerinde çalışan tek başına küme oluşturmak](service-fabric-cluster-creation-for-windows-server.md).

Düğümü düğümü nasıl güvenlik, istemci düğümü güvenlik ve rol tabanlı erişim denetimi, bkz: hakkında daha fazla bilgi için [küme güvenlik senaryoları](service-fabric-cluster-security.md).

Bkz: [güvenli kümeye Bağlan](service-fabric-connect-to-secure-cluster.md) PowerShell veya FabricClient kullanarak bağlanma örnekler.
