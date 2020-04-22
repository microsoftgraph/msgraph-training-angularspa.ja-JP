<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="22e6f-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="22e6f-102">このアプリケーションでは、microsoft graph[クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript)ライブラリを使用して microsoft graph への呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="22e6f-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="22e6f-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="22e6f-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="22e6f-104">という名前`./src/app` `event.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-104">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. <span data-ttu-id="22e6f-105">すべてのグラフ呼び出しを保持する新しいサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-105">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="22e6f-106">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-106">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="22e6f-107">以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="22e6f-108">コマンドが完了したら、 `./src/app/graph.service.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-108">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    <span data-ttu-id="22e6f-109">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="22e6f-109">Consider what this code is doing.</span></span>

    - <span data-ttu-id="22e6f-110">このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-110">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="22e6f-111">次の方法`getEvents`で Graph クライアントを使用する関数を実装します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-111">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="22e6f-112">呼び出される URL は `/me/events` です。</span><span class="sxs-lookup"><span data-stu-id="22e6f-112">The URL that will be called is `/me/events`.</span></span>
      - <span data-ttu-id="22e6f-113">この`select`メソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-113">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="22e6f-114">メソッド`orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-114">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="22e6f-115">この新しいメソッドを呼び出すための角度コンポーネントを作成し、呼び出しの結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-115">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="22e6f-116">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-116">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="22e6f-117">コマンドが完了したら、の`routes` `./src/app/app-routing.module.ts`配列にコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-117">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="22e6f-118">`./src/app/calendar/calendar.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-118">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

<span data-ttu-id="22e6f-119">ここでは、JSON のイベントの配列をページにレンダリングするだけです。</span><span class="sxs-lookup"><span data-stu-id="22e6f-119">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="22e6f-120">変更内容を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-120">Save your changes and restart the app.</span></span> <span data-ttu-id="22e6f-121">サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e6f-121">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="22e6f-122">すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-122">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="22e6f-123">結果の表示</span><span class="sxs-lookup"><span data-stu-id="22e6f-123">Display the results</span></span>

<span data-ttu-id="22e6f-124">これで、 `CalendarComponent`コンポーネントを更新して、よりわかりやすい方法でイベントを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-124">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="22e6f-125">`ngOnInit`関数から通知を追加する一時コードを削除します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-125">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="22e6f-126">更新された関数は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="22e6f-126">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="22e6f-127">クラスに関数を追加`CalendarComponent`して、オブジェクト`DateTimeTimeZone`を ISO 文字列に書式設定します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-127">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="22e6f-128">`./src/app/calendar/calendar.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-128">Open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="22e6f-129">これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。</span><span class="sxs-lookup"><span data-stu-id="22e6f-129">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="22e6f-130">変更を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="22e6f-130">Save the changes and restart the app.</span></span> <span data-ttu-id="22e6f-131">[**予定表**] リンクをクリックすると、アプリがイベントの表を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="22e6f-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![イベント表のスクリーンショット](./images/add-msgraph-01.png)
