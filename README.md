<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login tester</title>
</head>
<body>
<h2>Instance</h2>
<div>
    <label>udataUrl</label>
    <input id="udataUrl" name="udataUrl" required type="text" value="https://local-userdata.teachonmars.com"/>
</div>
<div>
    <label>webAppUrl</label>
    <input id="webAppUrl" name="webAppUrl" required type="text" value="https://dev-webapp.teachonmars.com"/>
</div>
<div>
    <label>appId</label>
    <input id="appId" name="appId" required type="text" value="com.teachonmars.tom.local"/>
</div>
<div>
    <label>apiKey</label>
    <input id="apiKey" name="apiKey" required type="text"
           value="pszFYCRoK1JJuUdGnC9PDO0cBhCnAlvsxHiANxSWmf9bu6LbpTrMb1ixARsEhx2m"/>
</div>
<div>
    <label>apiSecret</label>
    <input id="apiSecret" name="apiSecret" required
           type="text" value="pPRfust5VIvrQLGa8urJCHlTyGtHGUkAdammVfMVvLNiB86QBvaDUB88baAdQtqt"/>
</div>
<div>
    <h2>Learner</h2>
    <input id="learnerId" name="learnerId" required type="text" value="24bfdc1b-9016-467a-b9d8-2b043dc9054a"/>
</div>
<br>
<br>
<button id="loginButton" onclick="getLoginUrl()">Login</button>
<p id="loginUrlDisplay"></p>
<br>
<br>
<div>
    <h2>bundleID</h2>
    <input id="bundleID" name="bundleID" required type="text" value="com.labosvr.tom.svrfamily"/>
</div>
<button id="loginButton" onclick="getLoginUrl_app()">Login</button>
<p id="loginUrl_appDisplay"></p>

<script crossorigin="anonymous"
        integrity="sha512-E8QSvWZ0eCLGk4km3hxSsNmGWbLtSCSUcewDQPQWZF6pEU8GlT8a5fF32wOl1i8ftdMhssTrF/OhyGWwonTcXA=="
        referrerpolicy="no-referrer"
        src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js">

</script>
<script>
  async function getLoginUrl() {
    var learnerId = document.querySelector('#learnerId').value,
        jsonContent = {
          learnerId: learnerId
        },
        content = JSON.stringify(jsonContent),
        rts = Math.floor(Date.now() / 1000),
        appId = document.querySelector('#appId').value,
        apiKey = document.querySelector('#apiKey').value,
        secretKey = document.querySelector('#apiSecret').value,
        nonce = CryptoJS.SHA256(Math.random().toString(36).slice(2) + Math.random() + rts + learnerId).toString(),
        uri = document.querySelector('#udataUrl').value + '/api/v3/device/auth/token',
        md5 = CryptoJS.MD5(content + uri).toString(),
        hash = CryptoJS.SHA256(secretKey + rts + appId + md5 + nonce).toString();

    const response = await fetch(uri, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json',
        'X-TOM-APP': appId,
        'X-TOM-API-KEY': apiKey,
        'X-TOM-API-HASH': hash,
        'X-TOM-NONCE': nonce,
        'X-TOM-RTS': rts,
        'X-TOM-APPLICATION-LANGUAGE': 'en',
        'X-TOM-DEVICE-PLATFORM': 'WebApp'
      },
      body: content
    });
    const jsonResponse = await response.json();
    const loginUrl = document.querySelector('#webAppUrl').value + '/login?authToken=' + jsonResponse.response;

    // Afficher l'URL dans la page HTML
    document.querySelector('#loginUrlDisplay').textContent = 'URL construite : ' + loginUrl;

    // Ouvrir la nouvelle fenêtre
    window.open(loginUrl, '_blank');
    //window.open(document.querySelector('#webAppUrl').value + '/login?authToken=' + jsonResponse.response, '_blank');
  }



  async function getLoginUrl_app() {
    var learnerId = document.querySelector('#learnerId').value,
        jsonContent = {
          learnerId: learnerId
        },
        content = JSON.stringify(jsonContent),
        rts = Math.floor(Date.now() / 1000),
        appId = document.querySelector('#appId').value,
        apiKey = document.querySelector('#apiKey').value,
        secretKey = document.querySelector('#apiSecret').value,
        nonce = CryptoJS.SHA256(Math.random().toString(36).slice(2) + Math.random() + rts + learnerId).toString(),
        uri = document.querySelector('#udataUrl').value + '/api/v3/device/auth/token',
        md5 = CryptoJS.MD5(content + uri).toString(),
        hash = CryptoJS.SHA256(secretKey + rts + appId + md5 + nonce).toString();

    const response = await fetch(uri, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json',
        'X-TOM-APP': appId,
        'X-TOM-API-KEY': apiKey,
        'X-TOM-API-HASH': hash,
        'X-TOM-NONCE': nonce,
        'X-TOM-RTS': rts,
        'X-TOM-APPLICATION-LANGUAGE': 'en',
        'X-TOM-DEVICE-PLATFORM': 'WebApp'
      },
      body: content
    });
    const jsonResponse = await response.json();
    const loginUrl = document.querySelector('#bundleID').value + '://globals.login?authToken=' + jsonResponse.response;

    // Afficher l'URL dans la page HTML
    document.querySelector('#loginUrl_appDisplay').textContent = 'URL construite : ' + loginUrl;

    // Ouvrir la nouvelle fenêtre
    window.open(loginUrl, '_blank');
    //window.open(document.querySelector('#webAppUrl').value + '/login?authToken=' + jsonResponse.response, '_blank');
  }


</script>
</body>
</html>
