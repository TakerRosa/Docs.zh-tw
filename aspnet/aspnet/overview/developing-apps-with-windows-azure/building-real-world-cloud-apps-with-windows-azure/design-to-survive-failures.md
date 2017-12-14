---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: "設計存留失敗 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: a0ee790da07c99cdb1279a6bca637a4ce8076e84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="d479c-104">設計存留失敗 （使用 Azure 建置實際的雲端應用程式）</span><span class="sxs-lookup"><span data-stu-id="d479c-104">Design to Survive Failures (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="d479c-105">由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d479c-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d479c-106">[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="d479c-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="d479c-107">**建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。</span><span class="sxs-lookup"><span data-stu-id="d479c-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="d479c-108">它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d479c-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="d479c-109">E 書籍的相關資訊，請參閱[第一章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="d479c-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="d479c-110">其中一項操作，您必須考慮當您建置任何類型的應用程式，但特別是其中一個會在有許多人將使用它，在雲端中執行的是如何設計應用程式，讓它可以正常處理失敗並繼續傳遞值最多可達可能的。</span><span class="sxs-lookup"><span data-stu-id="d479c-110">One of the things you have to think about when you build any type of application, but especially one that will run in the cloud where lots of people will be using it, is how to design the app so that it can gracefully handle failures and continue to deliver value as much as possible.</span></span> <span data-ttu-id="d479c-111">有足夠時間，項目將在任何環境或任何軟體系統中發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d479c-111">Given enough time, things are going to go wrong in any environment or any software system.</span></span> <span data-ttu-id="d479c-112">您的應用程式如何處理這些情況下會決定您的客戶會收到如何感到生氣，多少時間花費分析和修正問題。</span><span class="sxs-lookup"><span data-stu-id="d479c-112">How your app handles those situations determines how upset your customers will get and how much time you have to spend analyzing and fixing problems.</span></span>

## <a name="types-of-failures"></a><span data-ttu-id="d479c-113">失敗類型</span><span class="sxs-lookup"><span data-stu-id="d479c-113">Types of failures</span></span>

<span data-ttu-id="d479c-114">有兩種基本的類別，您會想要以不同方式處理的失敗：</span><span class="sxs-lookup"><span data-stu-id="d479c-114">There are two basic categories of failures that you'll want to handle differently:</span></span>

- <span data-ttu-id="d479c-115">暫時性，自我修復失敗，例如間歇性的網路連線問題。</span><span class="sxs-lookup"><span data-stu-id="d479c-115">Transient, self-healing failures such as intermittent network connectivity issues.</span></span>
- <span data-ttu-id="d479c-116">持續的失敗需要介入。</span><span class="sxs-lookup"><span data-stu-id="d479c-116">Enduring failures that require intervention.</span></span>

<span data-ttu-id="d479c-117">針對暫時失敗，您可以實作重試原則以確保，大部分的應用程式會快速且自動復原的時間。</span><span class="sxs-lookup"><span data-stu-id="d479c-117">For transient failures, you can implement a retry policy to ensure that most of the time the app recovers quickly and automatically.</span></span> <span data-ttu-id="d479c-118">您的客戶可能會注意到稍微更長的回應時間，但否則它們不會受到影響。</span><span class="sxs-lookup"><span data-stu-id="d479c-118">Your customers might notice slightly longer response time, but otherwise they won't be affected.</span></span> <span data-ttu-id="d479c-119">我們將示範一些方法來處理這些錯誤在[暫時性錯誤處理章](transient-fault-handling.md)。</span><span class="sxs-lookup"><span data-stu-id="d479c-119">We'll show some ways to handle these errors in the [Transient Fault Handling chapter](transient-fault-handling.md).</span></span>

<span data-ttu-id="d479c-120">根進行分析，您可以實作監視和記錄功能，會發生問題，並進行根原因分析時通知您。</span><span class="sxs-lookup"><span data-stu-id="d479c-120">For enduring failures, you can implement monitoring and logging functionality that notifies you promptly when issues arise and that facilitates root cause analysis.</span></span> <span data-ttu-id="d479c-121">我們將示範一些可協助您保持在這類錯誤，在頂端[監視和遙測章節](monitoring-and-telemetry.md)。</span><span class="sxs-lookup"><span data-stu-id="d479c-121">We'll show some ways to help you stay on top of these kinds of errors in the [Monitoring and Telemetry chapter](monitoring-and-telemetry.md).</span></span>

## <a name="failure-scope"></a><span data-ttu-id="d479c-122">失敗的範圍</span><span class="sxs-lookup"><span data-stu-id="d479c-122">Failure scope</span></span>

<span data-ttu-id="d479c-123">您也需要思考如何失敗範圍-是否會影響一部電腦，或完整的服務，例如 SQL Database 或存放裝置或整個區域。</span><span class="sxs-lookup"><span data-stu-id="d479c-123">You also have to think about failure scope – whether a single machine is affected, a whole service such as SQL Database or Storage, or an entire region.</span></span>

![失敗的範圍](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a><span data-ttu-id="d479c-125">電腦的失敗</span><span class="sxs-lookup"><span data-stu-id="d479c-125">Machine failures</span></span>

<span data-ttu-id="d479c-126">在 Azure 中，失敗的伺服器會自動取代為由一個新和快速地自動從這類的失敗復原設計良好的雲端應用程式。</span><span class="sxs-lookup"><span data-stu-id="d479c-126">In Azure, a failed server is automatically replaced by a new one, and a well-designed cloud app recovers from this kind of failure automatically and quickly.</span></span> <span data-ttu-id="d479c-127">我們稍早強調無狀態 web 層，延展性的優勢，並輕鬆復原，從失敗的伺服器是無狀態的另一個優點。</span><span class="sxs-lookup"><span data-stu-id="d479c-127">Earlier we stressed the scalability benefits of a stateless web tier, and ease of recovery from a failed server is another benefit of statelessness.</span></span> <span data-ttu-id="d479c-128">輕鬆復原也是其中一個平台做為服務 (PaaS) 功能，例如 SQL Database 和 Azure App Service Web 應用程式的優點。</span><span class="sxs-lookup"><span data-stu-id="d479c-128">Ease of recovery is also one of the benefits of platform-as-a-service (PaaS) features such as SQL Database and Azure App Service Web Apps.</span></span> <span data-ttu-id="d479c-129">硬體故障很少見，但這些服務處理它們，而發生時您甚至不需要撰寫程式碼以處理電腦失敗，當您使用其中一個服務。</span><span class="sxs-lookup"><span data-stu-id="d479c-129">Hardware failures are rare, but when they occur these services handle them automatically; you don't even have to write code to handle machine failures when you're using one of these services.</span></span>

### <a name="service-failures"></a><span data-ttu-id="d479c-130">服務失敗</span><span class="sxs-lookup"><span data-stu-id="d479c-130">Service failures</span></span>

<span data-ttu-id="d479c-131">雲端應用程式通常會使用多個服務。</span><span class="sxs-lookup"><span data-stu-id="d479c-131">Cloud apps typically use multiple services.</span></span> <span data-ttu-id="d479c-132">比方說，它修正應用程式中使用 SQL Database 服務、 儲存體服務，並部署至 Azure App Service web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d479c-132">For example, the Fix It app uses the SQL Database service, the Storage service, and the web app is deployed to Azure App Service.</span></span> <span data-ttu-id="d479c-133">您的應用程式將要如果其中一個服務依存於失敗？</span><span class="sxs-lookup"><span data-stu-id="d479c-133">What will your app do if one of the services you depend on fails?</span></span> <span data-ttu-id="d479c-134">某些服務失敗的易記 「 很抱歉，稍後再試一次 」 訊息可能是最好的方式。</span><span class="sxs-lookup"><span data-stu-id="d479c-134">For some service failures a friendly "sorry, try again later" message might be the best you can do.</span></span> <span data-ttu-id="d479c-135">但在許多案例中您可以做得更好。</span><span class="sxs-lookup"><span data-stu-id="d479c-135">But in many scenarios you can do better.</span></span> <span data-ttu-id="d479c-136">比方說，當您的後端資料存放區已關閉，您可以接受使用者輸入、 顯示 「 您已收到要求，"並儲存其他某處輸入暫時;當您需要的服務再次運作為止，然後您可以擷取輸入，並處理它。</span><span class="sxs-lookup"><span data-stu-id="d479c-136">For example, when your back-end data store is down, you can accept user input, display "your request has been received," and store the input someplace else temporarily; then when the service you need is operational again, you can retrieve the input and process it.</span></span>

<span data-ttu-id="d479c-137">[佇列為主的工作模式](queue-centric-work-pattern.md)一章會示範一個方式來處理此案例。</span><span class="sxs-lookup"><span data-stu-id="d479c-137">The [Queue-centric Work Pattern](queue-centric-work-pattern.md) chapter shows one way to handle this scenario.</span></span> <span data-ttu-id="d479c-138">修正它應用程式會將工作儲存在 SQL Database 中，但不需要結束處理 SQL Database 已關閉時。</span><span class="sxs-lookup"><span data-stu-id="d479c-138">The Fix It app stores tasks in SQL Database, but it doesn't have to quit working when SQL Database is down.</span></span> <span data-ttu-id="d479c-139">在該章節，我們看到如何儲存在佇列中，工作的使用者輸入，並使用的工作者處理序讀取佇列並更新工作。</span><span class="sxs-lookup"><span data-stu-id="d479c-139">In that chapter we'll see how to store user input for a task in a queue, and use a worker process to read the queue and update the task.</span></span> <span data-ttu-id="d479c-140">如果 SQL 已關閉，讓您建立修正它的工作不受影響;工作者處理序可以等候，並使用 SQL Database 時處理新的工作。</span><span class="sxs-lookup"><span data-stu-id="d479c-140">If SQL is down, the ability to create Fix It tasks is unaffected; the worker process can wait and process new tasks when SQL Database is available.</span></span>

### <a name="region-failures"></a><span data-ttu-id="d479c-141">區域失敗</span><span class="sxs-lookup"><span data-stu-id="d479c-141">Region failures</span></span>

<span data-ttu-id="d479c-142">整個區域可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="d479c-142">Entire regions may fail.</span></span> <span data-ttu-id="d479c-143">天然災害可能會破壞資料中心、 簡維它可能會取得 meteor、 主幹行，資料中心，無法由埋藏與 backhoe 等 cow farmer 剪下。如果您的應用程式應的資料中心裝載該怎麼辦？</span><span class="sxs-lookup"><span data-stu-id="d479c-143">A natural disaster might destroy a data center, it might get flattened by a meteor, the trunk line into the datacenter could be cut by a farmer burying a cow with a backhoe, etc. If your app is hosted in the stricken datacenter what do you do?</span></span> <span data-ttu-id="d479c-144">很可能您的應用程式在 Azure 中設定多個區域中同時執行，以便如果其中一個沒有損毀，您就會繼續執行另一個區域中。</span><span class="sxs-lookup"><span data-stu-id="d479c-144">It's possible to set up your app in Azure to run in multiple regions simultaneously so that if there's a disaster in one, you continue running in another region.</span></span> <span data-ttu-id="d479c-145">這類失敗的情況很罕見的相符項目，和大部分的應用程式不透過確保不會中斷的服務，透過這種失敗 hoops 跳。</span><span class="sxs-lookup"><span data-stu-id="d479c-145">Such failures are extremely rare occurrences, and most apps don't jump through the hoops necessary to ensure uninterrupted service through failures of this sort.</span></span> <span data-ttu-id="d479c-146">請參閱 [資源] 區段，如需有關如何保持應用程式就算是經由區域失敗資訊章節的結尾。</span><span class="sxs-lookup"><span data-stu-id="d479c-146">See the Resources section at the end of the chapter for information about how to keep your app available even through a region failure.</span></span>

<span data-ttu-id="d479c-147">Azure 的目標是要讓所有的失敗中來得簡單許多，這類的處理，您會看到我們如何所做的下列章節中的一些範例。</span><span class="sxs-lookup"><span data-stu-id="d479c-147">A goal of Azure is to make handling all of these kinds of failures a lot easier, and you'll see some examples of how we're doing that in the following chapters.</span></span>

## <a name="slas"></a><span data-ttu-id="d479c-148">Sla</span><span class="sxs-lookup"><span data-stu-id="d479c-148">SLAs</span></span>

<span data-ttu-id="d479c-149">人員通常會反應在雲端環境中的服務等級協定 (Sla)。</span><span class="sxs-lookup"><span data-stu-id="d479c-149">People often hear about service-level agreements (SLAs) in the cloud environment.</span></span> <span data-ttu-id="d479c-150">基本上，這些是關於如何可靠的服務是可讓公司承諾。</span><span class="sxs-lookup"><span data-stu-id="d479c-150">Basically these are promises that companies make about how reliable their service is.</span></span> <span data-ttu-id="d479c-151">99.9 %sla 表示您應該預期服務以正確地運作 99.9%的時間。</span><span class="sxs-lookup"><span data-stu-id="d479c-151">A 99.9% SLA means you should expect the service to be working correctly 99.9% of the time.</span></span> <span data-ttu-id="d479c-152">這是相當一般的 SLA 值和聽起來很高的數字，但您可能不知道有多少停機時間。 %1 實際數量來。</span><span class="sxs-lookup"><span data-stu-id="d479c-152">That's a fairly typical value for an SLA and it sounds like a very high number, but you might not realize how much down time .1% actually amounts to.</span></span> <span data-ttu-id="d479c-153">以下是示範各種 SLA 百分比金額以透過一年、 月和週多少停機時間的資料表。</span><span class="sxs-lookup"><span data-stu-id="d479c-153">Here's a table that shows how much downtime various SLA percentages amount to over a year, a month, and a week.</span></span>

![SLA 資料表](design-to-survive-failures/_static/image2.png)

<span data-ttu-id="d479c-155">因此 99.9 %sla 表示您的服務可能已關閉 8.76 小時年份或月份 43.2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="d479c-155">So a 99.9% SLA means your service could be down 8.76 hours a year or 43.2 minutes a month.</span></span> <span data-ttu-id="d479c-156">這是更停機時間比想像大部分的人。</span><span class="sxs-lookup"><span data-stu-id="d479c-156">That's more down time than most people realize.</span></span> <span data-ttu-id="d479c-157">因此，身為開發人員要注意一段時間，則可能處理的非失誤性的方式。</span><span class="sxs-lookup"><span data-stu-id="d479c-157">So as a developer you want to be aware that a certain amount of down time is possible and handle it in a graceful way.</span></span> <span data-ttu-id="d479c-158">在某個時間點人正在使用您的應用程式和服務將會關閉，而且您想要該客戶的負面影響降到最低。</span><span class="sxs-lookup"><span data-stu-id="d479c-158">At some point someone is going to be using your app, and a service is going to be down, and you want to minimize the negative impact of that on the customer.</span></span>

<span data-ttu-id="d479c-159">您應該了解 SLA 的一件事是哪一個時段則是指： 無法取得重設時鐘每週、 每個月或每年嗎？</span><span class="sxs-lookup"><span data-stu-id="d479c-159">One thing you should know about an SLA is what time-frame it refers to: does the clock get reset every week, every month, or every year?</span></span> <span data-ttu-id="d479c-160">在 Azure 中我們重設時鐘每個月，這是比每年的 SLA，好讓您，因為每年的 SLA 無法移轉它們使用一系列的很好的月份來隱藏不好的月份。</span><span class="sxs-lookup"><span data-stu-id="d479c-160">In Azure we reset the clock every month, which is better for you than a yearly SLA, since a yearly SLA could hide bad months by offsetting them with a series of good months.</span></span>

<span data-ttu-id="d479c-161">當然我們一律渴望做得更好比 SLA;通常您會向下，多小於。</span><span class="sxs-lookup"><span data-stu-id="d479c-161">Of course we always aspire to do better than the SLA; usually you'll be down much less than that.</span></span> <span data-ttu-id="d479c-162">承諾時，如果我們曾向下的時間超過關閉時間的最大值可以要求退款。</span><span class="sxs-lookup"><span data-stu-id="d479c-162">The promise is that if we're ever down for longer than the maximum down time you can ask for money back.</span></span> <span data-ttu-id="d479c-163">您會回到可能的總金額不會完全彌補貴用戶停機時間，過多的商務影響的但 SLA 的這個部分做為強制執行原則，並可讓您知道我們需要它很嚴重。</span><span class="sxs-lookup"><span data-stu-id="d479c-163">The amount of money you get back probably wouldn't fully compensate you for the business impact of the excess down time, but that aspect of the SLA acts as an enforcement policy and lets you know that we do take it very seriously.</span></span>

### <a name="composite-slas"></a><span data-ttu-id="d479c-164">複合 Sla</span><span class="sxs-lookup"><span data-stu-id="d479c-164">Composite SLAs</span></span>

<span data-ttu-id="d479c-165">思考時您所看到的 Sla 很重要的事是，應用程式中使用多個服務，與每個服務需要不同的 SLA 的影響。</span><span class="sxs-lookup"><span data-stu-id="d479c-165">An important thing to think about when you're looking at SLAs is the impact of using multiple services in an app, with each service having a separate SLA.</span></span> <span data-ttu-id="d479c-166">例如，它修正應用程式使用 Azure App Service Web 應用程式、 Azure 儲存體和 SQL Database。</span><span class="sxs-lookup"><span data-stu-id="d479c-166">For example, the Fix It app uses Azure App Service Web Apps, Azure Storage, and SQL Database.</span></span> <span data-ttu-id="d479c-167">以下是在 2013 年 12 月，正在寫入此的電子書的日期為準，其 SLA 號碼：</span><span class="sxs-lookup"><span data-stu-id="d479c-167">Here are their SLA numbers as of the date this e-book is being written in December, 2013:</span></span>

![SLA 的網站，儲存體、 SQL 資料庫](design-to-survive-failures/_static/image3.png)

<span data-ttu-id="d479c-169">停機時間如預期般，這些服務的 Sla 為基礎的應用程式的最大值是什麼？</span><span class="sxs-lookup"><span data-stu-id="d479c-169">What is the maximum down time you would expect for the app based on these service SLAs?</span></span> <span data-ttu-id="d479c-170">您可能會認為您向下的時間會是最差的 SLA 百分比或 99.9%相等在此情況下。</span><span class="sxs-lookup"><span data-stu-id="d479c-170">You might think that your down time would be equal to the worst SLA percentage, or 99.9% in this case.</span></span> <span data-ttu-id="d479c-171">如果這三種服務永遠無法在相同的時間，但這不一定實際發生的狀況，也就是，則為 true。</span><span class="sxs-lookup"><span data-stu-id="d479c-171">That would be true if all three services always failed at the same time, but that isn't necessarily what actually happens.</span></span> <span data-ttu-id="d479c-172">每個服務可能會失敗獨立在不同的時間，所以您不必個別 SLA 數字乘以計算複合 SLA。</span><span class="sxs-lookup"><span data-stu-id="d479c-172">Each service may fail independently at different times, so you have to calculate the composite SLA by multiplying the individual SLA numbers.</span></span>

![複合 SLA](design-to-survive-failures/_static/image4.png)

<span data-ttu-id="d479c-174">因此您的應用程式可能已關機不只是 43.2 分鐘月份，3 次數量，一個月 – 108 分鐘，仍然可以在 Azure SLA 的限制範圍內。</span><span class="sxs-lookup"><span data-stu-id="d479c-174">So your app could be down not just 43.2 minutes a month but 3 times that amount, 108 minutes a month – and still be within the Azure SLA limits.</span></span>

<span data-ttu-id="d479c-175">這個問題並非 Azure 獨有的。</span><span class="sxs-lookup"><span data-stu-id="d479c-175">This issue is not unique to Azure.</span></span> <span data-ttu-id="d479c-176">我們實際上會提供最佳的雲端可用，任何雲端服務的 Sla，而且也有類似的問題，如果您使用任何廠商雲端服務處理。</span><span class="sxs-lookup"><span data-stu-id="d479c-176">We actually provide the best cloud SLAs of any cloud service available, and you'll have similar issues to deal with if you use any vendor's cloud services.</span></span> <span data-ttu-id="d479c-177">這醒目提示是思考如何設計您的應用程式依正常程序，處理無法避免服務失敗，因為它們可能影響客戶或使用者經常發生的重要性。</span><span class="sxs-lookup"><span data-stu-id="d479c-177">What this highlights is the importance of thinking about how you can design your app to handle the inevitable service failures gracefully, because they might happen often enough to impact your customers or users.</span></span>

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a><span data-ttu-id="d479c-178">相較於企業的停機時間體驗雲端 Sla</span><span class="sxs-lookup"><span data-stu-id="d479c-178">Cloud SLAs compared to enterprise down-time experience</span></span>

<span data-ttu-id="d479c-179">人員有時說 「 我的企業應用程式在我永遠不會有這些問題。 」</span><span class="sxs-lookup"><span data-stu-id="d479c-179">People sometimes say, "In my enterprise app I never have these problems."</span></span> <span data-ttu-id="d479c-180">如果您要求一個月的時間量向下他們實際上有，他們通常說"，它會偶爾。 」</span><span class="sxs-lookup"><span data-stu-id="d479c-180">If you ask how much down time a month they actually have, they usually say, "Well, it happens occasionally."</span></span> <span data-ttu-id="d479c-181">如果頻率要求，請他們承認和"有時候我們需要備份，或安裝新的伺服器或更新軟體 」。</span><span class="sxs-lookup"><span data-stu-id="d479c-181">And if you ask how often, they admit that "Sometimes we do need to back up or install a new server or update software."</span></span> <span data-ttu-id="d479c-182">當然，計算在停機時間。</span><span class="sxs-lookup"><span data-stu-id="d479c-182">Of course, that counts as down time.</span></span> <span data-ttu-id="d479c-183">大部分企業應用程式，除非它們是時間的特別重要的實際上是時間的向下超過我們服務的 Sla 所允許量。</span><span class="sxs-lookup"><span data-stu-id="d479c-183">Most enterprise apps unless they are especially mission-critical are actually down for more than the amount of time allowed by our service SLAs.</span></span> <span data-ttu-id="d479c-184">但當它是您的伺服器和基礎結構，您是負責它，並且在它的控制項時通常較少 angst 覺得，停機時間。</span><span class="sxs-lookup"><span data-stu-id="d479c-184">But when it's your server and your infrastructure and you're responsible for it and in control of it, you tend to feel less angst about down times.</span></span> <span data-ttu-id="d479c-185">在雲端環境中您是依存於其他人，且您不知道如何運作，因此您可能會變得更擔心。</span><span class="sxs-lookup"><span data-stu-id="d479c-185">In a cloud environment you're dependent on someone else and you don't know what's going on, so you might tend to get more worried about it.</span></span>

<span data-ttu-id="d479c-186">當企業會達成更大的執行時間百分比比您取得 SLA 的雲端時，執行所花費更多錢硬體上。</span><span class="sxs-lookup"><span data-stu-id="d479c-186">When an enterprise achieves a greater up-time percentage than you get from a cloud SLA, they do it by spending a lot more money on hardware.</span></span> <span data-ttu-id="d479c-187">雲端服務無法這樣做，但可能會收取更多的服務。</span><span class="sxs-lookup"><span data-stu-id="d479c-187">A cloud service could do that but would have to charge much more for its services.</span></span> <span data-ttu-id="d479c-188">相反地，您會利用符合成本效益的服務和設計您的軟體，因此無法避免的失敗會導致給您的客戶的最小中斷。</span><span class="sxs-lookup"><span data-stu-id="d479c-188">Instead, you take advantage of a cost-effective service and design your software so that the inevitable failures cause minimum disruption to your customers.</span></span> <span data-ttu-id="d479c-189">您為雲端應用程式的設計工具的工作不這麼避免失敗，避免發生災難，而且您執行此動作將焦點放在硬體上找不到的軟體。</span><span class="sxs-lookup"><span data-stu-id="d479c-189">Your job as a cloud app designer is not so much to avoid failure as to avoid catastrophe, and you do that by focusing on software, not on hardware.</span></span> <span data-ttu-id="d479c-190">企業應用程式，盡量以最大化失敗之間的平均時間，而雲端應用程式會盡可能減少復原時間。</span><span class="sxs-lookup"><span data-stu-id="d479c-190">Whereas enterprise apps strive to maximize mean time between failures, cloud apps strive to minimize mean time to recover.</span></span>

### <a name="not-all-cloud-services-have-slas"></a><span data-ttu-id="d479c-191">並非所有的雲端服務有 Sla</span><span class="sxs-lookup"><span data-stu-id="d479c-191">Not all cloud services have SLAs</span></span>

<span data-ttu-id="d479c-192">也請注意，並非每個雲端服務有 SLA。</span><span class="sxs-lookup"><span data-stu-id="d479c-192">Be aware also that not every cloud service even has an SLA.</span></span> <span data-ttu-id="d479c-193">如果您的應用程式會依據執行時間保證的服務，您可能已關閉比您想像的最長。</span><span class="sxs-lookup"><span data-stu-id="d479c-193">If your app is dependent on a service with no up-time guarantee, you could be down far longer than you might imagine.</span></span> <span data-ttu-id="d479c-194">例如，如果您啟用登入您的網站使用如 Facebook 或 Twitter 社交提供者時，檢查來找出，是否沒有 SLA，而且您可能會發現有不是其中一個服務提供者。</span><span class="sxs-lookup"><span data-stu-id="d479c-194">For example, if you enable log-in to your site using a social provider such as Facebook or Twitter, check with the service provider to find out if there is an SLA, and you might find out there isn't one.</span></span> <span data-ttu-id="d479c-195">但是，如果驗證服務當機或無法支援您在其擲回的要求數量，將您的客戶鎖定您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d479c-195">But if the authentication service goes down or is unable to support the volume of requests you throw at it, your customers are locked out of your app.</span></span> <span data-ttu-id="d479c-196">您可能已關閉天或更久。</span><span class="sxs-lookup"><span data-stu-id="d479c-196">You could be down for days or longer.</span></span> <span data-ttu-id="d479c-197">一個新的應用程式的建立者預期數百萬個下載和具有相依性 Facebook 驗證 –，但未向 Facebook 移至即時、 發現太晚之前時發生沒有該服務的 SLA。</span><span class="sxs-lookup"><span data-stu-id="d479c-197">The creators of one new app expected hundreds of millions of downloads and took a dependency on Facebook authentication – but didn't talk to Facebook before going live and discovered too late that there was no SLA for that service.</span></span>

### <a name="not-all-downtime-counts-toward-slas"></a><span data-ttu-id="d479c-198">並非所有的停機時間計算 Sla</span><span class="sxs-lookup"><span data-stu-id="d479c-198">Not all downtime counts toward SLAs</span></span>

<span data-ttu-id="d479c-199">如果您的應用程式過度使用，某些雲端服務可能會刻意拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="d479c-199">Some cloud services may deliberately deny service if your app over-uses them.</span></span> <span data-ttu-id="d479c-200">這稱為*節流*。</span><span class="sxs-lookup"><span data-stu-id="d479c-200">This is called *throttling*.</span></span> <span data-ttu-id="d479c-201">如果服務的 SLA，它應該清楚您可能會進行節流處理，以及條件您應用程式的設計應該避免這種情況，並適當地回應的節流設定，如果發生。</span><span class="sxs-lookup"><span data-stu-id="d479c-201">If a service has an SLA, it should state the conditions under which you might be throttled, and your app design should avoid those conditions and react appropriately to the throttling if it happens.</span></span> <span data-ttu-id="d479c-202">比方說，如果服務要求啟動失敗超過每秒的特定數字時，您要確定不太快，它們會導致繼續節流會發生自動重試。</span><span class="sxs-lookup"><span data-stu-id="d479c-202">For example, if requests to a service start to fail when you exceed a certain number per second, you want to make sure automatic retries don't happen so fast that they cause the throttling to continue.</span></span> <span data-ttu-id="d479c-203">我們將會有更多關於節流中[暫時性錯誤處理章](transient-fault-handling.md)。</span><span class="sxs-lookup"><span data-stu-id="d479c-203">We'll have more to say about throttling in the [Transient Fault Handling chapter](transient-fault-handling.md).</span></span>

## <a name="summary"></a><span data-ttu-id="d479c-204">總結</span><span class="sxs-lookup"><span data-stu-id="d479c-204">Summary</span></span>

<span data-ttu-id="d479c-205">本章嘗試協助您了解為什麼真實世界雲端應用程式具有設計繼續正常運作失敗。</span><span class="sxs-lookup"><span data-stu-id="d479c-205">This chapter has tried to help you realize why a real world cloud app has to be designed to survive failures gracefully.</span></span> <span data-ttu-id="d479c-206">從開始[下一章](monitoring-and-telemetry.md)，在這一系列的其餘模式移入更多詳細的一些策略，您可以使用執行此作業：</span><span class="sxs-lookup"><span data-stu-id="d479c-206">Starting with the [next chapter](monitoring-and-telemetry.md), the remaining patterns in this series go into more detail about some strategies you can use to do that:</span></span>

- <span data-ttu-id="d479c-207">具有良好[監控與遙測](monitoring-and-telemetry.md)，以便您快速找出有關失敗需要使用者介入，並有足夠的資訊加以解決。</span><span class="sxs-lookup"><span data-stu-id="d479c-207">Have good [monitoring and telemetry](monitoring-and-telemetry.md), so that you find out quickly about failures that require intervention, and you have sufficient information to resolve them.</span></span>
- <span data-ttu-id="d479c-208">[處理暫時性失敗](transient-fault-handling.md)藉由實作智慧型重試邏輯，使您的應用程式會自動復原時它，並會回復為[斷路器](transient-fault-handling.md#circuitbreakers)時它無法邏輯。</span><span class="sxs-lookup"><span data-stu-id="d479c-208">[Handle transient faults](transient-fault-handling.md) by implementing intelligent retry logic, so that your app recovers automatically when it can and falls back to [circuit breaker](transient-fault-handling.md#circuitbreakers) logic when it can't.</span></span>
- <span data-ttu-id="d479c-209">使用[分散式快取](distributed-caching.md)以便與資料庫存取權的輸送量、 延遲和連線問題降至最低。</span><span class="sxs-lookup"><span data-stu-id="d479c-209">Use [distributed caching](distributed-caching.md) in order to minimize throughput, latency, and connection problems with database access.</span></span>
- <span data-ttu-id="d479c-210">鬆散耦合，透過實作[佇列為主的工作模式](queue-centric-work-pattern.md)，如此一來，您的應用程式前端可以繼續工作關閉後端時。</span><span class="sxs-lookup"><span data-stu-id="d479c-210">Implement loose coupling via the [queue-centric work pattern](queue-centric-work-pattern.md), so that your app front end can continue to work when the back end is down.</span></span>

## <a name="resources"></a><span data-ttu-id="d479c-211">資源</span><span class="sxs-lookup"><span data-stu-id="d479c-211">Resources</span></span>

<span data-ttu-id="d479c-212">如需詳細資訊，請參閱此的電子書和下列資源中的後續章節。</span><span class="sxs-lookup"><span data-stu-id="d479c-212">For more information, see later chapters in this e-book and the following resources.</span></span>

<span data-ttu-id="d479c-213">文件集：</span><span class="sxs-lookup"><span data-stu-id="d479c-213">Documentation:</span></span>

- <span data-ttu-id="d479c-214">[Failsafe： 具有恢復功能雲端架構指引](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-214">[Failsafe: Guidance for Resilient Cloud Architectures](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx).</span></span> <span data-ttu-id="d479c-215">Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。</span><span class="sxs-lookup"><span data-stu-id="d479c-215">White paper by Marc Mercuri, Ulrich Homann, and Andrew Townhill.</span></span> <span data-ttu-id="d479c-216">FailSafe 影片系列網頁版本。</span><span class="sxs-lookup"><span data-stu-id="d479c-216">Web page version of the FailSafe video series.</span></span>
- <span data-ttu-id="d479c-217">[Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-217">[Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx).</span></span> <span data-ttu-id="d479c-218">Mark Simms 和 Michael Thomassy 詘躩裛。</span><span class="sxs-lookup"><span data-stu-id="d479c-218">White paper by Mark Simms and Michael Thomassy.</span></span>
- <span data-ttu-id="d479c-219">[Azure 業務續航力技術指引](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-219">[Azure Business Continuity Technical Guidance](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx).</span></span> <span data-ttu-id="d479c-220">Patrick Wickline 和 Jason Roth 詘躩裛。</span><span class="sxs-lookup"><span data-stu-id="d479c-220">White paper by Patrick Wickline and Jason Roth.</span></span>
- <span data-ttu-id="d479c-221">[災害復原與高可用性 Azure 應用程式](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-221">[Disaster Recovery and High Availability for Azure Applications](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx).</span></span> <span data-ttu-id="d479c-222">Michael McKeown、 Kommalapati 和 Jason Roth 詘躩裛。</span><span class="sxs-lookup"><span data-stu-id="d479c-222">White paper by Michael McKeown, Hanu Kommalapati, and Jason Roth.</span></span>
- <span data-ttu-id="d479c-223">[Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-223">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/en-us/library/dn568099.aspx).</span></span> <span data-ttu-id="d479c-224">請參閱多資料中心部署指導方針，斷路器模式。</span><span class="sxs-lookup"><span data-stu-id="d479c-224">See Multi Data Center Deployment guidance, Circuit breaker pattern.</span></span>
- <span data-ttu-id="d479c-225">[Azure 支援的服務等級協定](https://azure.microsoft.com/support/legal/sla/)。</span><span class="sxs-lookup"><span data-stu-id="d479c-225">[Azure Support - Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>
- <span data-ttu-id="d479c-226">[Azure SQL Database 的業務續航力](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-226">[Business Continuity in Azure SQL Database](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx).</span></span> <span data-ttu-id="d479c-227">SQL 資料庫高可用性和災害復原功能的相關文件。</span><span class="sxs-lookup"><span data-stu-id="d479c-227">Documentation about SQL Database high availability and disaster recovery features.</span></span>
- <span data-ttu-id="d479c-228">[高可用性和災害復原 Azure 虛擬機器中的 SQL Server](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d479c-228">[High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx).</span></span>

<span data-ttu-id="d479c-229">影片：</span><span class="sxs-lookup"><span data-stu-id="d479c-229">Videos:</span></span>

- <span data-ttu-id="d479c-230">[FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。</span><span class="sxs-lookup"><span data-stu-id="d479c-230">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="d479c-231">九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。</span><span class="sxs-lookup"><span data-stu-id="d479c-231">Nine-part series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="d479c-232">高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。</span><span class="sxs-lookup"><span data-stu-id="d479c-232">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="d479c-233">1 到 8 個劇集深度進入設計雲端應用程式不受失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="d479c-233">Episodes 1 and 8 go in depth into the reasons for designing cloud apps to survive failures.</span></span> <span data-ttu-id="d479c-234">另請參閱節流時段 2 開始 49:57、 的失敗點和失敗模式中的片段 2 開始 56:05，討論和斷路器中的時段 3 開始 40:55 討論中的待處理的討論。</span><span class="sxs-lookup"><span data-stu-id="d479c-234">See also the follow-up discussion of throttling in episode 2 starting at 49:57, the discussion of failure points and failure modes in episode 2 starting at 56:05, and the discussion of circuit breakers in episode 3 starting at 40:55.</span></span>
- <span data-ttu-id="d479c-235">[建置大型： 學到來自 Azure 客戶-II](https://channel9.msdn.com/Events/Build/2012/3-030)。</span><span class="sxs-lookup"><span data-stu-id="d479c-235">[Building Big: Lessons learned from Azure customers - Part II](https://channel9.msdn.com/Events/Build/2012/3-030).</span></span> <span data-ttu-id="d479c-236">Mark Simms 交談有關失敗的設計和檢測的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d479c-236">Mark Simms talks about designing for failure and instrumenting everything.</span></span> <span data-ttu-id="d479c-237">類似於保全數列但進入詳細的使用說明。</span><span class="sxs-lookup"><span data-stu-id="d479c-237">Similar to the Failsafe series but goes into more how-to details.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d479c-238">[上一頁](unstructured-blob-storage.md)
[下一頁](monitoring-and-telemetry.md)</span><span class="sxs-lookup"><span data-stu-id="d479c-238">[Previous](unstructured-blob-storage.md)
[Next](monitoring-and-telemetry.md)</span></span>