# Review Webpage Content for Information Leakage

- Review webpage comments and metadata to ﬁnd any information leakage.
- Gather JavaScript ﬁles and review the JS code to better understand the application and to ﬁnd any information leakage.
- Identify if source map ﬁles or other front-end debug ﬁles exist

## How to Test

### Review webpage comments and metadata

1. `<!—`
    
    ```html
    <div class="table2">
    <div class="col1">1</div><div class="col2">Mary</div>
    <div class="col1">2</div><div class="col2">Peter</div>
    <div class="col1">3</div><div class="col2">Joe</div>
    <!-- Query: SELECT id, name FROM app.users WHERE active='1' -->
    </div>
    ```
    
    ```html
    <!-- Use the DB administrator password for testing:
    ```
    
    Check HTML version information for valid version numbers and Data Type Deﬁnition (DTD) URLs
    
    - strict.dtd – default strict DTD
    - loose.dtd – loose DTD
    - frameset.dtd – DTD for frameset documents
2. `META`
    
    ```html
    <META name="Author" content="Andrew Muller">
    <META name="keywords" lang="en-us" content="OWASP, security, sunshine, lollipops">
    ```
    
    Although most web servers manage search engine indexing via the robots.txt ﬁle, it can also be managed by META tags. The tag below will advise robots to not index and not follow links on the HTML page containing the tag.
    
    ```html
    <META name="robots" content="none">
    ```
    

### Identifying JavaScript Code and Gathering JavaScript Files

- Look for JavaScript code between `<script>` and `</script>` tags.
- JavaScript ﬁles have the ﬁle extension `.js` and name of the JavaScript ﬁle usually put in the `src`
- Check JavaScript code for any sensitive information leaks which could be used by attackers to further abuse ormanipulate the system. Look for values such as: **API keys, internal IP addresses, sensitive routes, or credentials.** For example:
    
    ```jsx
    const myS3Credentials = {
    accessKeyId: config('AWSS3AccessKeyID'),
    secretAcccessKey: config('AWSS3SecretAccessKey'),
    };
    ```
    
- The tester may even ﬁnd something like this:
    
    ```jsx
    var conString = "tcp://postgres:1234@localhost/postgres";
    ```
    
- When an API Key is found, testers can check if the API Key restrictions are set per service or by IP, HTTP referrer,application, SDK, etc.
    
    
    For example, if testers found a Google Map API Key, they can check if this API Key is restricted by IP or restricted only per the Google Map APIs. If the Google API Key is restricted only per the Google Map APIs, attackers can still use that API Key to query unrestricted Google Map APIs and the application owner must to pay for that.
    
    ```jsx
    <script type="application/json">
    {"GOOGLE_MAP_API_KEY":"AIzaSyDUEBnKgwiqMNpDplT6ozE4Z0XxuAbqDi4",
    "RECAPTCHA_KEY":"6LcPscEUiAAAAHOwwM3fGvIx9rsPYUq62uRhGjJ0"}
    </script>
    ```
    
- In some cases, testers may ﬁnd sensitive routes from JavaScript code, such as links to internal or hidden admin pages.
    
    ```jsx
    <script type="application/json">
    "runtimeConfig":{"BASE_URL_VOUCHER_API":"[https://staging-voucher.victim.net/api](https://staging-voucher.victim.net/api)",
    	"BASE_BACKOFFICE_API":"[https://10.10.10.2/api](https://10.10.10.2/api)", "ADMIN_PAGE":"/hidden_administrator"}
    </script>
    ```
    

### Identifying Source Map Files

Source map ﬁles will usually be loaded when DevTools open. Testers can also ﬁnd source map ﬁles by adding the `.map` extension after the extension of each external JavaScript ﬁle. For example, if a tester sees a `/static/js/main.chunk.js`  ﬁle, they can then check for its source map ﬁle by visiting `/static/js/main.chunk.js.map` .

Check source map ﬁles for any sensitive information that can help the attacker gain more insight about the application. For example:

```jsx
"version": 3,
"file": "static/js/main.chunk.js",
"sources": [
"/home/sysadmin/cashsystem/src/actions/index.js",
"/home/sysadmin/cashsystem/src/actions/reportAction.js"
"/home/sysadmin/cashsystem/src/actions/cashoutAction.js",
"/home/sysadmin/cashsystem/src/actions/userAction.js",
"..."
],
"..."
}
```

When websites load source map ﬁles, the front-end source code will become readable and easier to debug.

### Tools

[https://github.com/streaak/keyhacks](https://github.com/streaak/keyhacks)
