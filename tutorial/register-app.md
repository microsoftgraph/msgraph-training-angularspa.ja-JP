<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a99d8-101">この演習では、アプリケーションレジストリポータル (ARP) を使用して、新しい Azure AD web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="a99d8-102">ブラウザーを開き、[アプリケーション登録ポータル](https://apps.dev.microsoft.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="a99d8-103">**個人アカウント**(別名: Microsoft アカウント) または**職場または学校のアカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="a99d8-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a99d8-104">ページの上部にある [**アプリの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a99d8-105">ページに [**アプリの追加**] ボタンが複数表示されている場合は、[収束した**アプリ**] リストに対応するボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="a99d8-106">[**アプリケーションの登録**] ページで、[**アプリケーション名**] を「**角度グラフチュートリアル**」に設定し、[**作成**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-106">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![アプリ登録ポータル web サイトで新しいアプリを作成するスクリーンショット](./images/arp-create-app-01.png)

1. <span data-ttu-id="a99d8-108">[**角度グラフのチュートリアル登録**] ページの [**プロパティ**] セクションで、後で必要になるように**アプリケーション Id**をコピーします。</span><span class="sxs-lookup"><span data-stu-id="a99d8-108">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![新しく作成されたアプリケーションの ID のスクリーンショット](./images/arp-create-app-02.png)

1. <span data-ttu-id="a99d8-110">[**プラットフォーム**] セクションまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="a99d8-110">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="a99d8-111">[**プラットフォームの追加**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-111">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="a99d8-112">[**プラットフォームの追加**] ダイアログで、[ **Web**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-112">In the **Add Platform** dialog, select **Web**.</span></span>

        ![アプリのプラットフォームを作成するスクリーンショット](./images/arp-create-app-03.png)

    1. <span data-ttu-id="a99d8-114">[ **Web**プラットフォーム] ボックスに、 `http://localhost:4200` **リダイレクト url**のを入力します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-114">In the **Web** platform box, enter `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![アプリケーションに新たに追加された Web プラットフォームのスクリーンショット](./images/arp-create-app-04.png)

1. <span data-ttu-id="a99d8-116">ページの一番下までスクロールして、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a99d8-116">Scroll to the bottom of the page and select **Save**.</span></span>