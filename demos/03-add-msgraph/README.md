# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="4ce68-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="4ce68-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ce68-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="4ce68-102">Prerequisites</span></span>

<span data-ttu-id="4ce68-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="4ce68-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="4ce68-104">開発用のコンピューターにインストールされている[node.js](https://nodejs.org) 。</span><span class="sxs-lookup"><span data-stu-id="4ce68-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="4ce68-105">node.js を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="4ce68-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="4ce68-106">(**注:** このチュートリアルは、ノードバージョン10.7.0 を使用して作成されています。</span><span class="sxs-lookup"><span data-stu-id="4ce68-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="4ce68-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="4ce68-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="4ce68-108">開発用コンピューターにインストールされている[角度 CLI](https://cli.angular.io/) 。</span><span class="sxs-lookup"><span data-stu-id="4ce68-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="4ce68-109">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="4ce68-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="4ce68-110">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="4ce68-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="4ce68-111">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="4ce68-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="4ce68-112">[office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="4ce68-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="4ce68-113">web アプリケーションをアプリケーション登録ポータルに登録する</span><span class="sxs-lookup"><span data-stu-id="4ce68-113">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="4ce68-114">ブラウザーを開き、[アプリケーション登録ポータル](https://apps.dev.microsoft.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-114">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="4ce68-115">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="4ce68-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="4ce68-116">ページの上部にある [**アプリの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-116">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="4ce68-117">**注:** ページに [**アプリの追加**] ボタンが複数表示されている場合は、[収束した**アプリ**] リストに対応するボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-117">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="4ce68-118">[**アプリケーションの登録**] ページで、[**アプリケーション名**] を「**角度グラフチュートリアル**」に設定し、[**作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-118">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![アプリ登録ポータル web サイトで新しいアプリを作成するスクリーンショット](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="4ce68-120">[**角度グラフのチュートリアル登録**] ページの [**プロパティ**] セクションで、後で必要になるように**アプリケーション Id**をコピーします。</span><span class="sxs-lookup"><span data-stu-id="4ce68-120">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![新しく作成されたアプリケーションの ID のスクリーンショット](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="4ce68-122">[**プラットフォーム**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="4ce68-122">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="4ce68-123">[**プラットフォームの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-123">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="4ce68-124">[**プラットフォームの追加**] ダイアログで、[ **Web**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-124">In the **Add Platform** dialog, select **Web**.</span></span>

        ![アプリのプラットフォームを作成するスクリーンショット](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="4ce68-126">[ **Web**プラットフォーム] ボックスに、 `http://localhost:4200` **リダイレクト url**の url を入力します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-126">In the **Web** platform box, enter the URL `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![アプリケーションに新たに追加された Web プラットフォームのスクリーンショット](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="4ce68-128">ページの一番下までスクロールして、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-128">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="4ce68-129">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="4ce68-129">Configure the sample</span></span>

1. <span data-ttu-id="4ce68-130">ファイルの`oauth.ts.example`名前を`oauth.ts`に変更します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-130">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="4ce68-131">`oauth.ts`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-131">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="4ce68-132">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4ce68-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="4ce68-133">コマンドラインインターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4ce68-133">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="4ce68-134">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="4ce68-134">Run the sample</span></span>

1. <span data-ttu-id="4ce68-135">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-135">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="4ce68-136">Web ブラウザーを開き、`http://localhost:4200` を参照します。</span><span class="sxs-lookup"><span data-stu-id="4ce68-136">Open a browser and browse to `http://localhost:4200`.</span></span>