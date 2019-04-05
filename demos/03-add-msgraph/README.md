# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="61859-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="61859-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61859-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="61859-102">Prerequisites</span></span>

<span data-ttu-id="61859-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="61859-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="61859-104">開発用のコンピューターにインストールされている[node.js](https://nodejs.org) 。</span><span class="sxs-lookup"><span data-stu-id="61859-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="61859-105">node.js を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="61859-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="61859-106">(**注:** このチュートリアルは、ノードバージョン10.7.0 を使用して作成されています。</span><span class="sxs-lookup"><span data-stu-id="61859-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="61859-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="61859-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="61859-108">開発用コンピューターにインストールされている[角度 CLI](https://cli.angular.io/) 。</span><span class="sxs-lookup"><span data-stu-id="61859-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="61859-109">Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または microsoft 職場または学校のアカウントのいずれか。</span><span class="sxs-lookup"><span data-stu-id="61859-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="61859-110">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="61859-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="61859-111">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="61859-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="61859-112">[office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="61859-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="61859-113">web アプリケーションを Azure Active Directory 管理センターに登録する</span><span class="sxs-lookup"><span data-stu-id="61859-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="61859-114">ブラウザーを開き、 [Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="61859-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="61859-115">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="61859-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="61859-116">左側のナビゲーションで [ **Azure Active Directory** ] を選択し、[**管理**] で [**アプリの登録 (プレビュー)** ] を選択します。</span><span class="sxs-lookup"><span data-stu-id="61859-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="61859-117">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="61859-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="61859-118">[**新しい登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="61859-118">Select **New registration**.</span></span> <span data-ttu-id="61859-119">[**アプリケーションの登録**] ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="61859-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="61859-120">**名前**をに`Angular Graph Tutorial`設定します。</span><span class="sxs-lookup"><span data-stu-id="61859-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="61859-121">**任意の組織ディレクトリおよび個人の Microsoft アカウントのアカウント**に、**サポートされているアカウントの種類**を設定します。</span><span class="sxs-lookup"><span data-stu-id="61859-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="61859-122">[**リダイレクト URI**] で、最初のドロップダウンを`Web`に設定し、値`http://localhost:4200`をに設定します。</span><span class="sxs-lookup"><span data-stu-id="61859-122">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="61859-124">[**登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="61859-124">Choose **Register**.</span></span> <span data-ttu-id="61859-125">[**角度グラフのチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="61859-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="61859-127">[**管理**] の下で [**認証**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="61859-127">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="61859-128">暗黙的な**grant**セクションを見つけ、**アクセストークン**と**ID トークン**を有効にします。</span><span class="sxs-lookup"><span data-stu-id="61859-128">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="61859-129">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="61859-129">Choose **Save**.</span></span>

    ![暗黙的な grant セクションのスクリーンショット](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="61859-131">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="61859-131">Configure the sample</span></span>

1. <span data-ttu-id="61859-132">ファイルの`oauth.ts.example`名前を`oauth.ts`に変更します。</span><span class="sxs-lookup"><span data-stu-id="61859-132">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="61859-133">`oauth.ts`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="61859-133">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="61859-134">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="61859-134">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="61859-135">コマンドラインインターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="61859-135">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="61859-136">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="61859-136">Run the sample</span></span>

1. <span data-ttu-id="61859-137">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="61859-137">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="61859-138">Web ブラウザーを開き、`http://localhost:4200` を参照します。</span><span class="sxs-lookup"><span data-stu-id="61859-138">Open a browser and browse to `http://localhost:4200`.</span></span>