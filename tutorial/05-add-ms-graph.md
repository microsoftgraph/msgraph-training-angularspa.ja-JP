<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft graph [クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript) ライブラリを使用して microsoft graph への呼び出しを行います。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

1. すべてのグラフ呼び出しを保持する新しいサービスを追加します。 CLI で次のコマンドを実行します。

    ```Shell
    ng generate service graph
    ```

    以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。

1. コマンドが完了したら、 **/src/app/graph.service.ts** を開き、その内容を次のように置き換えます。

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

    このコードの実行内容を考えましょう。

    - このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。
    - `getCalendarView`次の方法で Graph クライアントを使用する関数を実装します。
      - 呼び出される URL は `/me/calendarview` です。
      - `header`メソッドにヘッダーが含まれて `Prefer: outlook.timezone` いる場合、返されたイベントの開始時刻と終了時刻は、ユーザーの優先タイムゾーンにあります。
      - メソッドは、 `query` とパラメーターを追加し `startDateTime` `endDateTime` 、予定表ビューの時間のウィンドウを定義します。
      - `select`このメソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。
      - メソッドは、 `orderby` 結果を開始時刻で並べ替えます。

1. この新しいメソッドを呼び出すための角度コンポーネントを作成し、呼び出しの結果を表示します。 CLI で次のコマンドを実行します。

    ```Shell
    ng generate component calendar
    ```

1. コマンドが完了したら、/src/app/app-routing.module.ts の配列にコンポーネントを追加します `routes` **。**

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. **/Src/app/calendar/calendar.component.ts**を開き、その内容を次のように置き換えます。

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

ここでは、JSON のイベントの配列をページにレンダリングするだけです。 変更内容を保存し、アプリを再起動します。 サインインして、ナビゲーションバーの [ **予定表** ] リンクをクリックします。 すべてが正常に機能していれば、ユーザーのカレンダーにイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果の表示

これで、コンポーネントを更新して、 `CalendarComponent` よりわかりやすい方法でイベントを表示することができます。

1. 関数から通知を追加する一時コードを削除し `ngOnInit` ます。 更新された関数は、次のようになります。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. クラスに関数を追加して、 `CalendarComponent` `DateTimeTimeZone` オブジェクトを ISO 文字列に書式設定します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. **/Src/app/calendar/calendar.component.html**を開き、その内容を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。 変更を保存し、アプリを再起動します。 [ **予定表** ] リンクをクリックすると、アプリがイベントの表を表示するようになります。

![イベント表のスクリーンショット](./images/add-msgraph-01.png)
