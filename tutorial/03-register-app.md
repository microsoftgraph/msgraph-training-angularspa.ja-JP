<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5e00d-101">この演習では、Azure Active Directory 管理センターを使用して、新しい Azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="5e00d-102">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="5e00d-103">**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="5e00d-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="5e00d-104">左側のナビゲーションで [ **Azure Active Directory** ] を選択し、[**管理**] の下にある [**アプリの登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="5e00d-105">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="5e00d-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="5e00d-106">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-106">Select **New registration**.</span></span> <span data-ttu-id="5e00d-107">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="5e00d-108">`Angular Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="5e00d-109">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="5e00d-110">**[リダイレクト URI]** で、最初のドロップダウン リストを `Web` に設定し、それから `http://localhost:4200` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![[アプリケーションの登録] ページのスクリーンショット](./images/aad-register-an-app.png)

1. <span data-ttu-id="5e00d-112">[**登録**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-112">Select **Register**.</span></span> <span data-ttu-id="5e00d-113">[**角度グラフのチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="5e00d-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリの登録のアプリケーション ID のスクリーンショット](./images/aad-application-id.png)

1. <span data-ttu-id="5e00d-115">[**管理**] の下の [**認証**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="5e00d-116">暗黙的な**grant**セクションを見つけ、**アクセストークン**と**ID トークン**を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5e00d-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="5e00d-117">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5e00d-117">Select **Save**.</span></span>

    ![暗黙的な grant セクションのスクリーンショット](./images/aad-implicit-grant.png)
