---
layout: post
title:  "Adding cookie disclaimer"
date:   2017-10-11 11:14:17 +0100
category: [Javascript, HTML, CSS / Sass, Snippets]
tags: css snippets javascript
permalink: /adding-cookie-disclaimer/
---

## Step by step guide
---

1. Install js-cookie to the vendor folder: add js-cookie to the project bower.json file by adding this line to the list of dependencies (after jquery):

    ```
        "js-cookie": null,
    ```

    Run bower.json in the terminal

    ```
    bower install bower.json
    ```

2. Configure js-cookie and requirejs (if the project is using requirejs): add the path to js-cookie to `config.require.js` (/assets > js):

    ```
    paths: {
        ...
        'jscookie': '../vendor/js-cookie/src/js.cookie',
        ...
    }
    ```
    add to the shim in the same file:
    ```
    shim: {
        ...
        'jscookie': {
            exports: 'JScookie'
        },
        ...
    }
    ```

3. If using requirejs, create a `cookie.js` file with the module:

    ```
    define([
        'module',
        'jquery',
        'jscookie'
    ], function(module, $, cookie) {
        'use strict';

        $(document).ready(function() {
            if (!Cookies.get('YOURCOOKIENAME')) {
              $("#cookie-disclaimer").removeClass('hide');
            }
            // Set cookie
            $('#cookie-disclaimer .closeme').on("click", function() {
              Cookies.set('YOURCOOKIENAME', 'YOURCOOKIENAME-set', {
                  expires: 30
              });
            });

            $('.closeme').bind("click", function () {
                $('#cookie-disclaimer').addClass("hide");
                return false;
            });
        });

        return module;
    });
    ```

    If the project is *not* using requirejs, the snippet won't have any module definition. Make sure you include the snippet below in your Javascript:

    ```
    $(document).ready(function() {
        if (!cookie.get('YOURCOOKIENAME')) {
          $("#cookie-disclaimer").removeClass('hide');
        }
        // Set cookie
        $('#cookie-disclaimer .closeme').on("click", function() {
          cookie.set('YOURCOOKIENAME', 'YOURCOOKIENAME-set', {
              expires: 30
          });
        });

        $('.closeme').bind("click", function () {
            $('#cookie-disclaimer').addClass("hide");
            return false;
        });
    });
    ```

4. Ignore this if you're not using requirejs.
    Import `cookie.js` in the file were the require modules are imported (usually `main.js`)

    ```
    // Main
    require([
        ...
        'cookie',
        ...
    ], function(r, $) {
        'use strict';
        $(document).ready(function() {
            ...
        });
    });
    ```
    You only need to import main in `config.js` (make sure it's imported once only).

5. HTML and SCSS
    It's important to match *ids* and *classes* set in javascript. The example below matches ids and classes used in the snippet above.

    *HTML*
    ---

    ```
    <!-- Cookie disclaimer -->
    
    <div class="cookies-box hide" id="cookie-disclaimer">
      <p>We use cookies to track usage and preferences.</p>
      <p><button class="button closeme" aria-label="Dismiss alert" type="button">Accept</button></p>
      <p><a href="[URL TO PRIVACY AND COOKIE POLICY PAGE]">Privacy &amp; cookie policy</a></p>
    </div>
    ```

    *SCSS*
    ---

    * Check if your project CSS already has a class to hide elements (it could be `.hidden`, `.is-hidden`, `.hide` or similar).
    If a class already exists, make sure that your HTML and Javascript reflect it, otherwise add:

    ```
    .hide {
        display: none;
    }
    ```

    * Style the `<div>` that contains the disclaimer.

    ```
    .cookies-box {
        background: rgba(0, 0, 0, .8);
        bottom: 0;
        color: white;
        margin: 0;
        position: fixed;
        text-align: center;
        width: 100%;
        
        a {
            margin-bottom: 0;
            &.button {
                text-decoration: underline;
            }
        }
        
        a,
        button,
        .button {
            border: 0;
            color: white;
        }
        
        button {
            background: #fff;
            height: 3rem;
            line-height: 1rem;
            margin-top: .5rem;
            &:hover {
                background: #444 !important;
            }
        }
        
        p {
            margin: 1rem auto .5rem;
        }
    }
    ```

