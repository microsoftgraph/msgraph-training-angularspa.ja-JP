<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fe3ac-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="fe3ac-102">このアプリケーションでは、microsoft graph [クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript) ライブラリを使用して microsoft graph への呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="fe3ac-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="fe3ac-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="fe3ac-104">すべてのグラフ呼び出しを保持する新しいサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="fe3ac-105">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="fe3ac-106">以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="fe3ac-107">コマンドが完了したら、 **/src/app/graph.service.ts** を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            let token = await this.authService.getAccessToken()
              .catch((reason) => {
                done(reason, null);
              });

            if (token)
            {
              done(null, token);
            } else {
              done("Could not get an access token", null);
            }
          }
        });
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[]> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          let result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
      }
    }
    ```

    <span data-ttu-id="fe3ac-108">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="fe3ac-109">このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="fe3ac-110">`getCalendarView`次の方法で Graph クライアントを使用する関数を実装します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="fe3ac-111">呼び出される URL は `/me/calendarview` です。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="fe3ac-112">`header`メソッドにヘッダーが含まれて `Prefer: outlook.timezone` いる場合、返されたイベントの開始時刻と終了時刻は、ユーザーの優先タイムゾーンにあります。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="fe3ac-113">メソッドは、 `query` とパラメーターを追加し `startDateTime` `endDateTime` 、予定表ビューの時間のウィンドウを定義します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="fe3ac-114">`select`このメソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="fe3ac-115">メソッドは、 `orderby` 結果を開始時刻で並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="fe3ac-116">この新しいメソッドを呼び出すための角度コンポーネントを作成し、呼び出しの結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="fe3ac-117">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="fe3ac-118">コマンドが完了したら、/src/app/app-routing.module.ts の配列にコンポーネントを追加します `routes` **。**</span><span class="sxs-lookup"><span data-stu-id="fe3ac-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="fe3ac-119">**/Src/app/calendar/calendar.component.ts**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-119">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findOneIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findOneIana(this.authService.user.timeZone);
        const timeZone = ianaName!.valueOf() || this.authService.user.timeZone;

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user.timeZone)
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

<span data-ttu-id="fe3ac-120">ここでは、JSON のイベントの配列をページにレンダリングするだけです。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="fe3ac-121">変更内容を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-121">Save your changes and restart the app.</span></span> <span data-ttu-id="fe3ac-122">サインインして、ナビゲーションバーの [ **予定表** ] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="fe3ac-123">すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="fe3ac-124">結果の表示</span><span class="sxs-lookup"><span data-stu-id="fe3ac-124">Display the results</span></span>

<span data-ttu-id="fe3ac-125">これで、コンポーネントを更新して、 `CalendarComponent` よりわかりやすい方法でイベントを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="fe3ac-126">関数から通知を追加する一時コードを削除し `ngOnInit` ます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-126">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="fe3ac-127">更新された関数は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-127">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="fe3ac-128">クラスに関数を追加して、 `CalendarComponent` `DateTimeTimeZone` オブジェクトを ISO 文字列に書式設定します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-128">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="fe3ac-129">**/Src/app/calendar/calendar.component.html**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-129">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="fe3ac-130">これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="fe3ac-131">変更を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-131">Save the changes and restart the app.</span></span> <span data-ttu-id="fe3ac-132">[ **予定表** ] リンクをクリックすると、アプリがイベントの表を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="fe3ac-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![イベント表のスクリーンショット](./images/add-msgraph-01.png)
