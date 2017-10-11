### <a name="app-service-plan"></a>App Service planı
Web uygulamasını barındırmak için hizmet planını oluşturur. Planı aracılığıyla adını sağlayın **hostingPlanName** parametresi. Kaynak grubu için kullanılan aynı konuma plan konumudur. Fiyatlandırma katmanı ve çalışan boyutu belirtilir **sku** ve **workerSize** parametreleri

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

