[018 Coding tip of the day - main thread]
UI관련 코드들은 main 쓰레드에서 동작해야합니다. JSON 파싱과 네트워크 통신 등 다른 작업들은 background thread에서 동작해야합니다.

![018](./images/018.png)
