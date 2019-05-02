<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1f274-101">このチュートリアルでは、Microsoft Graph を使用してユーザーの予定表情報を取得する、角度のある単一ページのアプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1f274-101">This tutorial teaches you how to build an Angular single-page app that uses Microsoft Graph to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="1f274-102">完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-angularspa)をダウンロードするか、クローンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="1f274-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f274-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="1f274-103">Prerequisites</span></span>

<span data-ttu-id="1f274-104">このチュートリアルを開始する前に、開発用のコンピューターに[node.js](https://nodejs.org)をインストールしておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f274-104">Before you start this tutorial, you should have [Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="1f274-105">Node.js を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="1f274-105">If you do not have Node.js, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="1f274-106">このチュートリアルは、ノードバージョン10.15.3 を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="1f274-106">This tutorial was written with Node version 10.15.3.</span></span> <span data-ttu-id="1f274-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="1f274-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="1f274-108">フィードバック</span><span class="sxs-lookup"><span data-stu-id="1f274-108">Feedback</span></span>

<span data-ttu-id="1f274-109">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-angularspa)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="1f274-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-angularspa).</span></span>