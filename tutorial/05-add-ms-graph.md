<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft graph[クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript)ライブラリを使用して microsoft graph への呼び出しを行います。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

1. という名前`./src/app` `event.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. すべてのグラフ呼び出しを保持する新しいサービスを追加します。 CLI で次のコマンドを実行します。

    ```Shell
    ng generate service graph
    ```

    以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。

1. コマンドが完了したら、 `./src/app/graph.service.ts`ファイルを開き、その内容を次のように置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    このコードの実行内容を考えましょう。

    - このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。
    - 次の方法`getEvents`で Graph クライアントを使用する関数を実装します。
      - 呼び出される URL は `/me/events` です。
      - この`select`メソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。
      - メソッド`orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。

1. この新しいメソッドを呼び出すための角度コンポーネントを作成し、呼び出しの結果を表示します。 CLI で次のコマンドを実行します。

    ```Shell
    ng generate component calendar
    ```

1. コマンドが完了したら、の`routes` `./src/app/app-routing.module.ts`配列にコンポーネントを追加します。

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. `./src/app/calendar/calendar.component.ts`ファイルを開き、その内容を次のように置き換えます。

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

ここでは、JSON のイベントの配列をページにレンダリングするだけです。 変更内容を保存し、アプリを再起動します。 サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。 すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果の表示

これで、 `CalendarComponent`コンポーネントを更新して、よりわかりやすい方法でイベントを表示することができます。

1. `ngOnInit`関数から通知を追加する一時コードを削除します。 更新された関数は、次のようになります。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. クラスに関数を追加`CalendarComponent`して、オブジェクト`DateTimeTimeZone`を ISO 文字列に書式設定します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. `./src/app/calendar/calendar.component.html`ファイルを開き、その内容を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。 変更を保存し、アプリを再起動します。 [**予定表**] リンクをクリックすると、アプリがイベントの表を表示するようになります。

![イベント表のスクリーンショット](./images/add-msgraph-01.png)
