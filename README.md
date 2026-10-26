# line-message-tool
```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>神奇妙妙工具</title>
  <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
</head>
<body>
  <h2>神奇妙妙訊息器</h2>
  <p id="status">正在初始化……</p>
  <p id="preview"></p>
  <button id="send" disabled>確認並傳送</button>

  <script>
    const LIFF_ID = "2012206486-2r09reyF";

    const params = new URLSearchParams(location.search);
    const message = params.get("text") || "大家好！";

    const status = document.getElementById("status");
    const preview = document.getElementById("preview");
    const button = document.getElementById("send");

    preview.textContent = "訊息內容：" + message;

    async function main() {
      await liff.init({ liffId: LIFF_ID });

      if (!liff.isInClient()) {
        status.textContent = "請在 LINE 內開啟此連結。";
        return;
      }

      if (!liff.isApiAvailable("sendMessages")) {
        status.textContent = "目前環境不支援傳送訊息。";
        return;
      }

      status.textContent = "請確認上方訊息內容。";
      button.disabled = false;
    }

    button.addEventListener("click", async () => {
      button.disabled = true;

      try {
        await liff.sendMessages([
          { type: "text", text: message }
        ]);

        status.textContent = "傳送成功！";
        liff.closeWindow();
      } catch (error) {
        console.error(error);
        status.textContent = "傳送失敗：" + error.message;
        button.disabled = false;
      }
    });

    main().catch(error => {
      console.error(error);
      status.textContent = "初始化失敗：" + error.message;
    });
  </script>
</body>
</html>
```
