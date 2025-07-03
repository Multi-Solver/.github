# Multi-Solver Service 🤖

A service that solves multiple captchas

# Table of Contents
+ [**Examples**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#examples)
  - [**Turnstile**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#turnstile)
  - [**Hcaptcha (IMG)**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#hcaptcha-img)
  - [**Recaptcha (IMG)**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#recaptcha-img)
  - [**Recaptcha (TOKEN)**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#recaptcha-token)
  - [**Icon UP/DOWN**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#iconupdown)
  - [**IconCaptcha**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#iconcaptcha)
  - [**GET RESULT**](https://github.com/Multi-Solver/documentation?tab=readme-ov-file#result)

## Documentation

#### Supported captchas
+ Turnstile (Cloudflare)
+ Hcaptcha (IMG)
+ Recaptcha (IMG)
+ Recaptcha (TOKEN)
+ Recaptcha (V3)
+ Iconupdown
+ IconCaptcha

#### Base url
```http
https://multisolver-api.vercel.app
```
#### Authorization

> [!IMPORTANT]
> In every request you must add this header.
```http
Authorization: Bearer YOUR_KEY_HERE
```

#### Endpoints
#
```http
  GET /v1/captcha/rusult
```

| Params   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `taskId` | `string` | TaskId obtained after sending a request to some captcha endpoint |
#

```http
  POST /v1/captcha/turnstile
```

| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `pageurl` | `string` | Full URL of the page on which you solve turnstile |
| `sitekey` | `string` | The value of the sitekey parameter you found in the site code, it usually doesn't change and you just need to find it once |
| `action` | `string` | Optional  |
#

```http
  POST /v1/captcha/hcaptcha-img
```
| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `img` | `string` | Base64 image |
| `task` | `string` | Description of what is to be done in the image |
| `type` | `string` | Grid, canvas or content  |
#

```http
  POST /v1/captcha/recaptcha-img
```

| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `img` | `string` | Base64 image |
| `textinstructions` | `string` | The name of the object from the task, preferably in English |
| `sizex` | `string` | Grid size with tasks "9" for 3x3 or "16" for 4x4 |
#

```http
  POST /v1/captcha/recaptcha-token
```

| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `pageurl` | `string` | Full URL of the page on which you solve reCAPTCHA V2 (On some sites the recaptcha is in an iframe in this case it is necessary to give the url of this iframe) |
| `sitekey` | `string` | The value of the sitekey parameter you found in the site code, it usually doesn't change and you just need to find it once |
| `invisible` | `string` | Optional: Set "1" if the site has an invisible recaptcha (a captcha that appears only after clicking a button, and immediately in open view) |
| `data_s` | `string` | Optional: The value of the data-s parameter is found on the page. Relevant for ReCaptcha Enterprise |
| `cookies` | `string` | Optional: Your cookies that will be used by MultiSolver to solve the captcha. Format: KEY:Value, for example: KEY1:Value1;KEY2:Value2; |
| `userAgent` | `string` | Optional: MultiSolver will use your userAgent |
| `proxy` | `string` | Optional: MultiSolver will use your proxy |
| `proxytype` | `string` | Optional: Your proxy server type: HTTP, HTTPS, SOCKS4, SOCKS5 |
| `version` | `string` | Optional: "v3" - indicates that this is reCAPTCHA V3 |
| `enterprise` | `string` | Optional: "1" - indicates that this is reCAPTCHA Enterprise |
| `action` | `string` | ‘action’ should be pulled from the page code when solving ReCaptcha v3 |
# 

```http
  POST /v1/captcha/iconupdown
```

| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `img` | `string` | Base64 image |
#

```http
  POST /v1/captcha/iconcaptcha
```
> [!NOTE]
> This endpoint is the only one that does not return a taskId, it will return the result with the coordinates.

| Body   | Type       | Description                          |
| :---------- | :--------- | :---------------------------------- |
| `img` | `string` | Base64 image |
#

## Examples

#### 🤖Turnstile
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  pageurl: "https://payeer.com/ru/auth/",
  sitekey: "0x4AAAAAAADSGfZQH6o7UqM0",
  action: "login",
  };
fetch(base_url + "/turnstile", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "Content-Type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖Hcaptcha (IMG)
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  img: "Base64 img",
  task: "Please click on all animals similar to the following silhouette",
  type: "grid",
};
fetch(base_url + "/hcaptcha-img", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "content-type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖Recaptcha (IMG)
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  img: "Base64 img",
  textinstructions: "bicycles",
  sizex: "9",
};
fetch(base_url + "/recaptcha-img", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "content-type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖Recaptcha (TOKEN)
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  pageurl: "https://2captcha.com/demo/recaptcha-v2",
  sitekey: "6LfD3PIbAAAAAJs_eEHvoOl75_83eXSqpPSRFJ_u",
  invisible: "0",
  action: "verify",
};
fetch(base_url + "/recaptcha-token", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "content-type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖Iconupdown
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  img: "Base64 img"
};
fetch(base_url + "/iconupdown", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "content-type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖IconCaptcha
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
const body = {
  img: "Base64 img"
};
fetch(base_url + "/iconcaptcha", {
  method: "POST",
  headers: { Authorization: "Bearer KEY_HERE", "content-type": "application/json" },
  body: JSON.stringify(body),
});
```
#
#### 🤖Result
```javascript
const base_url = "https://multisolver-api.vercel.app/v1/captcha";
fetch(base_url + "/result?taskId=" + "turnstile-12345", {
  headers: { Authorization: "Bearer KEY_HERE" },
});
```
