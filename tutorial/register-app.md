<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b0a18-101">この演習では、azure Active Directory 管理センターを使用して、新しい azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="b0a18-102">ブラウザーを開き、 [Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="b0a18-103">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="b0a18-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b0a18-104">左側のナビゲーションで [ **Azure Active Directory** ] を選択し、[**管理**] で [**アプリの登録 (プレビュー)** ] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="b0a18-105">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="b0a18-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="b0a18-106">[**新しい登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-106">Select **New registration**.</span></span> <span data-ttu-id="b0a18-107">[**アプリケーションの登録**] ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="b0a18-108">**名前**をに`Angular Graph Tutorial`設定します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="b0a18-109">**任意の組織ディレクトリおよび個人の Microsoft アカウントのアカウント**に、**サポートされているアカウントの種類**を設定します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="b0a18-110">[**リダイレクト URI**] で、最初のドロップダウンを`Web`に設定し、値`http://localhost:4200`をに設定します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](./images/aad-register-an-app.png)

1. <span data-ttu-id="b0a18-112">[**登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-112">Choose **Register**.</span></span> <span data-ttu-id="b0a18-113">[**角度グラフのチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="b0a18-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](./images/aad-application-id.png)

1. <span data-ttu-id="b0a18-115">[**管理**] の下で [**認証**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="b0a18-116">暗黙的な**grant**セクションを見つけ、**アクセストークン**と**ID トークン**を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b0a18-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="b0a18-117">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b0a18-117">Choose **Save**.</span></span>

    ![暗黙的な grant セクションのスクリーンショット](./images/aad-implicit-grant.png)
