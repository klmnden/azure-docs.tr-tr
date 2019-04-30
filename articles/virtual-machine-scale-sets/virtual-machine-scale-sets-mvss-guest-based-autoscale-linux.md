---
title: Bir Linux ölçek kümesi şablonunun Konuk ölçümlerle Azure otomatik ölçeklendirme kullanın. | Microsoft Docs
description: Bilgi nasıl Konuk ölçümleri kullanarak bir Linux sanal makine ölçek kümesi şablonu otomatik ölçeklendirme
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: manayar
ms.openlocfilehash: deddcc8623803f9d003f3fafcef5252ebd34b813
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803351"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Bir Linux ölçek kümesi şablonunuzda Konuk ölçümleri kullanan otomatik ölçeklendirme

Ölçek kümeleri ve sanal makinelerden toplanan ölçümleri azure'da iki tür vardır: bazı VM konaktan gelen ve diğer Konuk VM gelir. Standart CPU, disk ve ağ ölçümleri kullanıyorsanız yüksek düzeyde, ardından konak ölçümlerini uygun olabilirsiniz. Ancak, daha büyük bir seçim ölçüm gerekiyorsa, Konuk ölçümleri bir daha iyi uyum olabilirsiniz. İkisi arasındaki farklar bir göz atalım:

Konak ölçümlerini daha basit ve daha güvenilir. VM, ana bilgisayar tarafından toplandığından Konuk ölçümleri yüklemenizi gerektirir ancak bunlar ek kurulum gerektirmeyen [Windows Azure tanılama uzantısını](../virtual-machines/windows/extensions-diagnostics-template.md) veya [Linux Azure tanılama uzantısını](../virtual-machines/linux/diagnostic-extension.md)Konuk VM içinde. Konuk ölçümleri yerine konak ölçümlerini kullanmak için yaygın nedenlerinden biri Konuk ölçümleri konak ölçümlerini daha büyük bir seçim ölçüm sağlamaktır. Böyle yalnızca konuk ölçümleri kullanılabilen bellek tüketimi ölçümleri bir örnektir. Desteklenen ana ölçümleri listelenen [burada](../azure-monitor/platform/metrics-supported.md), ve yaygın olarak kullanılan Konuk ölçümleri listelenen [burada](../azure-monitor/platform/autoscale-common-metrics.md). Bu makalede nasıl değiştirileceğini gösterir [en düşük uygun ölçek kümesi şablonunu](./virtual-machine-scale-sets-mvss-start.md) Linux ölçek kümeleri için konuk ölçümlerine göre otomatik ölçeklendirme kurallarını kullanmak için.

## <a name="change-the-template-definition"></a>Şablon tanımı değiştirme

En düşük uygun ölçek kümesi şablonunun görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), ve şablonun konuk tabanlı otomatik ölçeklendirme içeren Linux ölçek kümesi dağıtımı ayarlamak için görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Bu şablon oluşturmak için kullanılan fark inceleyelim (`git diff minimum-viable-scale-set existing-vnet`) parça parça:

İlk olarak, parametrelerini ekleyin `storageAccountName` ve `storageAccountSasToken`. Tanılama Aracısı ölçüm verileri depolar. bir [tablo](../cosmos-db/table-storage-how-to-use-dotnet.md) bu depolama hesabında. Linux tanılama Aracısı itibaren sürüm 3.0 depolama erişim anahtarı kullanarak artık desteklenmiyor. Bunun yerine, bir [SAS belirteci](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

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

Ardından, Ölçek kümesini değiştirme `extensionProfile` tanılama uzantısını eklenecek. Bu yapılandırmada, ölçümleri depolamak için kullanmak, ölçümleri yanı sıra depolama hesabı ve SAS belirteci toplamak için ölçek kaynak Kimliğini belirtin. (Bu durumda, yüzde kullanılan bellek) izlemek için hangi ölçümleri ne sıklıkta ölçümleri (Bu durumda dakika başı) toplanır ve belirtin. Ölçümleri yüzde dışında kullanılan bellek ve bu yapılandırma hakkında daha ayrıntılı bilgi için bkz: [bu belgeleri](../virtual-machines/linux/diagnostic-extension.md).

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

Son olarak, ekleme bir `autoscaleSettings` kaynak otomatik ölçeklendirme yapılandırmak için bu ölçümleri temel. Bu kaynağın bir `dependsOn` ölçek başvuran yan tümcesi ayarlayın ölçek kümesini otomatik ölçeklendirme, denemeden önce mevcut olduğundan emin olun. Otomatik ölçeklendirme için farklı bir ölçümü üzerinde tercih ederseniz, kullanacağınız `counterSpecifier` tanılama uzantısını yapılandırmasından `metricName` otomatik ölçeklendirme yapılandırması. Otomatik ölçeklendirme yapılandırması hakkında daha fazla bilgi için bkz. [otomatik ölçeklendirme en iyi](..//azure-monitor/platform/autoscale-best-practices.md) ve [Azure İzleyici REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn931928.aspx).

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
