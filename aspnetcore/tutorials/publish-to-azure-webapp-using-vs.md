---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。"
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: dd31e3a9583a0c152e97ae7cf6b215389298a20c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
/zh-TW

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service

由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs) 及 [Rachel Appel](https://twitter.com/rachelappel) 共同編纂

如果您是使用 Mac，請參閱[從 Visual Studio for Mac 發佈至 Azure](https://blog.xamarin.com/publish-azure-visual-studio-mac/) \(英文\)。

## <a name="set-up"></a>設定

* 如果沒有帳戶，請開啟[免費的 Azure 帳戶](https://aka.ms/K5y5yh)。 

## <a name="create-a-web-app"></a>建立 Web 應用程式

在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]

![檔案功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

完成 [新增專案] 對話方塊：

* 在左窗格中，選取 [.NET Core]。
* 在中央窗格中，選取 [ASP.NET Core Web 應用程式]。
* 選取 [確定]。

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

在 [新增 ASP.NET Core Web 應用程式] 對話方塊中：

* 選取 [Web 應用程式]。
* 選取 [變更驗證]。

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

[變更驗證] 對話方塊會隨即出現。 

* 選取 [個別使用者帳戶]。
* 選取 [確定] 返回 [新增 ASP.NET Core Web 應用程式]，然後再次選取 [確定]。

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio 會建立解決方案。

## <a name="run-the-app"></a>執行應用程式

* 按 CTRL+F5 執行專案。
* 測試 [關於] 和 [連絡人] 連結。

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>註冊使用者

* 選取 [註冊] 並註冊新的使用者。 您可以使用虛構的電子郵件地址。 提交時，頁面會顯示下列錯誤：

    「內部伺服器錯誤: 處理要求時資料庫作業失敗。SQL 例外狀況：無法開啟資料庫。為應用程式資料庫內容套用現有的移轉可能會解決此問題。」*
* 選取 [套用移轉]，並在頁面更新後重新整理頁面。

![內部伺服器錯誤：處理要求時資料庫作業失敗。 SQL 例外狀況：無法開啟資料庫。 為應用程式資料庫內容套用現有的移轉可能會解決此問題。](publish-to-azure-webapp-using-vs/_static/mig.png)

應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。

![在 Microsoft Edge 中開啟的 Web 應用程式。 [註冊] 連結會取代為文字「email@domain.com，您好！」](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

在 [發行] 對話方塊中：

* 選取 [Microsoft Azure App Service]。
* 選取齒輪圖示，然後選取 [建立設定檔]。
* 選取 [建立設定檔]。

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a>建立 Azure 資源

出現 [建立應用程式服務] 對話方塊：

* 輸入訂用帳戶。
* 會填入 [應用程式名稱]、[資源群組] 和 [App Service 方案] 輸入欄位。 您可以保留這些名稱，或變更它們。

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* 選取 [服務] 索引標籤來建立新的資料庫。

* 選取綠色的 **+** 圖示來建立新的 SQL 資料庫

![新增 SQL 資料庫](publish-to-azure-webapp-using-vs/_static/sql.png)

* 在 [設定 SQL 資料庫] 對話方塊上選取 [新增...] 來建立新的資料庫。

![新增 SQL 資料庫與伺服器](publish-to-azure-webapp-using-vs/_static/conf.png)

[設定 SQL Server] 對話方塊會隨即出現。

* 輸入系統管理員使用者名稱與密碼，然後選取 [確定]。 您可以保留預設的**伺服器名稱**。 

> [!NOTE]
> "admin" 不允許作為系統管理員使用者名稱。

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* 選取 [確定]。

Visual Studio 會回到 [建立 App Service] 對話方塊。

* 在 [建立 App Service] 對話方塊上選取 [建立]。

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

Visual Studio 會在 Azure 上建立 Web 應用程式和 SQL Server。 此步驟可能需要幾分鐘的時間。 如需所建立資源的資訊，請參閱[其他資源](#additonal-resources)。

部署完成時，請選取 [設定]：

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/set.png)

在 [發行] 對話方塊的 [設定] 頁面上：

  * 展開 [資料庫] 並選取 [在執行階段使用此連接字串]。
  * 展開 [Entity Framework 移轉] 並選取 [在發行時套用此移轉]。

* 選取 [儲存]。 Visual Studio 會回到 [發行] 對話方塊。 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

按一下 [發行] 。 Visual Studio 會將您的應用程式發行至 Azure。 部署完成時，會在瀏覽器中開啟應用程式。

### <a name="test-your-app-in-azure"></a>在 Azure 中測試應用程式

* 測試 [關於] 和 [連絡人] 連結

* 註冊新的使用者

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>更新應用程式

* 編輯 *Pages/About.cshtml* Razor 頁面，並變更其內容。 例如，您可以修改段落，使其說出 "Hello ASP.NET Core!"：[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* 以滑鼠右鍵按一下專案，然後再次選取 [發行...]。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* 發行應用程式後，請驗證您進行的變更在 Azure 上有效。

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>清除

當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。

* 選取 [資源群組]，然後選取您建立的資源群組。

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* 在 [資源群組] 頁面中，選取 [刪除]。

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* 輸入資源群組的名稱，並選取 [刪除]。 您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。

### <a name="next-steps"></a>後續步驟

* [使用 Visual Studio 與 Git 持續部署到 Azure](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a>其他資源

* [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [Azure 資源群組](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/)
