<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e1555-101">このチュートリアルでは、Microsoft Graph を使用してユーザーの予定表情報を取得する、角度のある単一ページのアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1555-101">This tutorial teaches you how to build an Angular single-page app that uses Microsoft Graph to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="e1555-102">完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-angularspa)をダウンロードするか、クローンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e1555-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1555-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="e1555-103">Prerequisites</span></span>

<span data-ttu-id="e1555-104">このチュートリアルを開始する前に、開発用のコンピューターに[node.js](https://nodejs.org)をインストールしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1555-104">Before you start this tutorial, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="e1555-105">Node.js を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="e1555-105">If you do not have Node.js, visit the previous link for download options.</span></span>

<span data-ttu-id="e1555-106">また、Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントを所有している必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1555-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="e1555-107">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="e1555-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="e1555-108">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="e1555-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="e1555-109">[Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="e1555-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="e1555-110">このチュートリアルは、ノードバージョン12.16.1 を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="e1555-110">This tutorial was written with Node version 12.16.1.</span></span> <span data-ttu-id="e1555-111">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="e1555-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="e1555-112">チュートリアルを見る</span><span class="sxs-lookup"><span data-stu-id="e1555-112">Watch the tutorial</span></span>

<span data-ttu-id="e1555-113">このモジュールは、Office 開発 YouTube チャネルで記録されており、利用できます。</span><span class="sxs-lookup"><span data-stu-id="e1555-113">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/KUPRTTOUzz8]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="e1555-114">フィードバック</span><span class="sxs-lookup"><span data-stu-id="e1555-114">Feedback</span></span>

<span data-ttu-id="e1555-115">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-angularspa)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="e1555-115">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>
