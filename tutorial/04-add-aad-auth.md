<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="be879-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="be879-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="be879-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="be879-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="be879-103">この手順では、アプリケーションに、 [角度の Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) を統合します。</span><span class="sxs-lookup"><span data-stu-id="be879-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="be879-104">「 **Oauth** 」という名前の新しいファイルを作成し、次のコードを追加し**ます。**</span><span class="sxs-lookup"><span data-stu-id="be879-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="be879-105">を `YOUR_APP_ID_HERE` アプリケーション登録ポータルからのアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="be879-106">Git などのソース管理を使用している場合は、この時点で **oauth** ファイルをソース管理から除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="be879-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="be879-107">**/Src/app/app.module.ts**を開き、次の `import` ステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="be879-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="be879-108">`MsalModule`を `imports` 宣言内の配列に追加 `@NgModule` し、アプリ ID で初期化します。</span><span class="sxs-lookup"><span data-stu-id="be879-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports" highlight="6-11":::

## <a name="implement-sign-in"></a><span data-ttu-id="be879-109">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="be879-109">Implement sign-in</span></span>

<span data-ttu-id="be879-110">このセクションでは、認証サービスを作成し、サインインとサインアウトを実装します。</span><span class="sxs-lookup"><span data-stu-id="be879-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="be879-111">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="be879-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="be879-112">このためのサービスを作成することにより、認証方法にアクセスする必要がある任意のコンポーネントに容易に注入することができます。</span><span class="sxs-lookup"><span data-stu-id="be879-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="be879-113">コマンドが完了したら、 **/src/app/auth.service.ts** を開き、その内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-113">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return null;
      }
    }
    ```

1. <span data-ttu-id="be879-114">**/Src/app/nav-bar/nav-bar.component.ts**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-114">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="be879-115">**/Src/app/home/home.component.ts**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-115">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-32":::

<span data-ttu-id="be879-116">変更を保存し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="be879-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="be879-117">[ **サインインするにはここを** クリック] ボタンをクリックすると、にリダイレクトさ `https://login.microsoftonline.com` れます。</span><span class="sxs-lookup"><span data-stu-id="be879-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="be879-118">Microsoft アカウントを使用してログインし、要求されたアクセス許可に同意します。</span><span class="sxs-lookup"><span data-stu-id="be879-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="be879-119">アプリページが更新され、トークンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="be879-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="be879-120">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="be879-120">Get user details</span></span>

<span data-ttu-id="be879-121">この時点で、認証サービスはユーザーの表示名と電子メールアドレスの定数値を設定します。</span><span class="sxs-lookup"><span data-stu-id="be879-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="be879-122">これでアクセストークンが作成されました。これらの値が現在のユーザーに対応するように、Microsoft Graph からユーザーの詳細を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="be879-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="be879-123">**/Src/app/auth.service.ts**を開き、次の `import` ステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="be879-123">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="be879-124">という名前のクラスに新しい関数を追加 `AuthService` `getUser` します。</span><span class="sxs-lookup"><span data-stu-id="be879-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="be879-125">アクセストークンを表示する警告を追加するメソッドで、次のコードを見つけて削除し `getAccessToken` ます。</span><span class="sxs-lookup"><span data-stu-id="be879-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="be879-126">メソッドから次のコードを見つけ、削除し `signIn` ます。</span><span class="sxs-lookup"><span data-stu-id="be879-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="be879-127">その場所に、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="be879-127">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="be879-128">この新しいコードでは、Microsoft Graph SDK を使用してユーザーの詳細を取得し、 `User` API 呼び出しから返された値を使用してオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="be879-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="be879-129">ユーザーが既にログインしているかどうかを `constructor` 確認するには、クラスのを変更し、その `AuthService` 場合は詳細情報を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="be879-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="be879-130">既存のを `constructor` 次のものに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="be879-131">一時コードをクラスから削除 `HomeComponent` します。</span><span class="sxs-lookup"><span data-stu-id="be879-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="be879-132">**/Src/app/home/home.component.ts**を開き、既存の `signIn` 関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="be879-132">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet":::

<span data-ttu-id="be879-133">変更を保存してアプリを開始した場合は、サインイン後にホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be879-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

<span data-ttu-id="be879-135">右上隅にあるユーザーアバターをクリックして、[ **サインアウト** ] リンクにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="be879-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="be879-136">**[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="be879-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="be879-138">トークンの格納と更新</span><span class="sxs-lookup"><span data-stu-id="be879-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="be879-139">この時点で、アプリケーションには、API 呼び出しのヘッダーで送信されるアクセストークンがあり `Authorization` ます。</span><span class="sxs-lookup"><span data-stu-id="be879-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="be879-140">これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。</span><span class="sxs-lookup"><span data-stu-id="be879-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="be879-141">ただし、このトークンは一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="be879-141">However, this token is short-lived.</span></span> <span data-ttu-id="be879-142">トークンが発行された後、有効期限が切れる時間になります。</span><span class="sxs-lookup"><span data-stu-id="be879-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="be879-143">アプリは MSAL ライブラリを使用しているため、トークンストレージを実装したり、ロジックを更新したりする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="be879-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="be879-144">は、 `MsalService` ブラウザーのストレージにトークンをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="be879-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="be879-145">この `acquireTokenSilent` メソッドは、最初にキャッシュされたトークンをチェックし、期限が切れていない場合はそれを返します。</span><span class="sxs-lookup"><span data-stu-id="be879-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="be879-146">有効期限が切れている場合は、新しい要求を取得するためにサイレント要求を行います。</span><span class="sxs-lookup"><span data-stu-id="be879-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
