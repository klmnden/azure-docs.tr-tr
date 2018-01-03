---
title: "Bir Linux ölçek kümesi şablonunda Konuk ölçümlerle Azure otomatik ölçeklendirme kullanın | Microsoft Docs"
description: "Bilgi Linux sanal makine ölçek kümesi bir şablonda Konuk ölçümleri kullanarak otomatik ölçeklendirme yapma"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 8e822d83dd3bafabfea60ad50224c87df226bdc6
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Bir Linux ölçek kümesi şablonunda Konuk ölçümleri kullanarak otomatik ölçeklendirme

Azure VM'lerin toplanır ve ölçek kümeleri ölçümlerini iki tür vardır: bazı VM ana bilgisayardan gelen ve diğer Konuk sanal gelir. Standart CPU, disk ve ağ ölçümleri kullanıyorsanız, yüksek düzeyde, ardından ana ölçümleri iyi bir tercihtir olabilirsiniz. Ancak, daha büyük bir seçim ölçümleri gerekiyorsa, daha sonra konuk büyük olasılıkla daha iyi bir uyum ölçümleridir. İkisi arasındaki farklar bir göz atalım:

Ana ölçümleri daha basit ve daha güvenilirdir. VM, ana bilgisayar tarafından toplanan çünkü Konuk ölçümleri yüklemenizi gerektirir ancak bunlar ek kurulum gerektirmeyen [Windows Azure tanılama uzantısını](../virtual-machines/windows/extensions-diagnostics-template.md) veya [Linux Azure tanılama uzantısını](../virtual-machines/linux/diagnostic-extension.md)VM konuk. Konuk ölçümleri yerine ana ölçümleri kullanmak için bir ortak neden Konuk ölçümleri ölçümleri ana ölçümleri daha büyük seçimi sağlamaktır. Böyle bir yalnızca konuk ölçümleri kullanılabilir bellek tüketimi ölçümlerini örnektir. Desteklenen ana ölçümleri listelenen [burada](../monitoring-and-diagnostics/monitoring-supported-metrics.md), ve yaygın olarak kullanılan Konuk ölçümleri listelenen [burada](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Bu makalede nasıl değiştirileceğini gösterir [minimum uygun ölçek kümesi şablonu](./virtual-machine-scale-sets-mvss-start.md) Linux ölçek kümeleri için konuk ölçümleri göre otomatik ölçeklendirme kurallarını kullanmak için.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirin

En düşük uygun ölçek kümesi şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), ve Linux ölçek dağıtma ile konuk tabanlı otomatik ölçeklendirme kümesi için şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Bu şablon oluşturmak için kullanılan fark inceleyelim (`git diff minimum-viable-scale-set existing-vnet`) tarafından parça parça:

İlk olarak, parametreleri ekleyin `storageAccountName` ve `storageAccountSasToken`. Tanılama Aracı ölçüm verileri depolayan bir [tablo](../cosmos-db/table-storage-how-to-use-dotnet.md) bu depolama hesabında. Linux Tanılama Aracı sürüm 3.0 sürümünden itibaren bir depolama erişim tuşunu kullanarak artık desteklenmemektedir. Bunun yerine, bir [SAS belirteci](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

Ardından, Ölçek kümesini değiştirme `extensionProfile` tanılama uzantısını eklenecek. Bu yapılandırmada, ölçümleri depolamak için kullanmak üzere, ölçümleri yanı sıra depolama hesabı ve SAS belirteci toplamak için ölçek kaynak Kimliğini belirtin. Ölçümleri (Bu durumda dakikada) ne sıklıkta toplanır ve (Bu durumda, yüzde kullanılan bellek) izlemek için hangi ölçümleri belirtin. Bu yapılandırma hakkında daha ayrıntılı bilgi ve ölçümleri yüzde dışında kullanılan bellek için bkz: [bu belgeleri](../virtual-machines/linux/diagnostic-extension.md).

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

Son olarak, ekleme bir `autoscaleSettings` otomatik ölçeklendirme yapılandırmak için kaynak tabanlı bu ölçümleri. Bu kaynak bir `dependsOn` ölçek başvuran yan tümcesi ayarlanmış ölçek kümesi için otomatik ölçeklendirme, denemeden önce mevcut olduğundan emin olun. Üzerinde otomatik ölçeklendirme için farklı bir ölçümü seçerseniz, kullanacağınız `counterSpecifier` tanılama uzantısını yapılandırmasından `metricName` otomatik ölçeklendirme yapılandırması. Otomatik ölçeklendirme yapılandırma hakkında daha fazla bilgi için bkz: [otomatik ölçeklendirme en iyi yöntemler](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) ve [Azure İzleyici REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn931928.aspx).

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
